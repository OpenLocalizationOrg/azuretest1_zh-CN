---
ms.openlocfilehash: a5ed6349cae437917cdf84d338042ac733f4b7fe
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="链接在 Azure 资源管理器的资源" 
    description="创建不同的资源组在 Azure 资源管理器中的资源之间的链接。" 
    services="azure-resource-manager" 
    documentationCenter="" 
    authors="tfitzmac" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="azure-resource-manager" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/16/2015" 
    ms.author="tomfitz"/>

# 链接在 Azure 资源管理器的资源

部署后，您可能想要查询的关系或资源之间的链接。 依赖项通知部署，但该生命周期结束部署。 一旦完成部署，有从属资源之间是没有确定的关系。

相反，Azure 资源管理器提供新功能称为资源链接建立和查询资源之间的关系。 您可以确定哪些资源链接到的资源或从资源链接的资源。 

资源链接的范围可以是订阅、 资源组或特定的资源。 这意味着资源链接可以记录的时间跨度资源组的关系。 当您开始将解决方案分解为多个模板和多个资源组，有一种机制来确定这些资源链接经证明很有用。 例如，是一种常见有数据库与自己生命驻留在一个资源组中，不同的生命周期与应用程序位于不同的资源组。 应用程序连接到数据库，以便在不同的资源组的资源之间没有链接。 

所有链接的资源必须属于相同的订阅。 每个资源可以其他资源链接到 50。 如果任何链接的资源被删除或移动，链接所有者必须清理剩余的链接。

## 在模板链接

下面的示例显示 unidirection 关系到网站、 通知集线器和 SQL 数据库的一组中创建的资源类型"Microsoft.AppService/apiapps"的模板。 

    {
        "$schema": "http://schemas.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "$system": {
                "type": "Object"
            }
        },
        "resources": [
            {
                "apiVersion": "2014-11-01",
                "type": "Microsoft.Web/sites",
                "name": "[parameters('$system').siteName]",
                "location": "[parameters('$system').location]",
                "resources": [
                    {
                        "apiVersion": "2014-11-01",
                        "name": "appsettings",
                        "type": "config",
                        "dependsOn": [
                            "[resourceId('Microsoft.NotificationHubs/namespaces/NotificationHubs', variables('notificationHubNamespace'), variables('notificationHubName'))]"
                        ],
                        "properties": {
                            "MS_MobileServiceName": "[parameters('$system').apiAppName]",
                            "MS_NotificationHubName": "[variables('notificationHubName')]",
                            "MS_NotificationHubConnectionString": "[listkeys(resourceId('Microsoft.NotificationHubs/namespaces/notificationHubs/authorizationRules', variables('notificationHubNamespace'), variables('notificationHubName'), 'DefaultFullSharedAccessSignature'), '2014-09-01').primaryConnectionString]"
                        }
                    }
                ]
            },
            {
                "apiVersion": "[parameters('$system').apiVersion]",
                "type": "Microsoft.AppService/apiapps",
                "name": "[parameters('$system').apiAppName]",
                "properties": {
                    "accessLevel": "PublicAnonymous"
                },
                "resources": [
                    {
                        "apiVersion": "2015-01-01",
                        "type": "providers/links",
                        "name": "Microsoft.Resources/mobile-codesite",
                        "dependsOn": [
                            "[resourceId('Microsoft.AppService/apiapps', parameters('$system').apiAppName)]",
                            "[resourceId('Microsoft.Web/Sites', variables('userSiteName'))]"
                        ],
                        "properties": {
                            "targetId": "[resourceId('Microsoft.Web/sites', variables('userSiteName'))]"
                        }
                    },
                    {
                        "apiVersion": "2015-01-01",
                        "type": "providers/links",
                        "name": "Microsoft.Resources/mobile-notificationhub",
                        "dependsOn": [
                            "[resourceId('Microsoft.AppService/apiapps', parameters('$system').apiAppName)]",
                            "[resourceId('Microsoft.NotificationHubs/namespaces/NotificationHubs', variables('notificationHubNamespace'), variables('notificationHubName'))]"
                        ],
                        "properties": {
                            "targetId": "[resourceId('Microsoft.NotificationHubs/namespaces/NotificationHubs', variables('notificationHubNamespace'), variables('notificationHubName'))]"
                        }
                    },
                    {
                        "apiVersion": "2015-01-01",
                        "type": "providers/links",
                        "name": "Microsoft.Resources/mobile-sqlserver",
                        "dependsOn": [
                            "[resourceId('Microsoft.AppService/apiapps', parameters('$system').apiAppName)]"
                        ],
                        "properties": {
                            "targetId": "[concat('/subscriptions/', parameters('userDatabase').subscriptionId, '/resourcegroups/', parameters('userDatabase').resourceGroupName, '/providers/Microsoft.Sql/servers/', parameters('userDatabase').serverName)]"
                        }
                    },
                    {
                        "apiVersion": "2015-01-01",
                        "type": "providers/links",
                        "name": "Microsoft.Resources/mobile-sqldb",
                        "dependsOn": [
                            "[resourceId('Microsoft.AppService/apiapps', parameters('$system').apiAppName)]"
                        ],
                        "properties": {
                            "targetId": "[concat('/subscriptions/', parameters('userDatabase').subscriptionId, '/resourcegroups/', parameters('userDatabase').resourceGroupName, '/providers/Microsoft.Sql/servers/', parameters('userDatabase').serverName, '/databases/', parameters('userDatabase').databaseName)]"
                        }
                    }
                ]
            }
        ]
    }

## 与 REST API，链接

若要定义部署的资源之间的链接，请运行︰

    PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/{provider-namespace}/{resource-type}/{resource-name}/providers/Microsoft.Resources/links/{link-name}?api-version={api-version}

您的订购 id 替换 {订阅 id}。 替换 {资源组}，{提供程序命名空间、 {资源-类型} 和 {资源名称} 标识链接中的第一个资源的值。 {链接名称} 替换为要创建的链接名称。 使用 2015年-01-01 api 版本。

在请求中，包括一个对象，定义链接中的第二个资源︰

    {
        "name": "{new-link-name}",
        "properties": {
            "targetId": "subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/{provider-namespace}/{resource-type}/{resource-name}/",
            "notes": "{link-description}",
        }
    }

属性元素包含第二个资源的标识符。

有关更多示例，包括如何检索链接信息，请参阅[链接的资源](https://msdn.microsoft.com/library/azure/mt238499.aspx)。

## 下一步行动

- 您还可以组织您的资源加上标记。 若要了解有关标记的资源，请参阅[使用标签来组织您的资源](resource-group-using-tags.md)。
- 有关如何创建模板，并定义要部署的资源的说明，请参阅[创作模板](resource-group-authoring-templates.md)。

测试

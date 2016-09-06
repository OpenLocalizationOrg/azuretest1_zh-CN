---
ms.openlocfilehash: cdb1abbb19bcdd41975066a0f638a93dc1febc15
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="使用与 Azure 资源管理器中的链接的模板"
   description="描述如何使用 Azure 资源管理器模板中链接的模板来创建模块化的模板解决方案。 演示如何将参数值传递，指定参数文件，并动态创建的 Url。"
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="wpickett"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/02/2015"
   ms.author="tomfitz"/>

# 使用与 Azure 资源管理器中的链接的模板

从在一个 Azure 资源管理器模板中，您可以链接到另一个模板，使您可以将分解成一组目标、 具有特定用途的模板部署。 就像分解应用程序分成多个代码类分解提供了测试、 重新使用，和可读性方面的好处。  

可以将主模板的参数传递到链接的模板，然后这些参数可以直接映射到的参数或变量调用模板由公开。 链接的模板还可以将输出变量传递回源模板，启用模板之间的双向数据交换。

## 链接到模板

可以通过添加指向模板链接的主模板内部署资源的两个模板之间的链接。 您的 uri 模板链接设置**templateLink**属性。 您可以提供参数值的链接的模板通过指定直接在您的模板中的值，或通过链接到参数文件。 下面的示例使用**参数**属性来直接指定参数值。

    "resources": [ 
      { 
         "apiVersion": "2015-01-01", 
         "name": "nestedTemplate", 
         "type": "Microsoft.Resources/deployments", 
         "properties": { 
           "mode": "incremental", 
           "templateLink": {
              "uri": "https://www.contoso.com/AzureTemplates/newStorageAccount.json",
              "contentVersion": "1.0.0.0"
           }, 
           "parameters": { 
              "StorageAccountName":{"value": "[parameters('StorageAccountName')]"} 
           } 
         } 
      } 
    ] 

资源管理器必须能够访问链接的模板，这意味着您不能指定一个本地文件的链接的模板。 您只可以提供一个 URI 值，包括**http**或**https**。 一个选项是将您的模板链接放在存储帐户，并为该项目，请使用的 URI 如下面所示。

    "templateLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
        "contentVersion": "1.0.0.0",
    }


## 链接到参数文件

下一个示例使用**parametersLink**属性链接到的参数文件。

    "resources": [ 
      { 
         "apiVersion": "2015-01-01", 
         "name": "nestedTemplate", 
         "type": "Microsoft.Resources/deployments", 
         "properties": { 
           "mode": "incremental", 
           "templateLink": {
              "uri":"https://www.contoso.com/AzureTemplates/newStorageAccount.json",
              "contentVersion":"1.0.0.0"
           }, 
           "parametersLink": { 
              "uri":"https://www.contoso.com/AzureTemplates/parameters.json",
              "contentVersion":"1.0.0.0"
           } 
         } 
      } 
    ] 

链接的参数文件的 URI 值不能为本地文件，并且必须包括**http**或**https**。

## 使用变量来链接模板

前面的示例显示硬编码模板链接的 URL 值。 这种方法可能适合一个简单的模板，但它不起作用时使用大量的模块化模板。 相反，可以创建一个静态变量，用于存储主模板的基 URL，然后为从该基 URL 链接的模板动态创建的 Url。 这种方法的优点是可以方便地移动或分叉模板，因为您只需更改主模板中的静态变量。 主模板传递整个分解后的模板的正确 Uri。

下面的示例演示如何使用基本 URL 创建两个 Url 的链接的模板 （**sharedTemplateUrl**和**vmTemplate**）。 

    "variables": {
        "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
        "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
        "tshirtSizeSmall": {
            "vmSize": "Standard_A1",
            "diskSize": 1023,
            "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
            "vmCount": 2,
            "slaveCount": 1,
            "storage": {
                "name": "[parameters('storageAccountNamePrefix')]",
                "count": 1,
                "pool": "db",
                "map": [0,0],
                "jumpbox": 0
            }
        }
    }

## 将值传递回从模板链接

如果您需要将一个值从模板链接传递到主模板，您可以在**输出**部分中的链接的模板创建一个值。 有关示例，请参见[Azure 资源管理器模板中的共享状态](best-practices-resource-manager-state.md)。

## 下一步行动
- [编写模板](./resource-group-authoring-templates.md)
- [部署模板](azure-portal/resource-group-template-deploy.md)

测试

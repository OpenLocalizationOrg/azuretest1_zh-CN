---
ms.openlocfilehash: ac302f1c0b61dddabece56e5acde819e416d5955
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="使用 API 的应用程序创建一个逻辑应用程序" 
    description="使用 Azure 资源管理器模板部署逻辑应用程序和 API 的应用程序。" 
    services="app-service\logic" 
    documentationCenter="" 
    authors="tfitzmac" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-logic" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/29/2015" 
    ms.author="tomfitz"/>

# 创建逻辑应用程序加上 API 应用程序使用一个模板

在本主题中，您将学习如何创建一个 Azure 资源管理器模板来创建逻辑应用程序与应用程序服务 API 的应用程序。 您可以使用阐明意图通过触发器和一系列的步骤，每个安全地处理身份验证和最佳做法，例如持久执行时调用 API 的应用程序的设计工作流逻辑的应用程序。

您将了解如何定义部署哪些资源以及如何定义参数指定当执行部署。 可以将此模板用于您自己的部署中，或自定义以满足您的要求。

逻辑应用程序属性的详细信息，请参阅[逻辑应用程序工作流管理 API](https://msdn.microsoft.com/library/azure/dn948513.aspx)。 该定义本身的示例，请参阅[作者逻辑应用程序定义](app-service-logic-author-definitions.md)。 

有关创建模板的详细信息，请参阅[创作 Azure 资源管理器模板](../resource-group-authoring-templates.md)。

完整的模板，请参阅[使用 API 的应用程序模板的逻辑应用程序](https://github.com/Azure/azure-quickstart-templates/blob/master/201-logic-app-api-app-create/azuredeploy.json)。

## 您将部署

使用此模板中，您将配置︰

- 逻辑应用程序
- API 的应用程序

若要部署都自动运行，请单击下面的按钮︰

[![D到 Azure eploy](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-logic-app-api-app-create%2Fazuredeploy.json) 

## 参数

[AZURE.INCLUDE [app-service-logic-deploy-parameters](../../includes/app-service-logic-deploy-parameters.md)]

### apiAppName

API 的应用程序的名称。

    "apiAppName": {
        "type": "string"
    }

### gatewayName

网关的名称。

    "gatewayName": {
        "type": "string"
    }

### gatewayToApiAppSecret

API 的应用程序的秘密。

    "gatewayToApiAppSecret": {
        "defaultValue": "0000000000000000000000000000000000000000000000000000000000000000",
        "type": "securestring"
    }
    
## 若要部署的资源

### 应用程序服务计划

创建应用程序服务计划。 

它被部署到该资源组使用相同的位置。

    {
        "apiVersion": "2014-06-01",
        "name": "[parameters('svcPlanName')]",
        "type": "Microsoft.Web/serverfarms",
        "location": "[resourceGroup().location]",
        "tags": {
            "displayName": "AppServicePlan"
        },
        "properties": {
            "name": "[parameters('svcPlanName')]",
            "sku": "[parameters('sku')]",
            "workerSize": "[parameters('svcPlanSize')]",
            "numberOfWorkers": 1
        }
    }

### Web 应用程序承载网关

创建 web 应用程序来承载网关。

    {
        "type": "Microsoft.Web/sites",
        "apiVersion": "2015-04-01",
        "name": "[parameters('gatewayName')]",
        "location": "[resourceGroup().location]",
        "kind": "gateway",
        "tags": {
            "displayName": "GatewayHost"
        },
        "resources": [
            {
                "type": "providers/links",
                "apiVersion": "2015-01-01",
                "name": "Microsoft.Resources/gateway",
                "dependsOn": [
                    "[resourceId('Microsoft.Web/sites',parameters('gatewayName'))]"
                ],
                "properties": {
                    "targetId": "[resourceId('Microsoft.AppService/gateways', parameters('gatewayName'))]"
                }
            }
        ],
        "dependsOn": [
            "[concat(resourceGroup().id, '/providers/Microsoft.Web/serverfarms/',parameters('svcPlanName'))]"
        ],
        "properties": {
            "name": "[parameters('gatewayName')]",
            "gatewaySiteName": "[parameters('gatewayName')]",
            "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', parameters('svcPlanName'))]",
            "siteConfig": {
                "appSettings": [
                    {
                        "name": "ApiAppsGateway_EXTENSION_VERSION",
                        "value": "latest"
                    },
                    {
                        "name": "EmaStorage",
                        "value": "D:\\home\\data\\apiapps"
                    },
                    {
                        "name": "WEBSITE_START_SCM_ON_SITE_CREATION",
                        "value": "1"
                    }
                ]
            }
        }
    }

### 网关

注意到网关，包括身份验证令牌的子资源。 此标记是逻辑应用程序用于调用到网关。

    {
        "type": "Microsoft.AppService/gateways",
        "apiVersion": "2015-03-01-preview",
        "name": "[parameters('gatewayName')]",
        "location": "[resourceGroup().location]",
        "tags": {
            "displayName": "Gateway"
        },
        "resources": [
            {
                "type": "providers/links",
                "apiVersion": "2015-01-01",
                "name": "Microsoft.Resources/gatewaySite",
                "dependsOn": [
                    "[resourceId('Microsoft.AppService/gateways',parameters('gatewayName'))]"
                ],
                "properties": {
                    "targetId": "[resourceId('Microsoft.Web/sites',parameters('gatewayName'))]"
                }
            },
            {
                "type": "tokens",
                "apiVersion": "2015-03-01-preview",
                "location": "[resourceGroup().location]",
                "name": "[parameters('logicAppName')]",
                "tags": {
                    "displayName": "AuthenticationToken"
                },
                "dependsOn": [
                    "[resourceId('Microsoft.AppService/gateways', parameters('gatewayName'))]"
                ]
            }
        ],
        "dependsOn": [
            "[resourceId('Microsoft.Web/sites', parameters('gatewayName'))]"
        ],
        "properties": {
            "host": {
                "resourceName": "[parameters('gatewayName')]"
            }
        }
    }

### Web 应用程序来承载 API 的应用程序

    {
        "type": "Microsoft.Web/sites",
        "apiVersion": "2015-04-01",
        "name": "[parameters('apiAppName')]",
        "location": "[resourceGroup().location]",
        "kind": "apiApp",
        "tags": {
            "displayName": "APIAppHost"
        },
        "dependsOn": [
            "[resourceId('Microsoft.AppService/gateways', parameters('gatewayName'))]"
        ],
        "resources": [
            {
                "type": "siteextensions",
                "tags": {
                    "displayName": "APIAppExtension"
                },
                "apiVersion": "2015-04-01",
                "name": "[variables('$packageId')]",
                "dependsOn": [
                    "[resourceId('Microsoft.Web/sites', parameters('apiAppName'))]"
                ],
                "properties": {
                    "type": "WebRoot",
                    "feed_url": "[variables('$nugetFeed')]"
                }
            },
            {
                "type": "providers/links",
                "apiVersion": "2015-01-01",
                "name": "Microsoft.Resources/apiApp",
                "dependsOn": [
                    "[resourceId('Microsoft.Web/sites', parameters('apiAppName'))]"
                ],
                "properties": {
                    "targetId": "[resourceId('Microsoft.AppService/apiapps', parameters('apiAppName'))]"
                }
            }
        ],
        "properties": {
            "name": "[parameters('apiAppName')]",
            "gatewaySiteName": "[parameters('gatewayName')]",
            "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', parameters('svcPlanName'))]",
            "siteConfig": {
                "appSettings": [
                    {
                        "name": "EMA_MicroserviceId",
                        "value": "[parameters('apiAppName')]"
                    },
                    {
                        "name": "EMA_Secret",
                        "value": "[parameters('gatewayToAPIappSecret')]"
                    },
                    {
                        "name": "EMA_RuntimeUrl",
                        "value": "[concat('https://', reference(resourceId('Microsoft.Web/sites', parameters('gatewayName'))).hostNames[0])]"
                    },
                    {
                        "name": "WEBSITE_START_SCM_ON_SITE_CREATION",
                        "value": "1"
                    }
                ]
            }
        }
    }

### API 的应用程序

    {
        "type": "Microsoft.AppService/apiapps",
        "apiVersion": "2015-03-01-preview",
        "name": "[parameters('apiAppName')]",
        "location": "[resourceGroup().location]",
        "tags": {
            "displayName": "APIApp"
        },
        "resources": [
            {
                "type": "providers/links",
                "apiVersion": "2015-01-01",
                "name": "Microsoft.Resources/apiAppSite",
                "dependsOn": [
                    "[resourceId('Microsoft.AppService/apiapps', parameters('apiAppName'))]"
                ],
                "properties": {
                    "targetId": "[resourceId('Microsoft.Web/sites', parameters('apiAppName'))]"
                }
            }
        ],
        "dependsOn": [
            "[resourceId('Microsoft.Web/sites/siteextensions', parameters('apiAppName'), 'Microsoft.ApiApp')]"
        ],
        "properties": {
            "package": {
                "id": "Microsoft.ApiApp"
            },
            "host": {
                "resourceName": "[parameters('apiAppName')]"
            },
            "gateway": {
                "resourceName": "[parameters('gatewayName')]"
            },
            "dependencies": [ ]
        }
    }

### 逻辑应用程序

创建逻辑应用程序。

逻辑应用程序要求的名称、 位置 （其中列出了该应用程序服务计划） 的 SKU，定义，以及参数 （可选）。

逻辑应用程序使用该标记来调用该网关的通知。

    {
        "type": "Microsoft.Logic/workflows",
        "apiVersion": "2015-02-01-preview",
        "name": "[parameters('logicAppName')]",
        "location": "[resourceGroup().location]",
        "tags": {
            "displayName": "LogicApp"
        },
        "dependsOn": [
            "[resourceId('Microsoft.AppService/apiApps', parameters('apiAppName'))]"
        ],
        "properties": {
            "sku": {
                "name": "[parameters('sku')]",
                "plan": {
                    "id": "[concat(resourceGroup().id, '/providers/Microsoft.Web/serverfarms/',parameters('svcPlanName'))]"
                }
            },
            "definition": {
                "$schema": "http://schema.management.azure.com/providers/Microsoft.Logic/schemas/2014-12-01-preview/workflowdefinition.json#",
                "contentVersion": "1.0.0.0",
                "parameters": {
                    "token": {
                        "defaultValue": "[reference(resourceId('Microsoft.AppService/gateways/tokens', parameters('gatewayName'), parameters('logicAppName'))).token]",
                        "type": "String",
                        "metadata": {
                            "token": {
                                "name": "token"
                            }
                        }
                    }
                },
                "triggers": {
                    "recurrence": {
                        "type": "recurrence",
                        "recurrence": {
                            "frequency": "Hour",
                            "interval": 1
                        }
                    }
                },
                "actions": {
                    "getValues": {
                        "type": "ApiApp",
                        "inputs": {
                            "apiVersion": "2015-01-14",
                            "host": {
                                "id": "[concat(resourceGroup().id, '/providers/Microsoft.AppServices/apiApps/',parameters('apiAppName'))]",
                                "gateway": "[concat('https://', reference(resourceId('Microsoft.Web/sites', parameters('gatewayName'))).hostNames[0])]"
                            },
                            "operation": "Values_Get",
                            "parameters": { },
                            "authentication": {
                                "type": "Raw",
                                "scheme": "Zumo",
                                "parameter": "@parameters('token')"
                            }
                        }
                    }
                },
                "outputs": {
                    "result": {
                        "type": "array",
                        "value": "@body('readValues')"
                    }
                }
            },
            "parameters": { }
        }
    }

## 要运行部署命令

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### PowerShell

    New-AzureResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-logic-app-api-app-create/azuredeploy.json -ResourceGroupName ExampleDeployGroup

### Azure CLI

    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-logic-app-api-app-create/azuredeploy.json -g ExampleDeployGroup


 

测试

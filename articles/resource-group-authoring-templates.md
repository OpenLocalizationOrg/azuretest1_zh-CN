---
ms.openlocfilehash: 2f8dac1c9676db04b7e6050b12724580cf3a2141
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="Azure 的资源管理器模板创作"
   description="创建使用 JSON 的声明性语法来部署应用程序到 Azure 的 Azure 资源管理器模板。"
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
   ms.date="08/20/2015"
   ms.author="tomfitz"/>

# 创作的 Azure 资源管理器模板

Azure 应用程序通常需要的资源 （如数据库服务器、 数据库或网站） 的组合来满足所需的目标。 而不是部署并分别管理每个资源，您可以创建部署和规定的所有应用程序在单一的协调操作中的资源的 Azure 资源管理器模板。 在模板中，您定义应用程序所需的资源，并指定部署参数输入不同的环境的值。 模板包含 JSON 和您可以使用它来构建您的部署的值的表达式。

本主题描述模板的部分。 对于实际的架构中，请参阅[Azure 资源管理器的架构](https://github.com/Azure/azure-resource-manager-schemas)。

您必须限制大小为 1 MB，您的模板和 64 KB 到每个参数文件。 它已展开与迭代资源定义和变量和参数的值后，1 MB 的限制将应用于模板的最终状态。 

## 模板格式

下面的示例演示一个模板的基本结构组成的部分。

    {
       "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "",
       "parameters": {  },
       "variables": {  },
       "resources": [  ],
       "outputs": {  }
    }

| 元素名称   | 是否必需 | 说明
| :------------: | :------: | :----------
| $schema        |   是    | 描述模板语言的版本的 JSON 架构文件的位置。
| contentVersion |   是    | 版本 （如 1.0.0.0) 的模板。 在部署时使用该模板资源，则可以使用此值以确保正在使用的模板。
| parameters     |   否     | 若要自定义的资源部署在执行部署时提供的值。
| 变量      |   否     | 作为 JSON 片断模板中用于简化模板语言中表达式的值。
| 资源      |   是    | 类型的服务部署或更新的资源组中。
| 输出        |   否     | 在部署之后返回的值。

我们将讨论此主题的后面部分更详细地模板的部分。 现在，我们将回顾一些构成该模板的语法。

## 表达式和函数

该模板的基本语法是 JSON;但是，表达式和函数扩展模板中使用 JSON，使您可以创建不是严格的文本值的值。 表达式的两边添加方括号 （[和]），并部署模板时进行评估。 表达式可以出现在 JSON 字符串值中的任意位置，并始终返回另一个 JSON 值。 如果您需要使用原义字符串开头方括号 [，必须使用两个方括号 [[。

通常情况下，您使用表达式函数来执行操作来配置部署。 就像在 JavaScript 中，函数调用的格式设置为**functionName(arg1,arg2,arg3)**。 您可以通过使用点和 [索引] 运算符引用的属性。

下面的列表显示了常见的功能。

- **parameters(parameterName)**

    返回执行部署时提供参数值。

- **variables(variableName)**

    返回模板中定义的变量。

- **concat(arg1,arg2,arg3,...)**

    合并多个字符串值。 此函数可以执行任意数量的参数。

- **base64(inputString)**

    返回输入字符串的 base64 表示。

- **resourceGroup()**

    返回表示当前资源组的结构化的对象 （与 id、 名称和位置的属性）。

- **资源 Id ([resourceGroupName] 资源类型，resourceName1，[resourceName2]...)**

    返回资源的唯一标识符。 可用于从另一个资源组中检索资源。

下面的示例演示如何使用函数的几个构造值时︰
 
    "variables": {
       "location": "[resourceGroup().location]",
       "usernameAndPassword": "[concat('parameters('username'), ':', parameters('password'))]",
       "authorizationHeader": "[concat('Basic ', base64(variables('usernameAndPassword')))]"
    }

现在，您充分了解表达式和函数了解模板的部分。 所有的模板功能，包括参数和返回的值的格式的更多详细信息请参阅[Azure 资源管理器模板函数](./resource-group-template-functions.md)。 


## 参数

在模板的参数部分中，您可以指定在部署资源时，用户可以输入的值。 可以使用这些模板中的参数值设置为已部署的资源的值。 只有在参数部分中声明的参数可以用模板的其他部分。

在参数部分，不能使用参数值来构造另一个参数值。 这种操作通常发生在变量部分。

定义参数具有以下结构︰

    "parameters": {
       "<parameterName>" : {
         "type" : "<type-of-parameter-value>",
         "defaultValue": "<optional-default-value-of-parameter>",
         "allowedValues": [ "<optional-array-of-allowed-values>" ]
       }
    }

| 元素名称   | 是否必需 | 说明
| :------------: | :------: | :----------
| 参数名称  |   是    | 参数的名称。 必须是有效的 JavaScript 标识符。
| 类型           |   是    | 参数值的类型。 请参阅下面的允许的类型的列表。
| 默认值   |   否     | 对于参数，如果未提供值的参数的默认值。
| allowedValues  |   否     | 允许的值的参数，以确保其中提供的正确值的数组。

允许的类型和值是︰

- 字符串或 secureString 的任何有效的 JSON 字符串
- int 的 JSON 的任何有效整数
- 布尔值的任何有效的 JSON 布尔值
- 对象或 secureObject 的任何有效的 JSON 对象
- 数组的任何有效的 JSON 数组


>[AZURE.NOTE] 所有的密码、 密钥和其他机密信息，应使用**secureString**类型。 在资源部署后无法读取与 secureString 类型的模板参数。 

下面的示例演示如何定义参数︰

    "parameters": {
       "siteName": {
          "type": "string"
       },
       "siteLocation": {
          "type": "string"
       },
       "hostingPlanName": {
          "type": "string"
       },  
       "hostingPlanSku": {
          "type": "string",
          "allowedValues": [
            "Free",
            "Shared",
            "Basic",
            "Standard",
            "Premium"
          ],
          "defaultValue": "Free"
       }
    }

## 变量

在变量部分中，您构建可用于简化模板语言中表达式的值。 通常情况下，这些变量将基于提供的参数值。

下面的示例演示如何定义一个变量来构造的两种参数值︰

    "parameters": {
       "username": {
         "type": "string"
       },
       "password": {
         "type": "secureString"
       }
     },
     "variables": {
       "connectionString": "[concat('Name=', parameters('username'), ';Password=', parameters('password'))]"
    }

下一个示例中显示的变量是一个复杂的 JSON 类型和构造从其它变量的变量︰

    "parameters": {
       "environmentName": {
         "type": "string",
         "allowedValues": [
           "test",
           "prod"
         ]
       }
    },
    "variables": {
       "environmentSettings": {
         "test": {
           "instancesSize": "Small",
           "instancesCount": 1
         },
         "prod": {
           "instancesSize": "Large",
           "instancesCount": 4
         }
       },
       "currentEnvironmentSettings": "[variables('environmentSettings')[parameters('environmentName')]]",
       "instancesSize": "[variables('currentEnvironmentSettings').instancesSize",
       "instancesCount": "[variables('currentEnvironmentSettings').instancesCount"
    }

## 资源

在资源部分中，您可以定义资源进行部署或更新。

定义资源具有以下结构︰

    "resources": [
       {
         "apiVersion": "<api-version-of-resource>",
         "type": "<resource-provider-namespace/resource-type-name>",
         "name": "<name-of-the-resource>",
         "location": "<location-of-resource>",
         "tags": "<name-value-pairs-for-resource-tagging>",
         "dependsOn": [
           "<array-of-related-resource-names>"
         ],
         "properties": "<settings-for-the-resource>",
         "resources": [
           "<array-of-dependent-resources>"
         ]
       }
    ]

| 元素名称             | 是否必需 | 说明
| :----------------------: | :------: | :----------
| apiVersion               |   是    | 支持资源的 api 版本。 可用版本和架构的资源，请参阅[Azure 资源管理器的架构](https://github.com/Azure/azure-resource-manager-schemas)。
| 类型                     |   是    | 资源的类型。 此值是资源提供者和资源提供程序支持的资源类型的命名空间的组合。
| name                     |   是    | 资源的名称。 该名称必须遵循 RFC3986 中定义的 URI 组件限制。
| 位置                 |   否     | 受支持的提供的资源的地理位置。
| 标记                     |   否     | 与资源相关联的标记。
| 取决于                |   否     | 取决于所定义的资源的资源。 在计算资源之间的依赖关系和其依存顺序部署资源。 当资源不是相互依存的时它们都试图并行部署。 值可以是以逗号分隔列表中的资源名称或资源的唯一标识符。
| 属性               |   否     | 资源的特定配置设置。
| 资源                |   否     | 取决于所定义的资源的子资源。

如果资源名称不是唯一的您可以使用**资源 Id** helper 函数 （如下所述） 可获得的任何资源的唯一标识符。

下面的示例演示一个**Microsoft.Web/serverfarms**资源和具有嵌套的**扩展**资源的**Microsoft.Web/sites**资源︰

    "resources": [
        {
          "apiVersion": "2014-06-01",
          "type": "Microsoft.Web/serverfarms",
          "name": "[parameters('hostingPlanName')]",
          "location": "[resourceGroup().location]",
          "properties": {
              "name": "[parameters('hostingPlanName')]",
              "sku": "[parameters('hostingPlanSku')]",
              "workerSize": "0",
              "numberOfWorkers": 1
          }
        },
        {
          "apiVersion": "2014-06-01",
          "type": "Microsoft.Web/sites",
          "name": "[parameters('siteName')]",
          "location": "[resourceGroup().location]",
          "tags": {
              "environment": "test",
              "team": "ARM"
          },
          "dependsOn": [
              "[resourceId('Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
          ],
          "properties": {
              "name": "[parameters('siteName')]",
              "serverFarm": "[parameters('hostingPlanName')]"
          },
          "resources": [
              {
                  "apiVersion": "2014-06-01",
                  "type": "Extensions",
                  "name": "MSDeploy",
                  "properties": {
                    "packageUri": "https://auxmktplceprod.blob.core.windows.net/packages/StarterSite-modified.zip",
                    "dbType": "None",
                    "connectionString": "",
                    "setParameters": {
                      "Application Path": "[parameters('siteName')]"
                    }
                  }
              }
          ]
        }
    ]


## 输出

在输出部分中，您可以指定部署从返回的值。 例如，可以返回来访问部署的资源的 URI。

下面的示例演示输出定义的结构︰

    "outputs": {
       "<outputName>" : {
         "type" : "<type-of-output-value>",
         "value": "<output-value-expression>",
       }
    }

| 元素名称   | 是否必需 | 说明
| :------------: | :------: | :----------
| outputName     |   是    | 输出值的名称。 必须是有效的 JavaScript 标识符。
| 类型           |   是    | 输出值的类型。 输出值支持模板输入参数为相同的类型。
| value          |   是    | 模板语言表达式，它将计算并返回作为输出值。


下面的示例显示在输出部分中返回一个值。

    "outputs": {
       "siteUri" : {
         "type" : "string",
         "value": "[concat('http://',reference(resourceId('Microsoft.Web/sites', parameters('siteName'))).hostNames[0])]"
       }
    }

## 更高级的方案。
本主题提供介绍性描述模板。 但是，您的方案可能要求更高级的任务。

您可能需要合并在一起的两个模板或使用子模板内的父模板。 有关详细信息，请参阅[使用链接的模板使用 Azure 资源管理器中](resource-group-linked-templates.md)。

以指定的次数循环时创建的资源类型，请参阅[创建资源 Azure 资源管理器中的多个实例](resource-group-create-multiple.md)。

您可能需要使用不同的资源组中存在的资源。 在使用存储帐户或共享跨多个资源组的虚拟网络时，这是常见的。 有关详细信息，请参阅[资源 Id 函数](../resource-group-template-functions#resourceid)。

## 完整的模板
以下模板部署 web 应用程序，并规定该.zip 文件中的代码。 

    {
       "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "1.0.0.0",
       "parameters": {
         "siteName": {
           "type": "string"
         },
         "hostingPlanName": {
           "type": "string"
         },
         "hostingPlanSku": {
           "type": "string",
           "allowedValues": [
             "Free",
             "Shared",
             "Basic",
             "Standard",
             "Premium"
           ],
           "defaultValue": "Free"
         }
       },
       "resources": [
         {
           "apiVersion": "2014-06-01",
           "type": "Microsoft.Web/serverfarms",
           "name": "[parameters('hostingPlanName')]",
           "location": "[resourceGroup().location]",
           "properties": {
             "name": "[parameters('hostingPlanName')]",
             "sku": "[parameters('hostingPlanSku')]",
             "workerSize": "0",
             "numberOfWorkers": 1
           }
         },
         {
           "apiVersion": "2014-06-01",
           "type": "Microsoft.Web/sites",
           "name": "[parameters('siteName')]",
           "location": "[resourceGroup().location]",
           "tags": {
             "environment": "test",
             "team": "ARM"
           },
           "dependsOn": [
             "[resourceId('Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
           ],
           "properties": {
             "name": "[parameters('siteName')]",
             "serverFarm": "[parameters('hostingPlanName')]"
           },
           "resources": [
             {
               "apiVersion": "2014-06-01",
               "type": "Extensions",
               "name": "MSDeploy",
               "dependsOn": [
                 "[resourceId('Microsoft.Web/sites', parameters('siteName'))]"
               ],
               "properties": {
                 "packageUri": "https://auxmktplceprod.blob.core.windows.net/packages/StarterSite-modified.zip",
                 "dbType": "None",
                 "connectionString": "",
                 "setParameters": {
                   "Application Path": "[parameters('siteName')]"
                 }
               }
             }
           ]
         }
       ],
       "outputs": {
         "siteUri": {
           "type": "string",
           "value": "[concat('http://',reference(resourceId('Microsoft.Web/sites', parameters('siteName'))).hostNames[0])]"
         }
       }
    }

## 下一步行动
- 您可以使用从模板中的函数的详细信息，请参阅[Azure 资源管理器模板函数](resource-group-template-functions.md)
- 如何部署已创建的模板，请参见[部署应用程序使用 Azure 资源管理器模板](azure-portal/resource-group-template-deploy.md)
- 部署应用程序的详细示例，请参见[提供和部署可预知在 Azure 中的 microservices](app-service-web/app-service-deploy-complex-application-predictably.md)
- 若要查看可用的架构，请参阅[Azure 资源管理器的架构](https://github.com/Azure/azure-resource-manager-schemas)

测试

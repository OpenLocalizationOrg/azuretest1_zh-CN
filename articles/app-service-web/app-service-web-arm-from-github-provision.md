---
ms.openlocfilehash: 1a98e33932165cfff7f78da0fd7c0adcb2cad696
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="部署 web 应用程序链接到 GitHub 存储库" 
    description="使用 Azure 资源管理器模板来部署 web 应用程序，其中包含从 GitHub 存储库中的项目。" 
    services="app-service\web" 
    documentationCenter="" 
    authors="tfitzmac" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/29/2015" 
    ms.author="tomfitz"/>

# 部署 web 应用程序链接到 GitHub 存储库

在本主题中，您将学习如何创建部署 web 应用程序链接到 GitHub 存储库中的项目的 Azure 资源管理器模板。 您将了解如何定义部署哪些资源以及如何定义参数指定当执行部署。 可以将此模板用于您自己的部署中，或自定义以满足您的要求。

有关创建模板的详细信息，请参阅[创作 Azure 资源管理器模板](../resource-group-authoring-templates.md)。

完整的模板，请参阅[Web 应用程序链接 GitHub 模板](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-github-deploy/azuredeploy.json)。

## 您将部署

使用此模板中，您将部署 web 应用程序包含在 GitHub 从项目中的代码。

若要部署都自动运行，请单击下面的按钮︰

[![D到 Azure eploy](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-github-deploy%2Fazuredeploy.json)

## 参数

[AZURE.INCLUDE [app-service-web-deploy-web-parameters](../../includes/app-service-web-deploy-web-parameters.md)]

### repoURL

GitHub 存储库，其中包含要部署的项目中的 URL。 此参数包含一个默认值，但该值仅用于向您展示如何为存储库中提供的 URL。 测试模板时，您可以使用此值，但想要使用该模板时自己的存储库中提供的 URL。

    "repoURL": {
        "type": "string",
        "defaultValue": "https://github.com/davidebbo-test/Mvc52Application.git"
    }

### 分支

在部署应用程序时要使用的资料库分支。 默认值是主，但您可以提供您要部署的存储库中所有分支的名称。

    "branch": {
        "type": "string",
        "defaultValue": "master"
    }
    
## 若要部署的资源

[AZURE.INCLUDE [app-service-web-deploy-web-host](../../includes/app-service-web-deploy-web-host.md)]

### Web 应用程序

创建链接到项目中，GitHub 的 web 应用程序。 

指定通过**网站名称**参数，使 web 应用程序的名称和 web 应用程序通过**siteLocation**参数的位置。 在**取决于**元素中，模板定义为依赖于宿主计划服务 web 应用程序。 因为它是依赖于宿主的计划，使 web 应用程序不会创建托管计划正在创建完成之前。 **取决于**元素仅用于指定部署顺序。 如果未标记为依赖于宿主的规划使 web 应用程序，Azure 资源管理器将尝试同时创建这两个资源，如果 web 应用程序创建的托管计划之前，您可能会收到错误。

使 web 应用程序还具有下面**资源**部分中定义的子资源。 此子资源定义为与 web 应用程序部署项目的源代码管理。 在此模板中，源控件链接到特定的 GitHub 资料库。 GitHub 存储库中定义的代码**"RepoUrl":"https://github.com/davidebbo-test/Mvc52Application.git"**您可能硬编码为资源库 URL，当您想要创建一个模板，反复要求参数的最小数目的同时部署单个项目。
而不是硬编码的资源库 URL，可以添加资源库 URL 的参数和使用的**RepoUrl**属性的值。 您可以看到在[Web 应用程序与 Web 作业模板](../app-service-web-deploy-web-app-with-webjobs.md)存储库 URL 参数的一个示例。

    {
      "apiVersion":"2015-04-01",
      "name":"[parameters('siteName')]",
      "type":"Microsoft.Web/sites",
      "location":"[parameters('siteLocation')]",
      "dependsOn":[
         "[resourceId('Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
      ],
      "properties":{
        "serverFarmId":"[parameters('hostingPlanName')]"
      },
       "resources":[
         {
           "apiVersion":"2015-04-01",
           "name":"web",
           "type":"sourcecontrols",
           "dependsOn":[
             "[resourceId('Microsoft.Web/Sites', parameters('siteName'))]"
           ],
           "properties":{
             "RepoUrl":"https://github.com/davidebbo-test/Mvc52Application.git",
             "branch":"master"
           }
         }
       ]
     }

## 要运行部署命令

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### PowerShell

    New-AzureResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json -siteName ExampleSite -hostingPlanName ExamplePlan -siteLocation "West US" -ResourceGroupName ExampleDeployGroup

### Azure CLI

    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json


 

测试

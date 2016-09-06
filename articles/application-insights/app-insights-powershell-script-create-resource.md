---
ms.openlocfilehash: 0a26720999969905eb29b11b97175d09cc2cca47
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="PowerShell 脚本来创建应用程序的见解资源" 
    description="自动创建应用程序建议的资源。" 
    services="application-insights" 
    documentationCenter="windows"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/17/2015" 
    ms.author="awills"/>

#  PowerShell 脚本来创建应用程序的见解资源

*在预览是应用程序的见解。*

当您想要监视新的应用程序-或新版本的应用程序-与[Visual Studio 应用程序见解](https://azure.microsoft.com/services/application-insights/)时，您设置 Microsoft Azure 中的新资源。 此资源是从您的应用程序的遥测数据是分析并显示在其中。 

您可以通过使用 PowerShell 自动化创建了一个新的资源。

例如，如果正在开发移动设备的应用程序，很可能，在任何时候，有几个已发布的版本的应用程序正在使用您的客户。 不想遥测结果得到混合起来的不同版本。 因此您获得您的生成过程以创建为每个生成新的资源。

## 要创建应用程序的见解资源脚本

*Output*

* 应用程序的见解名称 = erimattestapp
* IKey = 00000000-0000-0000-0000-000000000000

*PowerShell 脚本*  

```PowerShell

cls


##################################################################
# Set Values
##################################################################

#If running manually, comment this out before the first execution to login to the Azure Portal
#Add-AzureAccount

#Set the name of the Application Insights Resource
$appInsightsName = "TestApp"

#Set the application name used for the value of the Tag "AppInsightsApp" - http://azure.microsoft.com/documentation/articles/azure-preview-portal-using-tags/
$applicationTagName = "MyApp"

#Set the name of the Resource Group to use.  By default will use the application name as a starter
$resourceGroupName = "MyAppResourceGroup"

##################################################################
# Create the Resource and Output the name and iKey
##################################################################
#Set the script to Resource Manager - http://azure.microsoft.com/documentation/articles/powershell-azure-resource-manager/
Switch-AzureMode AzureResourceManager

#Select the azure subscription
Select-AzureSubscription -SubscriptionName "MySubscription"

#Create the App Insights Resource
$resource = New-AzureResource -Name $appInsightsName -ResourceGroupName $resourceGroupName -Tag @{ Name = "AppInsightsApp"; Value = $applicationTagName} -ResourceType "Microsoft.Insights/Components" -Location "Central US" -ApiVersion "2014-08-01" -PropertyObject @{"Type"="ASP.NET"} -Force 

#Give team owner access - http://azure.microsoft.com/documentation/articles/role-based-access-control-powershell/
New-AzureRoleAssignment -Mail "myTeam@fabrikam.com" -RoleDefinitionName Owner -Scope $resource.ResourceId | Out-Null

#Display iKey
Write-Host "App Insights Name = " $resource.Properties["Name"]
Write-Host "IKey = " $resource.Properties["InstrumentationKey"]

```

## 如何处理 iKey

每个资源都由其检测参数 (iKey) 标识。 IKey 是该资源创建脚本的输出。 生成脚本应提供在您的应用程序中嵌入到应用程序的见解 SDK iKey。

有两种方法，使 iKey SDK:
  
* 在[ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md): 
 * `<instrumentationkey>`*ikey*`</instrumentationkey>`
* 或在[初始化代码](app-insights-api-custom-events-metrics.md)中︰ 
 * `Microsoft.ApplicationInsights.Extensibility.
    TelemetryConfiguration.Active.InstrumentationKey = "`*iKey*`";`





 
测试

---
ms.openlocfilehash: ce44c8005d110f9968edb3b6a4cd79fcb9063ebe
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="单独的应用程序理解资源用于开发、 测试和生产" 
    description="监视的性能和用法的不同阶段开发的应用程序" 
    services="application-insights" 
    documentationCenter=""
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/02/2015" 
    ms.author="awills"/>

# 单独的应用程序理解资源的开发、 测试和生产


若要避免混合使用了遥测与调试，测试和生产版本的应用程序，创建独立[应用程序的见解][启动]资源，以接收来自每个版本的数据。

从应用程序接收到的数据存储和处理应用程序理解 Microsoft Azure 的*资源*中。 每个资源都由*检测键。* 在您的应用程序，该密钥提供给应用程序深入 SDK 以便它可以发送的数据，它可以收集到相应的资源。 可以在代码中或在 ApplicationInsights.config 中提供密钥。 通过更改在 SDK 中的键，可以将数据复制到不同的资源。 


## 创建应用程序的见解资源
  

在[portal.azure.com](https://portal.azure.com)中，添加应用程序理解的资源︰

![单击新的应用程序的见解](./media/app-insights-create-new-resource/01-new.png)


* **应用程序类型**将影响概述刀片和[公制的资源管理器][测量数据]中可用的属性上看到的内容。 如果您看不到您的应用程序类型，选择一个 web 页的网站类型和电话的其他设备类型之一。
* **资源组**是管理属性，如[访问控制](app-insights-resources-roles-access-control.md)提供了便利。 您可以使用单独的资源组用于开发、 测试和生产。
* **订阅**是 Azure 您的付款帐户。
* **位置**是，我们在其中保存您的数据。 目前无法更改它。
* **添加到 startboard**将所需的资源的快速访问拼贴放在 Azure 主页。 

创建资源需要几秒钟。 完成后，您将看到一个警报。

（您可以编写[PowerShell 脚本](app-insights-powershell-script-create-resource.md)自动创建的资源）。


## 复制检测密钥

检测关键字标识了您所创建的资源。 

![单击基础，单击检测项，CTRL + C](./media/app-insights-create-new-resource/02-props.png)

您将需要您的应用程序将数据发送给它的所有资源的检测项。


## <a name="dynamic-ikey"></a> 动态检测键

通常 SDK 获取 ApplicationInsights.config iKey。 相反，使其更容易通过设置它在代码中更改。

在初始化方法中，如 ASP.NET 服务中的 global.aspx.cs 设置项︰

*C#*

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey = 
          // - for example -
          WebConfigurationManager.AppSettings["ikey"];
      ...

在此示例中，ikeys 不同的资源放置在不同版本的 web 配置文件中。 换用 web 配置文件会交换的目标资源。

#### Web 页

IKey 用于您的应用程序中的 web 页，[你从快速入门刀片式服务器的脚本](app-insights-javascript.md)。 而不是编码它字面意义到脚本，生成它的服务器状态。 例如，在 ASP.NET 应用程序︰

*JavaScript 在 Razor 中*

    <script type="text/javascript">
    // Standard Application Insights web page script:
    var appInsights = window.appInsights || function(config){ ...
    // Modify this part:
    }({instrumentationKey:  
      // Generate from server property:
      @Microsoft.ApplicationInsights.Extensibility.
         TelemetryConfiguration.Active.InstrumentationKey"
    }) // ...





<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[诊断]: app-insights-diagnostic-search.md
[指标]: app-insights-metrics-explorer.md
[start]: app-insights-get-started.md

 
测试

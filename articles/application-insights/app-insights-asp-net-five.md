---
ms.openlocfilehash: 377dcec991e4f063ba49d8ee13f97edaf8c29f39
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="应用程序的 ASP.NET 5 的见解" 
    description="监视 web 应用程序的可用性、 性能和使用情况。" 
    services="application-insights" 
    documentationCenter=".net"
    authors="alancameronwills" 
    manager="ronmart"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2015" 
    ms.author="awills"/>

# 应用程序的 ASP.NET 5 的见解

Visual Studio 应用程序见解让您监视 web 应用程序的可用性、 性能和使用情况。 与处于放任状态获得关于性能和您的应用程序的有效性的反馈意见，您可以在每个开发周期中做出明智的选择，关于设计的方向。

![示例](./media/app-insights-asp-net-five/sample.png)

您将需要与[Microsoft Azure](http://azure.com)的订阅。 使用 Microsoft 帐户，可能具有 Windows、 XBox Live，或其他 Microsoft 云服务的登录。 

## 创建一个 ASP.NET 5 项目

...如果已经没有这样的。 

在 Visual Studio 2015年使用标准的 ASP.NET 5 项目模板。


## 创建应用程序的见解资源

在[Azure 门户网站][门户]中，创建新的应用程序理解资源。 选择 ASP.NET 选项。

![单击新建开发商服务，应用程序的见解](./media/app-insights-asp-net-five/01-new-asp.png)

打开[资源][角色]刀片式服务器都将有关您的应用程序看到的性能和使用情况数据。 若要返回到它下次您登录到 Azure，您应该开始在屏幕上发现它平铺。 或者单击浏览进行查找。

选择的应用程序类型设置资源刀片和属性的默认内容[指标]的[测量数据资源管理器中]可见。

##  用检测密钥来配置项目。

从您的应用程序理解资源复制密钥︰

![单击属性，选择的注册表项，然后按 ctrl + C](./media/app-insights-asp-net-five/02-props-asp.png)

在 ASP.NET 5 项目中，将其粘贴到`config.json`:

    {
      "ApplicationInsights": {
        "InstrumentationKey": "11111111-2222-3333-4444-555555555555"
      }
    }

或者，如果您希望使用为动态配置，您可以将此代码添加到应用程序的启动类︰

    configuration.AddApplicationInsightsSettings(
      instrumentationKey: "11111111-2222-3333-4444-555555555555");


## 向项目中添加应用程序的见解


#### NuGet 程序包的引用

查找[最新的版本数](https://github.com/Microsoft/ApplicationInsights-aspnet5/releases)NuGet 程序包。

打开`project.json`和编辑`dependencies`部分中︰

    {
      "dependencies": {
        // Replace 0.* with a specific version:
        "Microsoft.ApplicationInsights.AspNet": "0.*",

       // Add these if they aren't already there:
       "Microsoft.Framework.ConfigurationModel.Interfaces": "1.0.0-beta4",
       "Microsoft.Framework.ConfigurationModel.Json":  "1.0.0-beta4"
      }
    }

#### 分析配置文件

In `startup.cs`:

    using Microsoft.ApplicationInsights.AspNet;

    public IConfiguration Configuration { get; set; }

在`Startup`方法︰

    public Startup(IHostingEnvironment env, IApplicationEnvironment appEnv)
    {
        // Setup configuration sources.
        var builder = new ConfigurationBuilder(appEnv.ApplicationBasePath)
            .AddJsonFile("config.json")
            .AddJsonFile($"config.{env.EnvironmentName}.json", optional: true);
        builder.AddEnvironmentVariables();

        if (env.IsEnvironment("Development"))
        {
            builder.AddApplicationInsightsSettings(developerMode: true);
        }
    
        Configuration = builder.build();
    }

在`ConfigurationServices`方法︰

    services.AddApplicationInsightsTelemetry(Configuration);

在`Configure`方法︰

    // Add Application Insights monitoring to the request pipeline as a very first middleware.
    app.UseApplicationInsightsRequestTelemetry();

    // Any other error handling middleware goes here.

    // Add Application Insights exceptions handling to the request pipeline.
    app.UseApplicationInsightsExceptionTelemetry();

## 添加 JavaScript 客户端工具

如果您有一个 _Layout.cshtml 文件，插入以下代码。 否则，将代码放入您要跟踪的任何页。

在该文件中的最顶端定义注入︰

    @inject Microsoft.ApplicationInsights.Extensibility.TelemetryConfiguration TelemetryConfiguration

插入的 Html 帮助器之前`</head>`标记，和之前的任何其他脚本。 从页中要报告任何自定义 JavaScript 遥测应该注入后此代码段︰

    <head> 

      @Html.ApplicationInsightsJavaScript(TelemetryConfiguration) 

      <!-- other scripts -->
    </head>

## 运行您的应用程序

调试您的应用程序在 Visual Studio 中，或将其发布到 web 服务器。

## 查看有关您的应用程序的数据

返回到[Azure 门户]，[门户网站]，浏览到您的应用程序理解资源。 如果没有概述刀片式服务器中的数据，等待一分钟或两个，然后单击刷新。 

* [跟踪您的应用程序使用][使用]
* [诊断性能问题][检测]
* [设置 web 测试可用性监视][可用性]



## 打开源

[阅读并参加代码](https://github.com/Microsoft/ApplicationInsights-aspnet5)


<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apikey]: app-insights-api-custom-events-metrics.md#ikey
[可用性]: app-insights-monitor-web-app-availability.md
[azure]: ../insights-perf-analytics.md
[客户端]: app-insights-javascript.md
[检测]: app-insights-detect-triage-diagnose.md
[诊断]: app-insights-diagnostic-search.md
[knowUsers]: app-insights-overview-usage.md
[指标]: app-insights-metrics-explorer.md
[netlogs]: app-insights-asp-net-trace-logs.md
[性能]: app-insights-web-monitor-performance.md
[门户网站]: http://portal.azure.com/
[通过]: app-insights-troubleshoot-faq.md
[角色]: app-insights-resources-roles-access-control.md
[start]: app-insights-get-started.md
[使用]: app-insights-web-track-usage.md 

测试

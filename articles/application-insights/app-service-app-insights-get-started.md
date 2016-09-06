---
ms.openlocfilehash: 415e5aa904eade4428b2c0a84bbc205a7e86d159
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="开始使用应用程序的见解" 
    description="分析使用情况、 可用性和内部部署或 Microsoft Azure 应用程序理解的 web 应用程序的性能。" 
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
    ms.date="08/04/2015" 
    ms.author="awills"/>

# 开始使用 Visual Studio 应用程序见解

*在预览是应用程序的见解。*

发现问题、 解决问题，并不断改进您的应用程序。 快速诊断实时应用程序中的任何问题。 了解您的用户与它做什么。

配置是非常简单的并将在几分钟之内看到结果。

目前支持 iOS 和 Android，应用程序;J2EE 和 ASP.NET web 应用程序的 WCF 服务。 Web 应用程序可以在 Azure 或内部服务器上运行。 我们 JavaScript SDK 中的所有 web 页将运行。

## 入门

按任意顺序，此映射左侧的入口点的任意组合的开头。 请按照适合您的路径。

添加到您的应用程序，将遥测发送到[Azure 门户](http://portal.azure.com)的 SDK 应用程序深入工作。 有不同的平台、 语言和 Ide 支持的许多组合 Sdk。

您将需要在[Microsoft Azure](http://azure.com)中的帐户。 通过您的组织中，已经有一些属于一个组帐户的访问权限，或者您可能想要获取即付即用帐户。 应用程序的见解有自由层，因此您无需支付，直到您的应用程序受欢迎。 查看[定价页](https://azure.microsoft.com/pricing/details/application-insights/)。

您想要什么 | 要执行的操作 | 所获取的内容
---|---|---
 <a href="app-insights-start-monitoring-app-health-usage.md">![ASP.NET](./media/app-insights-get-started/appinsights-gs-i-01-perf.png)</a> | <a href="app-insights-start-monitoring-app-health-usage.md">将见解 SDK 应用程序添加到您的 web 项目</a> <br/> ![获取](./media/app-insights-get-started/appinsights-00arrow.png) | <a href="app-insights-start-monitoring-app-health-usage.md">![性能和使用情况监视](./media/app-insights-get-started/appinsights-gs-r-01-perf.png)</a>
<a href="app-insights-monitor-performance-live-website-now.md">![ASP.NET 网站已实时](./media/app-insights-get-started/appinsights-gs-i-04-red2.png)</a><br/><a href="app-insights-monitor-performance-live-website-now.md">![依赖项和性能监视](./media/app-insights-get-started/appinsights-gs-i-03-red.png)</a>|<a href="app-insights-monitor-performance-live-website-now.md">在您的 IIS 服务器上安装状态监视器</a> <br/> ![获取](./media/app-insights-get-started/appinsights-00arrow.png) | <a href="app-insights-monitor-performance-live-website-now.md">![ASP.NET 依赖项监视](./media/app-insights-get-started/appinsights-gs-r-03-red.png)</a>
<a href="insights-perf-analytics.md">![Azure 的 web 应用程序或虚拟机](./media/app-insights-get-started/appinsights-gs-i-10-azure.png)</a>|<a href="insights-perf-analytics.md">在 Azure 的 web 应用程序或虚拟机中启用的见解</a> <br/> ![获取](./media/app-insights-get-started/appinsights-00arrow.png) | <a href="insights-perf-analytics.md">![依赖项和性能监视](./media/app-insights-get-started/appinsights-gs-r-03-red.png)</a>
<a href="app-insights-java-get-started.md">![Java](./media/app-insights-get-started/appinsights-gs-i-11-java.png)</a>|<a href="app-insights-java-get-started.md">添加到 Java 项目的 SDK</a><br/>![获取](./media/app-insights-get-started/appinsights-00arrow.png) | <a href="app-insights-java-get-started.md">![性能和使用情况监视](./media/app-insights-get-started/appinsights-gs-r-10-java.png)</a>
<a href="app-insights-web-track-usage.md">![JavaScript](./media/app-insights-get-started/appinsights-gs-i-02-usage.png)</a>|<a href="app-insights-web-track-usage.md">您的 web 页中插入应用程序的见解脚本</a><br/>![获取](./media/app-insights-get-started/appinsights-00arrow.png) | <a href="app-insights-web-track-usage.md">![页面视图和浏览器的性能](./media/app-insights-get-started/appinsights-gs-r-02-usage.png)</a>
<a href="app-insights-monitor-web-app-availability.md">![可用性](./media/app-insights-get-started/appinsights-gs-i-05-avail.png)</a>|<a href="app-insights-monitor-web-app-availability.md">创建 web 测试</a><br/>![获取](./media/app-insights-get-started/appinsights-00arrow.png) | <a href="app-insights-monitor-web-app-availability.md">![可用性](./media/app-insights-get-started/appinsights-gs-r-05-avail.png)</a>
<a href="app-insights-windows-get-started.md">![窗口和 Windows Phone](./media/app-insights-get-started/appinsights-gs-i-06-device.png)</a>|<a href="app-insights-windows-get-started.md">添加到 Windows 应用程序项目的应用程序的见解</a><br/>![获取](./media/app-insights-get-started/appinsights-00arrow.png) | <a href="app-insights-windows-get-started.md">![崩溃和使用数据](./media/app-insights-get-started/appinsights-gs-r-06-device.png)</a>
<a href="app-insights-platforms.md">![iOS，Android 等](./media/app-insights-get-started/appinsights-gs-i-07-device.png)</a>|<a href="app-insights-platforms.md">添加到 iOS 或 Android 应用程序的应用程序的见解</a><br/>![获取](./media/app-insights-get-started/appinsights-00arrow.png) | <a href="app-insights-platforms.md">![崩溃和使用数据](./media/app-insights-get-started/appinsights-gs-r-06-device.png)</a>

## 支持和反馈

* 问题和事项︰
 * [故障排除][通过]
 * [MSDN 论坛](https://social.msdn.microsoft.com/Forums/vstudio/en-US/home?forum=ApplicationInsights)
 * [StackOverflow](http://stackoverflow.com/questions/tagged/ms-application-insights)
* 错误︰
 * [在浏览器中](https://connect.microsoft.com/VisualStudio/Feedback/LoadSubmitFeedbackForm?FormID=6076)
* 建议︰
 * [用户的声音](http://visualstudio.uservoice.com/forums/121579-visual-studio/category/77108-application-insights)



## <a name="video"></a>视频


> [AZURE.VIDEO 218]

> [AZURE.VIDEO usage-monitoring-application-insights]

> [AZURE.VIDEO performance-monitoring-application-insights]



<!--Link references-->

[通过]: app-insights-troubleshoot-faq.md

 

测试

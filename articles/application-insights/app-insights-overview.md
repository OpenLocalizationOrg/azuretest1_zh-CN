---
ms.openlocfilehash: 6ec6171ee0c922bd9b1afcdbb12f012c6a7c5129
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="应用程序的见解是什么？" 
    description="跟踪使用情况和性能的实时 web 或设备应用程序。  检测、 会审和诊断问题。 持续监控和改进与用户成功。" 
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
    ms.date="08/12/2015" 
    ms.author="awills"/>
 
# 应用程序的见解是什么？

应用程序的见解是可扩展的分析服务，帮助您了解性能和实时应用程序的使用情况。 它专为开发人员，可以帮助您不断改进的性能和可用性的应用程序。 

它适用于 web 和独立的应用程序在多种平台上︰.NET 或 J2EE，承载内部或在云;设备窗口、 iOS、 Android、 OSX 和其他平台上的应用程序。 

它旨在开发团队。 使用它，您可以︰

* [分析使用模式][knowUsers]以了解您的用户更好地和不断地改进您的应用程序。 
 * 页面视图计数、 新的和老的用户、 地理位置、 平台和其他核心使用情况统计信息
 * 跟踪使用路径来评估每个功能的成功。
* [检测，会审和诊断][检测]性能问题，大多数用户都知道先解决这些问题。
 *  有关性能变化或系统崩溃的通知。
 *  度量标准来帮助诊断性能问题，例如响应时间，CPU 使用率跟踪依赖项。
 *  Web 应用程序的可用性测试。
 *  故障和异常的报告和警报
 *  功能强大的诊断日志搜索 （包括日志跟踪从您最喜爱的日志记录框架）。

为每个平台 SDK 中包含各种监控直开箱即用的应用程序的模块。 此外，您可以编写代码更详细和定制分析自己遥测数据。

从应用程序收集到的遥测数据存储和分析在 Azure 门户中，其中有直观的视图和功能强大的工具，为快速诊断和分析。

![图表用户活动的统计信息，或深入到特定的事件。](./media/app-insights-overview/00-sample.png)

希望甚至更深层次的分析？ [导出](app-insights-export-telemetry.md)数据[到 SQL](app-insights-code-sample-export-telemetry-sql-database.md)、[业务智能电源](app-insights-export-power-bi.md)，或自己的工具。

![查看电源双中的数据](./media/app-insights-overview/210.png)

## 平台和语言

为不断增长的一系列平台有 Sdk。 当前列表包括︰

 * [ASP.NET 服务器]在 Azure 或您的 IIS 服务器上的[greenbrown]
 * [Azure 的云服务](app-insights-cloudservices.md)
 * [J2EE 服务器][java]
 * [Web 页][客户端]︰ HTML + JavaScript
 * [Windows Phone、 Windows 应用商店、 Windows 10 通用的应用程序和 Windows 10 开发人员门户与直接集成][窗口]
 * [Windows 桌面][桌面]
 * [iOS][ios]
 * [Android][android]
 * [其他平台][平台]的 Node.js，PHP，Python、 Ruby、 Joomla，SharePoint，WordPress

应用程序的见解也可获得遥测从 IIS 上的现有 ASP.NET web 应用程序而无需重新生成它们。

如果您的应用程序客户端、 服务器和其他组件，您可以检测所有这些。 将应用程序理解门户中集成数据，这样，例如，可以使用服务器上的事件关联客户端上的事件。


## 它的工作原理

在您的应用程序，安装了小型的 SDK 和设置应用程序信息门户中的帐户。 SDK 监视您的应用程序，并将遥测数据发送到门户网站。 门户显示统计图表，并提供了功能强大的搜索工具，可帮助您诊断问题。

![应用程序的见解 SDK 应用程序中将遥测发送到 Azure 门户中所需应用程序的见解的资源。](./media/app-insights-overview/01-scheme.png)

SDK 包含多个模块，其中收集遥测，例如与计数的用户、 会话及性能。 您还可以编写您自己的自定义代码将遥测数据发送到门户网站。 自定义遥测是跟踪用户情景尤其有用︰ 可以计数事件，如按钮单击，实现特定目标或用户误操作。

对于 ASP.NET 服务器和 Azure 的 web 应用程序，您也可以安装[状态监视器][redfield]，其中有两个用途。 它使您可以︰

* 监视 web 应用程序，而无需重新编译或重新安装它。
* 跟踪对依赖模块的调用。



### 什么是系统开销？

对性能的影响是非常小的。 非阻塞调用跟踪批处理和在单独的线程中发送。 



## 要开始工作

1. 您将需要对[Microsoft Azure](http://azure.com)的订阅。 它是免费注册，并可以选择的自由[定价层](https://azure.microsoft.com/pricing/details/application-insights/)的应用程序的见解。

2. 登录到[Azure 预览门户](http://portal.azure.com)
3. 创建一个应用程序的见解资源。 这是您将您的应用程序中看到的数据。

    ![加上开发商服务应用程序的见解。](./media/app-insights-overview/11-new.png)

    选择您的应用程序类型。

4. 打开新的资源，然后打开快速入门指南。
    
    ![浏览， ](./media/app-insights-overview/quickstart.png)

    这可以说明如何在您的应用程序安装 SDK。 如果它是一个 web 应用程序，还将了解如何向 web 页添加 SDK 并设置可用性测试。


有关更多详细信息，在该页的左侧的导航栏中选择您的应用程序类型下开始。

## 代码


[示例和演练](app-insights-code-samples.md)

[SDK 实验室](https://www.myget.org/gallery/applicationinsights-sdk-labs)-您可以安装 （以及卸载） 添加到应用程序的见解 SDK NuGet 程序包。 其实用性，给我们提供反馈 ！


## 支持和反馈

* 问题和事项︰
 * [故障排除][通过]
 * [MSDN 论坛](https://social.msdn.microsoft.com/Forums/vstudio/en-US/home?forum=ApplicationInsights)
 * [StackOverflow](http://stackoverflow.com/questions/tagged/ms-application-insights)
* 错误︰
 * [在浏览器中](https://connect.microsoft.com/VisualStudio/Feedback/LoadSubmitFeedbackForm?FormID=6076)
* 建议︰
 * [用户的声音](http://visualstudio.uservoice.com/forums/121579-visual-studio/category/77108-application-insights)


## 视频


> [AZURE.VIDEO 218]

> [AZURE.VIDEO usage-monitoring-application-insights]

> [AZURE.VIDEO performance-monitoring-application-insights]


<!--Link references-->

[android]:app-insights-android.md
[azure]: ../insights-perf-analytics.md
[客户端]: app-insights-javascript.md
[桌面]: app-insights-windows-desktop.md
[检测]: app-insights-detect-triage-diagnose.md
[greenbrown]: app-insights-start-monitoring-app-health-usage.md
[ios]: app-insights-ios.md
[java]: app-insights-java-get-started.md
[knowUsers]: app-insights-overview-usage.md
[平台]: app-insights-platforms.md
[门户网站]: http://portal.azure.com/
[通过]: app-insights-troubleshoot-faq.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[窗口]: app-insights-windows-get-started.md

 
测试

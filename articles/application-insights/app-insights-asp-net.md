---
ms.openlocfilehash: 716a25f141ab864688577f46daff418bac9df310
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="对于 ASP.NET 应用程序见解" 
    description="分析使用情况、 可用性和内部部署或 Microsoft Azure 应用程序理解的 web 应用程序的性能。" 
    services="application-insights" 
    documentationCenter=".net"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/14/2015" 
    ms.author="awills"/>


# 对于 ASP.NET 应用程序见解

*在预览是应用程序的见解。*

[AZURE.INCLUDE [app-insights-selector-get-started](../../includes/app-insights-selector-get-started.md)]


[Visual Studio 应用程序理解](http://azure.microsoft.com/services/application-insights)监控实时应用程序可帮助您[检测和诊断性能问题和异常][检测]，并[发现如何使用您的应用程序][knowUsers]。 它可与各种应用程序类型。 它可用于内部部署的 IIS 服务器上或在 Azure Vm 上承载的应用程序，以及 Azure 的 web 应用程序。 （[还涉及设备应用程序和 Java 服务器][开始]。）

![示例性能监视图表](./media/app-insights-asp-net/10-perf.png)


#### 在开始之前

你需要：

* 对[Microsoft Azure](http://azure.com)的订阅。 如果您的团队或组织有订阅了 Azure，所有者可以将您添加到它，使用您的[Microsoft 客户](http://live.com)。
* Visual Studio 2013年更新 3 或更高版本。

## <a name="ide"></a> 在 Visual Studio 项目中添加应用程序的见解

#### 如果它是一个新的项目...

在 Visual Studio 中创建一个新项目时，请确保选择了应用程序的见解。 


![创建一个 ASP.NET 项目](./media/app-insights-asp-net/appinsights-01-vsnewp1.png)


#### ...，或者如果它是现有项目

右击解决方案资源管理器中的项目，然后选择添加应用程序理解。

![选择添加应用程序的见解](./media/app-insights-asp-net/appinsights-03-addExisting.png)





#### 设置选项

如果这是您第一次，将要求您登录名或符号向上到 Microsoft Azure 预览。 （它是独立于您的 Visual Studio 的联机帐户。

如果此应用程序是一个更大的应用程序的一部分，您可以使用**配置设置，**可以将其放入与其他组件相同的资源组。 


####<a name="land"></a> 添加应用程序理解是怎么做？

命令未这些步骤 （如果您愿意，则您可以改为手动执行）︰

* 在[Azure 门户网站][门户]创建应用程序理解资源。 这是您看到您的数据。 它检索*检测键*标识的资源。
* 向项目中添加应用程序理解 Web SDK NuGet 程序包。 若要在 Visual Studio 中看到它，用鼠标右键单击项目，然后选择管理 NuGet 程序包。
* 将中的检测项`ApplicationInsights.config`。


## <a name="run"></a> 运行您的项目

F5 以运行应用程序并尝试一下︰ 打开不同的页面来生成某些遥测。

在 Visual Studio 中，您将看到已发送的事件计数。

![](./media/app-insights-asp-net/appinsights-09eventcount.png)

## <a name="monitor"></a> 打开应用程序的见解

在[Azure 门户网站][门户]打开您理解应用程序的资源。

![用鼠标右键单击项目，然后打开 Azure 门户](./media/app-insights-asp-net/appinsights-04-openPortal.png)


查找概述图表中的数据。 首先，您将看到一个或两个点。 例如︰

![单击，直至到达更多的数据](./media/app-insights-asp-net/12-first-perf.png)

单击通过任何图表以查看更详细的指标。 [了解更多有关度量。][性能]

现在部署的应用程序并观察积累的数据。


在调试模式下运行时，遥测通过管道，可以更快，这样，您应该看到在几秒钟内显示的数据。 在部署您的应用程序时，数据累积速度更慢。

#### 没有数据？

* 打开[搜索][诊断]方块，查看单个事件。
* 使用该应用程序，以便它会生成一些遥测打开不同的页面。
* 等待几秒钟，然后单击刷新。
* 请参阅[疑难解答][qna]。

#### 在生成服务器上的问题吗？

请参阅[故障排除该项](app-insights-troubleshoot-faq.md#NuGetBuild)。


## 添加浏览器监视

浏览器或客户端监视使您的用户、 会话、 页面视图和任何异常或崩溃发生在浏览器中的数据。 

![选择新建，开发商服务，应用程序的见解。](./media/app-insights-asp-net/16-page-views.png)   

您还可以编写您自己的代码来跟踪您的用户如何使用您的应用程序，直至单击和键击的详细级别。

#### 如果您的客户端 web 浏览器

如果您的应用程序显示 web 页，每页添加 JavaScript 代码段。 从您的应用程序理解资源中获取的代码︰

![在 web 应用程序中，打开快速启动，然后单击获取代码来监视我的网页](./media/app-insights-asp-net/02-monitor-web-page.png)

请注意，代码包含标识您的应用程序资源的检测项。

[了解有关跟踪 web 页的详细信息。](app-insights-web-track-usage.md)

#### 如果您的客户端设备应用程序

如果您的应用程序服务的客户端如电话或其他设备，添加到您的设备应用程序[相应的 SDK](app-insights-platforms.md) 。

如果您将客户端 SDK 配置与 SDK 服务器相同的检测密钥，以便可以同时看到它们将集成两个流。

## 使用跟踪

当您已经提供了新的用户情景时，您想要知道多少您的客户正在使用它，并且是实现他们的目标还是有困难。 通过在代码中，在客户端和服务器插入 TrackEvent() 和其他调用获取用户活动的详细的图片。 

[使用 API，用于跟踪使用情况][api]


## 诊断日志

[捕获日志跟踪]从您最喜爱的日志记录框架来帮助诊断问题的[netlogs] 。 日志条目将显示在[诊断搜索][诊断]与应用程序的见解遥测事件一起。

## 发布您的应用程序

如果您还没有发布您的应用程序 （因为您添加的应用程序的见解），，做到现在。 人们使用您的应用程序监视数据增长的组织结构图中。

### 保持独立资源的开发、 测试和发布

一个主要的应用程序，最好从调试、 测试和生产资源中[单独](app-insights-separate-resources.md)发送遥测数据。 



## 添加依赖项跟踪

[相关性指标](app-insights-dependencies.md)难以估量的以帮助您诊断性能问题。 他们衡量从您的应用程序到数据库、 REST Api 和其他外部组件调用。

![](./media/app-insights-asp-net/04-dependencies.png)

#### 如果在您的 IIS 服务器中运行您的应用程序

登录到您的服务器具有管理员权限，并安装[应用程序的见解状态监视器](http://go.microsoft.com/fwlink/?LinkId=506648)。 

（还可以使用状态监视器到[仪器已在运行的应用程序](app-insights-monitor-performance-live-website-now.md)，即使它以前没有建 sdk。）

#### 如果您的应用程序是 Azure Web 应用程序

在 Azure 的 Web 应用程序的控制面板中，将添加应用程序的见解扩展。

![在您的 web 应用程序，设置、 扩展、 加载、 应用程序的见解](./media/app-insights-asp-net/05-extend.png)

（扩展只可帮助您已生成了具有 SDK 应用程序。 与状态监视器，它能检测对现有应用程序。）

## 可用性 web 测试

[设置 web 测试][可用性]测试从外部应用程序是实时和快速响应。


![](./media/app-insights-asp-net/appinsights-10webtestresult.png)






## 若要升级到未来的 SDK 版本

若要升级到[新版本的 SDK](app-insights-release-notes-dotnet.md)，打开再次 NuGet 程序包管理器和筛选器在已安装的软件包。 选择 Microsoft.ApplicationInsights.Web，然后选择升级。

如果您对 ApplicationInsights.config 所做的任何自定义，请升级，并随后将所做的更改合并到新版本之前保存一份。



## <a name="video"></a>视频

> [AZURE.VIDEO getting-started-with-application-insights]


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
[redfield]: app-insights-monitor-performance-live-website-now.md
[角色]: app-insights-resources-roles-access-control.md
[start]: app-insights-get-started.md

 
测试

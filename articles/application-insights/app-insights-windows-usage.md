---
ms.openlocfilehash: c2fe04a455e2a8034b74d63b1db63c728d6eca77
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="监视应用程序的见解与 Windows 应用商店和电话应用程序中的使用情况" 
    description="分析使用 Windows 设备应用程序与应用程序的见解。" 
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
    ms.date="06/19/2015" 
    ms.author="awills"/>

#  监视在应用程序的见解与 Windows 应用商店和 Windows Phone 应用程序中的使用情况

*在预览是应用程序的见解。*

了解多少用户，以及哪些页面他们期盼在您的应用程序中。 应用程序的见解为您提供开箱即用的数据。 然后，通过在您的应用程序中插入几行代码，您可以了解有关用户采取的路径通过您的应用程序和它们与它的实现。

如果尚未完成此操作，添加[windows]的[应用程序到应用程序项目的见解]，并将其重新发布。 


## <a name="usage"></a>跟踪使用情况

从概述时间轴中，单击通过用户和会话的图表以查看更详细的分析。


![](./media/app-insights-windows-usage/appinsights-d018-oview.png)

* **用户**是以匿名的方式，进行跟踪，以便同一用户在不同的设备上将进行两次计数。
* 当应用程序被挂起 （为多个简短的时间间隔，以避免计算意外挂起） 计**会话**。

#### 分段

要通过各种条件进行分析的图表进行细分。 例如，若要查看多少用户正在使用您的应用程序的每个版本，打开用户图表和段由应用程序版本︰ 

![在用户图表中，切换到 Segmenting，然后选择应用程序版本](./media/app-insights-windows-usage/appinsights-d25-usage.png)


#### 网页视图

要搜索的用户通过您的应用程序执行的路径，插入[页面视图遥测][api]在您的代码︰

    var telemetry = new TelemetryClient();
    telemetry.TrackPageView("GameReviewPage");

页面视图图表上并打开其详细信息，请查看结果︰

![](./media/app-insights-windows-usage/appinsights-d27-pages.png)

单击任何页后，可以查看特定事件的详细信息。

#### 自定义事件

通过插入代码，以从您的应用程序发送自定义事件，您可以跟踪用户的行为和使用特定的功能和方案。 

例如︰

    telemetry.TrackEvent("GameOver");

数据将显示在网格中自定义事件。 可以查看聚合度量资源管理器，或单击以查看特定事件的任何事件。

![](./media/app-insights-windows-usage/appinsights-d28-events.png)


可以将字符串和数值属性添加到任何事件中。


    // Set up some properties:
    var properties = new Dictionary <string, string> 
       {{"Game", currentGame.Name}, {"Difficulty", currentGame.Difficulty}};
    var measurements = new Dictionary <string, double>
       {{"Score", currentGame.Score}, {"Opponents", currentGame.OpponentCount}};

    // Send the event:
    telemetry.TrackEvent("GameOver", properties, measurements);


单击要查看其详细的属性，包括那些您已定义的任何事情。


![](./media/app-insights-windows-usage/appinsights-d29-eventProps.png)

有关自定义事件的详细信息，请参阅[API 参考][api] 。

## 会话

会话是应用程序的深入见解，努力相关联的每个遥测事件-例如，崩溃或自定义事件您代码您自己的特定用户会话中的基本概念。 

丰富的上下文信息，需要收集每个会话，如设备特征、 地理位置、 操作系统等。

当[诊断问题][诊断]，您可以找到所有与的会话中出现问题，包括所有的请求，以及任何事件，异常或已记录的跟踪遥测。

会话提供良好如设备、 操作系统或位置的上下文中的受欢迎程度的措施。 通过显示按照设备的会话数，例如，可以更准确的频率该设备用于您的应用程序，比通过盘点网页视图计数。 这是问题的一个非常有用的输入要会审的任何特定于设备。


#### 什么是会话？

会话表示单个用户和应用程序之间遇到。 最简单的形式中会话与用户启动该应用程序将启动并完成用户离开应用程序时。 为移动应用程序，会话将终止应用程序挂起 （移至背景） 时，超过 20 秒。 如果应用程序被恢复，将启动一个新会话。 当然，用户可能会在一天中，或者甚至在一个小时内的多个会话。 

**会话的持续时间**是时间的一个指标，表示该会话的第一个和最后一个遥测项目之间跨度。 （它并不包括超时）。


在特定的时间间隔中的**会话数**被指某些活动与此时间间隔内的唯一会话数。 当上周的看一个长的时间范围，例如每日会话数时，这是通常相当于的会话总数。 

但是，当研究如每小时颗粒较短的时间范围内，跨多个小时的长时间的会话将会话处于活动状态的每个小时计数。 

## 用户和用户计数

每个用户会话都使用唯一的用户 id，生成应用程序的使用和保存在本地设备的存储相关联。 使用多个设备的用户将会不止一次计数。

在一定间隔的**用户计数**度量定义为唯一用户数与录制活动在此间隔内。 因此，具有长会话的用户可能考虑多次，以便颗粒小于一个小时左右，设置的时间范围。

**新用户**计算此时间间隔内发生的第一个会话的应用程序的用户。 


## <a name="debug"></a>调试和发布模式

#### Debug

如果在调试模式下生成，只要它们生成发送事件。 如果您断开 internet 连接，然后在重新连接之前退出该应用程序脱机遥测将被丢弃。

#### 发行版

如果在发布配置生成时，事件会存储在该设备中，然后发送时应用程序将继续。 数据还发送应用程序的第一次使用。 如果在启动时没有 internet 连接，以前遥测以及遥测数据的当前生命周期存储和发送时间为下一步继续。

## <a name="next"></a>下一步行动

[了解您的用户][knowUsers]

[了解更多关于标准浏览器][指标]


[故障排除][通过]




<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[诊断]: app-insights-diagnostic-search.md
[knowUsers]: app-insights-overview-usage.md
[指标]: app-insights-metrics-explorer.md
[门户网站]: http://portal.azure.com/
[通过]: app-insights-troubleshoot-faq.md
[窗口]: app-insights-windows-get-started.md


 
测试

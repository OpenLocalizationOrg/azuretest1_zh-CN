---
ms.openlocfilehash: 56e10376bffe780719e66f27415f42a3a45e1978
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="JavaScript 的应用程序理解 web 应用程序 |Microsoft Azure"
    description="获取页面视图和会话计数，web 客户端数据，并跟踪使用模式。 在 JavaScript 网页中检测到异常和性能问题。"
    services="application-insights"
    documentationCenter=""
    authors="alancameronwills"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/10/2015"
    ms.author="awills"/>

# JavaScript 的应用程序理解 web 应用程序

[AZURE.INCLUDE [app-insights-selector-get-started](../../includes/app-insights-selector-get-started.md)]

找出有关的性能和用法的 web 页。 添加到页面，Visual Studio 应用程序见解，您会发现您有，频率会返回，并且哪些页他们多少用户使用大多数。 此外，您还可以加载时间以及任何异常的报告。 添加几个[自定义事件和指标][跟踪]，并可以详细分析最常用的功能中，最常见的错误，并与您的用户调整页面成功。

![选择新建，开发商服务，应用程序的见解。](./media/app-insights-javascript/16-page-views.png)

如果您已经设置了您的[ASP.NET][greenbrown]或[Java][java] web 应用程序的服务器遥测数据，您将获得从客户端和服务器的角度的图片。 将在应用程序的见解门户集成两个流。

#### 简短的演示

如果您没有订阅了 Azure，并且想要尝试理解应用程序在 web 页上，请访问[尝试应用程序理解](http://aka.ms/ainow)。

## 创建应用程序的见解资源

应用程序的见解资源是其中显示有关页的性能和使用情况的数据。 （如果您已经创建了一个资源，或许您的 web 服务器，从收集数据跳过此步骤。）

在[Azure 门户](http://portal.azure.com)，创建新的应用程序理解资源︰

![选择新建，开发商服务，应用程序的见解。](./media/app-insights-javascript/01-create.png)

*已经问题吗？* [有关创建资源的详细信息][新]。


## 将该 SDK 脚本添加到您的应用程序或 web 页

在快速启动获取 web 页的脚本︰

![在您的应用程序概述刀片式服务器，选择快速启动上，获取代码来监视我的 web 页。 复制脚本。](./media/app-insights-javascript/02-monitor-web-page.png)

之前插入脚本&lt;头/&gt;想要跟踪的每一页的标记。 如果您的网站具有一个主页，您可以将脚本放那里。 例如︰

* 在 ASP.NET MVC 项目中，您可以将其放在 View\Shared\\_Layout.cshtml
* 在 SharePoint 网站，在控制面板中打开[网站设置 / 母版页](app-insights-sharepoint.md)。

脚本中包含将数据传递到您的应用程序理解资源的检测项。

*（如果您正在使用一个已知 web 页的框架，寻找的见解应用程序适配器。 例如，将[AngularJS 模块](http://ngmodules.org/modules/angular-appinsights)。)*


## <a name="run"></a>运行您的应用程序

运行您的 web 应用程序，使用它一段时间才能生成遥测，并等待几秒钟。 可以使用您的开发计算机上的**F5**键运行，或将其发布并使用户可以跟它一起玩。

如果您想要检查 web 应用程序发送到应用程序的见解的遥测数据，使用浏览器的调试工具 (**F12**在浏览器中很多)。 数据被发送到 dc.services.visualstudio.com。

## 浏览您的数据

在应用程序概述刀片式服务器，没有靠近顶部的位置，显示到浏览器中加载网页的平均时间的图表︰


![](./media/app-insights-javascript/05-browser-page-load.png)


*还没有数据？单击**刷新**在页面的顶部。仍然没有？请参阅[疑难解答][qna]。*

单击该图表，并获取更详细的版本︰

![](./media/app-insights-javascript/07-client-perf.png)

这是分成总页面加载时间[由 W3C 定义的标准计时](http://www.w3.org/TR/navigation-timing/#processing-model)堆积的图表。

![](./media/app-insights-javascript/08-client-split.png)

请注意，*网络连接*时间通常低于预期的那样，因为它是通过从浏览器到服务器的所有请求的平均。 许多单个请求具有连接时间为 0，因为已存在到服务器的活动连接。


### 页的性能

进一步向下在细节刀片式服务器，没有网格细分的页面的 URL:


![](./media/app-insights-javascript/09-page-perf.png)

如果您想要查看页的性能随着时间的推移，双击网格，然后更改其图表类型︰

![](./media/app-insights-javascript/10-page-perf-area.png)

## 客户端使用情况概述

在概述刀片式服务器，请单击**使用情况**︰

![](./media/app-insights-javascript/14-usage.png)

* **用户︰**在图表中的时间范围内的不同用户数的计数。 （cookie 用于标识返回的用户。）
* **会话︰**当用户不进行任何请求 30 分钟计会话。
* **网页视图**对 trackPageView()，通常在每个网页上一次调用的调用进行计数。


### 通过单击指向更详细信息

单击以查看更多详细信息的图表。 请注意，您可以更改图表的时间范围。

![](./media/app-insights-javascript/appinsights-49usage.png)


单击图表以查看其他指标，可以显示或添加一个新图表并选择显示的度量标准。

![](./media/app-insights-javascript/appinsights-63usermetrics.png)

> [AZURE.NOTE] 只能在某些组合显示指标。 当您选择一个指标时，将禁用不兼容的。



## 自定义页计数

默认情况下，页计数在每次新页加载到客户端浏览器时。  但是，您可能想要计算其他页面视图。 例如，页面可能会显示在选项卡的内容并且想要计算用户切换选项卡页。 或网页中的 JavaScript 代码可能会加载新的内容，而无需更改浏览器的 URL。

在相应的点在客户端代码中插入如下的 JavaScript 调用︰

    appInsights.trackPageView(myPageName);

页面名称可以包含相同的字符作为 URL，但任何后"#"或"？"将被忽略。


## 检查各个页面查看事件

通常页面视图遥测分析的应用程序的见解和查看仅累计报告，对所有用户的平均值。 但出于调试目的，您还可以查看单个页面查看事件。

在搜索诊断刀片式服务器，到页视图中设置筛选器。

![](./media/app-insights-javascript/12-search-pages.png)

选择任何事件以查看更多详细信息。 在详细信息页中，单击"..."以查看更多详细信息。

> [AZURE.NOTE] 如果您使用[搜索][诊断]，请注意，您需要匹配整个单词:"关于"和"糊涂"不匹配"关于"，但"关于*"。然后，您不能使用通配符搜索词。例如，搜索"*bou"将"关于"不匹配。

> [了解更多关于诊断搜索][诊断]

### 页面视图属性

* **页面查看持续时间**成本 / 收益分析;时间所需加载页，并开始运行脚本。 特别是，之间的时间间隔启动加载的页和 trackPageView 执行。 如果移动 trackPageView 从其正常位置的脚本初始化后，它将反映不同的值。

## 自定义使用情况跟踪

要了解您的用户使用您的应用程序做的什么？ 通过插入调用在客户端和服务器代码中，您可以发送自己遥测到应用程序的见解。 例如，您可以了解增多的用户谁创建订单没有完成，或者哪些验证错误命中大多数情况下或在游戏中的平均分。

* [了解有关自定义事件和度量 API][跟踪]。
* [API 参考](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md)

## 服务器遥测

如果尚未执行此操作，还可以深入了解来自您的服务器并显示数据以及客户端的数据，使您能够评估在服务器的性能和诊断的任何问题。

* [添加到 ASP.NET 应用程序的应用程序理解][greenbrown]
* [添加到 Java 的 web 应用程序的应用程序见解][java]


## <a name="video"></a> 视频︰ 跟踪使用情况

> [AZURE.VIDEO tracking-usage-with-application-insights]

## <a name="next"></a> 下一步行动

[使用自定义事件和指标跟踪使用情况][跟踪]




<!--Link references-->

[诊断]: app-insights-diagnostic-search.md
[greenbrown]: app-insights-start-monitoring-app-health-usage.md
[java]: app-insights-java-get-started.md
[新建]: app-insights-create-new-resource.md
[通过]: app-insights-troubleshoot-faq.md
[跟踪]: app-insights-api-custom-events-metrics.md

测试

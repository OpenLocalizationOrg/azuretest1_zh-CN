---
ms.openlocfilehash: ff136c9a7bb1359abbe5f5a8ed18eee8e001e0f2
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="为应用程序的见解管理定价和配额" 
    description="选择所需的价格计划" 
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
    ms.date="08/11/2015" 
    ms.author="awills"/>

# 为应用程序的见解管理定价和配额

*在预览是应用程序的见解。*

[定价][Visual Studio 应用程序见解][开始]的[定价]基于每个应用程序的数据量。 没有实质性自由层使获得的大多数功能有一些限制。

每个应用程序的见解资源作为独立的服务，收取和分配给您的订阅到 Azure 清单。

[请参阅定价方案][定价]。

## 查看您的应用程序理解资源配额和价格计划

可以打开配额 + 定价刀片式服务器从您的应用程序资源的设置。

![选择设置、 配额 + 定价。](./media/app-insights-pricing/01-pricing.png)

将影响您选择的定价方案︰

* [每月的配额](#monthly-quota)的遥测可以分析每个月的量。
* [数据速率](#data-rate)的数据从您的应用程序可以处理的最大速率。
* [保留](#data-retention)-时间数据保存在您要查看的应用程序理解门户。
* [连续导出](#continuous-export)-是否可以将数据导出到其他工具和服务。

为每个应用程序的见解资源分别设置这些限制。

### 优质的免费试用

当您首次创建新的应用程序理解资源时，它开始免费的层中。

在任何时候，您可以切换到 30 天免费特优试用版。 这使您可以特优层的好处。 30 天后，它会自动回复到任何分层此前在-除非您明确选择了另一层。 选择您想试用期间任何时候的层，但仍将获得免费试用 30 天期间结束之前。


## 每月的配额

* 在每个日历月，您的应用程序可以发送到指定数量的见解应用于遥测。 请参阅的[定价方案][定价]的具体数字。 
* 配额取决于您选择的定价层。
* 从每月的第一天的午夜 UTC 计配额。
* 数据点图表显示多少配额已使用了这个月。
* 配额以*的数据点。* 单个数据点是一种跟踪方法，调用是否显式调用，在代码中，或者通过一个标准的遥测模块。 数据点包括︰
 * 在[诊断搜索](app-insights-diagnostic-search.md)中看到的每一行。 
 * 如性能计数器[度量](app-insights-metrics-explorer.md)每个原始量化指标。 （看图的点是通常聚合多个原始数据点。）
 * 每个[web 测试 （可用性）](app-insights-monitor-web-app-availability.md)图表上的点。 
* 配额中未计入*会话数据*。 这包括用户、 会话、 环境和设备数据的计数。


### 超出数量

如果您的应用程序发送个以上的每月配额，您可以︰

* 支付的其他数据。 [定价方案][定价]的详细信息，请参阅。 您可以选择此选项提前。 此选项在中不可用自由定价层。
* 升级您的定价层。
* 不执行任何操作。 会话数据将继续记录，但在诊断搜索或测量数据资源管理器中将不会显示其他数据。


### 正在发送多少数据？

图表底部的定价刀片式服务器显示应用程序的数据点卷，分组数据点类型。 （您还可以创建此图表中度量资源管理器。）

![底部的定价刀片式服务器](./media/app-insights-pricing/03-allocation.png)

图表的详细信息，请单击或拖动鼠标跨越它，时间范围的详细信息，请单击 （+）。


## 数据速率

除了每月的配额中，数据速率上有限制的限制。 对于自由[定价层][定价]限制为 200 数据点每秒平均 5 分钟以上而带薪级则为 500/s 平均超过 1 分钟。 

有三个存储桶分别被计入︰

* [TrackTrace 调用](app-insights-api-custom-events-metrics.md#track-trace)和[捕获日志](app-insights-asp-net-trace-logs.md)
* [例外](app-insights-api-custom-events-metrics.md#track-exception)，50 点/秒的限制。
* 所有其他遥测 （页面视图、 会话、 请求、 依赖项、 指标、 自定义事件、 web 测试结果）。

如果您的应用程序发送个以上的限制，某些数据将被丢弃。 您将看到警告，这发生的通知。

### 降低数据率的法宝

如果您遇到的限制限制，下面是您可以执行以下操作︰

* 关闭不需要通过[编辑 ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md)收集模块。 例如，您可能决定性能计数器或互依关系数据是 inessential。
* 预聚合的度量值。 如果您已将对 TrackMetric 的调用放在您的应用程序，您可以减少通信使用重载接受您计算的平均值和标准偏差的测量的一批。 或者，您可以使用[预聚合的包](https://www.myget.org/gallery/applicationinsights-sdk-labs)。 


### 名称限制

1.  200 的唯一指标名称和应用程序的 200 唯一属性名称的最大值。 测量范围包括通过 TrackMetric，以及在其他数据类型，例如事件上的测量的数据发送。  [度量值和属性名称][api]是全球每个检测键范围不数据类型。
2.  [属性]仅在它们具有小于 100 个唯一值的每个属性时， [apiproperties]可以使用筛选和分组依据。 唯一值超过 100 后，属性仍可用于搜索和筛选，但不再的筛选器。
3.  如请求名称和页面 URL 的标准属性被限制为每周 1000年唯一值。 在 1000年个唯一值之后, 其他值标记为"其他值"。 原始值仍可用于全文搜索和筛选。

## 数据保留

您定价层确定多长时间的数据保留在门户，并因此距离后您可以设置在时间范围。


* 原始数据点 （即，您可以检查在诊断搜索的实例）︰ 7 和 30 天之间。
* 聚合的数据 （即计数、 求平均值和度量资源管理器中看到的其他统计数据） 将被保留在一丝一毫的 1 分钟为 30 天，然后 1 小时或 1 天 （具体取决于类型） 至少 13 个月。




## 查看您的订阅到 Azure 清单

应用程序的见解费用将添加到 Azure 账单。 您可以查看详细信息您 Azure 的计费部分的 Azure 的门户或在[Azure 计费门户](https://account.windowsazure.com/Subscriptions)来结帐。 

![在侧菜单中选择付费。](./media/app-insights-pricing/02-billing.png)

## 限值摘要

[AZURE.INCLUDE [application-insights-limits](../../includes/application-insights-limits.md)]


<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiproperties]: app-insights-api-custom-events-metrics.md#properties
[start]: app-insights-get-started.md
[定价]: http://azure.microsoft.com/pricing/details/application-insights/

 
测试

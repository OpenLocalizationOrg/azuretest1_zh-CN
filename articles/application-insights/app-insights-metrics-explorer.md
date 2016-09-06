---
ms.openlocfilehash: 52ba9c03d0cbe231b66a83d6a1df1df6c98a13b1
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="研究中应用程序建议的指标" 
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
    ms.date="08/07/2015" 
    ms.author="awills"/>
 
# 研究中应用程序建议的指标

在[应用程序的见解][开始]的度量标准可以测量的数值和遥测中从应用程序发送事件的计数。 它们帮助您检测性能问题并监视应用程序的使用方式的趋势。 没有了范围广泛的标准度量，并且您还可以创建您自己的自定义指标和事件。

指标和事件计数将显示在图表中的聚合值，如求和、 求平均值或计数。

下面是一个示例图表︰

![在 Azure 的门户打开概述刀片式服务器应用程序](./media/app-insights-metrics-explorer/01-overview.png)

有些图表分段︰ 任意点处的图表的总高度是显示的度量值的总和。 默认情况下的图例显示的最大数量。

虚线显示的度量值以前一周。



## 时间范围

您可以更改图表或任何刀片式服务器上的网格覆盖的时间范围。

![在 Azure 的门户打开概述刀片式服务器应用程序](./media/app-insights-metrics-explorer/03-range.png)


如果期望尚未尚未出现一些数据，单击刷新。 图表进行自我刷新的时间间隔，但时间间隔很长的更大的时间范围。 在发布模式下，可能需要一段时间通过分析管道到图表上的数据。

若要缩放的图表部分，拖动它，然后单击放大镜符号︰

![拖过图表的一部分。](./media/app-insights-metrics-explorer/12-drag.png)



## 粒度和点值

将鼠标悬停在此时显示的度量值的图表。

![将鼠标悬停在图表上](./media/app-insights-metrics-explorer/02-focus.png)

前一个采样时间间隔被聚合的度量值在特定的点。 

采样间隔或"粒度"显示在顶部的刀片式服务器。 

![刀片的标头。](./media/app-insights-metrics-explorer/11-grain.png)

您可以调整时间范围刀片式服务器中的粒度︰

![刀片的标头。](./media/app-insights-metrics-explorer/grain.png)

可用的粒度取决于您选择的时间范围。 显式的粒度是时间范围内的"自动"粒度的替代方法。 

## 测量数据资源管理器

若要查看更详细的相关的图表和网格概述刀片式服务器上的任何图表通过单击。 您可以编辑这些图表和网格可以专注于您感兴趣的细节。

例如，单击通过 web 应用程序的失败请求图表︰

![在概述刀片式服务器，请单击图表](./media/app-insights-metrics-explorer/14-trix.png)


## 数字的含义是什么？

在默认情况下侧的图例显示图表的段内的聚合的值。

每个数据点在图表上的也是前一个采样时间间隔或"粒度"中收到的数据值的聚合。 粒度显示在顶部的刀片，并随图表的总体时间刻度。

以不同的方式聚合的不同衡量标准︰ 

 * 例如，响应时间度量，值是**平均值**图表一段。
 * 有关的事件，例如失败的请求数量，聚合是段的人数的**总和**。
 * 对于用户的计数，聚合在段是**唯一**用户数。 （如果用户多次跟踪期间，他们将只计算一次。）

若要查找的值是否是求和，平均，或唯一单击图表上并向下滚动到所选的值。 您还可以度量的简短说明。

![将鼠标悬停在 (i)](./media/app-insights-metrics-explorer/06-total.png)
 


## 编辑图表和网格

刀片式服务器添加新的图表︰

![在测量数据资源管理器中，选择添加图表](./media/app-insights-metrics-explorer/04-add.png)

选择一种现有的或新的图表来编辑它的显示︰

![选择一个或多个度量标准](./media/app-insights-metrics-explorer/08-select.png)

虽然有的组合在一起可以显示有关的限制，您可以在图表中，显示多个指标。 只要选择一个标准衡量的其他部分将被禁用。 

如果您为您的应用程序 （调用 TrackMetric 和 TrackEvent） 编码的[自定义指标][跟踪]它们将在此处列出。

## 把数据分割

选择一个图表或栅格、 分组交换机并选择分组依据的属性︰

![组在分组依据中选择一个属性，然后选择分组上，](./media/app-insights-metrics-explorer/15-segment.png)

如果[自定义指标][跟踪]编码到您的应用程序，它们包括的属性值，您可以在列表中选择属性。

为图表太小而无法分段的数据？ 调整窗体的高度︰


![调整滑块](./media/app-insights-metrics-explorer/18-height.png)


## 筛选数据

若要查看仅选定的一组属性值的指标︰

![单击筛选，展开属性，并检查某些值](./media/app-insights-metrics-explorer/19-filter.png)

如果未选择任何特定属性的值，它等同于选择全部︰ 在该属性上没有任何筛选器。

请注意每个属性值旁边的事件计数。 选择一个属性的值时，会调整及其他属性值的计数。

## 删除 bot 和 web 测试通讯

使用**真实或综合通信**的筛选和检查**真实**。

您还可以筛选源**的合成流量**。

## 编辑图表类型

特别是，注意到可以栅格和图表之间切换︰

![选择栅格或图表，然后选择一种图表类型](./media/app-insights-metrics-explorer/16-chart-grid.png)

## 保存规格刀片

当您创建了一些图表时，请将它们保存为收藏。 如果您使用组织的帐户，可以选择是否与其他团队成员共享。

![选择收藏夹](./media/app-insights-metrics-explorer/21-favorite-save.png)

请参阅刀片式服务器，**请转到概述刀片式服务器**并打开收藏夹:

![在概述刀片式服务器，选择收藏夹](./media/app-insights-metrics-explorer/22-favorite-get.png)

如果您选择了相对的时间范围内，保存时，将使用的最新统计数据更新刀片式服务器。 如果您选择了绝对时间范围内，每次它都将显示相同的数据。

## 重置刀片式服务器

如果您编辑一个叶片，但再想要回到原始保存集，只需单击重置。

![在度量资源管理器顶部的按钮](./media/app-insights-metrics-explorer/17-reset.png)

## 设置警报

通知电子邮件的任何指标的异常值，添加通知。 您可以选择将电子邮件发送给帐户管理员或特定的电子邮件地址。

![在测量数据资源管理器中，选择添加预警的预警规则](./media/app-insights-metrics-explorer/appinsights-413setMetricAlert.png)

[了解更多关于通知][警报]。

## 导出到 Excel 中

您可以导出到 Excel 文件规格，资源管理器中显示的指标数据。 导出的数据包含所有图表和表格在门户网站中看到的数据。 


![在测量数据资源管理器中，选择添加预警的预警规则](./media/app-insights-metrics-explorer/31-export.png)

每个图表或表格的数据导出到 Excel 文件中的独立工作表。

您看到导出的内容。 如果您想要更改导出的数据的范围，更改的时间范围内或筛选器。 对于表，如果显示**加载更多**的命令，则您可以单击它之前单击导出，让更多的数据导出。

*目前仅用于 Internet Explorer 和镶边导出的工作原理。 我们正在增加对其他浏览器的支持。*

### 连续的导出

如果希望继续导出，以便您可以从外部处理数据，请考虑使用[连续导出](app-insights-export-telemetry.md)。

### 双电源

如果要变成更丰富的数据视图，您可以[将导出到电源 BI](app-insights-export-power-bi.md)。

## 下一步行动

* [监视应用程序的见解与使用](app-insights-overview-usage.md)
* [使用诊断搜索](app-insights-diagnostic-search.md)


<!--Link references-->

[警告]: app-insights-alerts.md
[start]: app-insights-get-started.md
[跟踪]: app-insights-api-custom-events-metrics.md

 
测试

---
ms.openlocfilehash: 495bf85196ea81b6a2bb73d4be3fe528a3503046
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="扩展的流分析作业，以增加吞吐量 |Microsoft Azure"
    description="了解如何通过配置输入的分区，优化查询的定义，并设置作业流单元来扩展流分析作业。"
    keywords="分析作业、 数据流、 数据传输"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="paulettm"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="08/19/2015"
    ms.author="jeffstok"/>

# 缩放 Azure 流分析作业，以增加吞吐量 #

了解如何计算流分析作业*流单位*以及如何通过配置输入的分区，优化查询的定义，并设置作业流单元来扩展流分析作业。 

## 一个流分析作业的部分是什么？ ##
Azure 流分析作业定义包括输入、 查询和输出。 输入是从作业在其中读取数据流、 查询用于将数据输入的流和输出是作业发送给作业结果的位置。  

作业要求至少一个输入的源的数据流。 在 Azure 服务总线事件中心或在 Azure Blob 存储可以存储的数据流的输入的源。 有关详细信息，请参阅[Azure 流分析的简介](stream-analytics-introduction.md)、[开始使用 Azure 流分析](stream-analytics-get-started.md)，和[Azure 流分析开发人员指南 》](../stream-analytics-developer-guide.md)。

## 配置流式处理单元 ##
流式处理单位 (SUs) 表示的资源和能力来执行 Azure 流分析作业。 SUs 提供了一种描述相对事件处理容量基于混合的 CPU、 内存和度量方法并读取和写入速度。 每个流单元对应于大约 1 MB/秒的吞吐量。 

选择多少 SUs 所需的特定作业是取决于在输入和查询为作业定义的分区配置。  您最多可以选择定额中流作业单位, 使用 Azure 的门户。 默认情况下每个 Azure 订阅特定区域中具有最多 50 个流单位所有分析作业的配额。  若要提高您的订阅的流式处理单位，请联系[Microsoft 技术支持](http://support.microsoft.com)。

流可以利用作业的单位的数目取决于输入和查询为作业定义的分区配置。 此外请注意，必须使用有效值的流设备。 有效的值始自 1，3，6，然后向上以 6，如下所示的增量。

![Azure 流分析流单位比例][img.stream.analytics.streaming.units.scale]

这篇文章将说明如何计算并优化查询，以提高吞吐量的分析作业。

## 计算流作业单位的最大值 ##
流可以使用流分析作业的单位的总数取决于作业和每个步骤的分区数定义的查询中的步骤数。

### 在查询中的步骤 ###
查询可以有一个或多个步骤。 每个步骤都使用 WITH 关键字定义的子查询。 只有外部 WITH 关键字的查询也被计入一步，例如，在下面的查询的 SELECT 语句︰

    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute,3), TollBoothId

上一个查询中包含两个步骤。

> [AZURE.NOTE] 在本文的后面将介绍此示例查询。

### 分区的步骤 ###

分区的步骤，需要以下条件︰

- 必须将分区的输入的源。 有关详细信息，请参阅[Azure 流分析开发人员指南](../stream-analytics-developer-guide.md)和[事件集线器编程指南](../azure-event-hubs-developer-guide.md)。
- 查询的 SELECT 语句必须读取从分区的输入源。
- 在该步骤中的查询必须有**分区 By**关键字

分区是一个查询，输入的事件将处理和聚合中的单独分区组，并输出事件生成的每个组。 如果组合的聚合是可取的则必须创建未分区的第二步聚合。

### 计算最高流作业单位 ###

所有非分区的步骤组合在一起可以扩展最多六个流流分析作业单位。 若要添加其他流单位，一步必须进行分区。 每个分区可以有六个流式处理单位。

<table border="1">
<tr><th>作业的查询</th><th>最大流作业单位</th></td>

<tr><td>
<ul>
<li>该查询包含了一步。</li>
<li>没有分区步骤。</li>
</ul>
</td>
<td>6</td></tr>

<tr><td>
<ul>
<li>3 通过划分输入的数据流。</li>
<li>该查询包含了一步。</li>
<li>该步骤进行分区。</li>
</ul>
</td>
<td>18</td></tr>

<tr><td>
<ul>
<li>该查询包含两个步骤。</li>
<li>这两个步骤进行分区。</li>
</ul>
</td>
<td>6</td></tr>



<tr><td>
<ul>
<li>3 分区流输入的数据。</li>
<li>该查询包含两个步骤。 输入的步骤进行分区并不是第二步。</li>
<li>从分区的输入读取 SELECT 语句。</li>
</ul>
</td>
<td>24 (18 个分区的步骤) + 6 的非分区的步骤</td></tr>
</table>

### 比例的例子 ###
以下查询计算的轿车经过具有的三分钟时间段内的三个过路收费站的收费站点的数量。 此查询可以最多 6 个流单元缩放。

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

若要使用更多流式单位查询，这两个数据流输入和查询必须进行分区。 假设数据流分区设置为 3，多达 18 流式单位可调整下列修改后的查询︰

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

当分区查询时，输入的事件处理并聚集在单独的分区组。 为每个组还生成输出事件。 在**分组依据**字段不是中流输入的数据的分区键时，分区可能会导致一些意外的结果。 例如，在前面的示例查询 TollBoothId 字段不是分区键的输入 1。 可以将过路 #1 中的数据分布在多个分区。

将流分析，通过分别处理每个输入 1 分区并将创建多个记录的同一翻转窗口中的同一个过路的汽车-通过传递计数。 如果输入的分区键不能更改，可以通过添加附加非分区步骤，例如解决此问题︰

    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute, 3), TollBoothId

此查询可调整至 24 流式传输单位。

>[AZURE.NOTE] 如果您加入的两个流，确保流进行分区的分区键的列，执行联接，并且这两种流中有相同的分区数。


## 配置流分析作业分区 ##

**若要调整作业流单元**

1. 登录到[管理门户](https://manage.windowsazure.com)。
2. 在左窗格中单击**流分析**。
3. 单击您想要扩展的流分析作业。
4. 单击页面顶部的**比例**。

![Azure 流分析流单位比例][img.stream.analytics.streaming.units.scale]


## 监视作业性能 ##

使用管理门户，您可以跟踪作业中的吞吐量事件/秒︰

![Azure 流分析监控作业][img.stream.analytics.monitor.job]

事件/秒来计算预期中的工作负荷的吞吐量。 如果吞吐量低于预期，调整输入的分区，优化查询，并将其他流设备添加到您的工作。

## ASA 在 Raspberry Pi 方案规模较大的吞吐量 ##


了解如何 ASA 进行扩展在典型方案中在处理吞吐量方面跨多个流单元，这里是将传感器数据 （客户端） 发送到事件中心实验、 ASA 处理它并将警报或统计数据作为输出发送给另一个事件的中心。

客户端是合成的传感器将数据发送到事件集线器到 ASA 的 JSON 格式和数据输出还是以 JSON 格式。  下面是示例数据的外观类似 –  

    {"devicetime":"2014-12-11T02:24:56.8850110Z","hmdt":42.7,"temp":72.6,"prss":98187.75,"lght":0.38,"dspl":"R-PI Olivier's Office"}

查询:"发送警报指示灯关闭时"  

    SELECT AVG(lght),
     “LightOff” as AlertText
    FROM input TIMESTAMP
    BY devicetime
     WHERE
        lght< 0.05 GROUP BY TumblingWindow(second, 1)

在此上下文中的吞吐量衡量吞吐量︰ 是输入处理的数据量由 ASA 在固定时间 (10minutes) 内。 为了达到最佳处理输入的数据吞吐量，这两个数据流输入和查询必须进行分区。 同时计数 （） 也包含在测量多少输入的事件已处理的查询。 为了确保 ASA 不只等待未来的输入事件，每个分区的输入事件中心已预先加载了足够的输入数据 (大约 300 MB)。  

下面是使用增加流单元数的结果和相应的分区计数事件集线器中。  

<table border="1">
<tr><th>输入的分区</th><th>输出的分区</th><th>流式处理单位</th><th>持续的吞吐量
</th></td>

<tr><td>12</td>
<td>12</td>
<td>6</td>
<td>4.06 MB/s</td>
</tr>

<tr><td>12</td>
<td>12</td>
<td>12</td>
<td>8.06 MB/s</td>
</tr>

<tr><td>48</td>
<td>48</td>
<td>48</td>
<td>38.32 MB/s</td>
</tr>

<tr><td>192</td>
<td>192</td>
<td>192</td>
<td>172.67 MB/s</td>
</tr>

<tr><td>480</td>
<td>480</td>
<td>480</td>
<td>454.27 MB/s</td>
</tr>

<tr><td>720</td>
<td>720</td>
<td>720</td>
<td>609.69 MB/s</td>
</tr>
</table>

![img.stream.analytics.perfgraph][img.stream.analytics.perfgraph]

## 获取帮助 ##
进一步的帮助，请尝试我们的[Azure 流分析论坛](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)。


## 下一步行动 ##

- [Azure 流分析简介](stream-analytics-introduction.md)
- [开始使用 Azure 流分析](stream-analytics-get-started.md)
- [小数位数 Azure 流分析作业](stream-analytics-scale-jobs.md)
- [Azure 流分析查询语言参考](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure 流分析管理 REST API 参考](https://msdn.microsoft.com/library/azure/dn835031.aspx)


<!--Image references-->

[img.stream.analytics.monitor.job]: ./media/stream-analytics-scale-jobs/StreamAnalytics.job.monitor.png
[img.stream.analytics.configure.scale]: ./media/stream-analytics-scale-jobs/StreamAnalytics.configure.scale.png
[img.stream.analytics.perfgraph]: ./media/stream-analytics-scale-jobs/perf.png
[img.stream.analytics.streaming.units.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsStreamingUnitsExample.jpg

<!--Link references-->

[microsoft.support]: http://support.microsoft.com
[azure.management.portal]: http://manage.windowsazure.com
[azure.event.hubs.developer.guide]: http://msdn.microsoft.com/library/azure/dn789972.aspx

[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
 
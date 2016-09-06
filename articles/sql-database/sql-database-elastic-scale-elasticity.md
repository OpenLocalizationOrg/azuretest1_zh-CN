---
ms.openlocfilehash: 657096805259928f4b6ce346e00535d5a72000c5
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="Shard 弹性" 
    description="介绍概念并举例背后 shard 弹性，能够很容易地扩展 SQL Azure 数据库的说明。" 
    services="sql-database" 
    documentationCenter="" 
    manager="jeffreyg" 
    authors="sidneyh" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="04/17/2015" 
    ms.author="sidneyh"/>

# Shard 弹性 

**Shard 弹性**使应用程序开发人员能够动态地增长和收缩数据库资源，根据需要，启用其中一个以优化其应用程序的性能以及成本降至最低。 Azure SQL 数据库和[基本、 标准和特优服务层](http://msdn.microsoft.com/library/azure/dn741340.aspx)的**弹性数据库工具**的组合提供了很有说服力的弹性方案。  弹性的数据库工具使水平缩放-一种设计模式，在其中添加或移除设置放大或缩小容量 shard 从数据库 （也[称为"shards"](sql-database-elastic-scale-glossary.md)）。 同样，SQL 数据库服务层提供了**垂直缩放**功能，向上或向下进行相应的匹配需求可以缩放单个数据库的资源。  在一起，一个 shard 的垂直缩放和水平缩放许多 shards，提供了应用程序开发人员一种非常灵活的环境，可以扩展以满足性能、 容量和成本优化的需要。

与新引入的**弹性数据库池**功能，垂直扩展是更易于实现。   池允许单个数据库的资源消耗，扩大或收缩*自动*在整个池之间共享预算范围内。 对于那些选择不利用弹性数据库池的应用程序，这篇文章将介绍其他技术用于实现基于策略的管理垂直缩放比例，以及自动化水平缩放操作的常见方案的机制。

## 水平缩放示例︰ 音乐会图文场

水平缩放规范方案是处理音乐会入场券的事务的应用程序。 在普通客户卷下，应用程序使用最少的数据库资源处理采购交易记录。  但是，当门票销售的流行音乐会上，单个数据库无法处理客户请求的较大峰值。 

处理交易记录的急剧增加，应用程序可以扩展水平。 应用程序现在可以将事务负载分布到多个 shards。 当不再需要额外的资源时，数据库层收缩后的正常使用。 这里水平缩放使应用程序能够向外扩展以匹配客户的需求和规模在不再需要资源时。   

## 垂直缩放示例︰ 遥测

垂直缩放规范方案是使用**shard 设置**的以存储操作遥测的应用程序。 在这种情况下，最好共同一天在一个 shard 上找到所有的遥测数据。 在此应用中，当天的数据 ingested 到 shard 和新 shard 提供后续的天。 然后可以老化和查询根据需要操作数据。 

若要接收在高负载的遥测数据，最好使用更高的服务层。 换句话说，高级数据库优于基本的数据库。 一旦数据库达到其容量限制，它从切换摄取到分析和报告。 对此，标准层的性能相当于任务。 因此一个可以垂直缩小规模上 （而不是最近创建的一个） 的 shards 的服务层以适应旧数据的低性能要求。 

垂直缩放比例还可用来增加单个数据库的性能为获得更高的性能。 例如，税务归档应用程序。 在归档时，最好要保证单个客户的数据都在相同数据库和提高该 shard 的性能。 这取决于应用程序中，垂直向上和向下的资源比例是有利优化成本和性能要求。 

在一起，水平和垂直缩放数据库层是应用程序可伸缩性和弹性 shard 的核心。 

此文档提供上下文 shard 弹性方案以及参考示例实现垂直和水平缩放。 

## 技巧的 shard 弹性 

垂直和水平缩放是函数的三个基本组件︰ 

1. **遥测**
2. **Rule**
3. **操作**   

## 遥测

**数据驱动的弹性**是灵活的伸缩应用程序的核心。 根据性能的要求，使用遥测数据驱动决策上是否垂直或水平缩放。  

#### 遥测数据源
在 Azure SQL 数据库的上下文中，有少数的可以被利用作为数据源的 shard 弹性的关键来源。 

1. 在五分钟期间，在**sys.resource_stats**视图中公开**性能遥测** 
2. 通过**sys.resource_usage**视图公开每小时**数据库容量遥测**。  

一个可以查询主 DB 使用下面的查询，其中 Shard_20140623 为目标数据库的名称来分析性能资源使用情况。 

    SELECT TOP 10 *  
    FROM sys.resource_stats  
    WHERE database_name = 'Shard_20140623'  
    ORDER BY start_time DESC 

要想删除瞬态行为，可以通过一段时间 （在下面的查询 7 天） 汇总**性能遥测**。 

    SELECT  
    avg(avg_cpu_percent) AS 'Average CPU Utilization In Percent', 
        max(avg_cpu_percent) AS 'Maximum CPU Utilization In Percent', 
        avg(avg_physical_data_read_percent) AS 'Average Physical Data Read Utilization In Percent', 
        max(avg_physical_data_read_percent) AS 'Maximum Physical Data Read Utilization In Percent', 
        avg(avg_log_write_percent) AS 'Average Log Write Utilization In Percent', 
        max(avg_log_write_percent) AS 'Maximum Log Write Utilization In Percent', 
        avg(active_session_count) AS 'Average # of Sessions', 
        max(active_session_count) AS 'Maximum # of Sessions', 
        avg(active_worker_count) AS 'Average # of Workers', 
        max(active_worker_count) AS 'Maximum # of Workers' 
    FROM sys.resource_stats  
    WHERE database_name = ' Shard_20140623' AND start_time > DATEADD(day, -7, GETDATE()); 

一个类似的查询针对**sys.resource_usage**视图时，可以测量**数据库容量**。 **Storage_in_megabytes**列的最大生成当前数据库的大小。 这种遥测可用于水平扩展应用程序，当特定 shard 达到其容量限制。 

    SELECT TOP 10 * 
    FROM [sys].[resource_usage] 
    WHERE database_name = 'Shard_20140623'  
    ORDER BY time DESC 

数据 ingested 到特定 shard 中，是非常有用的项目向前一天并确定 shard 是否有足够的容量来处理将来的工作负荷。 而没有真正的实现的线性回归，下面的查询返回的最大增量两个连续工作日之间的数据库容量。  这种遥测然后可以应用于一个规则，然后将导致正在采取行动 （或非操作）。 

    WITH MaxDatabaseDailySize AS( 
        SELECT 
            ROW_NUMBER() OVER (ORDER BY convert(date, [time]) DESC) as [Order], 
            CONVERT(date,[time]) as [date],  
            MAX(storage_in_megabytes) as [MaxSizeDay] 
        FROM [sys].[resource_usage] 
        WHERE  
            database_name = 'Shard_20140623' 
        GROUP BY CONVERT(date,[time]) 
        ) 
    
    SELECT 
        MAX(ISNULL(Size.[MaxSizeDay] - PreviousDaySize.[MaxSizeDay], 0)) 
    FROM  
        MaxDatabaseDailySize Size INNER JOIN 
        MaxDatabaseDailySize PreviousDaySize ON Size.[order]+1 = PreviousDaySize.[order] 
    WHERE 
        Size.[order] < 8 

## Rule  

规则是确定执行了某个操作的决策引擎。 某些规则都非常简单，一些复杂得多。 以下代码片段中所示，一个容量为中心的规则可以进行配置，以便新 shard shard 达到 $SafetyMargin，例如，80%，其最大容量的配置。

    # Determine if the current DB size plus the maximum daily delta size is greater than the threshold 
    if( ($CurrentDbSizeMb + $MaxDbDeltaMb) -gt ($MaxDbSizeMb * $SafetyMargin))  
    {#provision new shard} 

给出上述的数据源，可以为了实现大量 shard 弹性方案制定的规则数。 

## 操作  

基于规则的结果，操作的非操作） 是结果。 两种最常见的操作有︰

* 增加或减小的 shard 的服务层或性能级别 
* 添加或移除从 shard shard 的设置。

请注意，在水平和垂直缩放解决方案操作的结果不会立即。 当垂直缩放比例，例如，发出 ALTER DATABASE 命令增加到高级数据库的基本数据库服务层采用可变的时间量。 持续时间将很大程度上依赖于数据库的大小 （有关详细信息请参阅[更改服务层和性能级别](http://msdn.microsoft.com/library/azure/dn369872.aspx)。 同样，分配或资源调配新 shard 的也不是瞬时完成。 因此，对于垂直和水平缩放时，应用程序应考虑到更改或设置新数据库的非零时间。  

## Shard 弹性方案示例 

在下图中所示的示例中突出显示两种弹性比例情况︰ 
1. 水平缩放 shard 映射 
2. 垂直缩放单个 shard。  

![Operationl 数据提取][1]

水平比例，（根据日期或数据库大小） 的规则用于调配新 shard 和 shard 映射，因此水平不断增长的数据库层中注册它。 其次，要进行垂直缩放，第二个规则实现中的任何超过一天的 shard 将降级从高级版到标准版或基本版。 

再次考虑遥测的方案︰ 按日期应用程序 shards。 它会收集遥测数据不断，需要更高性能版本在加载时间，但是作为数据的低性能老化。 当天的数据 [Tnow] 写入高性能数据库 （高级用户）。 一旦时钟降临午夜前, 一天的 shard (现在 [T-1]) 将不再可用于接收。 当前的数据被 ingested 由当前 [Tnow]。 提前二天，但新 shard 必须设置并使用 shard 映射 ([T + 1]) 注册。  

这可以通过资源调配前每天新新 shard 或者调配新的 shard 当当前 shard ([Tnow]) 接近其最大容量。 使用以下任一方法将保留所有遥测的某一天写的数据局部性。 此外应用细粒度每小时分片。 一旦配置新 shard，并由于 [T-1] 用于查询和报告，最好是降低数据库的性能级别降低了成本。 作为在 DBs 时代的内容，可以进一步降低性能层和/或可以归档到 Azure 存储或删除根据应用程序数据库的内容。 

## 正在执行的 shard 弹性方案  

为了便于实际实现的水平和垂直缩放的情况下，多个[Shard 弹性示例脚本](http://go.microsoft.com/?linkid=9862617)已创建和过帐脚本中心。 这些 PowerShell 运行手册编写运行在 Azure 自动化服务，提供了大量与弹性数据库客户端库和 SQL Azure 数据库进行交互的方法。  构建时，或从这些代码示例提取，其中一个可为了自动化水平、 垂直或其应用程序这两个扩展方案编制所需的脚本。 

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-elasticity/data-ingestion.png

<!--anchors-->
[遥测]:#telemetry
[Rule]:#rule
[操作]:#action
 
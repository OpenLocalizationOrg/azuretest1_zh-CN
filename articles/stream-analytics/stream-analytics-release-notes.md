---
ms.openlocfilehash: 73ffba6fb1bd240df1aaaff95b67b9658ab89f6c
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="流分析发行说明 |Microsoft Azure" 
    description="流分析正式上市发行说明" 
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
    ms.date="08/20/2015" 
    ms.author="jeffstok"/>

#发行说明 Microsoft 流分析

## 08/20/2015年发布流分析的注意事项 ##

此版本包含了下面的更新。

标题|说明
---|---
添加最后一个函数 |[最后一个](http://msdn.microsoft.com/library/mt421186.aspx)函数使您可以检索最新的事件，在给定的时间范围内的事件流中的流分析作业中现。
新数组函数|数组函数[GetArrayElement](http://msdn.microsoft.com/library/mt270218.aspx)、 [GetArrayElements](http://msdn.microsoft.com/library/mt298451.aspx)和[GetArrayLength](http://msdn.microsoft.com/library/mt270226.aspx)现在是可用的。
新记录函数|现在提供了记录函数[GetRecordProperties](http://msdn.microsoft.com/library/mt270221.aspx)和[GetRecordPropertyValue](http://msdn.microsoft.com/library/mt270220.aspx) 。

## 07/30/2015年发布流分析的注意事项 ##

此版本包含了下面的更新。

标题|说明
---|---
双电源的组织 Id 分离从 Azure Id|此功能使[电源双输出](stream-analytics-power-bi-dashboard.md)ASA 在 Azure 账户类型 （Live Id 或组织 Id） 下的作业。 此外，可以为 Azure 帐户有一个组织 Id 和使用授权电源双输出的另一个。
服务总线队列输出的支持|[服务总线队列](stream-analytics-connect-data-event-outputs.md#service-bus-queues)输出目前有流分析作业。
服务总线主题输出的支持|[服务总线主题](stream-analytics-connect-data-event-outputs.md#service-bus-topics)输出目前有流分析作业。

## 07/09/2015年发布流分析的注意事项 ##

此版本包含了下面的更新。


标题|说明
---|---
自定义的 Blob 输出分区|Blob 存储输出现在允许一个选项，以指定的频率输出写入 blob 的结构和格式输出数据路径的文件夹结构。 

## 05/03/2015年发布流分析的注意事项 ##

此版本包含了下面的更新。


标题|说明
---|---
增加最大值为序的容差窗口|序的容差窗口的最大大小现在是 59:59 (MM:SS)
JSON 输出格式︰ 分隔线或数组|当输出到 Blob 存储或事件中心是一个数组或用新行分隔 JSON 对象的 JSON 对象作为输出，现在有是一个选项。 

## 04/16/2015年发布流分析的注意事项 ##


标题|说明
---|---
在 Azure 存储帐户配置延迟|在创建流分析作业区域中第一次时，将提示您创建一个新的存储帐户或指定为监视流分析作业在该地区现有的帐户。 由于在配置监视的滞后，在 30 分钟内创建在同一区域的另一个流分析作业将提示输入指定第二个存储帐户而不是显示在监视存储帐户下拉列表中的最近配置的一个。 为避免创建不必要的存储帐户，请等待 30 分钟后在一个区域中创建一个作业，设置在该地区的其他作业前第一次。
升级作业|到目前为止，流分析不支持实时编辑到定义或正在运行的作业的配置。 要更改的输入、 输出、 查询、 缩放或正在运行的作业的配置，您必须首先停止作业。
从输入源推断的数据类型|如果不使用 CREATE TABLE 语句，则输入的类型派生自输入格式，例如从 CSV 的所有字段都是字符串。 字段需要显式转换为正确的类型使用 CAST 函数，以避免出现类型不匹配错误。
缺少的字段是空值作为输出|引用不存在输入源中的字段将导致输出事件中的空值。
语句必须位于 SELECT 语句|在查询中，SELECT 语句必须遵循定义语句使用子查询。
内存不足问题|流式处理序的事件和/或维护大量的状态可能会导致耗尽内存，从而导致作业作业的复杂查询的大公差分析作业重新启动。 开始和停止操作将显示该作业的操作日志中。 若要避免此行为，请跨多个分区扩展出查询。 在将来的版本中，将通过降低而不是重新启动受影响的作业性能解决此限制。
大的 blob 输入没有负载的时间戳可能会导致内存不足问题|如果未指定通过时间戳的时间戳字段，使用从 Blob 存储大型文件可能会导致崩溃流分析作业。 若要避免此问题，请每个 blob 低于 10 MB 大小。
SQL 数据库事件卷的限制|作为输出目标使用 SQL 数据库，非常高的输出数据量可能会导致流分析作业超时。 要解决此问题，请通过使用聚合或筛选器运算符，减小输出音量或作为输出目标改为选择 Azure Blob 存储或事件集线器。
PowerBI 数据集只能包含一个表|PowerBI 不支持多个表中给定的数据集。

## 获取帮助
进一步的帮助，请尝试我们的[Azure 流分析论坛](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## 下一步行动

- [Azure 流分析简介](stream-analytics-introduction.md)
- [开始使用 Azure 流分析](../stream.analytics.get.started.md)
- [小数位数 Azure 流分析作业](stream-analytics-scale-jobs.md)
- [Azure 流分析查询语言参考](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure 流分析管理 REST API 参考](https://msdn.microsoft.com/library/azure/dn835031.aspx)
 

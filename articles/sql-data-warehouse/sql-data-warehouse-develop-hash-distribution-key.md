---
ms.openlocfilehash: 2e8d40cae08d11b39cd80b9f04da2253482e9f07
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="哈希分发和在 SQL 数据仓库查询性能的影响 |Microsoft Azure"
   description="了解分布式哈希表以及它们如何影响开发解决方案的 Azure SQL 数据仓库的查询性能。"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/26/2015"
   ms.author="JRJ@BigBangData.co.uk;barbkess"/>

# 哈希分发和在 SQL 数据仓库查询性能的影响

决策智能希分布是提高查询性能的最重要方法之一。  

事实上，有三个主要因素︰

1. 最小化数据移动
2. 避免数据不一致
3. 提供均衡

## 最小化数据移动
数据移动最常出现时都执行聚合表上的或表联接在一起。 分配表上的共享密钥的哈希是最小化这种移动的最有效方法之一。

但是，希分布来有效降低移动下列条件必须同时为真︰

1. 这两个表需要分布式哈希并共享的分发键联接
2. 这两个列的数据类型必须匹配
3. 联接列必须是同等联接 （即左的表的列中的值需要等于右边表的列中的值）
4. 该连接是**不** `CROSS JOIN`

> [AZURE.NOTE] 列中使用`JOIN`， `GROUP BY`， `DISTINCT` ，`HAVING`所有进行很好的哈希列候选的子句。 手中的其他列`WHERE`子句执行**不**好的哈希列候选人决定。 请参见下面的均衡。

移动数据可能也会产生查询语法 (`COUNT DISTINCT` ，`OVER`这两个子句是最好的例子) 与不包含哈希分布键的列一起使用时。

> [AZURE.NOTE] 循环复用表通常生成的数据移动。 表中的数据已分配以非确定的方式，因此必须首先将数据移动在大多数查询完成之前。

## 避免数据倾斜
为了使希分布有效非常重要所选的列显示以下属性︰

1. 列中包含大量的非重复值。
2. 列中不会受到**数据倾斜**。

每个非重复值将分配给通讯组。 因此，数据将需要生成不同的值，以确保足够的唯一的哈希值的合理数量。 否则，我们可能会得到质量较差的哈希。 如果分配的数量超过非重复值的数目，例如然后一些分配将留空。 这会降低性能。

同样，如果所有的哈希列中的行包含相同的值然后数据就是**扭曲的**。 在这种极端的情况下只有一个哈希值可能已创建导致结束一个通讯中的所有行。 理想情况下，哈希列中的每个不同值将具有相同的行数。

> [AZURE.NOTE] 循环复用表不表现出倾斜的迹象。 这是因为数据存储上均匀分布。

## 提供均衡
当每个通讯组具有相当的工作要实现均衡。 大规模并行处理 (MPP) 是一个团队游戏;每个人都跨线之前任何人都可以声明为胜者。 如果每个通讯组具有相同数量的工作 （即数据处理） 然后的所有查询将完成在大约相同的时间。 这称为均衡。

如曾出现数据不一致会影响均衡。 但是，也可希分配参数的选择。 如果已选择一列将出现在`WHERE`子句的查询，则是很可能的查询将不能平衡。  

> [AZURE.NOTE] `WHERE`子句通常有助于确定最适用于分区的列。

一个很好示例中显示的列的`WHERE`子句将日期字段。  日期字段是极好的分区列但往往差希分布列的经典例子。 通常情况下，数据仓库查询是通过如天、 周或月的指定的时间段。 按日期分布希可能实际上限制我们的 scalablity 和损害我们的性能。 如果指定的日期范围的例如是每周 7 天即哈希值的最大数量的每一天是 7 个。 这意味着只有 7 个我们分配将包含的数据。 剩余的分配将不包含任何数据。 如仅 7 分配处理数据，这会导致不均衡的查询执行。

> [AZURE.NOTE] 循环复用表通常提供平衡的执行。 这是因为数据存储上均匀分布。

## 建议
以最大化性能和总体查询吞吐量尝试，并确保分布式哈希表跟踪此尽最大可能的模式︰

哈希分发密钥︰

1. 在使用`JOIN`， `GROUP BY`， `DISTINCT`，或`HAVING`您的查询中的子句。
2. 在不使用`WHERE`子句
3. 有许多不同的值，至少 1000年。
4. 没有不均衡地大将散列到少量的分配的行数。
5. 被定义为非空。 空行将集中在一个分发。

## 摘要

哈希分布可以归纳为以下几点︰

- 哈希函数有确定性。 相同的值始终被指定给同一通讯组中。
- 一列用作分发列。 哈希函数使用指定的列计算行分配到分配。
- 哈希函数基于不上它们本身的值的列的类型
- 哈希表分发有时会被扭曲的表
- 分布式哈希表解析查询时，通常需要更少的数据移动，并因此提高大型事实表的查询性能。
- 观察到分布式哈希列选定内容来增强查询吞吐量的建议。

> [AZURE.NOTE] 在 SQL 数据仓库数据类型一致性问题 ！ 确保现有架构列以一致的方式使用相同的类型。 这是尤其重要的分配参数。 如果未同步分布键的数据类型，并且将表联接将产生不必要的数据移动。 这可能是成本高昂，如果表很大，将导致减少了的吞吐量和性能。


## 下一步行动
开发的更多提示，请参阅[开发概述][]。

<!--Image references-->

<!--Article references-->
[开发概述]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
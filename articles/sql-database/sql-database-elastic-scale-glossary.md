---
ms.openlocfilehash: 7032004b3236d96a7db8c616f46b2f0450d0f28e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="弹性数据库工具词汇表" 
    description="对于弹性数据库工具中使用的术语" 
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
    ms.date="07/24/2015" 
    ms.author="sidneyh"/>

# 弹性数据库工具词汇表
以下的术语定义的弹性数据库工具，SQL Azure 数据库的功能。 这些工具包括客户端库、 拆分合并工具、 弹性池和查询。

![弹性扩展条款][1]

**数据库**︰ Azure SQL 数据库。 

**数据依赖于路由**︰ 允许应用程序连接到给定特定分片键 shard 的功能。 比较**多的 Shard**查询。

**全球 shard 映射**︰ 分片键以及它们各自在**shard 设置**shards 之间的映射。 全球 shard 映射存储在**shard 地图管理器**中。 与**本地 shard 地图**进行比较。

**列表 shard 映射**︰ shard 地图在哪个分片键分别映射。 与**范围 Shard 地图**进行比较。   

**本地的 shard 映射**︰ 存储在 shard 上，本地 shard 映射包含驻留在 shard shardlets 的映射。

**多 shard 查询**︰ 能够发出一个查询对多个 shards;使用 UNION ALL 语义 （也称为"扇出查询"） 返回结果集。 与**数据相关的路由**。

**区域 shard 映射**︰ shard 地图 shard 分发策略基于多个连续的值范围。 

**参考表**︰ 表不是 sharded，但 shards 之间进行复制。 例如，可以引用表中存储邮政编码。 

**Shard**: Azure SQL 数据库，用于存储从 sharded 数据集的数据。 

**Shard 弹性**︰ 能够同时执行**水平比例**和**垂直缩放**。

**Sharded 表**︰ 表的 sharded，即，其数据被分布在 shards 基于分片键值。 

**分片键**︰ 决定如何将数据分布在 shards 列的值。 值类型可以是以下之一︰ **int**、 **bigint**、**低级**或**唯一标识符**。 

**Shard 设置**︰ shards，归功于在 shard 地图管理器相同的 shard 映射的集合。  

**Shardlet**︰ 所有使用单一值的 shard 上分片键相关联的数据。 Shardlet 时重新分发 sharded 表数据移动可能的最小单位。 

**Shard 映射**︰ 分片键和他们各自的 shards 之间的映射的集合。

**Shard 地图管理器**︰ 管理对象和数据存储包含 shard map(s)、 shard 位置和一个或多个 shard 集的映射。

![Mappings][2]


##谓词

**水平缩放**︰ 扩展出 （或） 法案的通过添加或删除 shards shard 映射，如下所示的 shards 集合。

![水平和垂直缩放][3]

**合并**︰ 将从两个 shards 的 shardlets 移动到一个 shard 和相应地更新 shard 地图的行为。

**Shardlet 移动**︰ 移动到不同的 shard 单个 shardlet 的行为。 

**Shard**︰ 跨多个数据库基于分片键行为水平分区具有相同的结构化的数据。

**拆分**︰ 几个 shardlets 从一个 shard 移动到另一个 （通常是新的） shard 的行为。 用户提供的分片键作为分割点。

**垂直缩放**︰ 缩放向上 （或向下） 的操作单个 shard 的性能级别。 例如，改为 shard 从标准津贴 （这会导致更多的计算资源）。 

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]  

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-glossary/glossary.png
[2]: ./media/sql-database-elastic-scale-glossary/mappings.png
[3]: ./media/sql-database-elastic-scale-glossary/h_versus_vert.png
 
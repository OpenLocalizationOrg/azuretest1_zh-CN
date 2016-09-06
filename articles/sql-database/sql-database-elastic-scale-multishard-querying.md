---
ms.openlocfilehash: 6384d861cffa089f815b6f7518d801c755082112
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="多 shard 查询" 
    description="Shards 使用弹性数据库客户端库在运行查询。" 
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

# 多 shard 查询

## 概述

**多 shard 查询**适用于任务，如数据收集/报告需要跨多个 shards 运行拉伸的查询。 （对比这[依赖于数据的路由](sql-database-elastic-scale-data-dependent-routing.md)，它在单个 shard 上执行所有工作到。）若要使用 SQL Server 管理工作室，请参阅[入门弹性数据库查询](sql-database-elastic-query-getting-started.md)。

弹性的数据库客户端库引入了一个新的命名空间，称为**Microsoft.Azure.SqlDatabase.ElasticScale.Query** ，提供查询多个 shards 使用单个查询和结果的能力。 它提供查询抽象的 shards 集合上。 它还提供了替代的执行策略，尤其是部分结果，在许多 shards 查询时处理故障。  

查询多 shard 的主入口点是**MultiShardConnection**类。 作为使用依赖于数据的路由时，API 都遵循熟悉的**[System.Data.SqlClient](http://msdn.microsoft.com/library/System.Data.SqlClient(v=vs.110).aspx)**类和方法的经验。 与**SqlClient**库中，第一步是创建**SqlConnection**，然后创建**SqlCommand**的连接，然后通过一种**执行**方法执行该命令。 最后， **SqlDataReader**循环从执行命令所返回的结果集。 经验多 shard 查询 Api 包括以下步骤︰ 

1. 创建**MultiShardConnection**。
2. 创建**MultiShardConnection** **MultiShardCommand** 。
3. 执行该命令。
4. 使用通过**MultiShardDataReader**的结果。 

关键的区别是多 shard 连接的结构。 其中的单个数据库上运行**SqlConnection** ， **MultiShardConnection**将接受为其输入***的 shards 集合***。 一个可以填充 shards 从 shard 映射的集合。 Shards 使用**UNION ALL**语义来组装一个总体结果的集合，然后执行查询。 （可选） 行源自 shard 的名称可以添加到命令上使用**ExecutionOptions**属性的输出。 下面的代码演示了多 shard 查询使用名为*myShardMap*的给定的**ShardMap**的用法。 

    using (MultiShardConnection conn = new MultiShardConnection( 
                                        myShardMap.GetShards(), 
                                        myShardConnectionString) 
          ) 
    { 
    using (MultiShardCommand cmd = conn.CreateCommand())
           { 
            cmd.CommandText = "SELECT c1, c2, c3 FROM ShardedTable"; 
            cmd.CommandType = CommandType.Text; 
            cmd.ExecutionOptions = MultiShardExecutionOptions.IncludeShardNameColumn; 
            cmd.ExecutionPolicy = MultiShardExecutionPolicy.PartialResults; 

            using (MultiShardDataReader sdr = cmd.ExecuteReader()) 
                { 
                    while (sdr.Read())
                        { 
                            var c1Field = sdr.GetString(0); 
                            var c2Field = sdr.GetFieldValue<int>(1); 
                            var c3Field = sdr.GetFieldValue<Int64>(2);
                        } 
                } 
           } 
    } 
 

请注意对**myShardMap.GetShards()**的调用。 此方法检索 shard 映射中的所有 shards，可以轻松地在所有相关的数据库运行查询。 返回的集合 shards 多 shard 查询可细化为进一步通过在集合上执行 LINQ 查询从对**myShardMap.GetShards()**的调用。 部分结果策略结合，多 shard 查询的当前功能旨在适用于数十到数百个 shards。
与多 shard 查询限制目前为 shards 和 shardlets 查询的有效性缺乏。 而依赖于数据的路由验证给定的 shard 的查询时 shard 映射的一部分，多 shard 查询不会执行此检查。 这可能导致由于 shard 地图从已删除的数据库上运行的多 shard 查询。

## 多 shard 查询和拆分合并操作

多 shard 查询不验证查询数据库中的 shardlets 都参与正在进行的拆分合并操作。 这可能会导致不一致的情况相同的 shardlet 中的行位置显示相同多的 shard 查询中的多个数据库。 注意这些限制并执行多 shard 查询时考虑排出日常拆分合并操作的 shard 映射的更改。

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
 
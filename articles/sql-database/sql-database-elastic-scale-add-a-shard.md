---
ms.openlocfilehash: c65a2767c4aa21461bd9c09a18b9688350ca9654
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="添加使用弹性数据库工具 shard" 
    description="如何使用弹性比例 Api shard 中添加新的 shards 设置。" 
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

# 添加使用弹性数据库工具 shard

## 若要添加新区域或密钥 shard  

应用程序经常需要只需添加新的 shards 来处理数据，需要从新的键或键范围，为 shard 映射已存在。 例如，按租户 ID sharded 应用程序可能需要为新的租户，设置新的 shard 或数据 sharded 每月可能需要配置的每个新的月份开始之前新 shard。 

如果新的键值范围尚不属于现有的映射，则很容易添加新的 shard 并将新的密钥或到该 shard 的范围相关联。 

### 示例︰ 将 shard 和其范围添加到现有的 shard 映射
在下面的示例中，已经创建了名为**sample_shard_2** ，在它的所有必要的架构对象的数据库来保存范围 [300，400）。  

    // sm is a RangeShardMap object.
    // Add a new shard to hold the range being added. 
    Shard shard2 = null; 

    if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
    { 
        shard2 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_2"));  
    } 

    // Create the mapping and associate it with the new shard 
    sm.CreateRangeMapping(new RangeMappingCreationInfo<long> 
                            (new Range<long>(300, 400), shard2, MappingStatus.Online)); 


## 若要添加为现有区域的空白部分 shard  

在某些情况下，您可能已映射到 shard 的范围和部分填充数据，但现在希望即将到来的数据定向到不同的 shard。 例如，您 shard 按日期范围和 shard 具有已分配的 50 天，但在第 24 天，希望以后的数据如何在不同的 shard 中着陆。 弹性数据库[拆分合并工具](sql-database-elastic-scale-overview-split-and-merge.md)可以执行此操作，但如果数据移动即不必要 （例如，数据的范围 [25，50 天），一天 25 到 50 排斥，包括尚不存在） 可以执行这完全直接使用 Shard 地图管理 Api。

### 示例︰ 拆分范围，并将空的部分分配给新添加的 shard

已创建了一个名为"sample_shard_2"，并在其内部的所有必要的架构对象。  

 
    // sm is a RangeShardMap object.
    // Add a new shard to hold the range we will move 
    Shard shard2 = null; 

    if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
    { 
    
        shard2 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_2"));  
    } 

    // Split the Range holding Key 25 

    sm.SplitMapping(sm.GetMappingForKey(25), 25); 

    // Map new range holding [25-50) to different shard: 
    // first take existing mapping offline 
    sm.MarkMappingOffline(sm.GetMappingForKey(25)); 
    // now map while offline to a different shard and take online 
    RangeMappingUpdate upd = new RangeMappingUpdate(); 
    upd.Shard = shard2; 
    sm.MarkMappingOnline(sm.UpdateMapping(sm.GetMappingForKey(25), upd)); 

**重要提示**︰ 使用这种技术，仅更新的映射如果您可以确信的范围为空。  因此最好在您的代码中包含检查上述两种方法不会检查范围要移动的数据。  如果要移动的范围中存在行，实际数据分发将不符合更新的 shard 映射。 使用[拆分合并工具](sql-database-elastic-scale-overview-split-and-merge.md)改为在这些情况下执行此操作。  


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
 
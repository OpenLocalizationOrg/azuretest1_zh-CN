---
ms.openlocfilehash: 84ed53faf67c1bcd263f7811ad5e23135f5ef958
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="Shard 地图管理" 
    description="如何使用 ShardMapManager，弹性数据库客户端库" 
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

# Shard 地图管理
在 sharded 数据库环境中，一个**shard 地图**维护允许应用程序连接到正确的数据库基于**分片键**的值的信息。 了解这些图如何构造对管理弹性的数据库客户端库使用的 shards 至关重要。

## Shard 地图和 shard 映射
 
### 受支持的.Net 类型分片键

弹性的比例支持下列.Net 框架类型为分片键︰

* integer
* long
* guid
* byte]  
* 日期时间
* 时间跨度
* 方法

### 列表和范围的 shard 映射
可以使用**单个分片密钥值的列表**，构造 shard 映射或它们可以用来构造使用**分片键的值的范围**。 

###列表 shard 映射
**Shards**包含**shardlets**和 shardlets 到 shards 的映射由 shard 地图进行维护。 **列表 shard 映射**是各个键值标识 shardlets 和 shards 作为数据库之间的关联。  **列表映射**是显式的 （例如，键 1 映射到数据库 A） 和不同的键值可以映射到同一个数据库 （3 和 6 引用数据库 B 项值）。

| Key | Shard 位置 |
|-----|----------------|
| 1   | Database_A     |
| 3   | Database_B     |
| 4   | Database_C     |
| 6   | Database_B     |
| ... | ...            |
 

### 区域 shard 映射 
**范围 shard 映射**中的键范围描述由一对**[低值、 高值）**其中的*低值*是在范围内，最小的键，*高价值*是高于该区域的第一个值。 

例如， **[0, 100)**包括所有大于或等于 0 且小于 100 的整数。 请注意，多个区域都可以指向同一数据库中，并支持不连续的范围 (例如，[100200) 和 [400,600) 都指向数据库 C 在下面的示例。

| Key       | Shard 位置 |
|-----------|----------------|
| [) 1,50    | Database_A     |
| [50100)  | Database_B     |
| [100200) | Database_C     |
| [) 400,600 | Database_C     |
| ...       | ...            

每个上面所示的表是一个**ShardMap**对象的概念示例。  每一行都是一个简化的示例 （用于列表 shard 图） 的单个**PointMapping**或**RangeMapping** （对于范围 shard 图） 对象。

## Shard 地图管理器 

客户端库，在 Shard 映射器 shard 映射的集合。 由**ShardMapManager** .Net 对象的数据保存在以下三个位置︰ 

1. **全球 Shard 映射 (GSM)**: 创建**ShardMapManager**时，您指定数据库作为其 shard 地图和映射的所有存储库。 特殊的表和存储的过程会自动创建管理信息。 这通常是一个小型的数据库，并轻轻地访问，但不是应为其他应用程序的需要。 表位于一个名为**__ShardManagement**的特殊架构中。 

2. **本地 Shard 映射 (LSM)**: 每个数据库指定为 shard shard 地图将被修改以包含几个小表和包含和管理特定于该 shard 的 shard 映射信息的特殊存储的过程。 此信息是对 GSM 中的信息冗余，但它允许应用程序不在 GSM; 上放置任何负载的情况下验证缓存的 shard 映射信息应用程序使用 LSM 确定缓存的映射是否仍然有效。 对应于每个 shard 上 LSM 的表是在架构**__ShardManagement**。

3. **应用程序缓存**︰ 访问**ShardMapManager**对象的每个应用程序实例维护其映射的本地内存中缓存。 它存储最近检索的路由信息。 

## 构建 ShardMapManager
**ShardMapManager**对象的应用程序中实例化使用工厂模式。 **ShardMapManagerFactory.GetSqlShardMapManager**方法在窗体中的**连接字符串**将凭据 （包括服务器名称和数据库名称包含 GSM），并返回**ShardMapManager**的实例。  

**ShardMapManager**应该只有一次被启动每个应用程序域中应用程序的初始化代码。 **ShardMapManager**可以包含任意数量的 shard 映射。 一个 shard 映射可能会不足，而许多应用程序，有时间另一个架构或为唯一目的，当使用不同的数据库集时，在这些情况下，多个 shard 映射可能更合适。 

在此代码中，应用程序试图打开现有的**ShardMapManager**。  如果在数据库内不存在对象表示全局**ShardMapManager** (GSM)，客户端库创建它们存在。

    // Try to get a reference to the Shard Map Manager via the Shard Map Manager database.  
    // If it doesn't already exist, then create it. 
    ShardMapManager shardMapManager; 
    bool shardMapManagerExists = ShardMapManagerFactory.TryGetSqlShardMapManager(
                                        connectionString, 
                                        ShardMapManagerLoadPolicy.Lazy, 
                                        out shardMapManager); 

    if (shardMapManagerExists) 
     { 
        Console.WriteLine("Shard Map Manager already exists");
    } 
    else
    {
        // Create the Shard Map Manager. 
        ShardMapManagerFactory.CreateSqlShardMapManager(connectionString);
        Console.WriteLine("Created SqlShardMapManager"); 

        shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager(
            connectionString, 
            ShardMapManagerLoadPolicy.Lazy);

        // The connectionString contains server name, database name, and admin credentials 
        // for privileges on both the GSM and the shards themselves.
    } 
 

### Shard 地图管理凭据

通常情况下，应用程序管理和操纵 shard 映射是不同于使用路由连接的 shard 映射。 

对于管理 shard 地图 （添加或更改 shards、 shard 地图、 shard 映射等） 的应用程序中，您必须实例化使用**的凭据具有读/写权限和每个数据库，用作 shard 上 GSM 数据库** **ShardMapManager** 。 凭据必须允许针对 GSM 和 LSM 中表写入如输入或更改，以及与创建新的 shards 上的 LSM 表 shard 站点地图信息。  

### 只有受影响的元数据 

方法用于填充或更改**ShardMapManager**数据不会改变 shards 本身中存储的用户数据。 例如，方法如**CreateShard**、 **DeleteShard**、 **UpdateMapping**等影响的 shard 映射元数据只。 不要删除、 添加或修改包含在 shards 中的用户数据。 相反，这些方法旨在用于配合独立的操作执行以创建或删除实际的数据库，或者，行一个 shard 之间移动来重新平衡 sharded 环境。  （附带弹性数据库工具**拆分合并**工具的合并情况使这些 Api 以及协调 shards 之间的实际的数据移动的使用）。 

## 填充一个 shard 映射︰ 示例
 
序列的操作，以填充特定的 shard 映射示例如下所示。 该代码执行以下步骤︰ 

1. 在 shard 地图管理器创建新的 shard 映射。 
2. 两种不同的 shards 的元数据添加到 shard 映射中。 
3. 添加不同的键范围映射，并显示整个 shard 映射的内容。 

编写的代码是一种方式，整个方法可以安全地重新运行时遇到意外的错误--每个请求测试是否 shard 或映射已存在，然后再尝试创建它的情况下。 下面的代码假定，名为**sample_shard_0**、 **sample_shard_1**和**sample_shard_2**的数据库已经在所引用的字符串**shardServer**服务器中创建。 

    public void CreatePopulatedRangeMap(ShardMapManager smm, string mapName) 
        {            
            RangeShardMap<long> sm = null; 

            // check if shardmap exists and if not, create it 
            if (!smm.TryGetRangeShardMap(mapName, out sm)) 
            { 
                sm = smm.CreateRangeShardMap<long>(mapName); 
            } 

            Shard shard0 = null, shard1=null; 
            // check if shard exists and if not, 
            // create it (Idempotent / tolerant of re-execute) 
            if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_0"), out shard0)) 
            { 
                Shard0 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_0")); 
            } 

            if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_1"), out shard1)) 
            { 
                Shard1 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_1"));  
            } 

            RangeMapping<long> rmpg=null; 

            // Check if mapping exists and if not,
            // create it (Idempotent / tolerant of re-execute) 
            if (!sm.TryGetMappingForKey(0, out rmpg)) 
            { 
                sm.CreateRangeMapping(new RangeMappingCreationInfo<long> 
                    (new Range<long>(0, 50), shard0, MappingStatus.Online)); 
            } 

            if (!sm.TryGetMappingForKey(50, out rmpg)) 
            { 
                sm.CreateRangeMapping(new RangeMappingCreationInfo<long> 
                    (new Range<long>(50, 100), shard1, MappingStatus.Online)); 
            } 

            if (!sm.TryGetMappingForKey(100, out rmpg)) 
            { 
                sm.CreateRangeMapping(new RangeMappingCreationInfo<long> 
                    (new Range<long>(100, 150), shard0, MappingStatus.Online)); 
            } 

            if (!sm.TryGetMappingForKey(150, out rmpg)) 
            { 
                sm.CreateRangeMapping(new RangeMappingCreationInfo<long> 
                    (new Range<long>(150, 200), shard1, MappingStatus.Online)); 
            } 

            if (!sm.TryGetMappingForKey(200, out rmpg)) 
            { 
               sm.CreateRangeMapping(new RangeMappingCreationInfo<long> 
                   (new Range<long>(200, 300), shard0, MappingStatus.Online)); 
            } 

            // List the shards and mappings 
            foreach (Shard s in sm.GetShards()
                                    .OrderBy(s => s.Location.DataSource)
                                    .ThenBy(s => s.Location.Database))
            { 
               Console.WriteLine("shard: "+ s.Location); 
            } 

            foreach (RangeMapping<long> rm in sm.GetMappings()) 
            { 
                Console.WriteLine("range: [" + rm.Value.Low.ToString() + ":" 
                        + rm.Value.High.ToString()+ ")  ==>" +rm.Shard.Location); 
            } 
        } 
 
作为一种替代方法可以使用 PowerShell 脚本来实现相同的结果。     

一旦填充 shard 映射了，数据访问应用程序来创建或修改映射的工作。 填充或操作映射需要之前不会重新**映射布局**需要进行更改。  

## 数据依赖于路由 

大多数使用 shard 地图管理器将来自应用程序需要的数据库连接来执行特定于应用程序的数据操作。 在 sharded 应用程序中，这些连接现在必须与正确的目标数据库。 这称为**数据依赖于路由**。  这些应用程序，实例化一个 shard 地图管理器对象从工厂使用 GSM 数据库具有只读访问权限的凭据。 单个连接的请求以后将提供所需的连接到相应的 shard 数据库的凭据。

请注意 （使用**ShardMapManager**使用只读的凭据打开） 这些应用程序将不能更改映射或映射。  针对这些需求，创建管理特定的应用或提供更高权限凭据，如前文所述的 PowerShell 脚本。   

有关详细信息，请参阅[数据依赖于路由](sql-database-elastic-scale-data-dependent-routing.md)。 

## 修改一个 shard 映射 

Shard 地图可以以不同的方式进行更改。 所有下列方法修改元数据描述 shards 和其映射，但他们不实际修改中 shards，数据也不请不要创建或删除实际的数据库。  一些在 shard 图如下所述的操作可能需要进行协调与管理操作的物理移动数据时，或者，添加和删除数据库作为 shards。

这些方法一起使用构建基块可用于修改 sharded 数据库环境中的数据的总体分布。  

* 若要添加或删除 shards︰ 使用**CreateShard**和**DeleteShard**。 
    
    服务器和表示目标 shard 的数据库必须已经存在为来执行这些操作。 这些方法本身，只在 shard 映射中的元数据的数据库没有任何影响。

* 若要创建或删除点或范围映射到 shards︰ 使用**CreateRangeMapping**， **DeleteMapping**， **CreatePointMapping**。 
    
    许多不同点或区域可以映射到同一个 shard 中。 这些方法只会影响元数据--不会影响任何数据，可能已经存在于 shards。 如果需要才能符合**DeleteMapping**操作从数据库中删除数据，将需要执行这些操作分别，但使用这些方法的结合。  

* 若要将现有区域拆分为两个，或为一个合并的相邻区域︰ 使用**SplitMapping**和**MergeMappings**。  

    拆分和合并操作**不会更改键值映射到的 shard**的注意。 剥离现有的区域分成两个部分，但离开都映射到同一个 shard。 已映射到同一个 shard，它们合并到单个区域的两个相邻区域合并操作。  移动的点或 shards 之间本身范围需要通过结合实际的数据移动使用**UpdateMapping**来进行协调。  可以使用**拆分/合并**服务协调 shard 映射更改与数据移动需要移动时的弹性数据库工具的一部分。 

* 重新映射 （或移动） 各点或范围到不同 shards︰ 使用**UpdateMapping**。  

    因为数据可能需要从一个 shard 之间移动才能符合**UpdateMapping**操作，您需要执行，移动分开，但使用这些方法的结合。

* 要使在线和离线的映射︰ 使用**MarkMappingOffline**和**MarkMappingOnline**来控制联机状态的映射。 

    在"脱机"状态，包括**UpdateMapping**和**DeleteMapping**中映射时只允许 shard 映射某些操作。 映射脱机时，基于该映射中包含的键依赖于数据的请求会返回错误。 此外，当第一次脱机时一个范围，所有连接到受影响的 shard 自动会都终止以防止不一致或不完整的结果，查询定向对被更改的范围。 

映射是.Net 中的不可变对象。  所有上述更改映射的方法还使对它们在代码中的任何引用。   可以更加轻松地执行的映射状态更改的操作序列，所有更改映射的方法返回一个新的映射引用，以便可以链接操作。  例如，若要删除现有的映射中包含的密钥 25 shardmap sm，您可以执行下列语句︰ 

        sm.DeleteMapping(sm.MarkMappingOffline(sm.GetMappingForKey(25)));

## 添加 shard 

应用程序经常需要只需添加新的 shards 来处理数据，需要从新的键或键范围，为 shard 映射已存在。 例如，按租户 ID sharded 应用程序可能需要为新的租户，设置新的 shard 或数据 sharded 每月可能需要配置的每个新的月份开始之前新 shard。 

如果新键值的范围已经不是映射的现有的一部分，并且无需移动数据是映射的必要的它是映射的非常简单，若要添加新的 shard 并将新的密钥或到该 shard 的范围相关联。 有关添加新 shards 的详细信息，请参见[添加新的 shard](sql-database-elastic-scale-add-a-shard.md)。

对于需要移动数据的方案，但是，拆分合并工具需要协调之间移动数据，结合必要的 shard 映射更新 shards。 使用拆分合并 yool 的详细信息，请参阅[拆分合并的概述](sql-database-elastic-scale-overview-split-and-merge.md) 

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
 
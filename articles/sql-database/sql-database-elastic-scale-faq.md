---
ms.openlocfilehash: fce9e18ac689153a33f3bb86f4b0e761a568b6b4
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="SQL azure 弹性比例的常见问题解答" 
    description="SQL Azure 数据库弹性比例有关的常见问题。" 
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

# 弹性数据库工具的常见问题 

#### 如果我有每个 shard 和没有分片键单租户，如何填充架构信息的分片键？
架构信息对象仅用于拆分合并方案。 如果应用程序本质上是单租户，不需要拆分合并工具，并因此没有必要来填充架构信息对象。

#### 我已经设置数据库和我已经有一个 Shard 地图管理器，如何将此新数据库注册为 shard？
请参阅**[添加到应用程序中使用弹性数据库客户端库 shard](sql-database-elastic-scale-add-a-shard.md)**。 

#### 弹性数据库工具的成本多少？
使用弹性数据库客户端库不会引起任何成本。 成本累算仅为 shards 和 Shard 地图管理器中，您使用 SQL Azure 数据库以及拆分合并工具为您配置 web/辅助角色。

#### 为什么我的凭据不工作时我从另一台服务器添加 shard？
不使用凭据"用户 ID=username@servername"的形式，改为只需使用"用户 ID = 用户名"。  还要确保"username"登录在 shard 上具有权限。

#### 是否需要 Shard 地图管理器中创建和填充 shards，每次我开始我的应用程序？
否 — Shard 地图管理器 (例如， **[ShardMapManagerFactory.CreateSqlShardMapManager](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.createsqlshardmapmanager.aspx)**) 的创建是一个一次性的操作。  您的应用程序应使用在应用程序启动时调用**[ShardMapManagerFactory.TryGetSqlShardMapManager()](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx)** 。  那里应只有一个此类调用每个应用程序域。

#### 我有关于使用弹性数据库工具的问题，如何获取这些回答吗？ 
请访问我们在[SQL Azure 数据库论坛](https://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted)。

#### 获取数据库连接使用分片键，当我仍可以相同的 shard 上其他分片键的查询数据。  这是设计使然吗？
弹性的扩展 Api 为您提供一个连接到正确的数据库为分片密钥，但不是提供分片键筛选。  如果需要，请添加**WHERE**子句向查询提供的分片键的限制范围。

#### 可以使用不同的 Azure 数据库版本的每个 shard shard 集中我的？
是，shard 是单独的数据库，并因此一个 shard 可以是更高级版本，而另一个是标准版。 此外，shard 的版本可以 shard 的生存期内多次放大或缩小。

#### 拆分合并工具提供 （或删除） 拆分或合并操作期间数据库吗？ 
No. **拆分**操作，目标数据库必须有适当的架构和使用 Shard 地图管理器进行注册。  **合并**操作，必须先从 shard 地图管理器删除 shard，然后删除该数据库。

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
 
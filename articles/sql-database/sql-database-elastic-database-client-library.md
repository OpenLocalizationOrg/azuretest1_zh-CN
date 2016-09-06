---
ms.openlocfilehash: 7e2b8b1393de26f773246920c100a883a01ad851
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="Azure 的 SQL 数据库的客户端库"
    description="构建可扩展的.NET 数据库应用程序"
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
    ms.date="07/29/2015"
    ms.author="sidneyh"/>

# 弹性的数据库客户端库概述

**弹性的数据库客户端库**可以帮助您轻松地开发使用数以百计的 sharded 应用程序 — — 或甚至数千个 — — 的 SQL Azure 数据库驻留在 Microsoft Azure 上。 这种设计通常用于软件作为服务 (SaaS) 应用程序，这通常单个租户的体系结构 — 每个租户提供数据库位置。 构建和管理这样的应用程序是库的目标。 客户端库是可安装到任何应用程序项目中使用[Visual Studio](sql-database-elastic-scale-add-references-visual-studio.md)和[Nuget](http://go.microsoft.com/?linkid=9862605).NET Framework 库。 客户端库是弹性数据库工具的一部分，这是专门[弹性数据库功能](sql-database-elastic-scale-introduction.md)。 

## 客户端功能

开发、 比例和管理使用*分片*（在下面讨论） 向外扩展的应用程序开发人员以及管理员提出的挑战。 客户端库使生活更容易为这两个角色。 下大纲图弹性数据库客户端库提供主要功能。 图显示了一个具有多个数据库，环境，每个数据库的 shard 对应。 在此示例中，许多客户位于在同一数据库中使用区域地图，虽然每个客户 （租户） 数据库是否同样适用。 这些工具使开发 sharded 的 SQL Azure 数据库应用程序更容易通过以下特定功能︰

这里使用的术语的定义，请参阅[弹性数据库工具词汇表](sql-database-elastic-scale-glossary.md)。

![灵活的伸缩能力][1]

1.  **Shard 地图管理**︰ 管理的 shards 集合，speical 称为"shard 映射经理"将创建一个数据库。 Shard 地图管理是应用程序来管理其 shards 有关的各种元数据的能力。 开发人员可以使用此功能注册为 shards 的数据库、 描述各个分片键或键范围到这些数据库的映射和维护数量为此元数据和数据库的组合发展以反映容量的变化。 没有弹性的数据库客户端库，您需要花费大量时间编写管理代码时实施分片。 有关详细信息，请参阅[Shard 映射管理](sql-database-elastic-scale-shard-map-management.md)。

* **数据依赖于路由**︰ 设想一下，进入应用程序的请求。 根据请求的分片密钥值，则应用程序需要确定正确持有此密钥的值的数据的数据库，然后打开用于处理请求的连接。 数据依赖于路由提供 shard 在地图应用程序一次轻松调用打开连接的能力。 数据依赖于路由时的现在都由弹性数据库客户端库中的功能的基础结构代码的另一个领域。 有关详细信息，请参阅[数据依赖于路由](sql-database-elastic-scale-data-dependent-routing.md)。

* **多 shard 查询 (MSQ)**: 当请求涉及几个 （或所有） shards 多 shard 查询工作。 多 shard 查询所有 shards 或 shards 一套上执行相同的 T SQL 代码。 从参与 shards 的结果合并到使用 UNION ALL 语义整体结果集。 通过客户端库中公开的功能处理多项任务，包括︰ 连接管理、 线程管理、 错误处理和处理的中间结果。 MSQ 可以查询达数百 shards。 有关详细信息，请参阅[多 shard 查询](sql-database-elastic-scale-multishard-querying.md)。

一般情况下，使用弹性数据库工具的客户可望提交 shard 本地操作，而不是有自己的语义的跨 shard 操作时获取完整 T SQL 功能。

## 下一步行动

请尝试将[示例应用程序](sql-database-elastic-scale-get-started.md)演示客户端函数。 

要安装此库，请转到[弹性数据库客户端库]( http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)。

使用拆分合并工具的说明，请参阅[拆分合并工具概述](sql-database-elastic-scale-overview-split-and-merge.md)。


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->
[1]:./media/sql-database-elastic-database-client-library/glossary.png


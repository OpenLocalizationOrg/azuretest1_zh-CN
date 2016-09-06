---
ms.openlocfilehash: 51d8e56bf2c1ab6004ae022f65cde9c9d95b03bc
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="视图在 SQL 数据仓库中的 |Microsoft Azure"
   description="在 Azure SQL 数据仓库中使用事务处理 SQL 视图，用于开发解决方案的提示。"
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
   ms.date="06/22/2015"
   ms.author="JRJ@BigBangData.co.uk;barbkess"/>

 
# SQL 数据仓库中数据的视图

视图在 SQL 数据仓库中是特别有用。 他们可以用许多不同方法来提高解决方案的质量。

本文着重介绍如何通过使用视图实现丰富您的解决方案的几个示例。 有一些还需要考虑的限制。

## 体系结构的抽象
一种非常常见的应用程序模式是重新创建表使用创建表作为选择 (CTAS) 跟一个对象，该对象加载数据的同时重命名模式。 

下面的示例将新的日期记录添加到日期维度。 请注意如何第一次创建一个新对象，DimDate_New，然后重命名，以替换对象的原始版本。 

```
CREATE TABLE dbo.DimDate_New
WITH (DISTRIBUTION = REPLICATE
, CLUSTERED INDEX (DateKey ASC)
)
AS 
SELECT *
FROM   dbo.DimDate  AS prod
UNION ALL
SELECT *
FROM   dbo.DimDate_stg AS stg
;

RENAME OBJECT DimDate TO DimDate_Old;
RENAME OBJECT DimDate_New TO DimDate;

```

但是，这可能导致出现和消失在 SSDT SQL Server 对象资源管理器视图中用户的表对象。 可以使用视图基础对象被重命名，而仓库数据使用者提供一个一致的表示层。 通过视图数据提供访问权限意味着用户不需要有基础表的可见性。 这提供了一致的用户体验，同时确保数据仓库设计人员可以衍生数据模型，并还在数据载入过程期间使用 CTAS 最佳性能。    

## 性能优化
视图是智能化的方式来实施最佳性能表之间的联接。 例如视图可以合并冗余分布键最小化数据移动的联接条件的一部分。  另一个原因可能是，强制一个特定查询或联接提示。 这保证了加入始终以最佳的方式执行，并不依赖于用户记住正确构建联接。

## 限制
SQL 数据仓库中数据的视图是仅元数据。 

因此以下选项将不可用︰
-   没有架构绑定选项
-   不能通过视图更新基表
-   不能在临时表上创建视图
-   没有用于展开支持 / NOEXPAND
-   在 SQL 数据仓库中没有索引的视图


## 下一步行动
开发的更多提示，请参阅[SQL 数据仓库开发概述][]。

<!--Image references-->

<!--Article references-->
[SQL 数据仓库开发概述]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->



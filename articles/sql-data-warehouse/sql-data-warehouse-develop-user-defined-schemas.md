---
ms.openlocfilehash: 40dcdda9dcafee12d6d7941090a93e3c1a9b7c47
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="SQL 数据仓库中的用户定义架构 |Microsoft Azure"
   description="在 Azure SQL 数据仓库中使用事务处理 SQL 架构，用于开发解决方案的提示。"
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

# 用户定义的 SQL 数据仓库中的架构

传统的数据仓库通常使用单独的数据库来创建基于工作负荷、 域或安全应用程序边界。 例如，传统的 SQL Server 数据仓库可能包括临时数据库、 数据仓库数据库，以及一些数据集市数据库。 在此拓扑中每个数据库工作负荷和体系结构中的安全边界。

与此相反，SQL 数据仓库运行在一个数据库内的整个数据仓库工作负载。 跨数据库不允许连接。 因此，SQL 数据仓库需要仓库用于存储在一个数据库中的所有表。

> [AZURE.NOTE] SQL 数据仓库不支持任何形式的跨数据库查询。 因此，利用此模式的数据仓库实现将需要进行修订。

## 建议 

这是通过使用用户定义架构整合工作负载、 域和职能界限的一些建议

1. 使用一个 SQL 数据仓库数据库运行您的整个数据仓库工作负载
2. 整合现有的数据仓库环境使用一个 SQL 数据仓库数据库
3. 利用**用户定义的架构**来提供以前使用数据库实现的边界。

如果用户定义的架构有没有使用过则有明确的结论。 只需为在 SQL 数据仓库数据库中用户定义架构为基础使用旧的数据库名称。

如果已使用架构您有几个选项︰

1. 删除旧的架构名称并重新开始
2. 通过预挂起旧架构名称与表名称保留旧的架构名
3. 通过实施通过重新创建的旧的架构结构附加架构中的表的视图将保留旧的架构名称。

> [AZURE.NOTE] 在第一个检查选项 3 似乎最有吸引力的选项。 但是，魔鬼是在细节中。 仅在 SQL 数据仓库中读取的视图。 任何数据或表格的修改将需要对基础表执行。 选项 3 还将视图层引入到您的系统。 您可以如果您使用的视图在您的体系结构中已经为此提供额外考虑一些事情。


### 示例︰

1. 实现用户定义架构基于数据库的名称

```
CREATE SCHEMA [stg]; -- stg previously database name for staging database
GO
CREATE SCHEMA [edw]; -- edw previously database name for the data warehouse
GO
CREATE TABLE [stg].[customer] -- create staging tables in the stg schema
(       CustKey BIGINT NOT NULL
,       ...
);
GO
CREATE TABLE [edw].[customer] -- create data warehouse tables in the edw schema
(       CustKey BIGINT NOT NULL
,       ...
);
```

2. 保留旧的架构名称由预挂起到表的名称。 架构用于工作负载边界。

```
CREATE SCHEMA [stg]; -- stg defines the staging boundary
GO
CREATE SCHEMA [edw]; -- edw defines the data warehouse boundary
GO
CREATE TABLE [stg].[dim_customer] --pre-pend the old schema name to the table and create in the staging boundary
(       CustKey BIGINT NOT NULL
,       ...
);
GO
CREATE TABLE [edw].[dim_customer] --pre-pend the old schema name to the table and create in the data warehouse boundary
(       CustKey BIGINT NOT NULL
,       ...
);
```

3. 保留旧的架构名称使用视图

```
CREATE SCHEMA [stg]; -- stg defines the staging boundary
GO
CREATE SCHEMA [edw]; -- stg defines the data warehouse boundary
GO
CREATE SCHEMA [dim]; -- edw defines the legacy schema name boundary
GO
CREATE TABLE [stg].[customer] -- create the base staging tables in the staging boundary
(       CustKey BIGINT NOT NULL
,       ...
)
GO
CREATE TABLE [edw].[customer] -- create the base data warehouse tables in the data warehouse boundary
(       CustKey BIGINT NOT NULL
,       ...
)
GO
CREATE VIEW [dim].[customer] -- create a view in the legacy schema name boundary for presentation consistency purposes only
AS
SELECT  CustKey
,       ...
FROM    [edw].customer
;
```

> [AZURE.NOTE] 战略架构中的任何更改需要数据库安全模型的评审。 在许多情况下，您可能能够通过分配架构级别的权限来简化安全模型。 如果然后，您可以使用数据库角色更为细化的权限是必需的。

## 下一步行动
开发的更多提示，请参阅[开发概述][]。

<!--Image references-->

<!--Article references-->
[开发概述]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
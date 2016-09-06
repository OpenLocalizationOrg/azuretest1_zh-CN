---
ms.openlocfilehash: 64d7fd96f811274fdabb6f2c5cf98f8e2a87bd7d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="将 SQL 代码迁移到 SQL 数据仓库 |Microsoft Azure"
   description="迁移到 Azure SQL 数据仓库的开发解决方案的 SQL 代码的提示。"
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
   ms.date="06/25/2015"
   ms.author="JRJ@BigBangData.co.uk;barbkess"/>

# 将 SQL 代码迁移到 SQL 数据仓库

为了确保您的代码符合 SQL 的数据仓库是很可能需要对您的代码基进行更改。 某些 SQL 数据仓库功能也大大可以提高性能，因为它们被设计为直接以分散的方式工作。 但是，若要保持性能和可扩展性，某些功能不可也。

## 事务处理 SQL 代码更改

下面的列表汇总了 Azure SQL 数据仓库中不支持的主要功能。 所提供的链接可将您带到不支持的功能的替代方法︰

- [有关更新 ANSI 联接][]
- [在删除的 ANSI 联接][]
- [merge 语句][]
- 跨数据库联接
- [游标][]
- [选择。.到][]
- [插入.执行][]
- output 子句
- 内嵌用户定义函数
- 多语句函数
- [递归公用表表达式 (CTE)](#Recursive-common-table-expressions-(CTE)
- [通过 CTEs 更新](#Updates-through-CTEs)
- CLR 函数和过程
- $partition 函数
- 表变量
- 表值参数
- 分布式的事务
- 提交 / 回滚工时
- 保存事务
- 执行上下文 (EXECUTE AS)
- [by 子句具有汇总分组 / 多维数据集 / 分组集选项][]
- [嵌套级别超过 8][]
- [通过视图更新][]
- [选择用于变量赋值][]
- [没有最大值数据类型动态 SQL 字符串][]

高兴地大部分这些限制可以变通方案。 说明已包括在上述引用的相关开发文章。

### 递归公用表表达式 (CTE)

这是与没有快速修复的复杂方案。 CTE 将需要在步骤中处理和分解。 通常，您可以使用一个相当复杂的循环;填充一个临时表，如迭代递归临时查询。 一旦填充临时表可以作为一个结果集然后返回数据。 采用了一种类似的方法来解决`GROUP BY WITH CUBE` [by 子句具有汇总分组 / 多维数据集 / 分组集选项][]文章中。

### 通过 CTEs 更新

如果非递归 CTE 然后可以重新编写该查询使用子查询。 您需要为递归 CTEs 建立结果集第一次为上述;然后加入到目标表的最终结果集，并执行更新。

### 系统函数

也有一些不支持的系统函数。 下面是一些主要的通常，您可能会发现在数据仓库中使用︰

- NEWID()
- NEWSEQUENTIALID()
- @@NESTLEVEL()
- @@IDENTITY()
- @@ROWCOUNT()
- ROWCOUNT_BIG
- ERROR_LINE()

再多的问题可以变通方案。 

例如，下面的代码是 @@ROWCOUNT 信息检索的替代解决方案︰

```
SELECT  SUM(row_count) AS row_count 
FROM    sys.dm_pdw_sql_requests 
WHERE   row_count <> -1 
AND     request_id IN 
                    (   SELECT TOP 1    request_id 
                        FROM            sys.dm_pdw_exec_requests 
                        WHERE           session_id = SESSION_ID() 
                        ORDER BY end_time DESC
                    )
;
``` 

## 下一步行动
建议开发代码请参阅[开发概述][]。

<!--Image references-->

<!--Article references-->
[有关更新 ANSI 联接]: sql-data-warehouse-develop-ctas.md
[在删除的 ANSI 联接]: sql-data-warehouse-develop-ctas.md
[merge 语句]: sql-data-warehouse-develop-ctas.md
[插入.执行]: sql-data-warehouse-develop-temporary-tables.md

[游标]: sql-data-warehouse-develop-loops.md
[选择。.到]: sql-data-warehouse-develop-ctas.md
[by 子句具有汇总分组 / 多维数据集 / 分组集选项]: sql-data-warehouse-develop-group-by-options.md
[嵌套级别超过 8]: sql-data-warehouse-develop-transactions.md
[通过视图更新]: sql-data-warehouse-develop-views.md
[选择用于变量赋值]: sql-data-warehouse-develop-variable-assignment.md
[没有最大值数据类型动态 SQL 字符串]: sql-data-warehouse-develop-dynamic-sql.md
[开发概述]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->

---
ms.openlocfilehash: bd4293d06e73a368ed7beb350bb35c89906d5f61
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="在 SQL 数据仓库中存储过程 |Microsoft Azure"
   description="实现技巧 Azure SQL 数据仓库中开发的解决方案的存储过程。"
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

# SQL 数据仓库中数据的存储的过程 

SQL 数据仓库支持的许多事务处理 SQL 功能在 SQL Server 中找到。 更重要的是有特定的功能，我们将想要利用最大化您的解决方案的性能扩展。

不过，以保持规模和 SQL 数据仓库的性能有也是某些功能和具有行为上的差别和不支持的其他功能。

这篇文章解释了如何实现 SQL 数据仓库中的存储的过程。

## 引入了存储的过程
存储的过程是一个好的方法，用于封装您的 SQL 代码;接近中数据仓库的数据存储。 通过将代码封装到可管理的单位存储的过程可帮助开发人员模块化及其解决办法;促进更好的代码 re-usability。 每个存储的过程还可以接受参数，以使其更加灵活。

SQL 数据仓库提供了简化和优化存储的过程实现。 与 SQL Server 相比最大的区别是，存储的过程不是预编译的代码。 在数据仓库中我们也通常不太关心编译时间。 它是更重要的是工作对大数据量时，存储的过程代码正确地优化。 目标是以小时、 分钟和秒不毫秒保存。 因此是更有帮助，可以看作是存储过程的 SQL 逻辑的容器。     
 
当 SQL 数据仓库执行存储的过程的 SQL 语句分析，翻译，并在运行时优化。 在此过程中的每个语句将被转换为分布式查询中。 实际执行对数据的 SQL 代码是不同于提交查询。

## 嵌套存储过程
当存储的过程调用其它存储的过程，或被称为嵌套的内部存储的过程或代码调用，则执行动态 sql。

SQL 数据仓库支持最多 8 个嵌套级别。 这是 SQL Server 到略有不同。 在 SQL Server 中的嵌套级别为 32。

顶层级别的存储的过程调用相当嵌套级别 1

```
EXEC prc_nesting
``` 
如果存储的过程也会使另一个 EXEC 调用然后这将会增加为 2 的嵌套级别
```
CREATE PROCEDURE prc_nesting
AS
EXEC prc_nesting_2  -- This call is nest level 2
GO
EXEC prc_nesting
```
如果第二个过程然后执行某些动态 sql，然后这将增加为 3 的嵌套级别
```
CREATE PROCEDURE prc_nesting_2
AS
EXEC sp_executesql 'SELECT 'another nest level'  -- This call is nest level 2
GO
EXEC prc_nesting
```

注意 SQL 数据仓库目前不支持 @@NESTLEVEL。 您将需要自己保留嵌套级别的轨道。 它不太可能将命中 8 嵌套级别限制，但如果这样做将需要重新运行您的代码并"拼合"它以使其适应在此限制内。 

## 插入.执行
SQL 数据仓库不允许您使用 INSERT 语句的存储过程的结果集。 但是，没有可以使用另一种方法。

请参考以下文章对[临时表]有关如何执行此操作的示例。

## 限制

有几个方面的事务处理 SQL 存储过程的 SQL 数据仓库中都没有实现。

它们是︰

- 临时存储的过程
- 带编号的存储的过程
- 扩展存储的过程
- CLR 存储过程
- 加密选项
- 复制选项
- 表值参数
- 只读的参数
- 默认参数
- 执行上下文
- 返回语句

## 下一步行动
开发的更多提示，请参阅[开发概述][]。

<!--Image references-->

<!--Article references-->
[临时表]: sql-data-warehouse-develop-temporary-tables.md
[开发概述]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[嵌套级别]: https://msdn.microsoft.com/en-us/library/ms187371.aspx

<!--Other Web references-->


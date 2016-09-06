---
ms.openlocfilehash: 24153a06aebe0976d83eb7e6d3d863a85833ee83
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="SQL 数据仓库中的交易记录 |Microsoft Azure"
   description="开发解决方案在 Azure SQL 数据仓库中实现事务的提示。"
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
   ms.date="06/26/2015"
   ms.author="JRJ@BigBangData.co.uk;barbkess"/>

# SQL 数据仓库中的交易记录

就象 SQL 数据仓库确实提供了对所有事务属性的支持。 但是，以确保 SQL 数据仓库的性能维护大规模某些功能将受到限制与 SQL Server 相比。 这文章突出显示差异，并列出了其他人。 

## 事务隔离级别
SQL 数据仓库实现 ACID 事务。 但是，事务处理支持的隔离仅限于`READ UNCOMMITTED`，这不能更改。 您可以实现大量的编码方法，以防止脏读的数据，如果这是您的一个问题。 最普遍的方法利用 CTAS 和表分区切换 （通常称为滑动窗口模式），以防止用户查询仍然准备的数据。 预筛选的数据的视图也是一种常用的方法。  

## 事务状态
SQL 数据仓库使用 XACT_STATE() 函数来报告一个失败的事务，使用值-2。 这意味着事务失败，并且被标记为仅回滚

> [AZURE.NOTE] 使用 XACT_STATE 函数来表示一个失败的事务-2 表示对 SQL Server 的不同的行为。 SQL Server 使用值-1 表示未提交的事务。 SQL Server 能够容忍没有被标记为未提交的事务内部的一些错误。 例如选择的 1 0 将导致错误，但不是强制到未提交状态的事务。 SQL Server 还允许读取未提交的事务中。 但是，在 SQLDW 中这不是这种情况。 如果错误发生在 SQLDW 事务将自动进入-2 状态︰ 包括 1/0 的选择错误。 因此，是重要的要检查的应用程序代码，以查看它是否使用 XACT_STATE()。

在 SQL Server 中可能会看到如下所示的代码片段︰

```
BEGIN TRAN
    BEGIN TRY
        DECLARE @i INT;
        SET     @i = CONVERT(int,'ABC');
    END TRY
    BEGIN CATCH

        DECLARE @xact smallint = XACT_STATE();

        SELECT  ERROR_NUMBER()    AS ErrNumber
        ,       ERROR_SEVERITY()  AS ErrSeverity
        ,       ERROR_STATE()     AS ErrState
        ,       ERROR_PROCEDURE() AS ErrProcedure
        ,       ERROR_MESSAGE()   AS ErrMessage
        ;

        ROLLBACK TRAN;

    END CATCH;
```

请注意，`SELECT`语句之前发生`ROLLBACK`语句。 此外请注意，该设置的`@xact`变量使用 DECLARE 和不`SELECT`。

SQL 数据仓库中的代码可能需要如下所示︰

```
BEGIN TRAN
    BEGIN TRY
        DECLARE @i INT;
        SET     @i = CONVERT(int,'ABC');
    END TRY
    BEGIN CATCH

        ROLLBACK TRAN;

        DECLARE @xact smallint = XACT_STATE();

        SELECT  ERROR_NUMBER()    AS ErrNumber
        ,       ERROR_SEVERITY()  AS ErrSeverity
        ,       ERROR_STATE()     AS ErrState
        ,       ERROR_PROCEDURE() AS ErrProcedure
        ,       ERROR_MESSAGE()   AS ErrMessage
        ;
    END CATCH;

SELECT @xact;
```

请注意，回滚事务已读取中的错误信息之前发生`CATCH`块。

## Error_Line() 函数
它也是值得注意 SQL 数据仓库不实施或支持 ERROR_LINE() 的功能。 如果您拥有此代码需要先删除它以符合 SQL 的数据仓库中。 改为使用查询代码中的标签来实现等效的功能。 请 [查询标签] 文章有关此功能的详细信息，参阅。

## 使用抛出和 RAISERROR
抛出引发异常 SQL 数据仓库中的更现代实现，但也支持 RAISERROR。 有几个值得但是关注到的差异。

- 用户定义的错误消息编号不能抛出的 100000 150000 范围内
- 在 50000 固定 RAISERROR 错误消息
- 不支持使用 sys.messages

## 限制
SQL 数据仓库有一些与交易记录相关的其他限制。

它们如下所示︰

- 没有分布式的事务
- 不允许使用嵌套的事务
- 无保存点允许

## 下一步行动
开发的更多提示，请参阅[开发概述][]。

<!--Image references-->

<!--Article references-->
[开发概述]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->

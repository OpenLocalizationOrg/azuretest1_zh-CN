---
ms.openlocfilehash: 25b24647920dd3922b8b4bc9d423e5159f31233e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="指定 SQL 数据仓库中的变量 |Microsoft Azure"
   description="将事务处理 SQL Azure SQL 数据仓库中的变量分配用于开发解决方案的提示。"
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

# 指定 SQL 数据仓库中的变量
使用设置 SQL 数据仓库中的变量`DECLARE`语句或`SET`语句。 

以下是最有效的方式来设置变量的值︰

## 设置与声明的变量

初始化声明的变量是最灵活的方法，以在 SQL 数据仓库中设置变量的值之一。

```
DECLARE @v  int = 0
;
```

您可以使用声明要一次设置多个变量。 您不能使用`SELECT`或`UPDATE`若要执行此操作︰

```
DECLARE @v  INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Smith')
,       @v1 INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Jones')
;
```

您不能初始化，并在同一条声明语句中使用变量。 来说明这一点下面的示例中不**是**允许因为 @p1 会初始化并在同一条声明语句中使用。 这将导致一个错误。

```
DECLARE @p1 int = 0
,       @p2 int = (SELECT COUNT (*) FROM sys.types where is_user_defined = @p1 )
;
```

## 设置值与集
集是设置一个变量的常用方法。

下面的示例中都有效的方法来使用一组设置一个变量︰

```
SET     @v = (Select max(database_id) from sys.databases);
SET     @v = 1;
SET     @v = @v+1;
SET     @v +=1;
```

您只能设置一个变量每次用一组。 但是，可以看到上面复合运算符是 permissable。

## 限制
您不能使用选择或更新变量赋值。


## 下一步行动
开发的更多提示，请参阅[开发概述][]。

<!--Image references-->

<!--Article references-->
[开发概述]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->

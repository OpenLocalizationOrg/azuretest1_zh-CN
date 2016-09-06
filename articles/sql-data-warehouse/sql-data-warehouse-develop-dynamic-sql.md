---
ms.openlocfilehash: 3f0df36b6ffe5dbf8e9aa202f30abdd2f2a825a8
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="SQL 数据仓库中的动态 SQL |Microsoft Azure"
   description="用于动态 SQL Azure SQL 数据仓库中开发的解决方案的提示。"
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

# SQL 数据仓库中的动态 SQL
为 SQL 数据仓库开发的应用程序代码时可能发现需要使用动态 sql，以帮助提供灵活的、 通用的和模块化解决方案。 但是，SQL 数据仓库不支持这一次 blob 数据类型。 如 blob 类型包括 varchar(max) 和 nvarchar(max) 的类型，这可能会限制您的字符串的大小。 您可能会发现您已经使用这些类型在应用程序代码中生成非常大串需要执行的动态 SQL 代码时。 

在这些情况下您需要将代码分成块并改为使用 EXEC 语句。 

一个简化的示例是使用下面的︰

```
DECLARE @sql_fragment1 VARCHAR(8000)=' SELECT name '
,       @sql_fragment2 VARCHAR(8000)=' FROM sys.system_views '
,       @sql_fragment3 VARCHAR(8000)=' WHERE name like ''%table%''';

EXEC( @sql_fragment1 + @sql_fragment2 + @sql_fragment3);
```

如果该字符串不是特别长然后可以使用[sp_executesql][]作为正常。


## 下一步行动
开发的更多提示，请参阅[开发概述][]。

<!--Image references-->

<!--Article references-->
[开发概述]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[sp_executesql]: https://msdn.microsoft.com/en-us/library/ms188001.aspx

<!--Other Web references-->

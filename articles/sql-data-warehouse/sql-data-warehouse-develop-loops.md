---
ms.openlocfilehash: 8501827f99b087ac43100d718ebccb38efe9458b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="SQL 数据仓库中的循环 |Microsoft Azure"
   description="事务处理 SQL 循环和替换的游标 Azure SQL 数据仓库中开发的解决方案的提示。"
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

# SQL 数据仓库中的循环
SQL 数据仓库支持[WHILE][]循环重复执行语句块。 这将继续，只要指定的条件为真或代码明确终止循环使用的`BREAK`关键字。 循环可以特别用于替换 SQL 代码中定义的游标。 幸运的是，编写 SQL 代码中的几乎所有游标都是快进的阅读只不同。 因此[WHILE]循环都是非常好的替代如果您发现自己无需替换其中一个。

## 利用循环和替换 SQL 数据仓库中的游标
不过之前在头前, 您应该问自己下面的问题:"可以此游标重新编写使用基于集合运算？"。 在许多情况下回答将是肯定的。 往往是最好的方法是做到这一点。 设置基于操作通常将大大快于一种逐行迭代方法执行。

仅快速向前读取的游标可以方便地替换循环构造。 下面是一个简单的示例来传递方法。 该代码示例更新数据库中所有表的统计信息。 通过循环访问表中的循环，我们将能够按顺序执行每个命令。

首先，创建一个临时表包含唯一的行号用于标识单个语句︰
  
```
CREATE TABLE #tbl 
WITH 
(   LOCATION     = USER_DB
,   DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT  ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) ) AS Sequence
,       [name]
,       'UPDATE STATISTICS '+QUOTENAME([name]) AS sql_code
FROM    sys.tables
;
```

其次，初始化执行循环所需的变量︰

```
DECLARE @nbr_statements INT = (SELECT COUNT(*) FROM #tbl)
,       @i INT = 1
;
```

现在对语句执行一个循环一次︰

```
WHILE   @i <= @nbr_statements
BEGIN
    DECLARE @sql_code NVARCHAR(4000) = (SELECT sql_code FROM #tbl WHERE Sequence = @i);
    EXEC    sp_executesql @sql_code;
    SET     @i +=1;
END
```

最后放在第一步中创建的临时表

```
DROP TABLE #tbl;
```


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## 下一步行动
开发的更多提示，请参阅[开发概述][]。

<!--Image references-->

<!--Article references-->
[开发概述]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[虽然]: https://msdn.microsoft.com/library/ms178642.aspx


<!--Other Web references-->



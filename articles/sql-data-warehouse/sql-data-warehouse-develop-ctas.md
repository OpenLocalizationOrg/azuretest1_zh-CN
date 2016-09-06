---
ms.openlocfilehash: 0a9071f049a446e8c804f0905a30ec18b55600cb
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="创建 SQL 数据仓库中选择 (CTAS) 表 |Microsoft Azure"
   description="用创建表编码作为 Azure SQL 数据仓库中数据的选择 (CTAS) 语句用于开发解决方案的提示。"
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

# 在 SQL 数据仓库中选择 (CTAS) 作为创建表
创建表作为选择或 CTAS 是其中一个最重要的 T SQL 功能。 它是完全并行化的操作，创建一个新表，根据 Select 语句的输出。 您可以将其视为鼎沸的选择版本.如果您将喜欢，INTO。

CTAS 还可以用来解决上面列出的受支持功能的数字。 这通常证明不仅将您的代码是符合标准，但它通常更快地执行 SQL 数据仓库会赢/情况。 这是由于其完全并行化设计。

> [AZURE.NOTE] 试着考虑"CTAS 第一"。 如果您认为您可以解决问题，通常是最好的方法-，即使结果是编写更多的数据，然后使用 CTAS。

可以与 CTAS 变通的方案包括︰

- 选择。.到
- 有关更新 ANSI 联接 
- 在删除的 ANSI 联接
- 合并语句

用 CTAS 编码时，该文档还包括某些最佳做法。

## 选择。.到
您可能会发现选择.INTO 将出现在您的解决方案中的位置的数字。 

选择.下面是示例为︰

```
SELECT *
INTO    #tmp_fct
FROM    [dbo].[FactInternetSales]
```

若要将其转换为 CTAS 是非常简单而直接︰

```
CREATE TABLE #tmp_fct
WITH
(
    DISTRIBUTION = ROUND_ROBIN
,   LOCATION = USER_DB
)
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
;
```

使用 CTAS，则意味着您还可以指定数据通讯首选项和可选索引同样适用于表。

## Update 语句的 ANSI 联接替代

您可能会发现有联接两个以上表一起使用 ANSI 加入语法来执行更新或删除的复杂更新。

设想一下，您必须更新此表︰

```
CREATE TABLE [dbo].[AnnualCategorySales]
(   [EnglishProductCategoryName]    NVARCHAR(50)    NOT NULL
,   [CalendarYear]                  SMALLINT        NOT NULL
,   [TotalSalesAmount]              MONEY           NOT NULL
)
WITH
(
    DISTRIBUTION = ROUND_ROBIN
)
;
```

所示出的原始查询如下所示︰

```
UPDATE  acs
SET     [TotalSalesAmount] = [fis].[TotalSalesAmount]
FROM    [dbo].[AnnualCategorySales]     AS acs
JOIN    (
        SELECT  [EnglishProductCategoryName]
        ,       [CalendarYear]
        ,       SUM([SalesAmount])              AS [TotalSalesAmount]
        FROM    [dbo].[FactInternetSales]       AS s
        JOIN    [dbo].[DimDate]                 AS d    ON s.[OrderDateKey]             = d.[DateKey]
        JOIN    [dbo].[DimProduct]              AS p    ON s.[ProductKey]               = p.[ProductKey]
        JOIN    [dbo].[DimProductSubCategory]   AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
        JOIN    [dbo].[DimProductCategory]      AS c    ON u.[ProductCategoryKey]       = c.[ProductCategoryKey]
        WHERE   [CalendarYear] = 2004
        GROUP BY
                [EnglishProductCategoryName]
        ,       [CalendarYear]
        ) AS fis
ON  [acs].[EnglishProductCategoryName]  = [fis].[EnglishProductCategoryName]
AND [acs].[CalendarYear]                = [fis].[CalendarYear]
;
```

因为 SQL 数据仓库不支持 ANSI 联接不能而不能稍有更改通过复制该代码。

您可以使用 CTAS 和隐式联接的组合将这段代码︰

```
-- Create an interim table
CREATE TABLE CTAS_acs
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT  ISNULL(CAST([EnglishProductCategoryName] AS NVARCHAR(50)),0)    AS [EnglishProductCategoryName]
,       ISNULL(CAST([CalendarYear] AS SMALLINT),0)                      AS [CalendarYear]
,       ISNULL(CAST(SUM([SalesAmount]) AS MONEY),0)                     AS [TotalSalesAmount]
FROM    [dbo].[FactInternetSales]       AS s
JOIN    [dbo].[DimDate]                 AS d    ON s.[OrderDateKey]             = d.[DateKey]
JOIN    [dbo].[DimProduct]              AS p    ON s.[ProductKey]               = p.[ProductKey]
JOIN    [dbo].[DimProductSubCategory]   AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
JOIN    [dbo].[DimProductCategory]      AS c    ON u.[ProductCategoryKey]       = c.[ProductCategoryKey]
WHERE   [CalendarYear] = 2004
GROUP BY
        [EnglishProductCategoryName]
,       [CalendarYear]
;

-- Use an implicit join to perform the update 
UPDATE  AnnualCategorySales
SET     AnnualCategorySales.TotalSalesAmount = CTAS_ACS.TotalSalesAmount
FROM    CTAS_acs
WHERE   CTAS_acs.[EnglishProductCategoryName] = AnnualCategorySales.[EnglishProductCategoryName]
AND     CTAS_acs.[CalendarYear]               = AnnualCategorySales.[CalendarYear]
;

--Drop the interim table
DROP TABLE CTAS_acs
;
```

## ANSI 联接代替删除语句
有时删除数据的最佳方法是使用 CTAS。 而不是简单地删除数据中，选择您想要保留的数据。 这尤其适用于使用 ansi 加入语法与 SQL 数据仓库不支持这样的删除语句。

可用下面是转换后的 DELETE 语句的示例︰

```
CREATE TABLE dbo.DimProduct_upsert
WITH 
(   Distribution=HASH(ProductKey)
,   CLUSTERED INDEX (ProductKey)
)
AS -- Select Data you wish to keep
SELECT     p.ProductKey
,          p.EnglishProductName
,          p.Color
FROM       dbo.DimProduct p 
RIGHT JOIN dbo.stg_DimProduct s 
ON         p.ProductKey = s.ProductKey
;

RENAME OBJECT dbo.DimProduct        TO DimProduct_old;
RENAME OBJECT dbo.DimProduct_upsert TO DimProduct;
```

## 替换合并语句
合并语句可以更换，至少在部分中，通过使用 CTAS。 您可以合并`INSERT`和`UPDATE`为一条语句。 需要在第二个语句关闭任何已删除的记录。

一种`UPSERT`可用如下︰

```
CREATE TABLE dbo.[DimProduct_upsert]
WITH 
(   DISTRIBUTION = HASH([ProductKey])
,   CLUSTERED INDEX ([ProductKey])
)
AS 
-- New rows and new versions of rows
SELECT      s.[ProductKey]
,           s.[EnglishProductName]
,           s.[Color]
FROM      dbo.[stg_DimProduct] AS s
UNION ALL  
-- Keep rows that are not being touched
SELECT      p.[ProductKey]
,           p.[EnglishProductName]
,           p.[Color]
FROM      dbo.[DimProduct] AS p
WHERE NOT EXISTS
(   SELECT  *
    FROM    [dbo].[stg_DimProduct] s
    WHERE   s.[ProductKey] = p.[ProductKey]
)
;

RENAME OBJECT dbo.[DimProduct]          TO [DimProduct_old];
RENAME OBJECT dbo.[DimpProduct_upsert]  TO [DimProduct];

```

## CTAS 建议︰ 明确说明数据类型和输出的为空性

迁移代码时您可能会发现在这种类型的编码模式运行︰

```
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE result
(result DECIMAL(7,2) NOT NULL
)
WITH (DISTRIBUTION = ROUND_ROBIN)

INSERT INTO result
SELECT @d*@f 
;
```

会自然而然地，您可能会认为您应该将此代码迁移到 CTAS，您可能是正确。 但是，还有一个隐藏的问题。 

下面的代码不会产生相同的结果︰

```
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455
;

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT @d*@f as result
;
```

请注意列"result"向前执行数据类型和可以为空值的表达式。 如果您不小心，这可能导致在值中的细微差异。

请尝试以下内容作为示例︰

```
SELECT result,result*@d
from result
;

SELECT result,result*@d
from ctas_r
;
```

存储结果的值是不同的。 随着其他表达式中使用结果列中的保留的值错误变得更为重要。

![][1]

这是特别重要的数据迁移。 即使第二个查询可以说更准确地没有问题。 数据是不同与源系统相比，这产生了在迁移过程中的完整性的问题。 这是一个在"错误"答案是真正正确的那些极少数的情况下 ！

我们可以看到，两个结果之间的这种不一致的原因是到隐式类型转换。 在第一个示例表定义的列定义。 在插入行时隐式类型转换。 在第二个示例中没有进行隐式类型转换为表达式定义了列的数据类型。 此外请注意，第二个示例中的列已定义为可以为 Null 的列而第一个示例还没有。 在第一个示例列的为空性中创建表时的明确定义。 在第二个示例省略了该只为表达式和默认情况下这将导致一个 NULL 定义。  

要解决这些问题必须选择部分的 CTAS 语句显式设置类型转换和为空性。 不能创建表部分中设置这些属性。

下面的示例演示如何修复代码︰

```
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT ISNULL(CAST(@d*@f AS DECIMAL(7,2)),0) as result
```

请注意以下事项︰
- 可能已使用 CAST 或 CONVERT
- ISNULL 用于强制不联合的为空性
- ISNULL 是最外层的函数
- ISNULL 的第二部分是一个常数，即 0

> [AZURE.NOTE] 要正确设置它为空性的至关重要使用 ISNULL 和不合并。 联合不是确定性的函数，那么表达式的结果将始终为可为空值。 ISNULL 是不同的。 它是确定的。 因此当 ISNULL 函数的第二部分是一个常量或文本，然后得到的值将为非 NULL。 

此提示不只是有助于确保您的计算的完整性。 也是非常重要的表分区切换。 假设您拥有此表定义为您的事实︰

```
CREATE TABLE [dbo].[Sales]
(
    [date]      INT     NOT NULL
,   [product]   INT     NOT NULL
,   [store]     INT     NOT NULL
,   [quantity]  INT     NOT NULL
,   [price]     MONEY   NOT NULL
,   [amount]    MONEY   NOT NULL
)
WITH 
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101,20020101
                    ,20030101,20040101,20050101
                    )
                )
) 
;
```

但是，值字段是计算的表达式，它不是源数据的一部分。

若要创建已分区数据集可能需要执行此操作︰

```
CREATE TABLE [dbo].[Sales_in]
WITH    
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]    
,   [product] 
,   [store] 
,   [quantity]
,   [price]   
,   [quantity]*[price]  AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create')
;
```

该查询将运行完全正常。 您尝试执行分区切换时，该问题。 表定义不匹配。 若要使与 CTAS 相匹配的表定义需要修改。

```
CREATE TABLE [dbo].[Sales_in]
WITH    
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]    
,   [product] 
,   [store] 
,   [quantity]
,   [price]   
,   ISNULL(CAST([quantity]*[price] AS MONEY),0) AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create');
```

您可以因此看到类型一致性和维护 CTAS 属性为空性是较好的工程最佳做法。 它有助于维护您的计算中的完整性，也确保了，可能会分区切换。

请参阅 MSDN 上使用[CTAS][]的详细信息。 它是 Azure SQL 数据仓库中的最重要的语句之一。 请确保您充分理解。

## 下一步行动
开发的更多提示，请参阅[开发概述][]。

<!--Image references-->
[1]: media/sql-data-warehouse-develop-ctas/ctas-results.png

<!--Article references-->
[开发概述]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[CTAS]: https://msdnstage.redmond.corp.microsoft.com/en-us/library/mt204041.aspx

<!--Other Web references-->

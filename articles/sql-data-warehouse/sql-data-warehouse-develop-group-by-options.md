---
ms.openlocfilehash: 3094f51bebb8caae79520a4267693dcbab671640
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="通过 SQL 数据仓库中的选项组 |Microsoft Azure"
   description="开发解决方案实施 Azure SQL 数据仓库中的选项组的提示。"
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

# 通过 SQL 数据仓库中的选项组

聚合数据到一个摘要行集使用[GROUP BY]子句。 它还具有一些扩展它的功能需要变通为 Azure SQL 数据仓库直接不支持的选项。

这些选项
- 分组依据具有汇总
- 对集进行分组
- 分组依据与多维数据集

## 汇总和分组设置选项
最简单的选项是使用`UNION ALL`改为执行汇总而不是依赖的显式语法。 结果是完全相同的

下面是一个示例使用语句组的`ROLLUP`选项︰

```
SELECT [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
,      SUM(SalesAmount)             AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t       ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY ROLLUP (
                        [SalesTerritoryCountry]
                ,       [SalesTerritoryRegion]
                )
;
```

通过汇总我们所请求的下列聚合︰
- 国家和地区
- 国家（地区）
- 总计

要替换此您将需要使用`UNION ALL`;指定聚合所需显式返回相同的结果︰

```
SELECT [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY 
       [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
UNION ALL
SELECT [SalesTerritoryCountry]
,      NULL
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY 
       [SalesTerritoryCountry]
UNION ALL
SELECT NULL
,      NULL
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey;
```

分组集所有我们需要做的是采用相同的主体，但只创建联合所有部分的我们想要看到的聚合级别

## 多维数据集选项
也可以创建一个组通过使用多维数据集使用 UNION ALL 方法。 问题是代码将很快变成繁重且不实用。 缓解这您可以使用此更高级的方法。

让我们使用上面的示例。

第一步是聚合的定义多维数据集用于定义所有我们想要创建级别。 务必要记下这两个派生表 CROSS JOIN。 这将为我们生成所有级别。 代码的其余部分的格式是真的有。

```
CREATE TABLE #Cube
WITH 
(   DISTRIBUTION = ROUND_ROBIN
,   LOCATION = USER_DB
)
AS
WITH GrpCube AS
(SELECT    CAST(ISNULL(Country,'NULL')+','+ISNULL(Region,'NULL') AS NVARCHAR(50)) as 'Cols'
,          CAST(ISNULL(Country+',','')+ISNULL(Region,'') AS NVARCHAR(50))  as 'GroupBy'
,          ROW_NUMBER() OVER (ORDER BY Country) as 'Seq'
FROM       ( SELECT 'SalesTerritoryCountry' as Country
             UNION ALL
             SELECT NULL
           ) c
CROSS JOIN ( SELECT 'SalesTerritoryRegion' as Region
             UNION ALL
             SELECT NULL
           ) r
)
SELECT Cols
,      CASE WHEN SUBSTRING(GroupBy,LEN(GroupBy),1) = ',' 
            THEN SUBSTRING(GroupBy,1,LEN(GroupBy)-1) 
            ELSE GroupBy 
       END AS GroupBy  --Remove Trailing Comma
,Seq
FROM GrpCube;
```

如下所示的 CTAS 的结果︰

![][1]

第二步是指定目标表来存储中间结果︰

```
DECLARE 
 @SQL NVARCHAR(4000)
,@Columns NVARCHAR(4000)
,@GroupBy NVARCHAR(4000)
,@i INT = 1
,@nbr INT = 0
;
CREATE TABLE #Results
(
 [SalesTerritoryCountry] NVARCHAR(50)
,[SalesTerritoryRegion]  NVARCHAR(50)
,[TotalSalesAmount]      MONEY
)
WITH
(   DISTRIBUTION = ROUND_ROBIN
,   LOCATION = USER_DB
)
;
```

第三步是对我们的列执行聚合的多维数据集进行循环。 查询将运行一次 #Cube 临时表中的每一行并将结果存储在 #Results 临时表

```
SET @nbr =(SELECT MAX(Seq) FROM #Cube);

WHILE @i<=@nbr
BEGIN
    SET @Columns = (SELECT Cols    FROM #Cube where seq = @i);
    SET @GroupBy = (SELECT GroupBy FROM #Cube where seq = @i);

    SET @SQL ='INSERT INTO #Results
              SELECT '+@Columns+'
              ,      SUM(SalesAmount) AS TotalSalesAmount
              FROM  dbo.factInternetSales s
              JOIN  dbo.DimSalesTerritory t  
              ON s.SalesTerritoryKey = t.SalesTerritoryKey
              '+CASE WHEN @GroupBy <>'' 
                     THEN 'GROUP BY '+@GroupBy ELSE '' END

    EXEC sp_executesql @SQL;
    SET @i +=1;
END
```

最后我们可以简单地通过 #Results 临时表读取返回的结果

```
SELECT * 
FROM #Results
ORDER BY 1,2,3
;
```

通过代码分解成部分，并生成循环构造代码变得更易于管理和维护。 


## 下一步行动
开发的更多提示，请参阅[开发概述][]。

<!--Image references-->
[1]: media/sql-data-warehouse-develop-group-by-options/sql-data-warehouse-develop-group-by-cube.png

<!--Article references-->
[开发概述]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[分组依据]: https://msdn.microsoft.com/en-us/library/ms177673.aspx


<!--Other Web references-->


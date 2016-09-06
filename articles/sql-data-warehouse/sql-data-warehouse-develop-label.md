---
ms.openlocfilehash: d1ecaa100b23aceed20221586af6d4401edbb5ff
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="在 SQL 数据仓库中使用标签对仪器查询 |Microsoft Azure"
   description="开发解决方案在 Azure SQL 数据仓库中使用标签到仪器的查询提示。"
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

# 对仪器查询 SQL 数据仓库中使用标签
SQL 数据仓库支持称作查询标签的概念。 让我们转到任意深度前看的一个示例︰

    ```
    SELECT *
    FROM sys.tables
    OPTION (LABEL = 'My Query Label')
    ;
    ```

这最后一个行标记查询到的字符串我的查询标签。 这是特别有用，因为标签是查询可以通过 Dmv。 这为我们提供一种机制，以追踪查询问题以及帮助您识别通过 ETL 运行进度了。 

实际上这里有助于良好的命名约定。 例如，类似于项目︰ 过程︰ 语句︰ 注释可帮助我们唯一地标识源控件中的所有代码中的查询。

按标签进行搜索可以使用下面的查询使用动态管理视图︰

    ```
    SELECT  *
    FROM    sys.dm_pdw_exec_requests r
    WHERE   r.[label] = 'My Query Label'
    ;
    ``` 

> [AZURE.NOTE] 在查询时包装方括号或双引号引起来的单词标签周围至关重要。 标签是保留的字，并将导致出现错误，如果没有分隔。


## 下一步行动
开发的更多提示，请参阅[开发概述][]。

<!--Image references-->

<!--Article references-->
[开发概述]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->




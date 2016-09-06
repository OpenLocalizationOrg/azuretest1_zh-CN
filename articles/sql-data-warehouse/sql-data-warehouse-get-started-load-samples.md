---
ms.openlocfilehash: 90e6bc4f7e6c3a95c22539e84d865aba216cd5db
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="将示例数据加载到 SQL 数据仓库 |Microsoft Azure"
   description="将大量数据加载到 SQL 数据仓库"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/05/2015"
   ms.author="lodipalm;barbkess"/>

#将示例数据加载到 SQL 数据仓库

现在，您已经设置 SQL 数据仓库实例可以很容易地将一些示例数据加载到其中。  以下说明将可帮助您创建您的数据库中名为 AdventureWorksPDW2012 的数据集。  一个虚构的公司结构样例数据仓库出此数据集模型称为 AdventureWorks。  请注意需要有 BCP 为以下步骤安装。  如果您当前没有安装的 BCP，请安装[Microsoft SQL Server 命令行实用程序][]。 

1. 若要开始单击可以下载[示例数据脚本][]。

2. 文件下载后，提取 AdventureWorksPDW2012.zip 文件的内容并打开新的 AdventureWorksPDW2012 文件夹中。 

3. 编辑 aw_create.bat 文件并在文件的顶部设置以下值︰

   一。 **服务器**︰ SQL 数据仓库所在的服务器的完全限定的名

   b。 **用户**︰ 用户输入上面的服务器
   
   c。 **密码**︰ 密码提供的服务器日志中
   
   d。 **数据库**︰ 想要加载到 SQL 数据仓库实例的名称
   
   确保不程序之间的任何空白 '=' 和这些参数。
   

4. 从它所在的目录运行 aw_create.bat。 这会创建架构并将数据加载到使用 BCP 的所有表。


## 连接到并查询您的示例

[连接和查询][]文档中所述您可以连接到使用 Visual Studio 和 SSDT 此数据库。  既然已经载入 SQL 数据仓库一些示例数据，您可以快速运行几个查询开始。 

我们可以运行简单的 select 语句，以获得员工的所有信息︰

    SELECT * FROM DimEmployee;

我们还可以运行使用构造，如按组每天看所有销售人员的总金额更复杂的查询︰

    SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
    FROM FactInternetSales
    GROUP BY OrderDateKey
    ORDER BY OrderDateKey;

我们甚至可以使用 WHERE 子句在某个日期之前过滤掉来自订单︰

    SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
    FROM FactInternetSales
    WHERE OrderDateKey > '20020801'
    GROUP BY OrderDateKey
    ORDER BY OrderDateKey;

事实上，SQL 数据仓库支持几乎所有 SQL Server 执行，T SQL 构造，您可以找到一些我们[将代码迁移][]的文档之间的区别。  

## 下一步行动
现在，我们给您一些时间来看如何[开发][]、[加载][]或[迁移][]示例数据检查与预热。

<!--Image references-->

<!--Article references-->
[迁移]:https://azure.microsoft.com/en-us/documentation/articles/sql-data-warehouse-overview-migrate/
[开发]:https://azure.microsoft.com/en-us/documentation/articles/sql-data-warehouse-overview-develop/
[加载]:https://azure.microsoft.com/en-us/documentation/articles/sql-data-warehouse-overview-load/
[连接和查询]:https://azure.microsoft.com/en-us/documentation/articles/sql-data-warehouse-get-started-connect-query/
[迁移代码]:https://azure.microsoft.com/en-us/documentation/articles/sql-data-warehouse-migrate-code/

<!--MSDN references-->
[对于 SQL Server 的 Microsoft 命令行实用程序]:http://www.microsoft.com/en-us/download/details.aspx?id=36433

<!--Other Web references-->
[脚本示例数据]:https://migrhoststorage.blob.core.windows.net/sqldwsample/AdventureWorksPDW2012.zip 

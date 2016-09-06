---
ms.openlocfilehash: 05d2e27e294c14e0e7b2b4462bed98cf14e2a230
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="使用 bcp 将数据加载到 SQL 数据仓库 |Microsoft Azure"
   description="了解哪些 bcp 以及如何使用它的数据仓储方案。"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="TwoUnder"
   manager="barbkess"
   editor="JRJ@BigBangData.co.uk"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/23/2015"
   ms.author="mausher;barbkess"/>


# 使用 bcp 的加载数据
**[bcp][]**是一个命令行大容量负载实用工具，允许您将 SQL Server、 数据文件和 SQL 数据仓库之间的数据复制。 使用 bcp 导入 SQL 数据仓库表的行数很多，或将数据从 SQL Server 表导出到数据文件。 除与 queryout 选项一起使用时，bcp 需要事务处理 SQL 没有知识。 

bcp 是快速方便地进出 SQL 数据仓库数据库移动较小的数据集。 建议通过 bcp 负载/提取的数据的确切金额将取决于连接到 Azure 数据中心的网络上。 一般情况下，可以加载维度表并将其提取但相当大的事实数据表可能需要很长的时间来加载或提取。 

使用 bcp 可以︰
- 一个简单的命令行实用程序用于将数据加载到 SQL 数据仓库。
- 使用一个简单的命令行实用程序从 SQL 数据仓库中提取数据。

本教程将介绍如何为︰ 
- 将数据导入命令中使用 bcp 的表
- 从表 uisng 命令 bcp 中导出数据

## 先决条件
要单步执行本教程，您需要︰
- SQL 数据仓库数据库
- Bcp 命令行实用程序安装
- SQLCMD 命令出现实用程序安装

>[AZURE.NOTE] 您可以从[Microsoft 下载中心][]下载的 bcp 和 sqlcmd 实用程序。

##将数据导入到 SQL 数据仓库
在本教程中，将 Azure SQL 数据仓库中创建一个表，并将数据导入表。

### 步骤 1︰ 在 Azure SQL 数据仓库中创建表
从命令提示符处，将连接到您的实例使用下面的命令替换为适当的值︰

```
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I
```
一旦连接，复制 sqlcmd 提示符处下面的表脚本，然后按 Enter 键︰

```
CREATE TABLE DimDate2 (DateId INT NOT NULL, CalendarQuarter TINYINT NOT NULL, FiscalQuarter TINYINT NOT NULL);
```

在下一行中，输入转到批处理终止符，然后按 Enter 键可执行语句︰

```
GO
```

### 步骤 2︰ 创建源数据文件

打开记事本，然后将下面的数据行复制到新文件中。

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

把它保存到您的本地临时目录，C:\Temp\DimDate2.txt。

> [AZURE.NOTE] 请务必记住，bcp.exe 不支持 utf-8 文件编码。 请使用 ASCII 编码文件或 utf-16 编码的文件时使用 bcp.exe。

### 第 3 步︰ 连接并导入数据
使用 bcp 可以连接并使用下面的命令替换为相应的值将数据导入︰

```
bcp DimDate2 in C:\Temp\DimDate2.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t  ','
```

您可以验证使用 sqlcmd 作为之前连接，执行以下的 TSQL 命令已加载数据︰

```
SELECT * FROM DimDate2 ORDER BY 1;
GO
```

这应返回以下结果︰

DateId |CalendarQuarter |FiscalQuarter
----------- |--------------- |-------------
20150101 |1 |3
20150201 |1 |3
20150301 |1 |3
20150401 |2 |4
20150501 |2 |4
20150601 |2 |4
20150701 |3 |1
20150801 |3 |1
20150801 |3 |1
20151001 |4 |2
20151101 |4 |2
20151201 |4 |2

## 从 SQL 数据仓库中导出数据
在本教程中，您将从 SQL 数据仓库中的表创建一个数据文件。 我们会将我们上面创建的数据导出到一个名为 DimDate2_export.txt 的新数据文件。 

### 步骤 1︰ 将数据导出

使用 bcp 实用工具，您可以连接并使用下面的命令替换为相应的值导出数据︰

```
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
您可以验证数据已正确导出通过打开新文件。 文件中的数据应与下面的文本相匹配︰

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

>[AZURE.NOTE] 分布式系统的性质，数据顺序可能不一定是相同的 SQL 数据仓库数据库。 （可选） 可以使用 queryout 参数来指定要运行哪个事务处理 SQL 查询。

## 下一步行动
加载的概述，请参见[加载到 SQL 数据仓库的数据][]。
开发的更多提示，请参阅[SQL 数据仓库开发概述][]。

<!--Image references-->

<!--Article references-->

[将数据加载到 SQL 数据仓库]: ./sql-data-warehouse-overview-load/
[SQL 数据仓库开发概述]:  ./sql-data-warehouse-overview-develop/

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx 


<!--Other Web references-->
[Microsoft 下载中心]: http://www.microsoft.com/download/details.aspx?id=36433


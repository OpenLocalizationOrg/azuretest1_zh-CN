---
ms.openlocfilehash: 49e549e57457269b2b926fd79bb639cef4af99e3
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="在 SQL 数据仓库教程 PolyBase |Microsoft Azure"
   description="了解 PolyBase 是什么以及如何使用它的数据仓储方案。"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="jhubbard"
   editor="jrowlandjones"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="05/09/2015"
   ms.author="sahajs;barbkess"/>


# 与 PolyBase 加载数据
PolyBase 技术允许您对数据进行查询和联接来自多个源，所有通过使用事务处理 SQL 命令。 

使用 PolyBase，可以查询 Azure blob 存储中存储的数据，并将其加载到 SQL 数据仓库数据库具有以下步骤︰

- 创建数据库主密钥和凭据。
- 创建 PolyBase 对象︰ 外部数据源、 外部文件格式和外部表。 
- 查询在 Azure blob 存储中存储的数据。
- Azure blob 存储的数据加载到 SQL 数据仓库中。


## 先决条件
要单步执行本教程，您需要︰

- Azure 存储帐户
- 数据存储在 Azure blob 存储为带分隔符的文本文件

首先，您将创建 PolyBase 要求连接和查询 Azure blob 存储中的数据的对象。

## 创建数据库主密钥
连接到要创建数据库主密钥服务器上的用户数据库。 该密钥用于加密凭据机密的下一步。 

```
-- Creating master key
CREATE MASTER KEY;
```

参考主题︰[创建主密钥 (事务处理 SQL)][]。

## 创建范围的数据库凭据
若要访问 Azure blob 存储，您需要创建一个范围的数据库凭据来存储您的 Azure 存储帐户的身份验证信息。 连接到数据仓库数据库，并创建数据库的应用范围限定为每个您要访问的 Azure 存储帐户凭据。 指定的标识名称和您的 Azure 存储帐户密钥作为秘密。 标识名称不会影响到 Azure 存储的身份验证。

若要查看是否已存在数据库范围的凭据，使用 sys.database_credentials，不是 sys.credentials 只显示服务器凭据。

```
-- Check for existing database-scoped credentials.
SELECT * FROM sys.database_credentials;

-- Create a database scoped credential
CREATE DATABASE SCOPED CREDENTIAL ASBSecret 
WITH IDENTITY = 'joe'
,    Secret = '<azure_storage_account_key>'
;
```

参考主题︰[创建凭据 (事务处理 SQL)][]。

若要删除一个数据库范围内凭据，您只需使用下面的语法︰

```
-- Dropping credential
DROP DATABASE SCOPED CREDENTIAL ASBSecret
;
```

参考主题︰[删除凭据 (事务处理 SQL)][]。

## 创建外部数据源
外部数据源为数据库对象，它存储的 Azure blob 存储数据和访问信息的位置。 您需要定义每个 Azure 存储容器，您要访问的外部数据源。

```
-- Creating external data source (Azure Blob Storage) 
CREATE EXTERNAL DATA SOURCE azure_storage 
WITH
(
    TYPE = HADOOP
,   LOCATION ='wasbs://mycontainer@ test.blob.core.windows.net/path'
,   CREDENTIAL = ASBSecret
)
;
```

参考主题︰[创建外部数据源 (事务处理 SQL)][]。

若要删除外部数据源的语法如下所述︰

```
-- Dropping external data source
DROP EXTERNAL DATA SOURCE azure_storage
;
```

参考主题︰[删除外部数据源 (事务处理 SQL)][]。

## 创建外部文件格式
外部文件格式是一个数据库对象，它指定对外部数据的格式。 在此示例中，我们有未在文本文件中的数据压缩和用管道字符分隔字段 (|)。 

```
-- Creating external file format (delimited text file)
CREATE EXTERNAL FILE FORMAT text_file_format 
WITH 
(   
    FORMAT_TYPE = DELIMITEDTEXT 
,   FORMAT_OPTIONS  (
                        FIELD_TERMINATOR ='|'
                    ,   USE_TYPE_DEFAULT = TRUE
                    )
)
;
```

PolyBase 可以使用带分隔符的文本，配置单元 rc 文件中的压缩和未压缩的数据，并配置单元 ORC 格式。 

参考主题︰[创建外部文件格式 (事务处理 SQL)][]。

若要删除外部文件格式的语法如下︰

```
-- Dropping external file format...
DROP EXTERNAL FILE FORMAT text_file_format
;
```
参考主题︰[除去外部文件格式 (事务处理 SQL)][]。

## 创建外部表

外部表的定义等同于一个关系表定义。 关键区别是数据的格式和位置。 外部表定义存储在 SQL 数据仓库数据库中。 数据存储在数据源所指定的位置。

位置选项指定的路径对数据从数据源的根。 在此示例中，数据位于 wasbs://mycontainer@ test.blob.core.windows.net/path/Demo/。 同一个表的所有文件都必须在 Azure BLOB 的同一逻辑文件夹下。

```
-- Creating external table pointing to file stored in Azure Storage
CREATE EXTERNAL TABLE [ext].[CarSensor_Data] 
(
     [SensorKey]     int    NOT NULL 
,    [CustomerKey]   int    NOT NULL 
,    [GeographyKey]  int        NULL 
,    [Speed]         float  NOT NULL 
,    [YearMeasured]  int    NOT NULL
)
WITH 
(
    LOCATION    = '/Demo/'
,   DATA_SOURCE = azure_storage
,   FILE_FORMAT = text_file_format      
)
;
```

> [AZURE.NOTE] 请注意，您不能创建统计信息在外表上这一次。

参考主题︰[创建外部表 (事务处理 SQL)][]。

您刚才创建的对象存储在 SQL 数据仓库数据库中。 您可以在 SQL Server 数据工具 (SSDT) 对象资源管理器中查看它们。 

若要删除外部表，您需要使用下面的语法︰

```
--Dropping external table
DROP EXTERNAL TABLE [ext].[CarSensor_Data]
;
```

> [AZURE.NOTE] 删除外部表时必须使用`DROP EXTERNAL TABLE`您**不能**使用`DROP TABLE`。 

参考主题︰[删除外部表 (事务处理 SQL)][]。

还有值得注意的外部表都显示在两个`sys.tables`，更具体地说就是在`sys.external_tables`目录视图。

## 旋转存储键

时常想要更改到您出于安全原因的 blob 存储访问键。 

执行此任务的最优雅的方法是按照该过程称为"轮换密钥"。 您可能已经注意到，有两个存储密钥 blob 存储帐户。 这样，您就可以转换 

旋转您的 Azure 存储帐户密钥是一个简单的三个步骤的过程

1. 创建第二个范围的数据库凭据基于辅助存储的访问键
2. 创建基于此新凭据的第二个外部数据源
3. 删除并创建指向新的外部数据源的外部表

时已迁移到新的外部数据源的所有外部都表，然后您可以执行清理任务︰
 
1. 放置第一个外部数据源
2. 放置第一个数据库范围基于主存储访问键的凭据
3. 登录到 Azure 并再生准备下一次主要的访问键

## 查询 Azure blob 存储数据
对外部表的查询，就好像它是一个关系表，只需使用表名。 

这是加入保险客户数据存储在 SQL 数据仓库中，因为汽车传感器数据存储在 Azure 存储 blob 的特别查询。 结果显示驱动程序该驱动器比其他人快。

```
-- Join SQL Data Warehouse relational data with Azure storage data. 
SELECT 
      [Insured_Customers].[FirstName]
,     [Insured_Customers].[LastName]
,     [Insured_Customers].[YearlyIncome]
,     [CarSensor_Data].[Speed]
FROM  [dbo].[Insured_Customers] 
JOIN  [ext].[CarSensor_Data]         ON [Insured_Customers].[CustomerKey] = [CarSensor_Data].[CustomerKey]
WHERE [CarSensor_Data].[Speed] > 60 
ORDER BY [CarSensor_Data].[Speed] DESC
;
```

## 从 Azure blob 存储加载数据
本示例将数据从 Azure blob 存储加载到 SQL 数据仓库数据库。

直接将数据存储中删除查询的数据传输时间。 存储与 columnstore 索引的数据提高了查询性能的分析查询的 10 倍。

此示例创建表作为 SELECT 语句加载数据。 新表继承命名查询中的列。 从外部表的定义，它继承了这些列的数据类型。 

创建表方式选择是高性能事务处理 SQL 语句替换插入......选择。  它最初为分析平台系统中的大规模并行处理 (MPP) 引擎开发，现在在 SQL 数据仓库中。

```
-- Load data from Azure blob storage to SQL Data Warehouse 

CREATE TABLE [dbo].[Customer_Speed]
WITH 
(   
    CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([CarSensor_Data].[CustomerKey])
)
AS 
SELECT * 
FROM   [ext].[CarSensor_Data]
;
```

请参阅[创建表作为选择 (事务处理 SQL)][]。


## 围绕 PolyBase utf-8 要求工作
在存在 PolyBase 支持加载了 utf-8 编码的数据文件。 Utf-8 使用相同字符编码为 ASCII PolyBase 还将提供 ASCII 编码的支持加载数据。 但是，PolyBase 不支持其他如 utf-16 字符编码 / Unicode 或扩展 ASCII 字符。 扩展 ascii 码包含字符替换为重点，如音符在德语中很常见的。

要变通解决此要求最好的回答是来重新编写为 utf-8 编码。

有几种方法来执行此操作。 下面是使用 Powershell 的两种方法︰ 

### 小文件的简单示例

下面是简单的一行 Powershell 脚本可创建该文件。
 
```
Get-Content <input_file_name> -Encoding Unicode | Set-Content <output_file_name> -Encoding utf8
```

但是，尽管这是一种重新编码数据的简单方法它是不是最有效。 下面的 io 流示例得更快了很多，并能达到相同的结果。

### 对于较大的文件 IO 流示例

下面的代码示例更复杂，但如流的行的数据源为目标是效率高得多。 对于大型文件使用此方法。

```
#Static variables
$ascii = [System.Text.Encoding]::ASCII
$utf16le = [System.Text.Encoding]::Unicode
$utf8 = [System.Text.Encoding]::UTF8
$ansi = [System.Text.Encoding]::Default
$append = $False

#Set source file path and file name
$src = [System.IO.Path]::Combine("C:\input_file_path\","input_file_name.txt")

#Set source file encoding (using list above)
$src_enc = $ansi

#Set target file path and file name
$tgt = [System.IO.Path]::Combine("C:\output_file_path\","output_file_name.txt")

#Set target file encoding (using list above)
$tgt_enc = $utf8

$read = New-Object System.IO.StreamReader($src,$src_enc)
$write = New-Object System.IO.StreamWriter($tgt,$append,$tgt_enc)

while ($read.Peek() -ne -1)
{
    $line = $read.ReadLine();
    $write.WriteLine($line);
}
$read.Close()
$read.Dispose()
$write.Close()
$write.Dispose()
```

## 下一步行动
开发的更多提示，请参阅[开发概述][]。

<!--Image references-->

<!--Article references-->
[使用 bcp 的加载数据]: sql-data-warehouse-load-with-bcp.md
[使用 PolyBase 加载]: sql-data-warehouse-load-with-polybase.md
[解决方案合作伙伴]: sql-data-warehouse-solution-partners.md
[开发概述]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[受支持的源接收器]: https://msdn.microsoft.com/library/dn894007.aspx
[复制活动]: https://msdn.microsoft.com/library/dn835035.aspx
[SQL Server 目标适配器]: https://msdn.microsoft.com/library/ms141095.aspx
[SSIS]: https://msdn.microsoft.com/library/ms141026.aspx


<!-- External Links -->
[创建外部数据源 (事务处理 SQL)]:https://msdn.microsoft.com/library/dn935022(v=sql.130).aspx
[创建外部文件格式 (事务处理 SQL)]:https://msdn.microsoft.com/library/dn935026(v=sql.130).aspx
[创建外部表 (事务处理 SQL)]:https://msdn.microsoft.com/library/dn935021(v=sql.130).aspx

[删除外部数据源 (事务处理 SQL)]:https://msdn.microsoft.com/en-us/library/mt146367.aspx
[删除外部文件格式 (事务处理 SQL)]:https://msdn.microsoft.com/en-us/library/mt146379.aspx
[删除外部表 (事务处理 SQL)]:https://msdn.microsoft.com/en-us/library/mt130698.aspx

[创建表作为选择 (事务处理 SQL)]:https://msdn.microsoft.com/library/mt204041.aspx
[创建主密钥 (事务处理 SQL)]:https://msdn.microsoft.com/en-us/library/ms174382.aspx
[创建凭据 (事务处理 SQL)]:https://msdn.microsoft.com/en-us/library/ms189522.aspx
[删除凭据 (事务处理 SQL)]:https://msdn.microsoft.com/en-us/library/ms189450.aspx



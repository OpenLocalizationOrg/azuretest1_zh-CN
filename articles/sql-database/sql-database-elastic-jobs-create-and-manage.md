---
ms.openlocfilehash: 09729735717519fca8a70e2bcb4d5b175a9d53eb
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="创建和管理弹性数据库作业"
    description="遍历一个弹性数据库作业的创建和管理。"
    services="sql-database"
    documentationCenter=""
    manager="jhubbard"
    authors="sidneyh"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/21/2015"
    ms.author="ddove; sidneyh"/>

# 创建和管理门户 （预览） 的 SQL 数据库弹性作业

> [AZURE.SELECTOR]
- [Azure 门户](sql-database-elastic-jobs-create-and-manage.md)
- [PowerShell](sql-database-elastic-jobs-powershell.md)


**弹性数据库作业**轻松地启用和可靠的数据库，通过简化管理架构更改，如操作的执行组的管理凭据管理，引用数据更新、 性能数据收集或租户 （客户） 遥测集合。 弹性数据库作业目前通过 Azure 门户和 PowerShell cmdlet。 但是，Azure 门户曲面减少跨[弹性数据库池 （预览）](sql-database-elastic-pool.md)中的所有数据库执行有限的功能。 若要访问其他功能和脚本的执行跨一组包括一个自定义的集合或 shard 集 （用[弹性数据库客户端库](sql-database-elastic-scale-introduction.md)创建） 数据库，请参阅[创建和管理作业使用 PowerShell](sql-database-elastic-jobs-powershell.md)。 有关作业的详细信息，请参阅[弹性数据库作业概述](sql-database-elastic-jobs-overview.md)。 

## 先决条件

* Azure 的订阅。 免费试用版，请参阅[免费一个月的试用版](http://azure.microsoft.com/pricing/free-trial/)。
* 弹性的数据库池。 请参阅[有关弹性数据库池](sql-database-elastic-pool.md)
* 弹性数据库作业服务组件的安装。 请参阅[安装弹性数据库作业服务](sql-database-elastic-jobs-service-installation.md)。

## 创建作业

1. 使用[Azure 的门户](https://portal.azure.com)，从现有的弹性数据库作业池，单击**创建作业**。
2. 键入用户名和密码 （在安装作业的创建） 数据库管理员的作业控制数据库 （元数据存储的作业）。

    ![命名作业、 键入或粘贴的代码，并单击运行][1]
2. 在**创建 Job**刀片式服务器，输入作业的名称。
3. 键入用户名和密码以连接到目标数据库具有足够权限的脚本的执行是成功的。
4. 粘贴或键入 T SQL 脚本。
5. 单击**保存**，然后单击**运行**。

    ![创建作业并运行][5]

## 运行幂等作业

当您对一组数据库运行脚本时，则必须确保该脚本是幂等。 即，该脚本必须能够运行多次，即使它出现故障之前处于不完整状态。 例如，脚本失败时，该作业将自动重试直到成功 （在限制之内，作为重试逻辑将最终停止重试）。 若要执行此操作的方法是使用"如果存在"子句并创建一个新对象之前删除任何发现的实例。 此处显示了一个示例︰

    IF EXISTS (SELECT name FROM sys.indexes
            WHERE name = N'IX_ProductVendor_VendorID')
    DROP INDEX IX_ProductVendor_VendorID ON Purchasing.ProductVendor;
    GO
    CREATE INDEX IX_ProductVendor_VendorID
    ON Purchasing.ProductVendor (VendorID);

或者，在创建新实例之前使用"如果不存在"子句︰

    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
     CREATE TABLE TestTable(
      TestTableId INT PRIMARY KEY IDENTITY,
      InsertionTime DATETIME2
     );
    END
    GO

    INSERT INTO TestTable(InsertionTime) VALUES (sysutcdatetime());
    GO

然后，此脚本更新以前创建的表。

    IF NOT EXISTS (SELECT columns.name FROM sys.columns INNER JOIN sys.tables on columns.object_id = tables.object_id WHERE tables.name = 'TestTable' AND columns.name = 'AdditionalInformation')
    BEGIN

    ALTER TABLE TestTable

    ADD AdditionalInformation NVARCHAR(400);
    END
    GO

    INSERT INTO TestTable(InsertionTime, AdditionalInformation) VALUES (sysutcdatetime(), 'test');
    GO


## 检查作业状态

开始作业后，您可以检查其进度。

1. 从弹性数据库池页中，单击**管理作业**。

    ![单击"管理作业"][2]

2. 单击作业的名称 (a)。 **状态**可以是"已完成"或"故障"。 该作业的详细信息显示 (b) 其日期和时间创建和运行。 下面的列表 (c) 将显示进度的每个数据库的脚本在池中，赋予其日期和时间的详细信息。

    ![检查已完成的作业][3]


## 检查失败的作业

如果作业失败，可以发现其执行的日志。 单击要查看其详细信息的失败作业的名称。

![检查失败的作业][4]


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-create-and-manage/screen-1.png
[2]: ./media/sql-database-elastic-jobs-create-and-manage/click-manage-jobs.png
[3]: ./media/sql-database-elastic-jobs-create-and-manage/running-jobs.png
[4]: ./media/sql-database-elastic-jobs-create-and-manage/failed.png
[5]: ./media/sql-database-elastic-jobs-create-and-manage/screen-2.png

 
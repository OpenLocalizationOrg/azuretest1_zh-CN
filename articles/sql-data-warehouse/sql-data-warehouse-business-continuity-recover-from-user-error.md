---
ms.openlocfilehash: 8247546c7730c460aaad6bdfab869db69abcf268
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="恢复数据库，请从 SQL 数据仓库中数据的用户错误 |Microsoft Azure"
   description="恢复数据库从 SQL 数据仓库中数据的用户错误的步骤。 "
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sahaj08"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/26/2015"
   ms.author="sahajs"/>

# 恢复数据库，请从 SQL 数据仓库中数据的用户错误

SQL 数据仓库提供从导致意外的数据损坏或删除的用户错误恢复的两个核心功能︰

- 实时数据库还原
- 还原已删除的数据库

这两种能力还原到新数据库中的相同服务器上。

## 实时数据库恢复
如果出现导致非有意的数据修改用户错误，可以在保持期内任何还原点还原数据库。 实时数据库的数据库快照会出现每隔 8 小时，并保留 7 天。 

### PowerShell

使用 PowerShell 以编程方式执行数据库恢复。 若要还原数据库，请使用[启动 AzureSqlDatabaseRestore][] cmdlet。

1. 选择您的帐户包含要还原的数据库的订阅。
2. 数据的数据库列表还原点 （需要 Azure 资源管理模式）
3. 选择所需的还原点，使用 RestorePointCreationDate。
3. 将数据库还原到所需的还原点。
4. 监视恢复进度。

```
Select-AzureSubscription -SubscriptionId <Subscription_GUID>

# List database restore points
Switch-AzureMode AzureResourceManager
Get-AzureSqlDatabaseRestorePoints -ServerName "<YourServerName>" -DatabaseName "<YourDatabaseName>" -ResourceGroupName "<YourResourceGroupName>"

# Pick desired restore point using RestorePointCreationDate
$PointInTime = "<RestorePointCreationDate>"

# Get the specific database to restore
Switch-AzureMode AzureServiceManagement
$Database = Get-AzureSqlDatabase -ServerName "<YourServerName>" –DatabaseName "<YourDatabaseName>"

# Restore database
$RestoreRequest = Start-AzureSqlDatabaseRestore -SourceServerName "<YourServerName>" -SourceDatabase $Database -TargetDatabaseName "<NewDatabaseName>" -PointInTime $PointInTime

# Monitor progress of restore operation
Get-AzureSqlDatabaseOperation -ServerName "<YourServerName>" –OperationGuid $RestoreRequest.RequestID
```

请注意，如果您的服务器为 foo.database.windows.net，使用"foo"为 powershell cmdlet 中的服务器名称。

### REST API，
其余部分可用于以编程方式执行数据库恢复。

1. 获取使用 Get 数据库还原点操作数据库还原点列表。
2. 通过使用[创建数据库还原请求][]操作开始还原。
3. 通过使用[数据库操作状态][]操作跟踪您还原的状态。

还原完成后，您可以配置您恢复的数据库，用于按照[完成恢复的数据库][]指南。

## 恢复已删除的数据库
事件数据库被删除，删除的时间可以还原已删除的数据库。 Azure SQL 数据仓库采用数据库快照数据库将被丢弃并将它保留 7 天之前。

### PowerShell
使用 PowerShell 以编程方式执行已删除的数据库恢复。 若要还原已删除的数据库，请使用[启动 AzureSqlDatabaseRestore][] cmdlet。

1. 查找已删除的数据库，并从列表中删除的数据库的删除日期

```
Get-AzureSqlDatabase -RestorableDropped -ServerName "<YourServerName>"
```

2. 获取特定的已删除的数据库并开始恢复。

```
$Database = Get-AzureSqlDatabase -RestorableDropped -ServerName "<YourServerName>" –DatabaseName "<YourDatabaseName>" -DeletionDate "1/01/2015 12:00:00 AM"

$RestoreRequest = Start-AzureSqlDatabaseRestore -SourceRestorableDroppedDatabase $Database –TargetDatabaseName "<NewDatabaseName>"

Get-AzureSqlDatabaseOperation –ServerName "<YourServerName>" –OperationGuid $RestoreRequest.RequestID
```

### REST API，
其余部分可用于以编程方式执行数据库恢复。

1.  列出所有您可还原已删除的数据库使用此[列表可还原丢失的数据库][]操作。
2.  获得您想要使用[Get 可还原已删除的数据库][]操作来恢复已删除的数据库的详细信息。
3.  通过使用[创建数据库还原请求][]操作开始还原。
4.  通过使用[数据库操作状态][]操作跟踪您还原的状态。

还原完成后，您可以配置您恢复的数据库，用于按照[完成恢复的数据库][]指南。


## 下一步行动
若要了解详细信息的其他 SQL Azure 数据库版本的业务连续性功能，请阅读[SQL Azure 数据库业务连续性概览][]。


<!--Image references-->

<!--Article references-->
[Azure SQL 数据库业务连续性概述]: sql-database/sql-database-business-continuity.md
[完成恢复的数据库]: sql-database/sql-database-recovered-finalize.md

<!--MSDN references-->
[创建数据库的恢复请求]: http://msdn.microsoft.com/library/azure/dn509571.aspx
[数据库操作状态]: http://msdn.microsoft.com/library/azure/dn720371.aspx
[获取可还原已删除的数据库]: http://msdn.microsoft.com/library/azure/dn509574.aspx
[列表可恢复删除数据库]: http://msdn.microsoft.com/library/azure/dn509562.aspx
[开始-AzureSqlDatabaseRestore]: https://msdn.microsoft.com/en-us/library/dn720218.aspx

<!--Other Web references-->

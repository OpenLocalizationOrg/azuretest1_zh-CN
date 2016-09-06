---
ms.openlocfilehash: a2b1648d16787844d8de8758a6e4136d52eb804c
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="SQL 数据库用户错误恢复" 
   description="了解如何从用户错误、 数据意外损坏或已删除的数据库，使用 SQL Azure 数据库的时间点恢复 (PITR) 功能恢复。" 
   services="sql-database" 
   documentationCenter="" 
   authors="elfisher" 
   manager="jeffreyg" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management" 
   ms.date="07/23/2015"
   ms.author="elfish"/>

# 从用户错误恢复 Azure SQL 数据库

Azure 的 SQL 数据库提供了从用户错误或非有意的数据修改中恢复的两个核心功能。

- 时间点还原 
- 还原已删除的数据库

您可以了解有关在此[博客文章](http://azure.microsoft.com/blog/2014/10/01/azure-sql-database-point-in-time-restore/)的这些功能的详细信息。

Azure 的 SQL 数据库始终还原到新数据库中。 这些功能提供对所有基本、 标准和高级数据库还原。
##恢复时间点还原
在发生用户错误或非有意的数据修改，点在时间还原可用来在您的数据库的保持期内及时将数据库还原到任何时间点。 

基本数据库有 7 天的保留、 标准数据库有 14 天的保留期和高级的数据库有 35 天的保留。 若要了解更多有关数据库保留，请阅读我们[的业务连续性概述](sql-database-business-continuity.md)。

> [AZURE.NOTE] 还原的数据库创建一个新数据库。 请务必确保还原具有足够 DTU 容量为新建的数据库服务器。 您可以通过[联系支持人员](http://azure.microsoft.com/blog/azure-limits-quotas-increase-requests/)请求此配额的增加。

###Azure 门户
1. 登录到[Azure 门户](https://portal.Azure.com)
2. 在屏幕的左侧选择**浏览**，然后选择**SQL 数据库**。
3. 导航到您的数据库，然后选择它。
4. 在数据库的刀片的顶部，选择**还原**。
5. 指定一个数据库名称，时间点，然后单击**创建**。
6. 数据库还原过程开始，并可以在屏幕的左侧使用**通知**进行监控。

还原完成后，您可以配置您恢复的数据库，用于按照[完成恢复数据库](sql-database-recovered-finalize.md)指南。
###PowerShell
使用 PowerShell 以编程方式执行数据库恢复。

要使用恢复的时间点还原数据库，请使用[启动 AzureSqlDatabaseRestore](https://msdn.microsoft.com/library/dn720218.aspx?f=255&MSPPError=-2147217396) cmdlet。 通过详细的审核，请参阅我们的[操作方法视频](http://azure.microsoft.com/documentation/videos/restore-a-sql-database-using-point-in-time-restore-with-microsoft-azure-powershell/)。

        $Database = Get-AzureSqlDatabase -ServerName "YourServerName" –DatabaseName “YourDatabaseName”
        $RestoreRequest = Start-AzureSqlDatabaseRestore -SourceDatabase $Database –TargetDatabaseName “NewDatabaseName” –PointInTime “2015-01-01 06:00:00”
        Get-AzureSqlDatabaseOperation –ServerName "YourServerName" –OperationGuid $RestoreRequest.RequestID
         
还原完成后，您可以配置您恢复的数据库，用于按照[完成恢复数据库](sql-database-recovered-finalize.md)指南。

###REST API， 
其余部分可用于以编程方式执行数据库恢复。

1. 获得您要还原使用[获取数据库](http://msdn.microsoft.com/library/azure/dn505708.aspx)操作的数据库。

2.  创建使用[创建数据库还原请求](http://msdn.microsoft.com/library/azure/dn509571.aspx)操作的恢复请求。
    
3.  跟踪使用[数据库操作状态](http://msdn.microsoft.com/library/azure/dn720371.aspx)操作的恢复请求。

还原完成后，您可以配置您恢复的数据库，用于按照[完成恢复数据库](sql-database-recovered-finalize.md)指南。

##恢复已删除的数据库
事件被删除数据库，SQL Azure 数据库允许您删除的时间点还原已删除的数据库。 Azure 的 SQL 数据库存储已删除的数据库备份数据库的保持期。

已删除的数据库的保持期取决于同时存在的数据库服务层或数日内在数据库存在，较少者为准。 要了解更多有关数据库保留阅读我们[的业务连续性概述](sql-database-business-continuity.md)。

> [AZURE.NOTE] 还原的数据库创建一个新数据库。 请务必确保还原具有足够 DTU 容量为新建的数据库服务器。 您可以通过[联系支持人员](http://azure.microsoft.com/blog/azure-limits-quotas-increase-requests/)请求此配额的增加。

###Azure 门户
1. 登录到[Azure 门户](https://portal.Azure.com)
2. 在屏幕的左侧选择**浏览**，然后选择**SQL 服务器**。
3. 导航到您的服务器，然后选择它。
4. 在您服务器的刀片式服务器上的**操作**，选择**删除数据库**。
5. 选择想要还原已删除的数据库。
6. 指定一个数据库名称，然后单击**创建**。
7. 数据库还原过程开始，并可以在屏幕的左侧使用**通知**进行监控。

还原完成后，您可以配置您恢复的数据库，用于按照[完成恢复数据库](sql-database-recovered-finalize.md)指南。

###PowerShell
使用 PowerShell 以编程方式执行数据库恢复。

若要还原已删除的数据库，请使用[启动 AzureSqlDatabaseRestore](https://msdn.microsoft.com/library/dn720218.aspx?f=255&MSPPError=-2147217396) cmdlet。  通过详细的审核，请参阅我们的[操作方法视频](http://azure.microsoft.com/documentation/videos/restore-a-deleted-sql-database-with-microsoft-azure-powershell/)。

1. 查找已删除的数据库，并从列表中删除的数据库的删除日期。
        
        Get-AzureSqlDatabase -RestorableDropped -ServerName "YourServerName"

2. 获取特定的已删除的数据库并开始恢复。

        $Database = Get-AzureSqlDatabase -RestorableDropped -ServerName "YourServerName" –DatabaseName “YourDatabaseName” -DeletionDate "1/01/2015 12:00:00 AM""
        $RestoreRequest = Start-AzureSqlDatabaseRestore -SourceRestorableDroppedDatabase $Database –TargetDatabaseName “NewDatabaseName”
        Get-AzureSqlDatabaseOperation –ServerName "YourServerName" –OperationGuid $RestoreRequest.RequestID
         
还原完成后，您可以配置您恢复的数据库，用于按照[完成恢复数据库](sql-database-recovered-finalize.md)指南。

###REST API， 
其余部分可用于以编程方式执行数据库恢复。

1.  列出所有您可还原已删除的数据库使用[列表可恢复删除数据库](http://msdn.microsoft.com/library/azure/dn509562.aspx)操作。
    
2.  获得您想要使用[获取可还原丢失的数据库](http://msdn.microsoft.com/library/azure/dn509574.aspx)操作来恢复已删除的数据库的详细信息。

3.  通过[创建请求数据库还原](http://msdn.microsoft.com/library/azure/dn509571.aspx)操作开始还原。
    
4.  通过使用[数据库操作状态](http://msdn.microsoft.com/library/azure/dn720371.aspx)操作跟踪您还原的状态。

还原完成后，您可以配置您恢复的数据库，用于按照[完成恢复数据库](sql-database-recovered-finalize.md)指南。
 

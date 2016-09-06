---
ms.openlocfilehash: 3ebc37408b1bb7710f888cb43eb93753d383dd59
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="SQL 数据库灾难恢复" 
   description="了解如何恢复数据库，请从区域数据中心停机或失败的 SQL Azure 数据库复制地理和地理还原功能。" 
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
   ms.date="07/14/2015"
   ms.author="elfish"/>

# 从停机中恢复 Azure SQL 数据库

Azure SQL 数据库提供几个故障恢复功能︰

- 活动地区复制[（博客）](http://azure.microsoft.com/blog/2014/07/12/spotlight-on-sql-database-active-geo-replication/)
- 标准的 Geo 复制[（博客）](http://azure.microsoft.com/blog/2014/09/03/azure-sql-database-standard-geo-replication/)
- 地区恢复[（博客）](http://azure.microsoft.com/blog/2014/09/13/azure-sql-database-geo-restore/)

若要了解有关准备灾难以及何时恢复您的数据库，请访问我们[的业务连续性设计](sql-database-business-continuity-design.md)的网页。 

##何时启动恢复 

恢复操作会影响应用程序。 它要求更改 SQL 连接字符串，并可能导致数据永久丢失。 因此它应该只当停电很可能持续更长时间比应用程序的 RTO。 应用程序部署到生产环境时应执行的应用程序运行状况的定期监控并使用以下数据点断言恢复属于保修范围︰

1. 永久性连接失败从应用层到数据库。
2. Azure 门户中具有广泛影响的区域显示有关事件的通知。

## Geo 复制辅助数据库的故障转移
> [AZURE.NOTE] 您必须配置[标准的 Geo 复制](https://msdn.microsoft.com/library/azure/dn758204.aspx)或[活动的地理复制](https://msdn.microsoft.com/library/azure/dn741339.aspx)具有辅助数据库用于故障转移。 Geo 复制功能仅适用于标准和特优数据库。 

在主数据库上停机时，您可以故障切换到辅助数据库上还原可用性。 要做这将需要强制终止连续复制关系。 有关终止连续副本的完整说明的关系转到[此处](https://msdn.microsoft.com/library/azure/dn741323.aspx)。 



###Azure 门户
1. 登录到[Azure 门户](https://portal.Azure.com)
2. 在屏幕的左侧选择**浏览**，然后选择**SQL 数据库**
3. 导航到您的数据库，然后选择它。 
4. 数据库刀片底部选择**地区复制映射**。
4. 在**辅助区域**下右键单击您希望恢复到和选择**停止**的数据库名称的行。

连续复制关系终止后，您可以配置您恢复的数据库，用于按照[完成恢复数据库](sql-database-recovered-finalize.md)指南。
###PowerShell
使用 PowerShell 若要以编程方式执行数据库恢复。

若要终止辅助数据库中的关系，使用[Stop AzureSqlDatabaseCopy](https://msdn.microsoft.com/library/dn720223) cmdlet。
        
        $myDbCopy = Get-AzureSqlDatabaseCopy -ServerName "SecondaryServerName" -DatabaseName "SecondaryDatabaseName"
        $myDbCopy | Stop-AzureSqlDatabaseCopy -ServerName "SecondaryServerName" -ForcedTermination
         
连续复制关系终止后，您可以配置您恢复的数据库，用于按照[完成恢复数据库](sql-database-recovered-finalize.md)指南。
###REST API， 
其余部分可用于以编程方式执行数据库恢复。

1. 获取数据库连续复制使用[获取数据库复制](https://msdn.microsoft.com/library/azure/dn509570.aspx)操作。
2. 停止数据库连续复制使用[停止数据库复制](https://msdn.microsoft.com/library/azure/dn509573.aspx)操作。
停止数据库副本请求 URI 中使用的辅助服务器名称和数据库名称

 连续复制关系终止后，您可以配置您恢复的数据库，用于按照[完成恢复数据库](sql-database-recovered-finalize.md)指南。
## 使用地区恢复恢复

数据库停机，可以从使用地理还原其最新地理冗余备份恢复数据库。 

> [AZURE.NOTE] 恢复的数据库创建一个新数据库。 请务必确保恢复具有足够 DTU 容量为新建的数据库服务器。 您可以通过[联系支持人员](http://azure.microsoft.com/blog/azure-limits-quotas-increase-requests/)请求此配额的增加。

###Azure 门户
1. 登录到[Azure 门户](https://portal.Azure.com)
2. 在左侧屏幕选择**新建**，然后选择**数据和存储**，然后选择**SQL 数据库**
2. 选择**备份**作为源，然后选择您想要恢复从地理冗余备份。
3. 指定的数据库属性的其余部分，然后单击**创建**。
4. 数据库还原过程开始，并可以在屏幕的左侧使用**通知**进行监控。

恢复数据库后可以配置它以使用按照[完成恢复数据库](sql-database-recovered-finalize.md)指南。
###PowerShell 
使用 PowerShell 若要以编程方式执行数据库恢复。

要启动的地区恢复请求，请使用[启动 AzureSqlDatabaseRecovery](https://msdn.microsoft.com/library/azure/dn720224.aspx) cmdlet。 通过详细的审核，请参阅我们的[操作方法视频](http://azure.microsoft.com/documentation/videos/restore-a-sql-database-using-geo-restore-with-microsoft-azure-powershell/)。

        $Database = Get-AzureSqlRecoverableDatabase -ServerName "ServerName" –DatabaseName “DatabaseToBeRecovered"
        $RecoveryRequest = Start-AzureSqlDatabaseRecovery -SourceDatabase $Database –TargetDatabaseName “NewDatabaseName” –TargetServerName “TargetServerName”
        Get-AzureSqlDatabaseOperation –ServerName "TargetServerName" –OperationGuid $RecoveryRequest.RequestID

恢复数据库后可以配置它以使用按照[完成恢复数据库](sql-database-recovered-finalize.md)指南。
###REST API， 

其余部分可用于以编程方式执行数据库恢复。

1.  获取使用此[列表可恢复的数据库](http://msdn.microsoft.com/library/azure/dn800984.aspx)操作可恢复的数据库的列表。
    
2.  获得您想要恢复使用[获取可恢复的数据库](http://msdn.microsoft.com/library/azure/dn800985.aspx)操作的数据库。
    
3.  创建使用[创建数据库恢复请求](http://msdn.microsoft.com/library/azure/dn800986.aspx)操作的恢复请求。
    
4.  跟踪恢复使用该[数据库操作状态](http://msdn.microsoft.com/library/azure/dn720371.aspx)操作的状态。

恢复数据库后可以配置它以使用按照[完成恢复数据库](sql-database-recovered-finalize.md)指南。
 

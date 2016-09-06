---
ms.openlocfilehash: d77b8f1bd8bb68a9b97249819e182b1b4019503b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="在 Azure PowerShell 使用地理还原 SQL Azure 数据库恢复" 
   description="Geo 还原，Microsoft Azure SQL 数据库，还原数据库中恢复数据库，Azure PowerShell" 
   services="sql-database" 
   documentationCenter="" 
   authors="elfisher" 
   manager="jeffreyg" 
   editor="v-romcal"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="storage-backup-recovery" 
   ms.date="07/24/2015"
   ms.author="elfish; v-romcal; v-stste"/>

# 在 Azure PowerShell 使用地理还原 SQL Azure 数据库恢复

> [AZURE.SELECTOR]
- [地区恢复的门户网站](sql-database-geo-restore-tutorial-management-portal.md)
- [Geo 还原-REST API，](sql-database-geo-restore-tutorial-rest.md)   

## 概述

本教程展示如何使用[Azure PowerShell](../powershell-install-configure.md)Geo 还原 SQL Azure 数据库恢复。 地区恢复是所有基本、 标准和最优 Azure SQL 数据库服务层包含核灾难恢复保护。

## 限制和安全

请参阅[使用 Azure 门户中的 Geo 还原 SQL Azure 数据库恢复](sql-database-geo-restore-tutorial-management-portal.md)。

## 如何︰ 在 Azure PowerShell 的地区恢复 SQL Azure 数据库恢复

> [AZURE.VIDEO restore-a-sql-database-using-geo-restore-with-microsoft-azure-powershell]

您必须使用基于证书的身份验证运行以下 cmdlet。 有关详细信息，请参阅[如何安装和配置 Azure PowerShell](../powershell-install-configure.md#use-the-certificate-method)的*使用证书的方法*一节。

1. 通过使用[Get AzureSqlRecoverableDatabase](http://msdn.microsoft.com/library/azure/dn720219.aspx) cmdlet 获取的可恢复的数据库的列表。 指定以下参数︰
    * **服务器名**的数据库所在的位置。 

    `PS C:\>Get-AzureSqlRecoverableDatabase -ServerName "myserver"`

2. 选择您想要使用[Get AzureSqlRecoverableDatabase](http://msdn.microsoft.com/library/azure/dn720219.aspx) cmdlet 从恢复的数据库。 指定以下参数︰
    * **服务器名**的数据库所在的位置。
    * 要从恢复的数据库的**数据库名称**。

    `PS C:\>$Database = Get-AzureSqlRecoverableDatabase -ServerName "myserver" –DatabaseName “mydb”`
     
3. 通过使用[开始 AzureSqlDatabaseRecovery](http://msdn.microsoft.com/library/dn720224.aspx) cmdlet 开始恢复。 指定以下参数︰   
    * 您想要恢复**SourceDatabase** 。
    * 要恢复到的数据库中的**TargetDatabaseName** 。
    * 您想要将数据库恢复到**TargetServerName** 。

    存储内容返回到一个名为**$RestoreRequest**的变量。 此变量包含用于监视还原状态的恢复请求 ID。

    `PS C:\>$RecoveryRequest = Start-AzureSqlDatabaseRecovery -SourceDatabase $Database –TargetDatabaseName “myrecoveredDB” –TargetServerName “mytargetserver”`
    
数据库恢复可能需要一些时间才能完成。 要监视恢复的状态，请使用[Get AzureSqlDatabaseOperation](http://msdn.microsoft.com/library/azure/dn546738.aspx) cmdlet 并指定以下参数︰

* 要还原到数据库的**服务器名称**。
* **OperationGuid**即恢复请求 ID 存储在**$RecoveryRequest**变量中，在第 3 步。

    `PS C:\>Get-AzureSqlDatabaseOperation –ServerName “mytargetserver” –OperationGuid $RecoveryRequest.ID`

**状态**和**完成百分比**域显示恢复的状态。

## 下一步行动

有关详细信息，请参阅以下文章︰  

[还原点还原的时间在使用 Azure 门户中 SQL Azure 数据库](sql-database-point-in-time-restore-tutorial-management-portal.md)

[还原已删除的 SQL Azure 数据库在 Azure 门户](sql-database-restore-deleted-database-tutorial-management-portal.md)

[SQL azure 数据库业务连续性](http://msdn.microsoft.com/library/azure/hh852669.aspx)

[SQL azure 数据库备份和恢复](http://msdn.microsoft.com/library/azure/jj650016.aspx)

[Azure SQL 数据库 Geo-还原 （博客）](http://azure.microsoft.com/blog/2014/09/13/azure-sql-database-geo-restore/)

[Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx)
 
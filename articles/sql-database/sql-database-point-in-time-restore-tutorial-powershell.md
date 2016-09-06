---
ms.openlocfilehash: a02f27c1ac6241654ed6ce02243758ee8e96c7ae
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="用点时间在 Azure PowerShell 还原 SQL Azure 数据库还原" 
   description="点时间还原，Microsoft Azure SQL 数据库，还原数据库，数据库，Azure PowerShell 恢复" 
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

# 用点时间在 Azure PowerShell 还原 SQL Azure 数据库还原

> [AZURE.SELECTOR]
- [点时间恢复的门户网站](sql-database-point-in-time-restore-tutorial-management-portal.md)
- [时间点还原的 REST API，](sql-database-point-in-time-restore-tutorial-rest.md) 

## 概述

本教程展示了如何用点时间在[Azure PowerShell](../powershell-install-configure.md)还原 SQL Azure 数据库还原。 Azure SQL 数据库都有内置的备份以支持自助服务点时间还原为基本、 标准和最优的服务层。

在还原的时间点创建一个新数据库。 该服务将自动选择基础的恢复点处使用的备份的服务层。 请确保您在逻辑服务器创建另一个数据库上具有可用配额。 如果您想要请求增加的配额，请联系[Azure 支持](http://azure.microsoft.com/support/options/)。

## 限制和安全

请参阅[还原 SQL Azure 数据库中使用 Azure 门户中恢复的时间点](sql-database-point-in-time-restore-tutorial-management-portal.md)。

## 如何︰ 用点时间在 Azure PowerShell 还原 SQL Azure 数据库还原

> [AZURE.VIDEO restore-a-sql-database-using-point-in-time-restore-with-microsoft-azure-powershell]

您必须使用基于证书的身份验证运行以下 cmdlet。 有关详细信息，请参阅[如何安装和配置 Azure PowerShell](../powershell-install-configure.md#use-the-certificate-method)的*使用证书的方法*一节。

1. 获取要使用[Get AzureSqlDatabase](http://msdn.microsoft.com/library/azure/dn546735.aspx) cmdlet 来还原的数据库。 指定以下参数︰
    * **服务器名**的数据库所在的位置。
    * 您想要还原的数据库的**数据库名称**。 

    `PS C:\>$Database = Get-AzureSqlDatabase -ServerName "myserver" –DatabaseName “mydb”`

2. 通过使用[开始 AzureSqlDatabaseRestore](http://msdn.microsoft.com/library/azure/dn720218.aspx) cmdlet 开始恢复。 指定以下参数︰  
    * 您想要还原从**SourceDatabase** 。
    * 要还原到的数据库中的**TargetDatabaseName** 。
    * 您想要恢复到**PointInTime** 。

    存储内容返回到一个名为**$RestoreRequest**的变量。 此变量包含用于监视还原状态的恢复请求 ID。 

    `PS C:\>$RestoreRequest = Start-AzureSqlDatabaseRestore -SourceDatabase $Database –TargetDatabaseName “myrestoredDB” –PointInTime “2015-01-01 06:00:00”`

恢复可能需要一些时间才能完成。 若要监视还原状态，使用[Get AzureSqlDatabaseOperation](http://msdn.microsoft.com/library/azure/dn546738.aspx) cmdlet 并指定以下参数︰

* 要还原到数据库的**服务器名称**。
* **OperationGuid**即恢复请求 ID 存储在**$RestoreRequest**变量中的步骤 2。

    `PS C:\>Get-AzureSqlDatabaseOperation –ServerName "myserver" –OperationGuid $RestoreRequest.RequestID`

**状态**和**完成百分比**域显示恢复的状态。 

## 下一步行动

有关详细信息，请参阅以下文章︰  

[SQL azure 数据库业务连续性](http://msdn.microsoft.com/library/azure/hh852669.aspx)

[SQL azure 数据库备份和恢复](http://msdn.microsoft.com/library/azure/jj650016.aspx)

[Azure SQL 数据库点时间还原 （博客）](http://azure.microsoft.com/blog/2014/10/01/azure-sql-database-point-in-time-restore/)

[Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx)
 
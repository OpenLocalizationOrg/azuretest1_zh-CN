---
ms.openlocfilehash: c9b4eb616442043b804e7cfbfd19c957a25b7c81
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="还原已删除的 SQL Azure 数据库使用 REST API，" 
   description="Microsoft Azure SQL 数据库，还原已删除的数据库、 恢复已删除的数据库，REST API，" 
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
   ms.author="elfish; v-romcal"/>

# 还原已删除的 SQL Azure 数据库使用 REST API，

> [AZURE.SELECTOR]
- [还原已删除的数据库-门户](sql-database-restore-deleted-database-tutorial-management-portal.md)
- [还原已删除的数据库-PowerShell](sql-database-restore-deleted-database-tutorial-powershell.md) 

## 概述

本指南介绍了如何还原已删除的 SQL Azure 数据库使用 REST API。 提供了指向更详细操作。

还原已删除的 SQL Azure 数据库创建新的数据库。 该服务将自动选择基础的恢复点处使用的备份的服务层。 请确保您在逻辑服务器创建另一个数据库上具有可用配额。 如果您想要请求增加的配额，请联系[Azure 支持](http://azure.microsoft.com/support/options/)。

## 限制和安全

请参阅[还原已删除的 SQL Azure 数据库在 Azure 的门户](sql-database-restore-deleted-database-tutorial-management-portal.md)。

## 如何︰ 还原已删除的 SQL Azure 数据库使用 REST API，

1.  列出所有您可还原已删除的数据库使用[列表可恢复删除数据库](http://msdn.microsoft.com/library/azure/dn509562.aspx)操作。
    
2.  获得您想要使用[获取可还原丢失的数据库](http://msdn.microsoft.com/library/azure/dn509574.aspx)操作来恢复已删除的数据库的详细信息。

3.  通过[创建请求数据库还原](http://msdn.microsoft.com/library/azure/dn509571.aspx)操作开始还原。
    
4.  通过使用[数据库操作状态](http://msdn.microsoft.com/library/azure/dn720371.aspx)操作跟踪您还原的状态。

## 下一步行动

有关详细信息，请参阅以下文章︰

[SQL azure 数据库业务连续性](http://msdn.microsoft.com/library/azure/hh852669.aspx)

[SQL azure 数据库备份和恢复](http://msdn.microsoft.com/library/azure/jj650016.aspx)

[服务管理 REST API 参考](http://msdn.microsoft.com/library/azure/ee460799.aspx) 
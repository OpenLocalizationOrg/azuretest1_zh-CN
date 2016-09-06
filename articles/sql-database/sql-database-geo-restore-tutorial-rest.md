---
ms.openlocfilehash: f3223d2902517a7b491c6dd2c7026d3f7407020c
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="使用 REST API，Geo 还原 SQL Azure 数据库恢复" 
   description="Geo 还原，Microsoft Azure SQL 数据库，还原数据库中恢复数据库，REST API，" 
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

# 使用 REST API，Geo 还原 SQL Azure 数据库恢复

> [AZURE.SELECTOR]
- [地区恢复的门户网站](sql-database-geo-restore-tutorial-management-portal.md)
- [Geo 还原-PowerShell](sql-database-geo-restore-tutorial-powershell.md)

## 概述

本指南介绍了如何使用 REST API，SQL Azure 数据库恢复。 提供了指向更详细操作。 地区恢复是所有基本、 标准和最优 Azure SQL 数据库服务层包含核灾难恢复保护。

## 限制和安全

请参阅[恢复使用 Azure 门户中的地区恢复 SQL Azure 数据库](sql-database-geo-restore-tutorial-management-portal.md)。

## 如何︰ 使用 REST API，SQL Azure 数据库恢复

1.  获取使用此[列表可恢复的数据库](http://msdn.microsoft.com/library/azure/dn800984.aspx)操作可恢复的数据库的列表。
    
2.  获得您想要恢复使用[获取可恢复的数据库](http://msdn.microsoft.com/library/azure/dn800985.aspx)操作的数据库。
    
3.  创建使用[创建数据库恢复请求](http://msdn.microsoft.com/library/azure/dn800986.aspx)操作的恢复请求。
    
4.  跟踪恢复使用该[数据库操作状态](http://msdn.microsoft.com/library/azure/dn720371.aspx)操作的状态。

## 下一步行动

有关详细信息，请参阅以下文章︰

[还原点还原的时间在使用 Azure 门户中 SQL Azure 数据库](sql-database-point-in-time-restore-tutorial-management-portal.md)

[还原已删除的 SQL Azure 数据库在 Azure 门户](sql-database-restore-deleted-database-tutorial-management-portal.md)

[SQL azure 数据库业务连续性](http://msdn.microsoft.com/library/azure/hh852669.aspx)

[SQL azure 数据库备份和恢复](http://msdn.microsoft.com/library/azure/jj650016.aspx)

[Azure SQL 数据库 Geo-还原 （博客）](http://azure.microsoft.com/blog/2014/09/13/azure-sql-database-geo-restore/)

[服务管理 REST API 参考](https://msdn.microsoft.com/library/azure/ee460799.aspx)
 
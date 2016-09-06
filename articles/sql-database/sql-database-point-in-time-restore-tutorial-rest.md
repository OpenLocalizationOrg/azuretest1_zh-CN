---
ms.openlocfilehash: 698beb216d1829771f5ee04c18a60820bba1bac3
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="将 SQL Azure 数据库使用在使用 REST API，恢复的时间点还原" 
   description="点时间还原，Microsoft Azure SQL 数据库，还原数据库，将恢复数据库，REST API，" 
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

# 将 SQL Azure 数据库使用在使用 REST API，恢复的时间点还原

> [AZURE.SELECTOR]
- [点时间恢复的门户网站](sql-database-point-in-time-restore-tutorial-management-portal.md)
- [时间点还原-PowerShell](sql-database-point-in-time-restore-tutorial-powershell.md) 

## 概述

本指南介绍了如何还原 SQL Azure 数据库使用在使用 REST API，恢复的时间点。 提供了指向更详细操作。

在还原的时间点创建一个新数据库。 该服务将自动选择基础的恢复点处使用的备份的服务层。 请确保您在逻辑服务器创建另一个数据库上具有可用配额。 如果您想要请求增加的配额，请联系[Azure 支持](http://azure.microsoft.com/support/options/)。

## 限制和安全

请参阅[还原 SQL Azure 数据库中使用 Azure 门户中恢复的时间点](sql-database-point-in-time-restore-tutorial-management-portal.md)。

## 如何︰ 使用 REST API，SQL Azure 数据库还原

1.  获得您要还原使用[获取数据库](http://msdn.microsoft.com/library/azure/dn505708.aspx)操作的数据库。

2.  创建使用[创建数据库还原请求](http://msdn.microsoft.com/library/azure/dn509571.aspx)操作的恢复请求。
    
3.  跟踪使用[数据库操作状态](http://msdn.microsoft.com/library/azure/dn720371.aspx)操作的恢复请求。

## 下一步行动

有关详细信息，请参阅以下文章︰ 

[SQL azure 数据库业务连续性](http://msdn.microsoft.com/library/azure/hh852669.aspx)

[SQL azure 数据库备份和恢复](http://msdn.microsoft.com/library/azure/jj650016.aspx)

[Azure SQL 数据库点时间还原 （博客）](http://azure.microsoft.com/blog/2014/10/01/azure-sql-database-point-in-time-restore/)

[服务管理 REST API 参考](https://msdn.microsoft.com/library/azure/ee460799.aspx)
 
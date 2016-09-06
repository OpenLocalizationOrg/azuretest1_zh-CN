---
ms.openlocfilehash: 0b90c6eef073954a02cc07469d9ff1cf05984aac
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="SQL 数据库业务连续性的常见问题解答" 
   description="常见问题及解答有关内置和可选功能要求提供与 SQL Azure 数据库的业务连续性和灾难恢复的客户。" 
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

# 业务连续性的常见问题解答

## 1.我还原点保留期间当我降级/升级服务层发生什么？
降级到较低的性能层之后, 该还原点的保留期是立即截断为性能层的当前数据库的保持期。 

如果升级后的数据库服务层，保留期间将开始扩展升级数据库后只。 

例如，如果数据库从 P1 降级到 S3，保留期限将从 35 天更改为立即 14 天，14 天之前的所有还原点将都不再可用。 随后，如果将其升级为 P1 再次，保留期将从 14 日开始，开始构建达 35 天。

## 2.多长时间是已删除的数据库的保持期？ 
保留期取决于同时存在的数据库服务层或数天在数据库存在，较少者为准。

## 3.如何可以恢复已删除的服务器？

没有这一次还原已删除的服务器的支持。 

## 4.若要还原某个数据库需要多长时间？

还原数据库所需的持续时间取决于多个因素，如大小的数据库、 事务日志、 网络带宽和等号。大部分数据库将在 12 小时内恢复。

## 5.是否可以更改我的数据库的还原点保留期？

No. 

## 6.如何为数据库查明可用的可用还原点？

从用户错误 – 恢复当前的时间是最新的还原点。 要找出最早的可用还原点，用于[获取数据库](https://msdn.microsoft.com/library/dn505708.aspx)(*RecoveryPeriodStartDate*) 最早的还原点 （非地理复制还原点）。

从停机中的恢复使用[获取可恢复数据库](https://msdn.microsoft.com/library/dn800985.aspx)(*LastAvailableBackupDate*) 来获取最新的地理复制还原点。

## 7.如何可以批量还原数据库下我的服务器？

没有内置功能执行批量还原。 您可以使用[SQL Azure 数据库︰ 完整服务器恢复](https://gallery.technet.microsoft.com/Azure-SQL-Database-Full-82941666)脚本来完成此任务。 

## 8.什么是标准的 geo 复制和活动的各地区复制之间的区别？

对于标准的地区复制，辅助数据库不可读。 它是仅可用于故障转移在停机期间。

对于活动的各地区复制，所有辅助数据库都是可读 （最多 4 次映像）。

## 9.当使用标准的 geo 复制或活动的各地区复制，复制延迟是什么？

Geo 复制使用连续复制。 因此，使用[sys.dm_continuous_copy_status](https://msdn.microsoft.com/library/azure/dn741329.aspx)动态管理视图 (Dmv) 来获取上次复制时间和其他信息。




 

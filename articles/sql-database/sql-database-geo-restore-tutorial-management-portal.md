---
ms.openlocfilehash: 61b5ec7d66d58ba29d26e5c5dc8becd91090be81
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="在 Azure 门户使用地理还原 SQL Azure 数据库恢复" 
   description="Geo 还原，Microsoft Azure SQL 数据库，还原数据库中恢复数据库，Azure 管理门户，Azure 门户" 
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

# 在 Azure 门户使用地理还原 SQL Azure 数据库恢复

> [AZURE.SELECTOR]
- [Geo 还原-PowerShell](sql-database-geo-restore-tutorial-powershell.md)
- [Geo 还原-REST API，](sql-database-geo-restore-tutorial-rest.md)   

## 概述

本教程展示如何使用[Azure 门户](http://manage.windowsazure.com)中的地区恢复 SQL Azure 数据库恢复。 地区恢复是所有基本、 标准和最优 Azure SQL 数据库服务层包含核灾难恢复保护。

## 限制和安全

* 地区恢复启用所有基本、 标准和最优的服务层。

* 备份保留期很基本，7 天内;标准，14 天;和特优，35 天。

* 只有管理员或共同管理员订阅可以提交的恢复请求。

* 服务器级别主体登录将已还原数据库的所有者。

* Web 层和商务版服务层不支持地区恢复。
 
    * 如果您有网站或商务版数据库中，可以使用数据库的副本来获取您的数据库的事务一致性拷贝，然后将复制的数据库导出到 Microsoft Azure 存储帐户。 有关详细信息，请参阅[如何︰ 使用数据库复制 （Azure SQL 数据库）](http://msdn.microsoft.com/library/azure/ff951631.aspx)和[如何︰ 在 SQL Azure 数据库中使用的导入和导出服务](http://msdn.microsoft.com/library/azure/hh335292.aspx)。

    * Web 版和商业版将终止 9 月 2015年。 有关详细信息，请参阅[Web 和企业版日落常见问题](http://msdn.microsoft.com/library/azure/dn741330.aspx)。

## 如何︰ 使用 Azure 的门户中的 Geo 还原 SQL Azure 数据库恢复

> [AZURE.VIDEO restore-a-sql-database-using-geo-restore]

1. 登录到[Azure 的门户网站](http://manage.windowsazure.com)使用您的 Microsoft 帐户并选择**SQL 数据库**。

2. 在左侧的导航中，单击**SQL 数据库**。

3. 在页面的顶部，单击**服务器**。

4. 在**服务器**列表中，单击**降级**服务器。

4. 在页面的顶部，单击**备份**。

5. 单击想要还原的数据库。

6. 在命令栏中的网页底部，单击**还原**。 这将启动**指定还原设置**对话框。 

7. 选择**数据库名称**和您要还原到数据库的**目标服务器**。 默认情况下数据库名称选择，但如果需要，可以更改它。   

9. 单击复选标记以将恢复请求提交。

恢复可能需要一些时间才能完成。 若要监视还原状态，单击命令栏中，在页面的底部右侧的状态图标，然后单击**详细信息**。

## 下一步行动

有关详细信息，请参阅以下文章︰ 

[还原点还原的时间在使用 Azure 门户中 SQL Azure 数据库](sql-database-point-in-time-restore-tutorial-management-portal.md)

[还原已删除的 SQL Azure 数据库在 Azure 门户](sql-database-restore-deleted-database-tutorial-management-portal.md)

[SQL azure 数据库业务连续性](http://msdn.microsoft.com/library/azure/hh852669.aspx)

[SQL azure 数据库备份和恢复](http://msdn.microsoft.com/library/azure/jj650016.aspx)

[Azure SQL 数据库 Geo-还原 （博客）](http://azure.microsoft.com/blog/2014/09/13/azure-sql-database-geo-restore/) 
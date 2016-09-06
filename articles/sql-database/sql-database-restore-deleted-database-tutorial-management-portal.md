---
ms.openlocfilehash: 0b562179743dcfdc6de7fc7928caccdea86ef2ba
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="还原已删除的 SQL Azure 数据库在 Azure 门户" 
   description="Microsoft Azure SQL 数据库，还原已删除的数据库，恢复已删除的数据库，Azure 管理门户，Azure 门户" 
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

# 还原已删除的 SQL Azure 数据库在 Azure 门户

> [AZURE.SELECTOR]
- [还原已删除的数据库-PowerShell](sql-database-restore-deleted-database-tutorial-powershell.md)
- [还原已删除的数据库的 REST API，](sql-database-restore-deleted-database-tutorial-rest.md)

## 概述

本教程展示如何还原已删除的 SQL Azure 数据库在[Azure 的门户](http://manage.windowsazure.com)。 您可以恢复期间保留期到点被删除已删除的数据库。 保留期取决于数据库的服务层。

还原已删除的 SQL Azure 数据库创建新的数据库。 该服务将自动选择基础的恢复点处使用的备份的服务层。 请确保您在逻辑服务器创建另一个数据库上具有可用配额。 如果您想要请求增加的配额，请联系[Azure 支持](http://azure.microsoft.com/support/options/)。

## 限制和安全

* 还原已删除的 SQL Azure 数据库启用所有基本、 标准和最优的服务层。 

* 备份保留期很基本，7 天内;标准，14 天;和特优，35 天。

* 只有管理员或共同管理员订阅可以提交的恢复请求。

* 服务器级别主体登录将已还原数据库的所有者。
 
* Web 层和商务版服务层不支持恢复已删除的 SQL 数据库。
 
    * 如果您有网站或商务版数据库中，可以使用数据库的副本来获取您的数据库的事务一致性拷贝，然后将复制的数据库导出到 Microsoft Azure 存储帐户。 有关详细信息，请参阅[如何︰ 使用数据库复制 （Azure SQL 数据库）](http://msdn.microsoft.com/library/azure/ff951631.aspx)和[如何︰ 在 SQL Azure 数据库中使用的导入和导出服务](http://msdn.microsoft.com/library/azure/hh335292.aspx)。
    * Web 版和商业版将终止 9 月 2015年。 有关详细信息，请参阅[Web 和企业版日落常见问题](http://msdn.microsoft.com/library/azure/dn741330.aspx)。

## 如何︰ 还原已删除的 SQL Azure 数据库在 Azure 门户

> [AZURE.VIDEO restore-a-deleted-sql-database]

1. 登录到[Azure 的门户网站](http://manage.windowsazure.com)使用您的 Microsoft 帐户。

2. 在左侧的导航中，单击**SQL 数据库**。

3. 在页面的顶部，单击**删除数据库**。 显示**可还原已删除的数据库**的列表。 

4. 单击想要还原的数据库。

6. 在命令栏中的网页底部，单击**还原**。 这将启动**指定还原设置**对话框。 

7. 选择一个**数据库名称**。 默认情况下数据库名称选择，但如果需要，可以更改它。   

    * 请注意，已删除的数据库只能被恢复到服务器删除，起始和结束点处被删除的时间。   

8. 单击复选标记以将恢复请求提交。

恢复可能需要一些时间才能完成。 若要监视还原状态，单击命令栏中，在页面的底部右侧的状态图标，然后单击**详细信息**。

## 下一步行动

有关详细信息，请参阅以下文章︰ 

[SQL azure 数据库业务连续性](http://msdn.microsoft.com/library/azure/hh852669.aspx)

[SQL azure 数据库备份和恢复](http://msdn.microsoft.com/library/azure/jj650016.aspx) 
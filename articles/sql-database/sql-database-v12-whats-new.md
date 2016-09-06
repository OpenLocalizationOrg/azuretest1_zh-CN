---
ms.openlocfilehash: 7caa3169c83cf73c3bd6aca6a3161f13233e2ff6
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="SQL 数据库 V12 中的新增功能 |Microsoft Azure" 
    description="描述为什么现在升级到版本 V12 受益在云环境中使用 SQL Azure 数据库的业务系统。" 
    services="sql-database" 
    documentationCenter="" 
    authors="MightyPen" 
    manager="jeffreyg" 
    editor=""/>


<tags 
    ms.service="sql-database" 
    ms.workload="data-management" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/10/2015" 
    ms.author="genemi"/>


# 在 SQL 数据库 V12 中的新增功能


本主题介绍 V12 新版的 SQL Azure 数据库版本 V11 相比具有许多优点。


我们将继续为 V12 添加功能。 因此，我们鼓励您访问我们的服务更新网页的 Azure，并使用其筛选器︰


- 被过滤到[SQL 数据库服务](http://azure.microsoft.com/updates/?service=sql-database)。
- 被过滤到 SQL 数据库功能的一般可用性[(GA) 的公告](http://azure.microsoft.com/updates/?service=sql-database&update-type=general-availability)。


在介绍了资源限制为 SQL 数据库的最新信息︰<br/>[SQL azure 数据库资源限制](sql-database-resource-limits.md)。


## 提高了应用程序与 SQL Server 的兼容性


SQL 数据库 V12 的一个主要目标是提高与 Microsoft SQL Server 2014年的兼容性。 其他方面，V12 实现奇偶校验与 SQL Server 中可编程性的重要区域。 例如︰


- [公共语言运行时 (CLR) 程序集](http://msdn.microsoft.com/library/ms189524.aspx)
- [窗口函数](https://msdn.microsoft.com/library/bb934097.aspx)，[通过](http://msdn.microsoft.com/library/ms189461.aspx) 
- [XML 索引](https://msdn.microsoft.com/library/bb934097.aspx)和[选择性的 XML 索引](http://msdn.microsoft.com/library/jj670104.aspx)
- [更改跟踪](http://msdn.microsoft.com/library/bb933875.aspx)
- [请选择...到](http://msdn.microsoft.com/library/ms188029.aspx)
- [全文搜索](http://msdn.microsoft.com/library/ms142571.aspx)


请参阅[此处](http://msdn.microsoft.com/library/azure/ee336281.aspx)的少量在 SQL 数据库中尚不支持的功能的。


## 更多的最优性能，新的性能级别


V12，我们增加了在不增加成本的 25%分配给所有高级性能级别的数据库吞吐量单位 (DTUs)。 新功能，如可以实现更高的性能增益︰


- 对于内存中[columnstore 索引](http://msdn.microsoft.com/library/gg492153.aspx)的支持。
- 与[截断表](http://msdn.microsoft.com/library/ms177570.aspx)相关的增强功能的[表分区的行](http://msdn.microsoft.com/library/ms187802.aspx)。
- 可用性的动态管理视图[(Dmv)](http://msdn.microsoft.com/library/ms188754.aspx)和扩展事件[(XEvents)](https://msdn.microsoft.com/library/bb630282.aspx) ，以帮助监视和优化性能。


## SaaS 供应商更好地支持的云


V12，只能在我们发布的新标准性能级别 S3 和[弹性数据库池](sql-database-elastic-pool.md)的公共预览。
这是一个特别适合云解决方案 SaaS 供应商。  有弹性的数据库池，您可以︰


- 在降低成本的大量数据库的数据库之间共享 DTUs。
- 执行[弹性数据库作业](sql-database-elastic-jobs-overview.md)管理在规模较大的数据库。


## 增强的安全功能


安全性是主要关心的在云环境中运行他们的业务的任何人。 在 V12 发布最新的安全功能包括︰


- [行级别安全性](http://msdn.microsoft.com/library/dn765131.aspx)(RLS)
- [动态数据屏蔽](sql-database-dynamic-data-masking-get-started.md)
- [包含的数据库](http://msdn.microsoft.com/library/azure/ff394108.aspx)
- [应用程序角色](http://msdn.microsoft.com/library/ms190998.aspx)通过授予、 拒绝、 吊销
- [透明数据加密](http://msdn.microsoft.com/library/0bf7e8ff-1416-4923-9c4c-49341e208c62.aspx)(TDE)


## 在需要恢复时，增加了业务连续性


V12 提供了显著改进了的恢复点目标 (Rpo) 和估计的恢复时间 (Ert):


| 业务连续性功能 | 早期版本 | V12 |
| :-- | :-- | :-- |
| 地区恢复 | • RPO < 24 小时。<br/>• ERT < 12 小时。 | • RPO < 1 小时。<br/>• ERT < 12 小时。 |
| 标准的 geo 复制 | • RPO < 30 分钟。<br/>• ERT < 2 小时。 | • RPO < 5 秒。<br/>• ERT < 30 秒。 |
| 活动的各地区复制 | • RPO < 5 分钟。<br/>• ERT < 1 小时。 | • RPO < 5 秒。<br/>• ERT < 30 秒。 |


有关更多信息，请参见[SQL 数据库业务连续性](https://msdn.microsoft.com/library/azure/hh852669.aspx)。


## 现在升级的几大原因


有很多的理由为什么客户应立即升级到 Azure SQL 数据库 V12 V11 从︰


- SQL 数据库 V12 有许许多多以外方 V11 的特征。
- 我们继续将新功能添加到 V12，但是没有新的功能将被添加到 V11。
- 之前他们被释放的 Microsoft SQL Server 在 SQL 数据库 V12 发布最新功能。


## 您是否已使用 V12？


看是否有数据库或 SQL 数据库服务的早期版本上运行的逻辑服务器的一种简单方法是执行下列操作︰


1. 转到[Azure 预览门户](http://portal.azure.com/)。
2. 单击**浏览**。
3. 单击**SQL 服务器**。
4. 您的服务器或数据库旁边的图标告诉故事︰
 - ![V12 服务器图标](./media/sql-database-v12-whats-new/v12_icon.png) **V12 逻辑服务器**
 - ![较早版本服务器图标](./media/sql-database-v12-whats-new/earlier_icon.png)**早期版本逻辑服务器**


另一种方法来确定版本是运行`SELECT @@version;`语句在数据库中，并查看输出结果类似于︰


- **12**.0.2000.10 &nbsp; *（版本 V12）*
- **11**.0.9228.18 &nbsp; *（版本 V11）*


只有在 V12 逻辑服务器上可以承载 V12 数据库。 和 V12 服务器可以承载只有 V12 数据库。


如果您将尚未运行的 V12，您可以按照在[升级到 SQL 数据库 V12 到位](sql-database-v12-upgrade.md)的升级您的逻辑服务器。


## <a name="V12AzureSqlDbPreviewGaTable"></a> 一般的可用性区域


- 于 2015 年 7 月 31 日，所有区域有被都提升了对常规可用性 (GA)。
- V12 已发布 12 月 2014 年，但只在预览的状态。

[使用 Microsoft Azure 预览的补充条款](http://azure.microsoft.com/support/legal/preview-supplemental-terms/)。

---
ms.openlocfilehash: 4063b98b80f210b6cdba0e8235fe6baf097bcc53
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="SQL 数据库灾难恢复训练" 
   description="学习指导和最佳使用 SQL Azure 数据库执行灾难恢复训练，以帮助将保存您的任务关键业务应用程序可恢复到故障和停机。" 
   services="sql-database" 
   documentationCenter="" 
   authors="mihaelablendea" 
   manager="jeffreyg" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management" 
   ms.date="07/15/2015"
   ms.author="mihaelab"/>

#执行灾难恢复钻

最好定期执行验证的应用程序准备恢复工作流。 验证应用程序行为和影响的数据丢失和/或中断涉及故障转移是很好的工程实践。 它也是作为业务连续性认证的一部分由大多数行业标准要求。

执行灾难恢复钻组成︰

- 模拟数据层故障
- 恢复 
- 验证应用程序完整性开机自检故障恢复

具体如何[实现业务连续性设计](sql-database-business-continuity.md)、 工作流执行钻取异。 下面我们将介绍执行灾难恢复钻取的 SQL Azure 数据库上下文中的最佳做法。 

##地区恢复

为进行灾难恢复钻时防止数据丢失，我们建议采取通过创建生产环境的副本并使用它来验证应用程序的故障转移工作流中使用测试环境的钻孔。
 
####故障模拟

- 通过删除或重命名源数据库模拟中断，并导致应用程序连接性故障。 通过这种方式可以验证故障检测/报警并恢复持续时间内测量 RTO。

####恢复

- 执行 Geo 数据库还原到另一台服务器描述在[这里](sql-database-disaster-recovery.md)。 
- 更改应用程序配置连接到已恢复的数据库并[完成恢复数据库](sql-database-recovered-finalize.md)指南以完成恢复。

####验证

- 通过验证应用程序完整性后恢复 （即连接字符串、 登录、 基本功能测试或验证的其他部分标准应用 signoffs 过程） 完成钻。

##标准的 Geo 复制

使用标准的 Geo 复制受保护的数据库只能有一个不可读的辅助数据库。 钻练习会涉及到的链接，此时数据库将得不到保护的强制的终止。 此外，没有数据丢失，可能因此不建议在生产数据库上执行此类测试的客户。 相反，我们建议创建生产环境的副本并使用它来验证应用程序的故障切换的工作流。

####故障模拟

- 在主数据库上的负载来模拟。 如果主活动终止数据丢失时可能会出现，这将使钻更逼真。
- 删除主数据库或侧辅助数据库链接的[执行强制的终止](sql-database-disaster-recovery.md)。

####恢复

- 更改要连接到前者只读辅助这将变得完全可供访问，应用程序可以将其用作新的主应用程序配置。 
- 按照[完成恢复数据库](sql-database-recovered-finalize.md)指南以完成恢复。

####验证

- 通过验证应用程序完整性后恢复 （即连接字符串、 登录、 基本功能测试或验证的其他部分标准应用 signoffs 过程） 完成钻。

##活动的各地区复制

将通过使用并行目标服务器并在其中创建另一组读的唯一辅助进行灾难恢复演练。 应使用测试版本的应用程序层以强制终止后，针对该服务器运行测试以验证操作的运行状况和数据完整性。 

####故障模拟

- [创建新的活动的地理复制链接](sql-database-business-continuity-design.md)从主数据库的辅助测试服务器。 如果主在终止可能发生数据丢失时处于活动状态，这将使钻更逼真。
- 在辅助数据库驻留在测试服务器上的链接的[执行强制终止](sql-database-disaster-recovery.md)。

####恢复

- 更改应用程序配置连接到仅次以前的读这将变为可写入终止后。
- 按照[完成恢复数据库](sql-database-recovered-finalize.md)指南以完成恢复。

####验证

- 通过验证应用程序完整性后恢复 （即连接字符串、 登录、 基本功能测试或验证的其他部分标准应用 signoffs 过程） 完成钻。

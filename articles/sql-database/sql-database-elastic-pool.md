---
ms.openlocfilehash: ff1e38c6c2696028f897d60f957fa1e19e5c27ab
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="有弹性的数据库控制的爆炸式增长" 
    description="SQL Azure 数据库弹性数据库池是由一组弹性数据库共享的可用资源的集合。" 
    services="sql-database" 
    documentationCenter="" 
    authors="stevestein" 
    manager="jeffreyg" 
    editor=""/>

<tags 
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="08/14/2015" 
    ms.author="sstein" 
    ms.workload="data-management" 
    ms.topic="article" 
    ms.tgt_pltfrm="NA"/>


# 有弹性的数据库控制的爆炸式增长

如果您是 SaaS 开发人员具有几十、 上百或甚至上千个数据库，弹性数据库池简化了创建、 维护和管理中的预算，您可以控制这些数据库性能的过程。 

对于每个数据库具有不同的客户，每个都有不同的和不可预知 （DTU 与汇总 CPU/IO/内存） 的资源消耗是一种常见的 SaaS 应用程序模式。 与这些高峰值和低谷值对每个数据库的需求，它可能很难预测，因此调配资源。 正面临着两种选择;或者是过度调配资源使用高峰期，所基于的数据库资源，overpay。 或未得到充分的规定，以节省成本，牺牲在高峰期的性能和客户满意度。 

Microsoft 创建弹性数据库池来帮助您解决此问题。

> [AZURE.VIDEO elastic-databases-helps-saas-developers-tame-explosive-growth]


弹性的数据库池提供解决方案的客户的需要确保他们的数据库获得所需，在需要的时候，同时还提供了一种简单的资源分配机制和可预测的预算的性能资源。 因为在池中的每个数据库使用从共享集与池相关联的 eDTUs 弹性数据库池内的单个数据库按需性能扩展是可能的。 这样下重负荷消耗的详细信息以满足需求，而在轻负载下的数据库占用较少，并在无负载情况下的数据库不会消耗任何 eDTUs, 的数据库。 资源调配资源池，而不是单个数据库不仅简化多个数据库的管理，也有可预测预算，否则为不可预测的工作负载。 

如果需要更多的 eDTUs 以适应 （其他数据库添加到池或现有的数据库开始，使用多个 eDTUs） 池的需求，额外的 eDTUs 可以添加到现有池没有数据库停机时间或在数据库上的负面影响。 同样，如果不再需要额外的 eDTUs 他们可从任意点处的现有池的时间。  

![数据库共享 eDTUs][1]

通常是非常适用弹性数据库池的数据库有活动和其他活动的时间。 请考虑上面的例子中可以看到单个数据库、 4 数据库和最后一个弹性数据库池，其中 20 个数据库的活动。 与各种活动随着时间的推移这些数据库是非常适用弹性数据库池，因为它们不是所有活动的同时，可以共享 eDTUs。 并不是所有的数据库都适合这种模式。 有了更加稳定的资源需求的数据库，这些数据库是更好地适合于基本、 标准和特优服务层级分别分配资源。 以便帮助确定如果在弹性数据库池有益您的数据库，请参阅[弹性数据库池的价格和性能注意事项](sql-database-elastic-pool-guidance.md)。

在几分钟内使用 Microsoft Azure 门户、 PowerShell，或 C#，您可以创建一个弹性的数据库池。 有关详细信息，请参阅[创建并管理弹性的数据库池](sql-database-elastic-pool-portal.md)。 关于弹性数据库池，包括 API 和错误的详细信息，详细信息请参阅[弹性数据库池引用](sql-database-elastic-pool-reference.md)。


> [AZURE.NOTE] 弹性的数据库池目前在预览中，唯一可用的 SQL 数据库 V12 服务器。

## 方便地管理大量数据库与弹性数据库工具

除了提供更高效的资源利用率和可预知的性能，弹性数据库池使 SaaS 应用程序的开发工具，用于简化构建和管理您的数据层，更容易。 执行维护任务，并实现跨大量的数据库更改，一个以往耗时且复杂的过程，已减至弹性作业中运行脚本。 创建并运行一个弹性数据库作业的能力消除了大多数所有繁重的工作与管理成百上千的数据库相关联。 关于弹性数据库作业服务，使池中的所有弹性数据库运行的事务处理 SQL 脚本的信息，请参阅[弹性数据库作业概述](sql-database-elastic-jobs-overview.md)。

丰富且功能强大的开发工具，用于实施弹性的数据库应用程序模式套也是可用的。 对于 sharded 数据库，拆分合并工具等其他工具使您能够将数据从一个 shard 拆分和合并到另一个。 这大大降低了管理大规模的 sharded 数据库的工作。 有关详细信息，请参阅[弹性数据库工具的主题地图](sql-database-elastic-scale-documentation-map.md)。

## 业务连续性的数据库功能的池中

目前在预览中，弹性数据库支持大多数[的业务连续性功能](https://msdn.microsoft.com/library/azure/hh852669.aspx)可用的 V12 服务器上的单个数据库。

### 备份和还原数据库 （[在时间还原点](https://msdn.microsoft.com/library/azure/hh852669.aspx#BKMK_PITR)）

中的弹性数据库池的数据库会自动备份系统和备份保留策略等同于单个数据库的相应服务层。 更具体地说，弹性基本池中可以将数据库还原到任何还原点过去 7 天内、 弹性在标准池可以将数据库还原到任何还原点在过去的 14 天内和弹性特优池中可以将数据库还原到任何还原点过去 35 天内。 在预览时，将池中的数据库还原到新数据库中相同的池。 到这一服务层的最低性能级别始终为拖放的数据库还原为池之外的独立数据库。 例如，将为 S0 数据库恢复弹性数据库中被删除的标准池。 您可以执行数据库还原操作通过 Azure 门户或使用 REST API 以编程方式。 PowerShell cmdlet 支持即将登场。

### [地区恢复](https://msdn.microsoft.com/library/azure/hh852669.aspx#BKMK_GEO)

地区恢复允许您恢复一个池到不同区域中的服务器中的数据库。 在预览时，要还原的数据库在池中，在不同的服务器上，目标服务器需要具有源池具有相同名称的池。 如果需要请在目标服务器上创建一个新池，并为其指定相同的名称还原数据库。 如果池具有相同名称的目标服务器上不存在地理还原操作将失败。
您还可以使用 Azure 门户或 REST API 的 Geo 还原操作。 PowerShell cmdlet 支持即将登场。


### [Geo 复制](https://msdn.microsoft.com/library/azure/dn783447.aspx)

数据库已经包含 Geo 复制启用可以移出弹性数据库池和复制将继续一如既往的工作。 您可以启用地理复制上已经处于一个池，如果您指定的目标服务器与源池具有一个池具有相同名称的数据库。 

### 导入和导出

支持出口中的一个数据库池内。 目前，不支持的数据库的导入直接入池，但可以导入到一个数据库，然后将数据库移动到一个池。 


<!--Image references-->
[1]: ./media/sql-database-elastic-pool/databases.png

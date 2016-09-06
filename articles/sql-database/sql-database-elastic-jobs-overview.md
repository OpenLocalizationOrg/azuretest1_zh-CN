---
ms.openlocfilehash: 14c147c4fe0c6ba57760140ab2a7db8112b17d54
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    title="Elastic database jobs overview" 
    pageTitle="弹性数据库作业概述" 
    description="说明了弹性数据库作业服务" 
    metaKeywords="azure sql database elastic databases" 
    services="sql-database" documentationCenter=""  
    manager="jeffreyg" 
    authors="sidneyh"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/21/2015" 
    ms.author="ddove; sidneyh" />

# 弹性数据库作业概述

TThe**弹性数据库作业**功能 （预览） 可以可靠地执行事务处理 SQL (T-SQL) 脚本或一组数据库包括[弹性数据库池 （预览）](sql-database-elastic-pool.md)或 shard 集 （用[弹性数据库客户端库](sql-database-elastic-database-client-library.md)创建） 中的自定义集合的数据库，所有数据库在应用 DACPAC。 在预览中，**弹性数据库作业**目前客户承载 Azure 云服务使特别和调度的管理任务，被称为作业的执行。 使用此功能，您可以轻松可靠地管理 SQL Azure 数据库大规模跨数据库的整个一组通过运行事务处理 SQL 脚本，以执行管理操作，例如，架构更改、 凭据管理、 引用数据更新、 性能数据收集或租户 （客户） 遥测集。 通常情况下，您必须连接到每个数据库单独来运行事务处理 SQL 语句或执行其他管理任务。 **弹性数据库作业**处理登录，并可靠地记录的每个数据库执行状态时运行该脚本，这一任务。 有关安装说明，请转到[安装弹性数据库作业组件](sql-database-elastic-jobs-service-installation.md)。

![弹性数据库作业服务][1]

## 优点
* 定义 Azure SQL 数据库的自定义组
* 定义、 维护和保持执行跨一组 Azure SQL 数据库事务处理 SQL 脚本 
* 部署数据层应用程序 (DACPAC)
* 执行事务处理 SQL 脚本或应用程序的 DACPACs 可靠地与自动重试和大规模
* 跟踪作业执行状态
* 定义执行计划
* 在 Azure 将结果保存到单个目标表的 SQL 数据库的集合执行数据收集

> [AZURE.NOTE] Azure 门户曲面中的**弹性数据库作业**功能与 SQL Azure 弹性池上设置，也降低了的功能有限。 使用 PowerShell Api 来访问当前功能的完整集。

## 方案

* 性能管理任务，例如，部署新的架构
* 更新引用数据，例如产品信息通用甚至使用日程安排自动更新每个工作日工作时间以外的所有数据库。
* 重新生成索引以提高查询性能。 可以配置重建如在非高峰时间执行跨在定期的基础上的数据库的集合。
* 查询结果从收集一组数据库向中央表在日常的基础上。 性能查询可以不断执行和配置为触发器要执行的其他任务。
* 通过大量的数据库，例如客户遥测的集合执行更长时间运行的数据处理查询。 结果收集到单个目标表中以便进一步分析。

## 简单的弹性数据库作业的端到端检查
1.  将**弹性数据库作业**组件安装。 有关详细信息，请参阅[安装弹性数据库作业](sql-database-elastic-jobs-service-installation.md)。 如果安装失败，请参阅[如何卸载](sql-database-elastic-jobs-uninstall.md)。
2.  使用 PowerShell Api 来访问更多的功能，例如创建自定义的数据库集合，添加计划和/或收集的结果集。 使用简单安装和创建/监视限于对**弹性数据库池**执行的作业的门户。 
3.  创建用于执行作业和[添加到组中的每个数据库用户 （或角色）](sql-database-elastic-jobs-add-logins-to-dbs.md)的加密的凭据。
4.  创建幂等 T SQL 脚本，可以对组中的每个数据库运行。
5.  请按照以下步骤运行该脚本︰[创建和管理弹性数据库作业](sql-database-elastic-jobs-create-and-manage.md) 

## 组件和定价 
以下组件协同工作，以创建 Azure 云服务，使临时执行管理作业。 组件安装，请在您的订购安装过程中自动配置。 因为他们都有同一个自动生成的名称，您可以标识服务。 该名称是唯一的并且包含前缀"edj"21 随机生成的字符后跟。

* **Azure 云服务**︰ 弹性数据库作业 （预览） 作为客户承载 Azure 云服务来执行所需任务的执行。 从门户，服务部署和托管 Microsoft Azure 订阅中。 默认部署服务运行以获得高可用性的两个工作者角色的最小值。 在 A0 实例上运行每个工作者角色 (ElasticDatabaseJobWorker) 的默认大小。 定价，请参阅[云服务定价](http://azure.microsoft.com/pricing/details/cloud-services/)。 
* **SQL azure 数据库**︰ 该服务使用称为**控制数据库**Azure SQL 数据库来存储所有的作业元数据。 默认服务层是 S0。 定价，请参阅[SQL 数据库定价](http://azure.microsoft.com/pricing/details/sql-database/)。
* **Azure 服务总线**︰ Azure 服务总线是 Azure 云服务工作的协调。 请参阅[服务总线定价](http://azure.microsoft.com/pricing/details/service-bus/)。
* **Azure 存储**︰ 使用 Azure 存储帐户存储诊断输出记录的问题需要进行进一步调试 （ [Azure 诊断](../cloud-services-dotnet-diagnostics.md)的常用做法）。 定价，请参阅[Azure 存储定价](http://azure.microsoft.com/pricing/details/storage/)。

## 弹性数据库作业的工作原理
1.  Azure SQL 数据库指定一个管理数据库，它存储了所有的元数据和状态数据。
2.  通过**弹性数据库作业**启动和跟踪作业来执行访问控制数据库。
3.  与控制数据库通信的两个不同的角色︰ 
    * 控制器︰ 确定哪些作业需要通过创建新的工作任务执行请求的作业，然后重试失败的作业任务。
    * 作业任务执行︰ 执行作业任务。

### 作业任务类型
有多种类型的工作任务，执行作业的执行︰

* ShardMapRefresh︰ 查询 shard 地图来确定为 shards 所使用的全部数据库
* ScriptSplit︰ 拆分跨 '转' 语句批处理脚本
* ExpandJob︰ 从作业为目标的一组数据库创建子作业的每个数据库
* ScriptExecution︰ 执行对特定使用定义的凭据数据库脚本
* Dacpac︰ 将 DACPAC 应用到特定的数据库，使用特定的凭据

## 端到端作业执行的工作流
1.  使用门户或 PowerShell API，作业被插入到**管理数据库**。 该作业请求针对一组使用特定凭据的数据库事务处理 SQL 脚本的执行。
2.  该控制器标识新的作业。 创建和执行可将该脚本拆分并刷新组的数据库的工作任务。 最后，创建并执行展开作业，作业其中各子作业指定的组中执行针对单个数据库的事务处理 SQL 脚本创建新子一份新工作。
3.  该控制器标识创建的子作业。 对于每个作业，控制器创建并触发针对数据库执行脚本的作业任务。 
4.  完成所有的作业任务后，控制器更新作业完成状态。 在作业执行期间的任何时候，PowerShell API 可以用于查看执行作业的当前状态。 在 UTC 表示 PowerShell Api 返回的所有时间。 如果需要，可以启动取消请求停止作业。 

## 下一步行动
[安装这些组件](sql-database-elastic-jobs-service-installation.md)，然后[创建并添加到组中的数据库的每个数据库的日志中](sql-database-elastic-jobs-add-logins-to-dbs.md)。 若要进一步了解作业的创建和管理，请参阅[创建和管理弹性数据库作业](sql-database-elastic-jobs-create-and-manage.md)。

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-overview/elastic-jobs.png
<!--anchors-->

 
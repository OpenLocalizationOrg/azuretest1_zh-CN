---
ms.openlocfilehash: 3d4e849698ab5dd3fead64d823fe8012f49938c5
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="针对业务连续性的 SQL 数据库设计" 
   description="指导您选择在此部分，将提供指导的如何选择哪些 BCDR 应使用功能和时。 这将包括什么您可以自动获得使用 SQL 数据库的说明。"
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

#业务连续性的设计

设计应用程序的业务连续性要求您回答以下问题︰

1. 哪种业务连续性功能是适合我的应用程序防止停机？
2. 是否使用什么级别的冗余和复制拓扑？

##何时使用地理还原

默认情况下，SQL 数据库提供内置的基本保护的每个数据库。 它是通过将数据库备份存储在地理冗余的 Azure 存储 (GRS)。 如果您选择此方法，任何特殊的配置或其他资源分配是必需的。 使用这些备份，您可以恢复您的数据库中使用地理 Restore 命令任何地区。 [从停机中恢复](sql-database-disaster-recovery.md)部分地区恢复用于恢复应用程序的详细信息，请使用 

如果您的应用程序满足以下条件，则应使用内置的保护︰

1. 它不被视为使命关键。 没有绑定 SLA 因此为 24 小时或更长时间的停机时间不会导致财务责任。
2. 数据变化率较低 （如每小时的事务）。 1 小时的 RPO 不会导致大量数据丢失。
3. 应用程序成本敏感并不能证明其他地区复制的成本 

> [AZURE.NOTE] Geo 还原不会预先分配任何特定地区的计算能力，在停用期间，从备份中还原活动数据库。 该服务将管理与方式对该区域中的现有数据库的影响减到最小的地区恢复请求的工作负荷和其产能需求将具有的优先级。 因此，数据库的恢复时间将取决于多少个其他数据库将同时恢复在同一区域。 

##何时使用地区复制

Geo 复制创建在不同的地区，从您的主要副本数据库 （次）。 确保您的数据库将具有所需的数据和计算资源来支持恢复后应用程序的工作负载。 请参阅[从停机中恢复](sql-database-disaster-recovery.md)为使用故障转移恢复应用程序的部分。

如果您的应用程序满足以下条件，则应使用地区复制︰

1. 它是一项关键。 它具有与大胆的 RPO 和 RTO SLA 绑定。 可用性和数据的丢失将导致的财政责任。 
2. 数据更改率较高 （例如事务每分钟或秒为单位）。 与默认保护很可能会导致不可接受的数据丢失的 1 小时的 RPO。
3. 与使用地区复制相关的成本将大大降低于潜在的财务责任和相关的业务损失。

> [AZURE.NOTE] 如果您的应用程序使用基本层不支持数据库 Geo-Repliation

##何时选择标准与活动地区复制

标准层数据库没有使用活动的地区复制，因此如果您的应用程序使用标准的数据库，并且满足上述条件则应启用标准地区复制的选项。 另一方面，高级的数据库可以选择选项。 标准的 Geo 复制被设计为更简单、 成本较低的灾难恢复解决方案，特别适合于应用程序使用该方法来防止计划外停机事件。 与标准 Geo-复制只能使用 DR 成对的地区恢复和有创建多个次映像的能力。 这后一项功能是关键的应用程序的升级方案。 因此如果这种情况下对于您的应用程序至关重要，您应启用活动的地理复制。 请参阅[升级应用程序，而无需停机](sql-database-business-continuity-application-upgrade.md)有关其他详细信息。 

> [AZURE.NOTE] 活动的各地区复制还支持从而提供只读的工作负载增加容量辅助数据库的只读访问权限。 

##如何启用地理复制

您可以启用 Geo-Replicatiom 使用 Azure 门户或通过调用 REST API 或 PowerShell 命令。

###Azure 门户

[AZURE.VIDEO sql-database-enable-geo-replication-in-azure-portal]

1. 登录到[Azure 门户](https://portal.Azure.com)
2. 在屏幕的左侧选择**浏览**，然后选择**SQL 数据库**
3. 导航到您的数据库刀片式服务器，选择的**地区复制映射**，单击**配置地区复制**。
4. 导航到地区复制刀片式服务器。 选择的目标地区。 
5. 导航到创建辅助刀片式服务器。 在目标区域中选择现有的服务器，或新建一个。
6. 选择辅助类型 （*可读*或*未读*）
7. 单击**创建**以完成配置

> [AZURE.NOTE] Geo 复制刀片式服务器上的灾难恢复配对的区域将被标记为*建议*。 如果您使用特优层数据库，您可以选择不同的地区。 如果您使用的标准的数据库不能更改它。 高级数据库将具有 （*可读*或*未读*） 的辅助类型的一个选择。 只能选择一个*非读*辅助标准数据库。


###PowerShell

使用[启动 AzureSqlDatabaseCopy](https://msdn.microsoft.com/library/dn720220.aspx) PowerShell cmdlet 自动 Geo 复制配置。

若要创建与津贴或标准数据库的不可读第二地区复制︰
        
        Start-AzureSqlDatabaseCopy -ServerName "SecondaryServerName" -DatabaseName "SecondaryDatabaseName" -PartnerServer "PartnerServerName" –ContinuousCopy -OfflineSecondary
若要创建与高级数据库的可读第二地区复制︰

        Start-AzureSqlDatabaseCopy -ServerName "SecondaryServerName" -DatabaseName "SecondaryDatabaseName" -PartnerServer "PartnerServerName" –ContinuousCopy
         
此命令是异步的。 在其之后返回使用[Get AzureSqlDatabaseCopy](https://msdn.microsoft.com/library/dn720235.aspx) cmdlet 检查此操作的状态。 在操作完成时，所返回对象的 ReplicationState 字段将具有 CATCH_UP 的值。

        Get-AzureSqlDatabaseCopy -ServerName "PrimaryServerName" -DatabaseName "PrimaryDatabaseName" -PartnerServer "SecondaryServerName"


###REST API， 

使用[启动数据库副本](https://msdn.microsoft.com/library/azure/dn509576.aspx)API 以编程方式创建一个 Geo 复制配置。

此 API 是异步的。 其返回后使用[获取数据库副本](https://msdn.microsoft.com/library/azure/dn509570.aspx)API 复选此操作的状态。 在操作完成时，响应正文的 ReplicationState 字段将具有 CATCH_UP 的值。


##如何选择故障转移配置 

设计您的业务连续性的应用程序时应考虑若干配置选项。 选择将取决于应用程序部署的拓扑结构和应用程序的哪些部分是最容易受到中断。 请参阅[设计云解决方案用于灾难恢复使用活动地区-复制](https://msdn.microsoft.com/library/azure/dn741328.aspx)的指南。


 

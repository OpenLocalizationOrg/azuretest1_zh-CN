---
ms.openlocfilehash: 6e7ab2f09cc8749f2632376627e7f51578ee0b26
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="SQL Server 在 Azure 的虚拟机的备份和恢复"
    description="介绍了 Azure 虚拟机上运行的 SQL Server 数据库的备份和恢复注意事项。"
    services="virtual-machines"
    documentationCenter="na"
    authors="rothja"
    manager="jeffreyg"
    editor="monicar" />

<tags 
    ms.service="virtual-machines"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/05/2015"
    ms.author="jroth" />

# SQL Server 在 Azure 的虚拟机的备份和恢复

## 概述

备份 SQL Server 数据库中的数据是由于应用程序或用户错误而导致的数据丢失保护战略的重要组成部分。 这是 SQL Server 运行在 Azure 虚拟机 (Vm) 同样如此。

对于在 Azure 的虚拟机中运行的 SQL Server，可以使用本机备份和还原技术使用附加的磁盘进行备份文件的目的地。 但是，是可以附加到 Azure 的虚拟机，根据[大小的虚拟机](virtual-machines-size-specs.md)的磁盘数限制。 也是考虑的磁盘管理的开销。

从 SQL Server 2014年开始，您可以备份和还原到 Microsoft Azure Blob 存储。 SQL Server 2016年还提供了有关此选项的增强功能。 此外，对于存储在 Microsoft Azure Blob 存储的数据库文件，SQL Server 2016年提供一个选项用于几乎瞬间即可备份和快速恢复使用 Azure 的快照。 本文将概述这些选项，并可以在[SQL Server 备份和还原与 Microsoft Azure Blob 存储服务](https://msdn.microsoft.com/library/jj919148(v=sql.130).aspx)找到其他信息。

>[AZURE.NOTE] 非常大的数据库进行备份的选项的讨论，请参阅[多兆兆字节 SQL Server 数据库备份策略的 Azure 的虚拟机](http://blogs.msdn.com/b/igorpag/archive/2015/07/28/multi-terabyte-sql-server-database-backup-strategies-for-azure-virtual-machines.aspx)。

以下各节包含特定于 Azure 的虚拟机中支持的 SQL Server 的不同版本的信息。

## 备份注意事项时数据库文件存储在 Microsoft Azure Blob 服务

数据库文件存储在 Microsoft Azure Blob 存储时，将更改执行数据库备份和基础备份技术本身的原因。 在 Azure blob 存储中存储数据库文件的详细信息，请参阅[在 Azure 中的 SQL Server 数据文件](https://msdn.microsoft.com/library/jj919148.aspx)。

- 您不再需要执行数据库备份，以提供对硬件或介质故障的保护，因为 Microsoft Azure 提供这种保护作为 Microsoft Azure 服务的一部分。

- 您仍需要执行数据库备份，以提供保护防止用户错误或存档目的、 法规方面的原因，或管理目的。

- 您还可以几乎即时的备份和快速恢复 Microsoft SQL Server 2016年社区技术预览 2 (CTP2) 中使用的 SQL Server 文件快照备份功能。 有关详细信息，请参阅[Azure 中的数据库文件的文件快照备份](https://msdn.microsoft.com/library/mt169363.aspx)。

## 备份和还原在 Microsoft SQL Server 2016年社区技术预览 (CTP2) 2

Microsoft SQL Server 2016年社区技术预览 2 (CTP2) 支持[备份和恢复使用 Azure blob](https://msdn.microsoft.com/library/jj919148.aspx)功能在 SQL Server 2014年发现，如下所述。 但是，它还包括了以下增强功能︰

- **条带化**︰ SQL Server 2016年到 Microsoft Azure blob 存储备份，当支持备份到多个 blob 以启用备份大型数据库、 12.8 t B 的最多。

- **快照备份**︰ 通过使用 Azure 快照，SQL Server 文件快照备份提供几乎即时的备份和快速恢复使用 Azure Blob 存储服务存储的数据库文件。 此功能使您能够简化您的备份和还原策略。 文件快照备份还支持时间还原点。 有关详细信息，请参阅[在 Azure 中的数据库文件的快照备份](https://msdn.microsoft.com/library/mt169363%28v=sql.130%29.aspx)。

- **管理备份的日程安排**︰ SQL Server 管理备份到 Azure 现在支持自定义日程安排。 有关详细信息，请参阅[SQL Server 管理备份到 Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx)。

>[AZURE.NOTE] 有关 SQL Server 2016 时使用 Azure Blob 存储功能的教程，请参见[教程︰ Microsoft Azure Blob 存储服务使用 SQL Server 2016年数据库](https://msdn.microsoft.com/library/dn466438.aspx)。

## 备份和还原 SQL Server 2014

SQL Server 2014年包括以下增强功能︰

1. **备份和恢复到 Azure**:

 - 现在， *url 的 SQL Server 备份*SQL Server 管理 Studio 中拥有支持。 在 SQL Server 管理 Studio 中使用备份或还原任务或维护计划向导时，备份到 Azure 选项现已推出。 有关详细信息，请参阅[SQL Server 备份到的 URL](https://msdn.microsoft.com/library/jj919148%28v=sql.120%29.aspx)。
 - *SQL Server 管理备份到 Azure*具有新的功能，使自动化的备份管理。 这是特别适合自动化备份管理 Azure 机器上运行的 SQL Server 2014年实例。 有关详细信息，请参阅[SQL Server 管理备份到 Microsoft Azure](https://msdn.microsoft.com/library/dn449496%28v=sql.120%29.aspx)。
 - *自动备份*提供了其他自动化所有现有的和新的数据库为 SQL Server 虚拟机在 Azure 中自动启用*SQL Server 管理备份到 Azure* 。 有关详细信息，请参阅[SQL Server 在 Azure 的虚拟机的自动备份](virtual-machines-sql-server-automated-backup.md)。
 - SQL Server 2014年备份到 Azure 的所有选项的概述，请参阅[SQL Server 备份和还原与 Microsoft Azure Blob 存储服务](https://msdn.microsoft.com/library/jj919148%28v=sql.120%29.aspx)。

1. **加密**︰ SQL Server 2014年支持创建备份时对数据进行加密。 它支持多个加密算法和使用 osf 证书或非对称密钥。 有关详细信息，请参阅[备份加密](https://msdn.microsoft.com/library/dn449489%28v=sql.120%29.aspx)。

## 备份和还原在 SQL Server 2012

有关 SQL Server 备份和还原 SQL Server 2012年中的详细信息，请参阅[备份和还原的 SQL Server 数据库 (SQL Server 2012)](https://msdn.microsoft.com/library/ms187048%28v=sql.110%29.aspx)。

启动 SQL Server 2012 SP1 累积更新 2 中，您可以备份到并恢复从 Azure Blob 存储服务。 此增强功能可用于在 Azure 的虚拟机或内部部署的实例上运行的 SQL Server 备份 SQL Server 数据库。 有关详细信息，请参阅[SQL Server 备份和还原与 Azure Blob 存储服务](https://msdn.microsoft.com/library/jj919148%28v=sql.110%29.aspx)。

一些使用 Azure Blob 存储服务的好处包括︰ 能够绕过 16 个磁盘的限制连接的磁盘，易于管理，直接到 Azure 的虚拟机上运行的 SQL Server 实例的另一个实例或内部实例迁移或灾难恢复的备份文件的可用性。 使用 Azure blob 存储服务的 SQL Server 备份的好处的完整列表，请参阅[SQL Server 备份和还原与 Azure Blob 存储服务](https://msdn.microsoft.com/library/jj919148%28v=sql.110%29.aspx)的*好处*一节。

有关建议的最佳做法和疑难解答信息，请参阅[备份和还原的最佳做法 （Azure Blob 存储服务）](https://msdn.microsoft.com/library/jj919149%28v=sql.110%29.aspx)。

## 备份和还原在其他版本的 SQL Server 支持 Azure 虚拟机

对于 SQL Server 备份和还原 SQL Server 2008 R2 中的，请参阅[备份和还原数据库 SQL Server (SQL Server 2008 R2)](https://msdn.microsoft.com/library/ms187048%28v=sql.105%29.aspx)。

对于 SQL Server 备份和还原 SQL Server 2008年中的，请参阅[备份和还原数据库 SQL Server (SQL Server 2008)](https://msdn.microsoft.com/library/ms187048%28v=sql.100%29.aspx)。

## 下一步行动

如果您仍计划在 Azure VM 中的 SQL Server 的部署，您可以在下面的教程找到提供指导︰[在 Azure 上一个 SQL Server 虚拟机的资源调配](virtual-machines-provision-sql-server.md)。

尽管可以使用备份和恢复来迁移数据，有可能更容易到 Azure VM 上的 SQL Server 的数据迁移路径。 迁移选项和建议的完整讨论，请参阅[迁移到 Azure VM 上的 SQL Server 数据库](virtual-machines-migrate-onpremises-database.md)。

查看其他[运行 SQL Server 在 Azure 的虚拟机的资源](virtual-machines-sql-server-infrastructure-services.md)。

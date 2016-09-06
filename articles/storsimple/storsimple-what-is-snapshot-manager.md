---
ms.openlocfilehash: d18b6a4a32dcbfed4b3661688794121c86801f6d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="StorSimple 快照管理器是什么？ |Microsoft Azure"
   description="介绍 StorSimple 快照管理器，其体系结构，以及它的功能。"
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="adinah"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="07/13/2015"
   ms.author="v-sharos" />

# StorSimple 快照管理器是什么？

## 概述

StorSimple 快照管理器是一个 Microsoft 管理控制台 (MMC) 管理单元接它简化了数据保护和备份管理 Microsoft Azure StorSimple 环境中。 使用 StorSimple 快照管理器中，您可以管理 Microsoft Azure StorSimple 数据在数据中心和云作为一个单一的集成的存储解决方案，因而简化了备份过程并降低了成本。

本概述介绍 StorSimple 快照管理器、 描述其功能，并说明其在 Microsoft Azure StorSimple 中的角色。 

整个 Microsoft Azure StorSimple 系统，包括 SharePoint，StorSimple 设备、 StorSimple 管理器服务，StorSimple 快照管理器和 StorSimple 适配器的概述，请参阅[是 StorSimple？](storsimple-overview.md)和[StorSimple 组件是什么？](storsimple-components.md)。 
 
## StorSimple 快照管理器用途和体系结构

StorSimple 快照管理器提供了一个集中管理控制台，可用于创建一致、 时间点备份的本地和云数据。 例如，可以使用到控制台︰

- 配置、 备份和删除卷。
- 配置卷组，以确保备份的数据是应用程序一致。
- 管理备份策略，以便按预定计划备份数据。
- 创建独立的数据，它可以在云中存储和用于灾难恢复的副本。

使用 StorSimple 快照管理器中，装入的卷，然后将其配置成卷组，通常由应用程序。 StorSimple 快照管理器使用这些卷组生成应用程序一致的备份副本。 （应用程序一致性存在时所有相关的文件和数据库同步和表示的特定点时间的应用程序的真实状态）。 

StorSimple 快照管理器备份所需增量快照捕获自上次备份后的更改窗的体。 因此，备份需要更少的存储，并可以创建和快速恢复。 StorSimple 快照管理器使用 Windows 卷影复制服务 (VSS) 以确保快照捕获应用程序一致的数据。 （详细信息，请转到与 Windows 卷影复制服务部分集成。）使用 StorSimple 快照管理器中，可以创建备份时间表或根据需要采取即时备份。 如果您需要恢复数据从备份、 StorSimple 快照管理器允许您选择从本地目录或云快照。 Azure StorSimple 恢复只需要的数据根据需要在还原操作过程中数据的可用性防止延迟）。

![StorSimple 快照管理器体系结构](./media/storsimple-what-is-snapshot-manager/HCS_SSM_Overview.png)

**图 1: StorSimple 快照管理器体系结构** 

## 多个卷类型的支持

您可以使用 StorSimple 快照管理器来配置和备份以下类型的卷︰ 

- **基本卷**– 基本卷是基本磁盘上的单个分区。 

- **简单卷**– 简单卷是动态卷包含由单一动态磁盘的磁盘空间。 简单卷由磁盘上的单个区域或同一磁盘链接在一起的多个区域组成。 （您可以创建简单卷只能在动态磁盘上。简单卷不是具备容错能力。

- **动态卷**— 卷是在动态磁盘上创建的卷。 动态磁盘使用一个数据库来跟踪在计算机中的动态磁盘上所包含的卷的信息。 

- 在 RAID 1 体系结构上构建**镜像动态卷**使用镜像动态卷。 与 RAID 1 在两个或多个磁盘上，产生一个镜像的集写入相同的数据。 然后可以处理包含所请求的数据的任何磁盘读取的请求。

- **群集共享卷**– 使用群集共享卷 (Csv) 在故障转移群集中的多个节点可以同时读取或写入到同一磁盘。 从一个节点到另一个节点故障转移可以快速，发生而无需更改驱动器所有权或装载、 卸载、 和删除卷。 

>[AZURE.IMPORTANT] 不要混合使用 Csv 和非 Csv 中的同一个快照。 不支持混合使用 Csv 和非 Csv 在快照中。 
 
StorSimple 快照管理器可用于还原整个卷组或克隆单个卷和恢复单个文件。

- [卷和卷组](#volumes-and-volume-groups) 
- [备份类型和备份策略](#backup-types-and-backup-policies) 

有关 StorSimple 快照管理器的功能以及如何使用它们的详细信息，请参阅[StorSimple 快照管理器用户界面](storsimple-use-snapshot-manager.md)。

## 卷和卷组

使用 StorSimple 快照管理器中，创建的卷，然后将其配置为卷组。 

StorSimple 快照管理器使用卷组创建应用程序一致的备份副本。 应用程序一致性存在时所有相关的文件和数据库同步和表示的特定点时间的应用程序的真实状态。 卷组 （这也被称为*一致性组*） 形成基础的备份或还原作业。

>[AZURE.NOTE] 卷组没有卷容器相同。 一个卷容器包含共享一个云存储帐户和其他属性，如加密和带宽消耗的一个或多个卷。 单个卷容器可以包含最多 256 个精简资源调配的 StorSimple 卷。 有关卷容器的详细信息，请转到[管理卷容器](storsimple-manage-volume-containers.md)。 卷组是配置来简化备份操作的卷的集合。 如果选择两个卷，属于不同的卷容器，将其放在单独的卷组中，然后创建该卷组的备份策略，在适当的卷容器，使用适当的存储帐户将备份的每个卷。

## Windows 卷阴影复制服务与集成

StorSimple 快照管理器使用 Windows 卷影复制服务 (VSS) 来捕获应用程序一致的数据。 VSS 通过通信与 vss 的应用程序来协调增量快照的创建有助于保证应用程序一致性。 VSS 可以确保应用程序暂时处于非活动状态，或静止，执行快照时。 

VSS 的 StorSimple 快照管理器实现适用于 SQL Server 和一般的 NTFS 卷。 此过程如下所示︰ 

1. 请求程序，通常是数据管理和保护解决方案 （如 StorSimple 快照管理器） 或备份应用程序，调用 VSS，并要求它从编写软件，在目标应用程序中收集的信息。

2. VSS 联系要检索数据的说明的编写器组件。 编写器返回的数据要备份的说明。 

3. VSS 发出信号要准备备份的应用程序的编写器。 编写器更新事务日志等，通过完成未结交易记录，准备用于备份数据，然后通知 vss。

4. VSS 指示要暂时停止该应用程序的数据存储，请确保在创建卷影副本数据不写入到卷的编写器。 此步骤可确保数据的一致性，并采用不能超过 60 秒。

5. VSS 指示提供程序创建卷影副本。 提供程序，它可以是软件或硬件基于管理当前正在运行，并根据需要创建的卷影副本的卷。 提供程序创建卷影副本，并完成后通知 VSS。

6. 联系，VSS 编写器以通知应用程序可以继续执行 I/O 以及确认 I/O 已成功暂停在卷影复制创建过程。 

7. 如果复制成功，VSS 将返回到请求程序副本的位置。 

8. 如果数据被写入卷影副本的创建时，备份将不一致。 VSS 卷影副本中删除，并通知请求方。 请求程序可以自动重复备份过程或通知管理员，稍后重试。

请参阅下图。

![VSS 进程](./media/storsimple-what-is-snapshot-manager/HCS_SSM_VSS_process.png)

**图 2: Windows 卷影复制服务流程** 

## 备份类型和备份策略

使用 StorSimple 快照管理器中，可以备份数据并将其存储在云本地及。 您可以使用 StorSimple 快照管理器立即备份数据或可用的备份策略来创建自动执行备份的日程安排。 备份策略还使您能够指定多少快照将被保留。 

### 备份类型

您可以使用 StorSimple 快照管理器可以创建以下类型的备份︰

- **本地快照**--本地快照的时间点副本的卷 StorSimple 设备存储的数据。 通常情况下，可以创建这种类型的备份和快速恢复中。 就像一个本地备份副本，您可以使用本地快照。

- **云的快照**— 快照的时间点副本的卷存储在云中的数据的云。 一个云快照相当于复制到不同的站点外存储系统上的快照。 云的快照是灾难恢复情形中特别有用。

### 按需和定时备份

使用 StorSimple 快照管理器中，您可以启动立即创建一个一次性备份，也可以使用备份策略来安排定期备份操作。

备份策略是一组可用于安排定期备份的自动规则。 备份策略允许您定义的频率和拍摄快照的特定卷组的参数。 您可以使用策略来指定开始日期和到期日期、 时间、 频率和保留要求，对这两个本地和云快照。 其定义后立即应用策略。 

您可以使用 StorSimple 快照管理器来配置或重新配置备份策略，必要时。 

配置您创建的每个备份策略的以下信息︰

- **名称**– 所选的备份策略的唯一的名称。

- **类型**-类型的备份策略;本地快照或云快照。

- **卷组**– 所选的备份策略分配给卷组。

- **保留**--备份要保留的副本数。 如果您选中了**所有**的框，所有的备份副本将保留，直到达到最大数量的每个卷的备份副本，此时该策略将失败并产生错误消息。 或者，可以指定备份保留 （1 到 64） 之间的一个数字。

- **日期**– 备份策略的创建日期。

有关配置备份策略的信息，请转到[使用 StorSimple 快照管理器可以创建和管理备份策略](storsimple-snapshot-manager-manage-backup-policies.md)。

### 备份作业的监控和管理

StorSimple 快照管理器用于监视和管理即将到来、 计划和已完成的备份作业。 此外，StorSimple 快照管理器提供最多 64 个已完成的备份的目录。 可以使用该目录以找到并恢复卷或单个文件。 

有关监视备份作业的信息，请转到[使用 StorSimple 快照管理器来查看和管理备份作业](storsimple-snapshot-manager-manage-backup-jobs.md)。


## 下一步行动

[了解更多关于 StorSimple 快照管理器任务和工作流](storsimple-snapshot-manager-admin.md)

[下载 StorSimple 快照管理器](https://www.microsoft.com/download/details.aspx?id=44220)。
---
ms.openlocfilehash: f37fa55af9523e236e9742ce3d76abaa45073314
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="管理虚拟机的可用性 |Microsoft Azure"
    description="了解如何使用多个虚拟机以确保 Azure 应用程序的高可用性。"
    services="virtual-machines"
    documentationCenter=""
    authors="kenazk"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/23/2015"
    ms.author="kenazk"/>

#管理虚拟机的可用性

## 了解计划内与计划外的维护
有两种类型的 Microsoft Azure 平台事件可能会影响您的虚拟机的可用性︰ 维护和非计划的维护计划。

- **计划维护事件**是定期更新由微软对底层 Azure 平台以提高整体可靠性、 性能和您的虚拟机上运行的平台基础结构的安全。 大多数的这些更新程序而不影响您的虚拟机或云服务时执行。 但是，有的实例，这些更新要求您将必需的更新应用到平台基础结构的虚拟机重新启动。

- 当硬件或基础虚拟机的物理基础结构已出现故障以某种方式发生**计划外的维护事件**。 这可能包括本地网络故障、 本地磁盘故障或其他机架级故障。 检测到此类故障时，Azure 平台将自动迁移虚拟机不正常承载状态良好的物理机到虚拟机的物理计算机。 这样的事件很少见，但也可能会导致您的虚拟机重新启动。

## 设计您应用程序的高可用性时遵循的最佳做法
以减少停机时间，因为一个或多个这些事件的影响，我们建议以下的高可用性的虚拟机的最佳做法︰

* [在冗余可用性设置中配置多个虚拟机]
* [为可用性组单独配置每个应用程序层]
* [结合可用性设置负载平衡器]
* [避免可用性组中的单个实例虚拟机]

### 在冗余可用性设置中配置多个虚拟机
若要提供对应用程序冗余，我们建议您组合两个或多个虚拟机的可用性设置。 此配置可确保在计划内或计划外维护事件期间至少一个虚拟机将可用并满足 99.95%Azure SLA。 有关服务级别协议要求的详细信息，请参阅在[服务级别协议要求](../../../support/legal/sla/)的"云服务、 虚拟机和虚拟网络"部分。

在您的可用性设置每个虚拟机分配更新域 (UD) 和故障域 (FD) 基础 Azure 平台。 对于给定的可用性设置，五个非用户配置 UDs 分配来表示虚拟机和可重新启动一次的底层物理硬件组。 五个以上的虚拟机配置中单个可用性设置，第六个虚拟机将被放入同一 UD 作为第一个虚拟机，七中的第二个虚拟机，同一 UD，依此类推。 正在重新启动的 UDs 的顺序不能在计划内维护期间按顺序继续，但只有一个 UD 将重新启动一次。

FDs 定义组的共享通用电源源和网络交换机的虚拟机。 默认情况下，虚拟机配置中您的可用性设置分隔跨两个 FDs 中。 虽然可用性设置在放置您的虚拟机不能防止您的应用程序的操作系统或特定应用程序的故障，但它却限制潜在的物理硬件故障、 网络服务中断或电源中断的影响。

<!--Image reference-->
   ![UD FD 配置](./media/virtual-machines-manage-availability/ud-fd-configuration.png)

>[AZURE.NOTE] 有关说明，请参阅[如何配置虚拟机的可用性设置] []。

### 为可用性组单独配置每个应用程序层
如果您的可用性设置中的虚拟机都几乎相同，用于您的应用程序相同的目的，我们建议您配置设置每一层的应用程序可用性。  如果将两个不同的层放在同一可用性设置中，可以同时重新启动相同的应用程序层中的所有虚拟机。 通过在每一层的可用性设置配置至少两个虚拟机，您保证至少一个虚拟机中每个层都将可用。

例如，您可以将所有虚拟机都放在前端应用程序在单个可用性设置运行 IIS、 Apache，Nginx 等。 请确保仅前端虚拟机放在相同的可用性设置。 同样，请确保仅数据层的虚拟机位于自己的可用性设置，如复制的 SQL Server 虚拟机或 MySQL 的虚拟机中。

<!--Image reference-->
   ![应用程序层](./media/virtual-machines-manage-availability/application-tiers.png)


### 结合可用性设置负载平衡器
结合以获得最大的应用程序恢复能力可用性设置 Azure 负载平衡器。 Azure 负载平衡器分布多个虚拟机之间的通信。 我们标准层的虚拟机，将包括 Azure 负载平衡器。 请注意，不是所有虚拟机层都包括 Azure 负载平衡器。 有关负载平衡您的虚拟机的详细信息，请阅读[负载平衡虚拟机](../load-balance-virtual-machines.md)。

如果没有配置负载平衡器以平衡多个虚拟机之间的通信，所有计划内的维护事件将影响唯一通信服务虚拟机，从而导致停机到应用层。 放置在同一个负载平衡器和可用性设置在同一层的多个虚拟机允许通信量可以持续提供至少一个实例。

### 避免可用性组中的单个实例虚拟机
避免使可用性设置中的单个实例虚拟机本身。 在此配置中的虚拟机不适合 SLA 保证，将面临在 Azure 计划内的维护事件期间的停机时间。 请注意，单个虚拟机实例中可用性设置，还将在多实例虚拟机计划维护通知收到高级电子邮件通知。 

<!-- Link references -->
[在冗余可用性设置中配置多个虚拟机]: #configure-multiple-virtual-machines-in-an-availability-set-for-redundancy
[为可用性组单独配置每个应用程序层]: #configure-each-application-tier-into-separate-availability-sets
[结合可用性设置负载平衡器]: #combine-the-load-balancer-with-availability-sets
[避免可用性组中的单个实例虚拟机]: #avoid-single-instance-virtual-machines-in-availability-sets
[为虚拟机配置设置的可用性的如何]: virtual-machines-how-to-configure-availability.md

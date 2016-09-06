---
ms.openlocfilehash: 4dec4b2badb92a9f29ebc166e2f324cc0c09df18
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

<properties
    pageTitle="设置与一个 VMM 服务器保护"
    description="Azure 站点恢复协调的复制、 故障切换和恢复的虚拟机位于内部 VMM 云到 Azure 或辅助的 VMM 云。"
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="backup-recovery"
    ms.date="08/05/2015"
    ms.author="raynew"/>

#  设置与一个 VMM 服务器保护

## 概述

Azure 站点恢复通过协调复制、 故障切换和恢复大量的部署方案中的虚拟机将影响您的业务连续性和灾难恢复 (BCDR) 战略。 有关部署方案的完整列表，请参阅[Azure 站点恢复概述](site-recovery-overview.md)。

如果只有单个的 VMM 服务器可以部署基础结构中到 Azure，云站点恢复复制在 VMM 中的虚拟机，也可以复制单个的 VMM 服务器上的云间。 我们建议您只这样做如果你无法部署两个 VMM 服务器 （每个站点一个），因为故障转移和恢复不是此部署中无缝。 恢复将需要手动故障转移从 VMM 服务器之外 Azure 站点故障恢复控制台 （使用 Hyper-V 管理器控制台中的 Hyper-V 副本）。

您可以设置复制，使用两种方法中的单个 VMM 服务器︰

### 独立的部署

部署独立的 VMM 服务器作为主站点中的虚拟机并将此虚拟机复制到辅助站点使用站点恢复和 Hyper-V 副本。 减少停机时间可以在 VMM 虚拟机上安装 SQL Server。 如果 VMM 使用远程 SQL Server 需要恢复的第一次之前恢复 VMM 服务器。

![独立虚拟 VMM 服务器](./media/site-recovery-single-vmm/SingleVMMStandalone.png)

### 群集部署

若要使高度可用的 VMM 可以作为 Windows 故障转移群集中的虚拟机将进行部署。 这是因为它可以确保工作负载的可用性，还能防止正在其运行 VMM 主机的硬件故障转移由 VMM 管理关键工作负载的情况下很有用。 若要部署单个站点恢复 VMM 使用 VMM 服务器虚拟机应部署通过延伸群集跨地理位置上分散的站点。 VMM 使用的 SQL Server 数据库应该用 SQL Server AlwaysOn 可用性组在辅助站点上的副本进行保护。 如果发生灾难时的 VMM 服务器和它对应的 SQL Server 数据库的故障转移，从辅助站点访问。

![群集虚拟的 VMM 服务器](./media/site-recovery-single-vmm/SingleVMMCluster.png)


## 在开始之前

- 演练中的步骤介绍如何用单个独立的 VMM 服务器部署站点恢复。
- 请确保在开始部署之前到位有[先决条件](site-recovery-vmm-to-vmm.md/#before-you-start)。
- 单个的 VMM 服务器必须配置至少两个群。 一个云将作为受保护的云，其他正在执行保护。
- 您想要保护的云彩必须包含以下信息︰
    - 一个或多个 VMM 主机组
    - 每个主机组中的一个或多个 Hyper-V 主机服务器
    - 在每台主机服务器上的一个或多个 Hyper-V 虚拟机

如果您遇到了问题，这种情况下设置[Azure 恢复服务论坛](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr)上张贴您的问题。



## 配置单个服务器部署

1. 如果部署不是 VMM，设有 VMM 在虚拟机上安装 SQL Server 数据库。 阅读[系统要求](https://technet.microsoft.com/library/dn771747.aspx) 
2. 设置在 VMM 服务器上的至少两个群。 了解更多信息，请访问︰

    - [什么是私有云与 System Center 2012 R2 VMM 中的新增功能](http://channel9.msdn.com/Events/TechEd/NorthAmerica/2013/MDC-B357#fbid=)和[VMM 2012](http://www.server-log.com/blog/2011/8/26/vmm-2012-and-the-clouds.html)中的群。 
    - [配置 VMM 云结构](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric)
    - [创建私有云在 VMM](https://technet.microsoft.com/library/jj860425.aspx)和[演练︰ 使用 System Center 2012 SP1 VMM 创建私有云](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx)。
3. 添加源 Hyper-V 主机服务器上您要保护的虚拟机是云到位于您要保护 （源云）。 将目标 Hyper-V 主机服务器添加到需要提供保护的 VMM 服务器上的云。
4. [创建](site-recovery-vmm-to-vmm.md/#step-1-create-a-site-recovery-vault)Azure 站点恢复保险存储并生成存储库的注册密钥。
4. [安装](site-recovery-vmm-to-vmm.md/#step-3-install-the-azure-site-recovery-provider)VMM 服务器和服务器存储库中的注册 Azure 站点恢复服务提供商。 
5. 请确保云出现在站点恢复门户和[云的保护设置](site-recovery-vmm-to-vmm.md/#step-4-configure-cloud-protection-settings)。
    - 在**源位置**和**目标位置**，指定单个的 VMM 服务器的名称。
    - 在**复制方法**，选择**通过网络**进行初始复制因为云彩都位于同一台服务器上。

6. （可选）[配置网络映射](site-recovery-vmm-to-vmm.md/#step-5-configure-network-mapping)︰

    - **源**和**目标**中指定单个的 VMM 服务器的名称。
    - **源网络**中选择虚拟机网络配置为云正在保护。
    - **在目标系统上的网络**中选择虚拟机网络配置为要用于保护云。
    - 两个相同的 VMM 服务器上的虚拟机 (VM) 网络之间可以配置的网络映射。 如果相同的 VMM 网络位于两个不同的站点，您可以映射相同的网络之间。
7. 您想要保护 VMM 云环境中的虚拟机的[启用保护](site-recovery-vmm-to-vmm.md/#step-7-enable-virtual-machine-protection)。 
7. 在 Hyper-V 管理器控制台中，VMM 虚拟机与 Hyper-V 复制副本的复制设置。 VMM 虚拟机不能加入任何 VMM 群。


## 故障切换和恢复

### 创建恢复计划

恢复计划组在一起的虚拟机应故障切换和恢复到一起。 

1. 当**源**中创建一个恢复计划和**目标**指定单个的 VMM 服务器的名称。 在**选择虚拟机**，将显示主云与相关联的虚拟机。
2. 然后，[创建和自定义的恢复计划](https://msdn.microsoft.com/library/azure/dn337331.aspx)。


### 恢复

在发生灾难时可使用下列步骤恢复工作负荷︰

1. 手动故障转移复制副本 VMM 虚拟机到恢复站点从 Hyper-V 管理器控制台。
2. VMM 虚拟机恢复后，登录到 Hyper-V 恢复管理器控制台从门户和到恢复站点从主做计划外故障切换的虚拟机。
3.  计划外故障切换完成后用户可以访问主站点上的所有资源。


 
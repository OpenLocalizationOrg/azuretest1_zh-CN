---
ms.openlocfilehash: 0487886347216422679c4753e4d8bd295154815d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="从 Amazon Web 服务的 Windows 虚拟机迁移到 Microsoft Azure"
    description="使用 Azure 站点恢复迁移 Windows 虚拟机运行在 Amazon Web 服务 (AWA) 到 Azure。"
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
    ms.date="08/26/2015"
    ms.author="raynew"/>

#  迁移到 Azure 的 Windows 虚拟机在 Amazon Web 服务 (AWS)


## 概述

Azure 站点恢复通过协调复制、 故障切换和恢复的中多个部署的虚拟机将影响您的业务连续性和灾难恢复 (BCDR) 战略。 有关部署方案的完整列表，请参阅[Azure 站点恢复概述](site-recovery-overview.md)。

本文介绍如何使用站点恢复迁移或故障转移到 Azure AWS 中运行的 Windows 实例。 文章使用大部分[设置保护之间内部部署 VMware 虚拟机或物理服务器和 Azure](site-recovery-vmware-to-azure.md)中描述的步骤。 我们建议您阅读该文章了解在部署中的每个步骤的详细说明。

## 入门

下面是您之前，您需要启动︰

- **配置服务器**︰ Azure 作为配置服务器的虚拟机。 配置服务器协调内部机和 Azure 服务器之间的通信。
- **主目标服务器**︰ Azure 虚拟机作为主目标服务器。 此服务器接收并保留复制的数据从受保护的计算机。
- **进程内服务器**︰ 运行 Windows Server 2012 R2 的虚拟机。 受保护的虚拟机将复制数据发送到此服务器。
- **EC2 VM 实例**︰ 您要迁移，然后保护的实例。

- 阅读有关这些组件的详细信息[需要什么？](site-recovery-vmware-to-azure.md#what-do-i-need)
- 此外应阅读有关[容量规划](site-recovery-vmware-to-azure.md#capacity-planning)的准则，并确保您具有所有[部署系统必备组件](site-recovery-vmware-to-azure.md#before-you-start)在之前的地方开始。

## 部署步骤

1. [创建存储库](site-recovery-vmware-to-azure.md#step-1-create-a-vault)
2. 作为 Azure VM[部署配置服务器](site-recovery-vmware-to-azure.md#step-2-deploy-a-configuration-server)。
3. [主目标服务器部署](site-recovery-vmware-to-azure.md#step-2-deploy-a-configuration-server)为 Azure VM。
4. [部署进程内服务器](site-recovery-vmware-to-azure.md#step-4-deploy-the-on-premises-process-server)。 请注意︰

    - 应部署在同一子网/Amazon 虚拟私有云 EC2 实例作为进程内的服务器。 
        ![EC2 实例](./media/site-recovery-migrate-aws-to-azure/ASR_AWSMigration1.png)

    - 进程内服务器已经部署后验证它可以传达与要迁移的 EC2 实例。
    - 您想要保护的每个虚拟机都需要安装了移动服务。 此服务将数据发送到进程内服务器。 可以手动安装或推送和进程内服务器已启用虚拟机的防护安装自动移动服务。 应配置您想迁移的 EC2 实例上的防火墙规则允许推送安装此服务。 EC2 实例的安全组应具有下列规则︰

        ![防火墙规则](./media/site-recovery-migrate-aws-to-azure/ASR_AWSMigration2.png)

    - 过程完成后服务器部署和站点恢复存储库中的它应该显示在站点故障恢复控制台中的**配置服务器**选项卡下的配置服务器注册。 这可能需要 15 分钟。
    
        ![进程内服务器](./media/site-recovery-migrate-aws-to-azure/ASR_AWSMigration3.png)

5. [安装最新的更新](site-recovery-vmware-to-azure.md#step-5-install-latest-updates)。 请确保您已安装的所有组件服务器都处于最新状态。
6. [创建保护组](site-recovery-vmware-to-azure.md#step-7-create-a-protection-group)。 按顺序启动保护迁移的实例使用站点恢复到您需要创建保护组。 您指定的一组复制设置，它们将应用于您将添加到该组的所有实例。 
7. [设置虚拟机](site-recovery-vmware-to-azure.md#step-8-set-up-machines-you-want-to-protect)。 您将需要获取 （自动或手动），每个实例上安装了移动服务。
8. [启用虚拟机的保护](site-recovery-vmware-to-azure.md#step-9-enable-protection)。 启用保护实例添加到保护组。 请注意︰

    - 您可以发现您要迁移到 Azure 使用专用的 IP 地址，您可以从控制台 EC2 实例的 EC2 实例。
    -  在创建保护组的选项卡，单击添加计算机 > 物理机 ![EC2 发现](./media/site-recovery-migrate-aws-to-azure/ASR_AWSMigration4.png)
    - 指定的实例的专用 IP 地址。
        - ![EC2 发现](./media/site-recovery-migrate-aws-to-azure/ASR_AWSMigration5.png)
    - 将启用保护，初始复制将运行按照保护组的初始复制设置
9. [运行计划的故障转移](site-recovery-failover.md#run-an-unplanned-failover)。 完成初始复制后可以到 Azure 从 AWS 运行计划的故障转移。 （可选） 您可以创建恢复计划和运行计划外故障切换，将多个虚拟机从 AWS 迁移到 Azure。 [了解更多](site-recovery-create-recovery-plans.md)关于恢复计划。
        
## 下一步行动

在[站点恢复论坛](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)中张贴任何意见或问题



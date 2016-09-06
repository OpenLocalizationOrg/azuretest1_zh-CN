---
ms.openlocfilehash: 7e6d79a4a6dc3feff24ffc298361061de96d8177
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="介绍 Azure 备份 |Microsoft Azure"
    description="本文概述的 Azure 备份服务，从而使备份数据到 Azure 和 Azure 中的客户"
    services="backup"
    documentationCenter=""
    authors="trinadhk"
    manager="shreeshd"
    editor="tysonn"/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/18/2015"
    ms.author="trinadhk"; "jimpark"/>

# Azure 备份简介
本文提供了 Microsoft 的云集成的备份解决方案，使客户备份其数据存在或者内部部署的或在 Azure 中的高级别概述。

## 什么是 Azure 备份？
Azure 的备份是一项多租用 Azure 服务，使您可以备份您的数据提供任何地方︰ 内部部署或在 Azure 中。 它将替换您现有的内部部署或异地备份解决方案的可靠、 安全和经济竞争的云服务。 它还提供保护资产在云中运行的灵活性。 Azure 备份是可伸缩的、 持久的和高度可用的世界类基础结构之上构建。 使用此解决方案，您可以备份数据和应用程序从其系统中心 Data Protection Manager (SCDPM) 服务器、 Windows 服务器，Windows 客户机或 Azure IaaS 的虚拟机。 Azure 备份和 SCDPM 都是构成微软云集成备份解决方案的基本技术。

> [AZURE.VIDEO what-is-azure-backup]

## 设计点云
传统的备份解决方案已向前发展以云视为类似于磁盘或磁带的终结点。 虽然这种方法简单、 易于部署并提供一致的体验，它具有有限的使用并不会不完全利用的基础平台。 这将转换为效率低下、 成本高昂的解决方案对最终客户。 通过将 Azure 作为"只是作为存储终结点"，是无法进入的丰富性和公共云平台强大功能的备份解决方案。 Azure 备份，另一方面，提供真正的服务利用云提供的强大和经济的备份解决方案。 它可以与内部备份解决方案 (SCDPM) 提供的端到端混合解决方案进行集成。

这种方法的优点是︰

- 高效的云存储体系结构，提供了低成本、 可恢复的数据存储
- 非侵入式，自动调整服务的高可用性保证
- 一致的方法来备份部署、 混合和 IaaS 部署

此解决方案的关键功能包括︰

1. **可靠的服务**︰ 可以通过采用 Azure 备份，备份的服务，即高可用性。 服务是多租用和您不需要担心管理底层计算或存储。

2. **可靠的存储**︰ Azure 备份建立在可靠的云存储的后盾较高的 Sla。 您不必担心如何维护存储的资本或操作费用。 此外您还必须备份到 LRS （本地冗余存储） 或 GRS （Geo 复制存储） 存储的选择。 LRS 可以启用 3 个拷贝的数据在同一地理来防止本地硬件故障;GRS 在成对的数据中心提供了 3 个附加拷贝 （共 6 份）。 这可以确保您的备份数据具有高可用性。 即使没有 Azure 站点级灾难，备份数据是与我们的安全。

3. **安全**︰ Azure 的备份数据进行加密源，在传输和存储加密在 Azure。  加密密钥存储在源代码和永远不会传输或存储在 Azure。 恢复数据所需的加密密钥，只有客户服务中有数据的完全访问权限。

4. **异地保护**︰ 而不是支付非现场磁带备份解决方案，客户可以备份到 Azure 的有吸引力的解决方案提供了磁带类似的语义以非常低的成本。

5. **简单性**︰ Azure 的备份提供了熟悉的界面，可以扩展以保护任意规模的部署。  随着服务的发展，集中管理等功能将允许您从一个位置管理您的备份基础架构。

6. **较便宜**︰ 备份 Azure 的价格包括的每个实例备份管理费用和成本的存储 （块斑点价格） 在 Azure 上消耗。  与其他备份基于云的产品种类，不同 Azure 备份不能充电客户执行任何还原操作。 此外，客户不是收取任何出口 （出站） 的数据传输，在还原操作期间费用。

7. **备份中云**︰ Azure 备份提供了基于 VSS 的应用程序一致性备份 Azure IaaS 运行而无需关闭虚拟机的虚拟机。 它还可以备份 Linux 虚拟机在 Azure 中文件系统的一致性。


## 应用程序和工作负载

| 工作负荷 | 源计算机 | Azure 的备份解决方案 |
| --- | --- |---|
| 文件和文件夹 | Windows 服务器，Windows 客户端 | Azure 的备份代理 |
| 文件和文件夹 | Windows 服务器，Windows 客户端 | System Center DPM |
| Hyper-V 虚拟机 (Windows) | Windows 服务器 | System Center DPM |
| Hyper-V 虚拟机 (Linux) | Windows 服务器 | System Center DPM |
| Microsoft SQL Server | Windows 服务器 | System Center DPM |
| Microsoft SharePoint | Windows 服务器 | System Center DPM |
| Microsoft Exchange |  Windows 服务器 | System Center DPM |
| Azure IaaS Vm (Windows)|  - | Azure 的备份 |
| Azure IaaS Vm (Linux) | - | Azure 的备份 |

## 下一步行动
- [请尝试 Azure 的备份](backup-try-azure-backup-in-10-mins.md)
- 常见问题列出 Azure 备份服务上的问题是[这里](backup-azure-backup-faq.md)。
- 访问[备份 Azure 的论坛](http://go.microsoft.com/fwlink/p/?LinkId=290933)。

测试

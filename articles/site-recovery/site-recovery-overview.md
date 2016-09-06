---
ms.openlocfilehash: 33cdefda4f90993cb5fadaa167abcdd1e64aa02e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="站点故障恢复概述" 
    description="Azure 站点恢复协调复制、 故障转移和恢复虚拟机和物理服务器位于内部部署到 Azure 或内部部署辅助站点。" 
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
    ms.workload="storage-backup-recovery" 
    ms.date="08/05/2015" 
    ms.author="raynew"/>

#  站点故障恢复概述

网站恢复服务有助于强健的业务连续性和灾难恢复 (BCDR) 解决方案，保护内部物理服务器和虚拟机通过协调和自动化复制和故障转移到 Azure，或辅助部署数据中心。 

- **简化**的站点恢复有助于通过轻松地配置复制，简化您的 BCDR 策略和运行故障切换和恢复您的内部工作负载和应用程序。
- **复制**-到 Azure 存储，或辅助的数据中心，您可以将复制内部工作负载。 
- **存储库**的管理复制设置选择 Azure 区域中的站点恢复电子仓库。 该区域中的所有元数据仍保留。
- **元数据**的应用程序数据不会发送到 Azure。 网站恢复只需元数据，如虚拟机和 VMM 云名称、 协调复制和故障切换。 
- **连接到 Azure**-这取决于您的部署方案与 Azure 通信管理服务器。 例如，如果您正在复制虚拟机位于内部 VMM 云，VMM 服务器通过出站加密的 HTTPS 连接与站点恢复通信。 未连接，则需要从虚拟机或 Hyper-V 主机。
- **Hyper-V 副本**的 Azure 站点恢复利用 Hyper-V 副本复制过程，并可以同时使用 SAN 复制︰ 如果您正在复制两个内部 VMM 站点。 网站恢复使用智能复制，复制仅数据块和不进行初始复制整个 VHD。 为进行中的复制复制仅增量更改。 网站恢复支持离线数据传输，适用于 WAN 优化程序。
- **定价**-您可以[阅读更多](pricing/details/site-recovery)有关站点恢复定价。


## 部署方案

下表汇总了支持的站点恢复的复制方案。

**复制到** | **复制 （内部部署）** | **详细信息** | **文章**
---|---|---|---
Azure | Hyper-V 站点 | 复制一个或多个内部部署 Hyper-V 主机服务器被定义为对 Azure 的 Hyper-V 网站上的虚拟机。 不需要的 VMM 服务器。 | [阅读更多](site-recovery-hyper-v-site-to-azure.md)
Azure| VMM 服务器 | 复制一个或多个虚拟机内部 VMM 云到 Azure 中的 Hyper-V 主机服务器。 | [阅读更多](site-recovery-vmm-to-azure.md) 
Azure | 物理 Windows 服务器 | 将物理 Windows 或 Linux 服务器复制到 Azure | [阅读更多](site-recovery-vmware-to-azure.md)
Azure | VMware 虚拟机 | 复制到 Azure 的 VMware 虚拟机 | [阅读更多](site-recovery-vmware-to-azure.md)
次要数据中心 | VMM 服务器 | 复制的虚拟机上部署 Hyper-V 主机服务器位于另一个数据中心中的辅助 VMM 服务器到 VMM 云 | [阅读更多](site-recovery-vmm-to-vmm.md)
次要数据中心 | VMM 服务器与 SAN | 复制的虚拟机上部署 Hyper-V 主机服务器位于 VMM 使用 SAN 复制的另一个数据中心中的辅助 VMM 服务器到云| [阅读更多](site-recovery-vmm-san.md)
次要数据中心 | 单个的 VMM 服务器 | 复制的虚拟机上部署 Hyper-V 主机服务器位于相同的 VMM 服务器上的辅助云到 VMM 云 | [阅读更多](site-recovery-single-vmm.md) 


## 工作负荷指南

ASR 复制技术可以在虚拟机中运行的任何应用程序与兼容。 进行进一步的测试为进一步支持应用程序产品团队合作中每个应用程序了。

**工作负荷** | <p>**复制 Hyper-V 虚拟机**</p> <p>**（为辅助站点）**</p> | <p>**复制 Hyper-V 虚拟机**</p> <p>**（到 Azure)**</p> | <p>**复制的 VMware 虚拟机**</p> <p>**（为辅助站点）**</p> | <p>**复制的 VMware 虚拟机**</p><p>**（到 Azure)**</p>
---|---|---|---|---
Active Directory DNS | Y | Y | Y | 即将推出 
Web 应用程序 （IIS、 SQL） | Y | Y | Y | 即将推出
SCOM | Y | Y | Y | 即将推出
Sharepoint | Y | Y | Y | 即将推出
<p>SAP</p><p>对于非群集复制 SAP 站点到 Azure</p> | Y （由 Microsoft 测试） | Y （由 Microsoft 测试） | Y （由 Microsoft 测试） | 即将推出
交换 (非 DAG) | Y | 即将推出 | Y | 即将推出
远程桌面/VDI | Y | Y | Y | 即将推出 
<p>Linux</p> <p>（操作系统和应用程序）</p> | Y （由 Microsoft 测试） | Y （由 Microsoft 测试） | Y （由 Microsoft 测试） | 即将推出 
动态 AX | Y | Y | Y | 即将推出
CRM dynamics | 即将推出 | 即将推出 | Y | 即将推出
Oracle | 即将推出 | 即将推出 | Y （由 Microsoft 测试） | 即将推出


## 功能和要求 

下表汇总了主站点恢复功能以及他们如何处理到 Azure，复制到辅助站点使用默认 Hyper-V 副本复制和使用 SAN 复制过程。

功能|复制到 Azure|复制到辅助站点 （Hyper-V 副本）|复制到辅助站点 (SAN)
---|---|---|---
数据复制|有关内部部署服务器和虚拟机的元数据存储在站点恢复存储库中。</p> <p>在 Azure 存储中存储复制的数据。|有关内部部署服务器和虚拟机的元数据存储在站点恢复存储库中。</p> <p>已复制的数据存储在指定目标 Hyper-V 服务器的位置。|有关内部部署服务器和虚拟机的元数据存储在站点恢复存储库中。</p> <p>已复制的数据都存储在目标阵列存储。
保险存储要求|Azure 站点恢复服务帐户|Azure 站点恢复服务帐户|Azure 站点恢复服务帐户
复制|复制源 Hyper-V 主机到 Azure 存储的虚拟机。 故障恢复到的源位置。|复制源 Hyper-V 主机到目标 Hyper-V 主机的虚拟机。 故障恢复到的源位置。|将虚拟机源 SAN 存储设备复制到目标 SAN 设备。 故障恢复到的源位置。
虚拟机|在 Azure 存储中存储的虚拟机硬盘|存储在 Hyper-V 主机上的虚拟机硬盘|在 SAN 存储阵列上存储的虚拟机硬盘
Azure 存储|需要用来存储复制的虚拟机硬盘|不适用|不适用
SAN 存储阵列|不适用|不适用|SAN 存储阵列的源和目标站点中必须可用，且由 VMM 管理
VMM 服务器|只有在源站点仅 VMM 服务器。|建议使用 VMM 服务器在源站点和目标站点。 您可以复制单个的 VMM 服务器上的云间。|VMM 服务器在源和目标 VMM 站点。 云彩必须包含至少一个 Hyper-V 群集。
VMM 版本|System Center 2012 R2<p>System Center 2012 sp1|System Center 2012 R2|System Center 2012 R2 具有 VMM 更新汇总 5.0
VMM 配置|设置源站点和目标站点中云</p><p>在源网站和目标网站的虚拟机网络设置<p>设置存储在源站点和目标站点的分类 <p>在源上安装的提供程序和目标 VMM 服务器|设置源站点中的云彩</p><p>设置 SAN 存储</p><p>设置源站点中的虚拟机网络</p><p>在源 VMM 服务器安装的提供程序</p><p>启用虚拟机保护|设置源站点和目标站点中云</p><p>在源站点和目标站点中的虚拟机网络设置</p><p>在源和目标的 VMM 服务器安装的提供程序</p><p>启用虚拟机保护
Azure 站点恢复服务提供商</p><p>用于通过站点恢复到 HTTPS 连接|在源 VMM 服务器上安装|安装在源和目标 VMM 服务器|安装在源和目标 VMM 服务器
Azure 恢复服务代理</p><p>用于通过站点恢复到 HTTPS 连接|在 Hyper-V 主机服务器上安装|不需要|不需要
虚拟机的恢复点|按时间设置恢复点。</p> <p>指定多长时间恢复点应保留 （0-24 小时）|设置恢复点的量。</p> <p>指定应保留多少个额外的恢复点 (0-15)。 默认的恢复点创建每小时|在数组存储设置配置
网络映射|将 VM 网络映射到 Azure 的网络。</p> <p>网络映射可确保所有在同一源 VM 网络故障切换的虚拟机可以连接在故障转移之后。 另外如果目标 Azure 网络网关然后虚拟机可以连接到内部部署的虚拟机。 </p><p>如果映射不是故障切换相同的恢复计划中可以相互连接的故障转移到 Azure 后的启用唯一的虚拟机。|将源 VM 网络映射到目标 VM 网络。</p> <p>网络映射用于最佳 Hyper-V 主机的服务器上，将已复制的虚拟机，并确保在故障转移之后与源 VM 网络相关联的虚拟机将与映射的目标网络相关联。 </p><p>如果没有启用映射复制虚拟机不能连接到网络。|将源 VM 网络映射到目标 VM 网络。</p> <p>网络映射可确保在故障转移之后与源 VM 网络相关联的虚拟机将与映射的目标网络相关联。 </p><p>如果没有启用映射复制虚拟机不能连接到网络。
存储映射|不适用|将源 VMM 服务器上的存储分类映射到存储在目标 VMM 服务器上的分类。</p> <p>与映射启用虚拟机源存储分类中的硬盘将位于目标存储分类在故障转移之后。</p><p>如果未启用存储映射复制虚拟硬盘都将存储在目标 Hyper-V 主机服务器上的默认位置。|存储阵列和池在主站点和辅助站点之间的映射。

## 下一步行动

在完成本概述后[阅读的最佳做法](site-recovery-best-practices.md)，以帮助您开始使用部署计划。 
 

---
ms.openlocfilehash: 47e74105d4f146c089395daf33684acbce9924e7
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="了解站点到 Azure 的保护" 
    description="使用本文来了解技术的概念，它可以帮助您成功地安装、 配置和管理 Azure 站点恢复。" 
    services="site-recovery" 
    documentationCenter="" 
    authors="anbacker" 
    manager="mkjain" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery" 
    ms.date="09/01/2015" 
    ms.author="anbacker"/>


# 了解 Hyper-V 或 VMM Azure 保护站点

本文介绍的技术概念从而帮助您成功配置和管理 Hyper-V 站点或 Azure 保护使用 Azure 站点恢复到 VMM 站点。

## 了解组件

### Hyper-V 站点或 VMM 站点部署内部和 Azure 之间的复制。

作为组成部分之间内部和 Azure; 设置灾难恢复Azure 站点恢复提供商需要下载和安装以及 Azure 恢复服务代理，它们需要在每台 Hyper-V 主机上安装 VMM 服务器上。

![对于内部部署和 Azure 之间复制的 VMM 站点部署](media/site-recovery-understanding-site-to-azure-protection/image00.png)

Hyper-V 站点部署是相同 VMM 部署 — 只区别在于提供者和代理获取安装 Hyper-V 主机本身上。

## 了解工作流

### 启用保护
一旦您从门户或内部保护虚拟机，名为*启用保护*ASR 作业将启动并可进行监控的作业选项卡下。 

![内部部署 Hyper-V 问题的疑难解答](media/site-recovery-understanding-site-to-azure-protection/image01.png)

*启用保护*作业会在调用[CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx)创建到 Azure 使用输入在保护过程中配置复制之前检查系统必备组件。 *启用保护*作业启动初始复制从内部通过调用[StartReplication](https://msdn.microsoft.com/library/hh850303.aspx)将虚拟机的虚拟磁盘发送到 Azure。

![内部部署 Hyper-V 问题的疑难解答](media/site-recovery-understanding-site-to-azure-protection/image02.png)

### 最终确定的保护
初始复制触发时拍摄[Hyper-V 虚拟机快照](https://technet.microsoft.com/library/dd560637.aspx)。 虚拟硬盘会逐个处理，直到所有的磁盘都上载到 Azure。 这通常要花费一段时间才能完成基于磁盘大小和带宽。 优化了网络使用率，请[如何管理内部到 Azure 保护网络带宽的使用](https://support.microsoft.com/kb/3056159)。 初始复制完成后*完成保护虚拟机上的*作业配置的网络和复制后设置。 正在进行初始复制时增量复制部分下面所述获取跟踪写入磁盘上的所有更改。 将快照和 HRL 文件消耗额外的磁盘存储，初始复制时正在进行中。 在完成初始复制，Hyper-V 虚拟机快照将被删除的合并数据更改将导致发送初始复制到父磁盘。

![内部部署 Hyper-V 问题的疑难解答](media/site-recovery-understanding-site-to-azure-protection/image03.png)

### 增量复制
Hyper-V 副本复制跟踪，Hyper-V 复制副本的复制引擎跟踪的更改到 Hyper-V 复制日志 (*.hrl) 作为虚拟硬盘的一部分。 HRL 文件将位于自相关联的磁盘位于同一目录中。 为复制配置的每个磁盘将具有关联的 HRL 文件。 此日志已完成初始复制后 （) 发送给客户的存储帐户。 转移到 Azure 日志时，在相同的目录中的另一个日志文件跟踪的主要更改。

初始复制和增量复制过程虚拟机复制状况可以在 VM 视图中已提到在[监视复制虚拟机的运行状况](./site-recovery-monitoring-and-troubleshooting.md#monitor-replication-health-for-virtual-machine)监视。  

### 重新同步 
增量复制失败和完整的初始复制是极大地降低网络带宽或已完成完整的初始复制所花费的时间时，将重新同步标记虚拟机。 例如当 HRL 文件大小成堆达 50%的磁盘总大小，则虚拟机重新同步标记。 重新同步将通过计算的源和目标虚拟机磁盘复选的总和并发送差异仅通过网络发送的数据量降至最低。 

重新同步完成后，应恢复正常增量复制。 在停机时 （例如网络中断、 VMM 故障等），可恢复重新同步。 

默认情况下在非 office 工作时间配置*自动计划重新同步*。 如果虚拟机需要手动重新同步，选择从门户网站，请单击该虚拟机重新同步。
![内部部署 Hyper-V 问题的疑难解答](media/site-recovery-understanding-site-to-azure-protection/image04.png)

重新同步使用 fixed 块分块算法在源文件和目标文件分为固定块;对每个块的校验和生成，并进行比较以确定其正确源必要应用于目标。 

### 重试逻辑
复制错误发生时内置的重试逻辑。 这可以分为两类，如下所示。

| Category                  | 方案                                    |
|---------------------------|----------------------------------------------|
| 不可恢复的错误     | 不能重试将尝试。 虚拟机复制状态将显示为关键，并不需要管理员干预。 示例包括 <ul><li>断链 VHD</li><li>复制副本虚拟机处于无效状态</li><li>网络身份验证错误</li><li>授权错误</li><li>如果在独立 Hyper-V 服务器中找不到虚拟机</li></ul>|
| 可恢复的错误         | 重试出现指数级增长使用延迟，这增加了从一开始的 1、 2、 4 （8，10 分钟） 第一次尝试的重试间隔每个复制时间间隔。 如果错误仍然存在，请重试时间间隔为 30 分钟。 示例包括 <ul><li>网络错误</li><li>磁盘空间不足</li><li>内存不足</li></ul>|

## 了解 Hyper-V 虚拟机保护和恢复生命周期

![了解 Hyper-V 虚拟机保护与恢复生命周期](media/site-recovery-understanding-site-to-azure-protection/image05.png)

## 其他参考

- [监视并排除针对 VMware，VMM，Hyper-V 和物理站点保护](./site-recovery-monitoring-and-troubleshooting.md)
- [交往的 Microsoft 技术支持](./site-recovery-monitoring-and-troubleshooting.md#reaching-out-for-microsoft-support)
- [ASR 的常见错误和及其解决方法](./site-recovery-monitoring-and-troubleshooting.md#common-asr-errors-and-their-resolutions)
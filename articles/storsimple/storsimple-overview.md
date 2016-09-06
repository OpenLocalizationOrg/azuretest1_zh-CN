---
ms.openlocfilehash: bd14617bf20f9ca3e0f9bec0272c94e2a41d6d5f
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="StorSimple 是什么？ |Microsoft Azure" 
   description="介绍了 StorSimple 数据管理和保护过程、 好处和体系结构，并介绍了 StorSimple 组件。" 
   services="storsimple" 
   documentationCenter="NA" 
   authors="SharS" 
   manager="carolz" 
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD" 
   ms.date="08/26/2015"
   ms.author="v-sharos@microsoft.com"/>

# StorSimple 8000 系列︰ 混合云存储解决方案 

## 概述

Microsoft Azure StorSimple 是高效的、 经济的和易于管理的解决方案，以消除的许多问题和与企业存储和数据保护的支出。 它使用专有设备 （Microsoft Azure StorSimple 设备）、 与云服务的集成和一套管理工具提供了无缝的所有企业级存储，包括云存储视图。

StorSimple 使用存储分层管理跨不同的存储介质存储的数据。 当前工作集是存储的内部固态驱动器 (SSDs) 上，较少使用的数据存储在硬盘驱动器 (Hdd) 上存档数据推送到云。 此外，StorSimple 使用重复数据消除和压缩来减少数据占用的存储量。 存储分层过程，如下所示︰

1. 系统管理员设置 Microsoft Azure 云存储帐户。
2. 管理员使用串行控制台和 StorSimple 管理器服务 （在 Azure 管理门户运行） 来配置设备和文件服务器上，创建卷和数据保护策略。 内部文件服务器使用互联网小型计算机系统接口 (iSCSI) 来访问 StorSimple 设备。
3. 最初，StorSimple 快速 SSD 层设备上存储数据。
4. SSD 层接近容量，StorSimple deduplicates，压缩的最旧的数据块，并将它们移动到硬盘的存储层。
5. 作为 HDD 层方法产，StorSimple 最旧的数据块进行加密，并将它们安全地发送到 Microsoft Azure 存储帐户通过 HTTPS。
6. Microsoft Azure 创建数据的多个副本在其数据中心和远程数据中心，确保发生灾难时，可以恢复数据。 
7. 当文件服务器请求数据存储在云中时，StorSimple 将其无缝地返回，SSD 层 StorSimple 设备上存储的副本。

除了存储管理，StorSimple 的数据保护功能使您可以创建按需定时备份，并将其存储在本地或在云中。 窗体中的增量快照，这意味着它们可以被创建并快速恢复执行备份。 因为它们替换辅助存储系统 （如磁带备份），并允许您在必要时还原数据到您的数据中心或备用站点，云快照可以在灾难恢复情形中非常重要。 

>[AZURE.NOTE] StorSimple 8000 系列软件更新 1 或更高版本支持 Amazon S3 与 RR、 HP 和 OpenStack 云服务，以及 Microsoft Azure。  （您需要 Microsoft Azure 存储帐户用于设备管理的目的。）有关详细信息，请参阅[配置新的存储帐户的服务](storsimple-deployment-walkthrough.md#configure-a-new-storage-account-for-the-service)。

## 为什么使用 StorSimple？

Microsoft Azure StorSimple 提供了以下好处︰

- **透明集成**— Microsoft Azure StorSimple 使用 iSCSI 协议以不可见方式链接数据存储设施。 这可确保该数据存储在云数据中心，或在远程服务器上显示存储在单个位置。
- **降低了存储成本**--Microsoft Azure StorSimple 分配足够本地或云存储，以满足当前的需求，并扩展了云存储只有在必要时。 通过消除冗余数据版本的同一个 （重复数据消除），并通过压缩进一步减少了存储需求和支出。
- **简化存储管理**--Microsoft Azure StorSimple 提供了系统管理工具可用于配置和管理存储数据内部，在远程服务器上，并在云中。 此外，您可以管理备份和恢复从 Microsoft 管理控制台 (MMC) 管理单元的功能。 StorSimple 提供一个单独的、 可选的界面，可用于扩展到内容存储在 SharePoint 服务器上的 StorSimple 管理和数据保护的服务。 
- **改进了灾难恢复和法规遵从性**--Microsoft Azure StorSimple 不需要长的恢复时间。 而是根据需要恢复数据。 这意味着正常操作可以继续进行最低程度的中断。 此外，还可以配置要指定备份时间表和数据保留策略。
- 可以从其他站点的恢复和迁移的目的访问**数据移动性**– 上载到 Microsoft Azure 云服务的数据。 此外，StorSimple 可用于在 Microsoft Azure 上运行的虚拟机 (Vm) 上配置 StorSimple 虚拟设备。 虚拟机可以使用虚拟设备访问存储的数据的测试或故障恢复目的。 

下图提供了 Microsoft Azure StorSimple 解决方案的高级视图。

![StorSimple 的体系结构](./media/storsimple-overview/hcs-data-services-storsimple-system-architecture.png)

**Microsoft Azure StorSimple 体系结构**

## StorSimple 组件

此 Microsoft Azure StorSimple 解决方案包括以下组件︰

- **Microsoft Azure StorSimple 设备**— 固态驱动器 (SSDs) 和硬盘驱动器 (Hdd) 都包含冗余控制器和自动故障转移功能以及内部混合存储阵列。 控制器管理存储分层、 当前使用 （或热） 将数据放在本地存储 （在设备或内部服务器中），而较少使用频繁的数据迁移到云。
- **StorSimple 虚拟设备**-也称为 StorSimple 虚拟应用装置，这是复制体系结构的 StorSimple 设备的软件版本和物理混合存储设备的功能。 在 Azure 的虚拟机中的一个节点上运行 StorSimple 的虚拟设备。 虚拟设备是适用于测试和小型试验方案中的使用。 不能在 StorSimple 设备或内部部署服务器上创建 StorSimple 的虚拟设备。
- **StorSimple 的 Windows PowerShell** – 管理 StorSimple 设备您可以使用一个命令行界面。 StorSimple 的 Windows PowerShell 具有功能，允许您注册您的 StorSimple 设备、 在您的设备上配置网络接口、 安装更新的特定类型、 诊断通过访问支持会话中，您的设备和设备状态更改。 通过连接到串行控制台或使用 Windows PowerShell 远程处理，您可以访问 Windows PowerShell 的 StorSimple。
- **Azure PowerShell StorSimple cmdlet** – Windows PowerShell cmdlet，您可以自动执行从命令行的服务级别和迁移任务的集合。 针对 StorSimple Azure PowerShell cmdlet 的详细信息，请转到[cmdlet 参考](https://msdn.microsoft.com/library/dn920427.aspx)。
- **StorSimple 管理器服务**--使您能够从单个 web 界面管理的 StorSimple 设备或虚拟设备的 StorSimple Azure 管理门户的扩展。 StorSimple 管理器服务可用于创建和管理服务、 查看和管理设备、 查看警报、 管理卷，并查看和管理备份策略和备份目录。
- **StorSimple 快照管理器**– 一个 mmc 管理单元，使用卷组和 Windows 卷影复制服务来生成应用程序一致的备份。 此外，您可以使用 StorSimple 快照管理器创建备份时间表和克隆或恢复卷。 
- **SharePoint 的 StorSimple 适配器**-透明地扩展了 Microsoft Azure StorSimple 存储和数据保护到 SharePoint 服务器场中，同时使从 SharePoint 管理门户 StorSimple 存储可查看和管理的工具。

## 下一步行动

了解有关[StorSimple](https://azure.microsoft.com/documentation/services/storsimple/)。

阅读有关[StorSimple 组件和术语](storsimple-components.md)的详细信息。


 

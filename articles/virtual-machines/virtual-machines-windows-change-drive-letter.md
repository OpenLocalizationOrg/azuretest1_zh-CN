---
ms.openlocfilehash: 0af315dc011307ac4f3a4473ef6a257c3618bc15
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="如何更改 Windows 临时磁盘的驱动器号"
    description="描述如何重新映射基于 Windows Azure 中 VM 上的临时磁盘"
    services="virtual-machines"
    documentationCenter=""
    authors="KBDAzure"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/27/2015"
    ms.author="kathydav"/>

#如何更改 Windows 临时磁盘的驱动器号

如果需要使用 D 驱动器来存储数据，请按照这些说明来临时磁盘使用不同的驱动器。 不应使用临时磁盘来存储您需要保留的数据。

在开始之前，您需要连接到虚拟机，以便您可以在此过程中存储 Windows 页面文件 (pagefile.sys) 的数据磁盘。 如果您没有，看到[如何将附加到 Windows 虚拟机的数据磁盘][连接]。 如何找出哪些磁盘连接的说明，请参阅[如何分离从 Windows 虚拟机的数据磁盘][分离]中的"查找磁盘"。

如果您想要使用现有的数据磁盘在驱动器 D 上，请确保您也已上载 VHD 的存储帐户。 有关说明，请参阅步骤 3 和 4 中[创建并上载到 Azure Windows 服务器 VHD][VHD]。

> [AZURE.WARNING] 调整虚拟机并执行，将虚拟机移动到另一台主机，如果临时磁盘更改回 D 驱动器。

##更改驱动器号

1. 登录到虚拟机。 有关详细信息，请参阅[如何登录到运行 Windows 服务器的虚拟机上][登录]。

2. 将 pagefile.sys 从 D 驱动器移到另一个驱动器。

3. 重新启动虚拟机。

4. 再次登录，并更改的驱动器号从 D 到 e。

5. 从[Azure 的门户](http://manage.windowsazure.com)，可以附加现有数据磁盘或空数据磁盘。

6.  再次登录到虚拟机、 初始化该磁盘，并将 D 作为只是附加的磁盘的驱动器号分配。

7.  验证电子映射到临时磁盘。

8.  将 pagefile.sys 从其他驱动器移到 E 驱动器。

## 其他资源
[如何登录到运行 Windows 服务器的虚拟机][登录]

[如何分离从 Windows 虚拟机的数据磁盘][分离]

[关于 Azure 存储帐户][存储]

<!--Link references-->
[附加]: storage-windows-attach-disk.md



[VHD]: virtual-machines-create-upload-vhd-windows-server.md

[登录]: virtual-machines-log-on-windows-server.md

[分离]: storage-windows-detach-disk.md

[Storage]: ../storage-whatis-account.md

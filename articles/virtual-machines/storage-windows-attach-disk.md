---
ms.openlocfilehash: 71269cdc5c0e068059b3c84629df0881fb8a4552
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="附加到虚拟机的磁盘 |Microsoft Azure"
    description="了解如何附加到 Azure 的虚拟机的数据磁盘，并将其初始化，使其可供使用。"
    services="virtual-machines, storage"
    documentationCenter=""
    authors="KBDAzure"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/16/2015"
    ms.author="kathydav"/>

# 如何附加到 Windows 虚拟机的数据磁盘

您可以附加空磁盘和磁盘的数据。 在这两种情况下，磁盘的实际驻留在 Azure 存储帐户的.vhd 文件。 此外在这两种情况下，附加磁盘之后, 您需要对其进行初始化，因此可以使用。

> [AZURE.NOTE] 它是一种最佳做法，使用一个或多个单独的磁盘来存储虚拟机的数据。 Azure 的虚拟机创建时，它具有映射到驱动器 C 的操作系统磁盘和临时磁盘映射到驱动器 D**不使用驱动器 D 来存储数据。** 正如名称所示，驱动器 D 提供临时存储区。 它提供没有冗余或备份，因为它并不驻留在 Azure 存储中。

## 视频的演练

这里是本教程中的步骤的演练。

[AZURE.VIDEO attaching-a-data-disk-to-a-windows-vm] 

[AZURE.INCLUDE [howto-attach-disk-windows-linux](../../includes/howto-attach-disk-windows-linux.md)]

## <a id="initializeinWS"></a>如何︰ 初始化 Windows 服务器中的新数据磁盘

1. 连接到虚拟机。 有关说明，请参阅[如何登录到运行 Windows 服务器的虚拟机上][登录]。

2. 登录到虚拟机后，打开**服务器管理器**。 在左窗格中，选择**文件和存储服务**。

    ![打开服务器管理器](./media/storage-windows-attach-disk/fileandstorageservices.png)

3. 展开菜单，然后选择**磁盘**。

4. **磁盘**部分列出了磁盘 0、 1、 磁盘和磁盘 2。 磁盘 0 是操作系统所在磁盘、 磁盘 1 是临时磁盘 （它不应该使用的数据存储），和磁盘 2 是您附加到虚拟机的数据磁盘。 数据磁盘具有 5 GB，根据附加磁盘所指定的容量。 用鼠标右键单击磁盘 2，并选择**初始化**。

5.  在通知您初始化该磁盘时，所有数据将被都删除。 单击**是**以确认警告并初始化该磁盘。 然后，请再次右键单击磁盘 2 并选择**新的卷**。

6.  完成向导，使用默认值。 完成该向导后，**卷**一节列出的新卷。 磁盘现在处于联机状态并且准备好存储数据。

    ![已成功初始化卷](./media/storage-windows-attach-disk/newvolumecreated.png)

> [AZURE.NOTE] 虚拟机的大小将决定多少磁盘，您可以将附加到它。 有关详细信息，请参阅[虚拟机大小](virtual-machines-size-specs.md)。

## 其他资源

[如何分离从 Windows 虚拟机的磁盘](storage-windows-detach-disk.md)

[有关磁盘和虚拟机的 Vhd](virtual-machines-disks-vhds.md)

[登录]: virtual-machines-log-on-windows-server.md

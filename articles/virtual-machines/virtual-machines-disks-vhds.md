---
ms.openlocfilehash: 284257cda2dea788765c31d6b33545cc0a71cbbf
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="有关磁盘和虚拟机的 Vhd"
    description="了解磁盘和 Vhd 的 Azure 中的虚拟机的基础知识。"
    services="virtual-machines"
    documentationCenter=""
    authors="KBDAzure"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/30/2015"
    ms.author="kathydav"/>

# 有关磁盘和虚拟机的 Vhd

创建虚拟机--一个是操作系统磁盘和另一种是临时的本地磁盘中，有时也称为资源磁盘时，Azure 中的所有虚拟机都配置了至少两个磁盘。 操作系统所在磁盘创建从图像，并且操作系统磁盘和图像实际上虚拟硬盘 (Vhd) 存储在 Azure 存储帐户。 虚拟机还可以具有数据磁盘，并且它们还存储为 Vhd。

>[AZURE.WARNING] 不在临时磁盘上存储数据。 它提供临时存储应用程序和过程，并打算只存储数据，如网页或交换文件。 若要重新映射该磁盘的 Windows 虚拟机，请参阅[更改 Windows 临时磁盘的驱动器号](virtual-machines-windows-change-drive-letter.md)。

## 有关磁盘

就像任何其他计算机，Azure 中的虚拟机使用磁盘作为存储操作系统、 应用程序和数据。 Azure 的所有虚拟机都具有至少两个磁盘 – 操作系统磁盘和临时磁盘。 它们还可以有一个或多个数据磁盘。

- **操作系统磁盘**上的每个虚拟机都具有一个附加的操作系统磁盘。 它已注册为一个 SATA 驱动器，并标记为 c︰ 驱动器。 这张磁盘有 1023 千兆字节 (GB) 的最大容量。 在 Azure 创建操作系统磁盘时，磁盘的三份为高耐用性。 此外，如果您配置的 geo 复制虚拟机，您的 VHD 还可复制到不同的站点相隔超过 400 英里。
- **临时磁盘**自动为您创建。 Windows 的虚拟机，该磁盘标记为 d︰ 驱动器。 Linux 虚拟机，该磁盘通常是 /dev/sdb 和格式并装载到 /mnt/resource Azure Linux 代理。
- **数据磁盘**已连接到虚拟机存储应用程序数据或其他数据需要保留一个 VHD。 数据磁盘注册为 SCSI 驱动器，并标有您选择一个字母。  数据的每个磁盘都有 1023 GB 的最大容量。 虚拟机的大小将决定多少可以附加到它的存储类型的数据磁盘可用于主机磁盘。

    有关虚拟机容量的详细信息，请参阅[虚拟机大小](virtual-machines-size-specs.md)。

Azure 创建操作系统磁盘时从图像创建一个虚拟机。 如果您使用包含数据磁盘映像，Azure 时创建虚拟机，也创建数据磁盘。 （您可以使用从 Azure 或合作伙伴，或提供一个图像）。否则，您在创建虚拟机后添加数据磁盘。

您可以添加数据磁盘虚拟机在任何时候，通过附加磁盘到虚拟机。 您可以使用已上载或复制到您的存储帐户，或一个 Azure 将为您创建一个 VHD。 通过在 VHD 上放置租约，因此不能删除从存储在它附加到虚拟机与虚拟机中，附加数据磁盘将 VHD 文件从您的存储帐户。

## 有关 Vhd

在 Azure 中使用 Vhd 是作为在 Azure 中的标准或特优的存储帐户页面 blob 存储.vhd 文件。 （高级存储中提供了某些区域。）有关页面 blob 的详细信息，请参阅[了解块 blob 和页面 blob](https://msdn.microsoft.com/library/ee691964.aspx)。 高级存储的详细信息，请参阅[高级存储︰ Azure 的虚拟机的工作负载的高性能存储](../storage-premium-storage-preview-portal.md)。

在 Azure，之外 VHD 或 VHDX 格式，可以使用虚拟硬盘。 他们可以也修复，动态扩展，或差异。 Azure 支持 VHD 格式，固定磁盘。 固定格式的布局逻辑磁盘线性在文件中，因此该磁盘偏移量 X 存储 blob 的偏移量 X。Blob 的结尾处小脚注说明 VHD 的属性。 通常情况下，固定格式浪费的空间，因为它们大多数磁盘上有未使用的较大区域。 但是，Azure 存储.vhd 文件稀疏的格式，所以在同一时间收到固定和动态磁盘的好处。 有关详细信息，请参阅[入门虚拟硬盘](https://technet.microsoft.com/library/dd979539.aspx)。

您想要用作源创建磁盘或图像的 Azure 中的所有.vhd 文件都是只读的。 当您创建磁盘或图像时，Azure 将.vhd 文件的副本。 这些副本可以是只读或读写，这取决于您如何使用 VHD。

 从图像创建虚拟机时，Azure 创建源.vhd 文件副本的虚拟机的磁盘。 为了防止意外删除，Azure 上用于创建图像、 操作系统磁盘或为数据磁盘任何源.vhd 文件放置租约。

您可以删除源.vhd 文件之前，您需要通过删除一个或多个图像中删除租约。 要删除正在使用虚拟机操作系统磁盘作为.vhd 文件，您可以删除虚拟机，该操作系统磁盘，并且源.vhd 一次性全部通过虚拟机中删除，并删除所有相关联的文件的磁盘。 但是，删除的源数据磁盘，.vhd 文件需要几个步骤中设置顺序--分离从虚拟机磁盘，删除磁盘，然后删除.vhd 文件。

>[AZURE.WARNING] 如果您从存储中删除源.vhd 文件或删除您的存储帐户，Microsoft 不能恢复该数据。

## 下一步行动

Linux 虚拟机︰

-  [附加的磁盘并准备使用](virtual-machines-linux-how-to-attach-disk.md)
-  [捕获一个 Linux 虚拟机](virtual-machines-linux-capture-image.md)
-  [分离一个磁盘](virtual-machines-linux-how-to-detach-disk.md)

Windows 虚拟机︰

-  [连接的磁盘并准备使用](storage-windows-attach-disk.md)
-  [捕获 Windows 虚拟机](virtual-machines-capture-image-windows-server.md)
-  [分离一个磁盘](storage-windows-detach-disk.md)

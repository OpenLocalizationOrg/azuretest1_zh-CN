---
ms.openlocfilehash: 543a04e0ee01ea31fb02d1541cca9859004604a1
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="配置软件 RAID Azure 中运行 Linux 的虚拟机上" 
    description="了解如何使用 mdadm Azure 中配置 RAID 在 Linux 上。" 
    services="virtual-machines" 
    documentationCenter="" 
    authors="szarkos" 
    writer="szark" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="virtual-machines" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/29/2015" 
    ms.author="szark"/>



# 配置软件 RAID 在 Linux 上
它是一种常见的情况，可以在 Azure 中使用软件 RAID Linux 虚拟机作为一个独立的 RAID 设备显示附加的数据的多个磁盘。 通常这可用来提高性能，并允许更大的吞吐量比使用单个磁盘。


## 附加数据磁盘
通常将需要两个或多个空数据磁盘 RAID 设备进行配置。  这篇文章将深入详细的介绍如何连接到 Linux 虚拟机的数据磁盘。  请参阅 Microsoft Azure 文章[附加磁盘](storage-windows-attach-disk.md#attachempty)详细说明如何为 Linux 虚拟机在 Azure 上附加一个空数据磁盘。

>[AZURE.NOTE] ExtraSmall 的 VM 大小不支持附加到虚拟机的多个数据磁盘。  请参见[虚拟机并为 Microsoft Azure 的云服务大小](https://msdn.microsoft.com/library/azure/dn197896.aspx)有关 VM 大小的详细的信息和支持的数据磁盘的数量。


## 安装 mdadm 实用程序

- **Ubuntu**

        # sudo apt-get update
        # sudo apt-get install mdadm

- **CentOS 和 Oracle Linux**

        # sudo yum install mdadm

- **SLES openSUSE**

        # zypper install mdadm


## 创建磁盘分区
在本例中我们将在 /dev/sdc 上创建单个磁盘分区。 新的磁盘分区然后可以调用 /dev/sdc1。

- 启动 fdisk 来开始创建分区

        # sudo fdisk /dev/sdc
        Device contains neither a valid DOS partition table, nor Sun, SGI or OSF disklabel
        Building a new DOS disklabel with disk identifier 0xa34cb70c.
        Changes will remain in memory only, until you decide to write them.
        After that, of course, the previous content won't be recoverable.

        WARNING: DOS-compatible mode is deprecated. It's strongly recommended to
                 switch off the mode (command 'c') and change display units to
                 sectors (command 'u').

- 按 n 在提示符下创建**n**ew 分区︰

        Command (m for help): n

- 接下来，按下 p 来创建一个**p**（） 分区︰

        Command action
            e   extended
            p   primary partition (1-4)
        p

- 按"1"选择分区编号 1:

        Partition number (1-4): 1

- 选择该新分区的起始点，或只按`<enter>`若要接受默认设置，以便将分区放在该驱动器上的可用空间的开头︰

        First cylinder (1-1305, default 1):
        Using default value 1

- 选择分区的大小，例如，键入 +10G，创建 10 个千兆字节分区。 或者，只需按`<enter>`创建一个分区跨越整个驱动器︰

        Last cylinder, +cylinders or +size{K,M,G} (1-1305, default 1305): 
        Using default value 1305

- 接下来，为 fd （Linux raid 自动） ID 更改 ID 和**t**键入默认 id"83"的分区 (Linux):

        Command (m for help): t
        Selected partition 1
        Hex code (type L to list codes): fd

- 最后，分区表写入驱动器并退出 fdisk:

        Command (m for help): w
        The partition table has been altered!


## 创建 RAID 阵列

1. 下面的示例将条带 （RAID 级别 0） 在三个单独的数据磁盘 （sdc1、 sdd1、 sde1） 位于三个分区︰

        # sudo mdadm --create /dev/md127 --level 0 --raid-devices 3 \
          /dev/sdc1 /dev/sdd1 /dev/sde1

在此示例中，调用新的 RAID 设备运行此命令后将创建**/dev/md127** 。 此外请注意，是否这些数据磁盘我们以前已失效的另一个 RAID 阵列的一部分它可能会需要添加`--force`参数与`mdadm`命令。


2. 创建新的 RAID 设备上的文件系统

    **CentOS、 Oracle Linux、 SLES 12、 openSUSE 和 Ubuntu**

        # sudo mkfs -t ext4 /dev/md127

    **SLES 11**

        # sudo mkfs -t ext3 /dev/md127

3. **SLES 11 和 openSUSE** -启用 boot.md 和创建 mdadm.conf

        # sudo -i chkconfig --add boot.md
        # sudo echo 'DEVICE /dev/sd*[0-9]' >> /etc/mdadm.conf

    >[AZURE.NOTE] SUSE 系统上进行这些更改之后，可能需要重新启动。 此步骤*不*必需在 SLES 12。


## 将新的文件系统添加到 /etc/fstab

**注意︰**不正确地编辑 /etc/fstab 文件中，可能会导致系统无法启动。 如果不确定，请参阅有关如何正确编辑此文件的信息的分发文档。 此外建议，在编辑之前创建 /etc/fstab 文件的备份。

1. 创建所需的装入点为新的文件系统，例如︰

        # sudo mkdir /data

2. 在编辑 /etc/fstab 时，应使用**UUID**引用文件系统，而不是设备的名称。  使用`blkid`实用程序，以确定新文件系统的 UUID:

        # sudo /sbin/blkid
        ...........
        /dev/md127: UUID="aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee" TYPE="ext4"

3. 在文本编辑器中打开 /etc/fstab 并添加一项新的文件系统，例如︰

        UUID=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext4  defaults  0  2

    或在**SLES 11 和 openSUSE**上︰

        /dev/disk/by-uuid/aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext3  defaults  0  2

    然后，保存并关闭 /etc/fstab。

4. 测试 fstab 条目正确︰

        # sudo mount -a

    如果此命令将导致错误消息，请检查 /etc/fstab 文件中的语法。

    下一次运行`mount`命令来确保已装入的文件系统︰

        # mount
        .................
        /dev/md127 on /data type ext4 (rw)

5. （可选）故障安全引导参数

    **fstab 配置**

    许多分配包括`nobootwait`或`nofail`装载可能会添加到 /etc/fstab 文件的参数。 这些参数装载特定的文件系统时，允许失败，允许 Linux 系统可以继续工作，即使它不能正确装入 RAID 文件系统引导。 请参考您的发行版的文档了解更多有关这些参数。

    (Ubuntu) 的示例︰

        UUID=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext4  defaults,nobootwait  0  2

    **Linux 引导参数**

    除了上述参数，内核参数"`bootdegraded=true`"可以允许系统引导即使 RAID，所以我们认为损坏或降级，对于示例如果无意中从虚拟机中删除数据驱动器。 默认情况下这也可能导致不可引导的系统。

    请参考您的发行版的文档，了解如何正确编辑内核参数。 例如，在许多分布 CentOS、 Oracle Linux （SLES 11） 这些参数可添加手动为"`/boot/grub/menu.lst`"文件。  在 Ubuntu 上可以将此参数添加到`GRUB_CMDLINE_LINUX_DEFAULT`变量上"/ 等/默认/grub"。

 
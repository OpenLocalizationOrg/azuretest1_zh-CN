---
ms.openlocfilehash: 343a4de30ed3feefaccb75050e892865b0c30524
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="附加到运行 Linux 在 Azure 中的虚拟机的磁盘"
    description="了解如何附加到 Azure 的虚拟机的数据磁盘，并将其初始化，使其可供使用。"
    services="virtual-machines"
    documentationCenter=""
    authors="dsk-2015"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2015"
    ms.author="dkshir"/>

# 如何附加到 Linux 虚拟机的数据磁盘

您可以附加空磁盘和磁盘包含数据。 在这两种情况下，磁盘的实际驻留在 Azure 存储帐户的.vhd 文件。 此外在这两种情况下，附加磁盘之后, 您需要对其进行初始化，因此可以使用。 这篇文章是指使用传统部署模型创建的虚拟机。

> [AZURE.NOTE] 它是一种最佳做法，使用一个或多个单独的磁盘来存储虚拟机的数据。 当您创建 Azure 的虚拟机时，它具有操作系统磁盘和临时磁盘。 **不使用临时磁盘来存储数据。** 正如名称所示，它提供了临时存储区。 它提供没有冗余或备份，因为它不驻留在 Azure 存储中。
> 临时磁盘是通常由 Azure Linux 代理管理，自动装载到**/mnt/resource** （或**/mnt**上 Ubuntu 的图像）。 另一方面，数据磁盘的名称可以是 Linux 内核类似`/dev/sdc`，而您需要进行分区、 格式化，并装载该资源。 [Azure Linux 代理用户指南][代理]的详细信息，请参阅。

[AZURE.INCLUDE [howto-attach-disk-windows-linux](../../includes/howto-attach-disk-linux.md)]

## 如何︰ 初始化 Linux 中的新数据磁盘

1. 连接到虚拟机。 有关说明，请参阅[如何登录到运行 Linux 的虚拟机上][登录]。



2. 接下来，您需要找到要初始化的数据磁盘的设备标识符。 有两种方法可以做到这一点︰

    a） 中的 SSH 窗口中，键入以下命令，并创建用于管理虚拟机的帐户，然后输入的密码︰

            $sudo grep SCSI /var/log/messages

    对于最近的 Ubuntu 版本，您可能需要使用`sudo grep SCSI /var/log/syslog`因为登录`/var/log/messages`可能默认情况下禁用。

    您可以查找最后一个显示的消息中添加的数据磁盘的标识符。

    ![获取磁盘信息](./media/virtual-machines-linux-how-to-attach-disk/DiskMessages.png)

    运算，在指定数据点中设置指定位数。

    b） 利用`lsscsi`命令来查看设备 id。 `lsscsi` 可以通过以下任一方式安装`yum install lsscsi`（Red Hat 基于分配） 或`apt-get install lsscsi`（Debian 基于分配）。 您可以找到您正在寻找其_lun_或**逻辑单元号**的磁盘。 例如，附加的磁盘的_lun_可以很容易地看到从`azure vm disk list <virtual-machine>`为︰

            ~$ azure vm disk list ubuntuVMasm
            info:    Executing command vm disk list
            + Fetching disk images
            + Getting virtual machines
            + Getting VM disks
            data:    Lun  Size(GB)  Blob-Name                         OS
            data:    ---  --------  --------------------------------  -----
            data:         30        ubuntuVMasm-2645b8030676c8f8.vhd  Linux
            data:    1    10        test.VHD
            data:    2    30        ubuntuVMasm-76f7ee1ef0f6dddc.vhd
            info:    vm disk list command OK

    这与输出的`lsscsi`对相同样本虚拟机︰

            adminuser@ubuntuVMasm:~$ lsscsi
            [1:0:0:0]    cd/dvd  Msft     Virtual CD/ROM   1.0   /dev/sr0
            [2:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sda
            [3:0:1:0]    disk    Msft     Virtual Disk     1.0   /dev/sdb
            [5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc
            [5:0:0:1]    disk    Msft     Virtual Disk     1.0   /dev/sdd
            [5:0:0:2]    disk    Msft     Virtual Disk     1.0   /dev/sde

    元组中的每一行中的最后一个数字是_lun_。 请参阅`man lsscsi`的详细信息。

3. 在 SSH 窗口中，键入下面的命令以创建一个新的设备，然后输入帐户密码︰

        $sudo fdisk /dev/sdc

    >[AZURE.NOTE] 在此示例中，您可能需要使用`sudo -i`上如果 /sbin 或/usr/sbin 不在某些分配您`$PATH`。


4. 出现提示时，键入**n**以创建新的分区。


    ![创建新的设备](./media/virtual-machines-linux-how-to-attach-disk/DiskPartition.png)

5. 出现提示时，输入类型**p**主磁盘分区类型**1**进行分区以使它的第一个分区，然后键入要接受的默认值为圆柱。


    ![创建分区](./media/virtual-machines-linux-how-to-attach-disk/DiskCylinder.png)



6. 键入**p**有关进行分区的磁盘的详细信息，请参阅。


    ![列出磁盘信息](./media/virtual-machines-linux-how-to-attach-disk/DiskInfo.png)



7. 键入**w**要写入磁盘的设置。


    ![写入磁盘的更改](./media/virtual-machines-linux-how-to-attach-disk/DiskWrite.png)

8. 请在新分区上的文件系统。 例如，键入下面的命令，然后输入帐户密码︰

        # sudo mkfs -t ext4 /dev/sdc1

    ![创建文件系统](./media/virtual-machines-linux-how-to-attach-disk/DiskFileSystem.png)

    >[AZURE.NOTE] 请注意 SUSE Linux 企业 11 系统只针对 ext4 文件系统支持只读访问权限。  这些系统的建议为 ext3 而不是 ext4 格式的新文件系统。


9. 创建一个目录，安装新的文件系统。 例如，键入下面的命令，然后输入帐户密码︰

        # sudo mkdir /datadrive


10. 键入以下命令以装载驱动器︰

        # sudo mount /dev/sdc1 /datadrive

    现在可以使用**/datadrive**作为数据磁盘了。


11. 将新驱动器添加到 /etc/fstab:

    确保该驱动器已重新装入自动重新启动后它必须添加到 /etc/fstab 文件。 此外，强烈建议的 UUID （通用唯一标识符） 用于在 /etc/fstab 引用驱动器，而不是只是设备名称 (例如 /dev/sdc1)。 若要查找新驱动器的 UUID 可以使用**blkid**实用程序︰

        # sudo -i blkid

    输出将类似于以下形式︰

        /dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
        /dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
        /dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"


    >[AZURE.NOTE] 不正确地编辑**/etc/fstab**文件中，可能会导致系统无法启动。 如果不确定，请参阅有关如何正确编辑此文件的信息的分发文档。 此外建议，在编辑之前创建 /etc/fstab 文件的备份。

    接下来，在文本编辑器中打开**/etc/fstab**文件。 请注意该/等/fstab 是系统文件，因此您需要使用`sudo`若要编辑此文件，例如︰

        # sudo vi /etc/fstab

    在本例中我们将使用 UUID 值的新的**/dev/sdc1**设备，用前面的步骤，并装载点**/datadrive**。 将以下行添加到**/etc/fstab**文件的末尾︰

        UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults   1   2

    或者，基于 SUSE Linux 系统上，您可能需要使用稍有不同的格式︰

        /dev/disk/by-uuid/33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /   ext3   defaults   1   2

    现在可以测试，只需卸载，然后即重新挂载文件系统中，通过正确装载文件系统 该示例使用装入点`/datadrive`在前面步骤中创建︰

        # sudo umount /datadrive
        # sudo mount /datadrive

    如果`mount`命令将产生一个错误，请检查 / /etc/fstab 文件语法是否正确。 如果创建附加的数据驱动器或分区将需要输入/等/fstab 分别也。


>[AZURE.NOTE] 随后删除数据磁盘，而无需编辑 fstab 可能会导致虚拟机无法启动。 如果这是很常见，大多数版本提供以下任一`nofail`和/或`nobootwait`fstab 选项，将允许系统引导即使磁盘出现故障时在装载启动时间。 请有关这些参数的详细信息，参阅您的发行版的文档。

## 其他资源
[如何登录到运行 Linux 的虚拟机][登录]

[如何分离从 Linux 虚拟机的磁盘 ](virtual-machines-linux-how-to-detach-disk.md)

[与服务管理 API 使用 Azure CLI](virtual-machines-command-line-tools.md)

<!--Link references-->
[代理]: virtual-machines-linux-agent-user-guide.md
[登录]: virtual-machines-linux-how-to-log-on.md

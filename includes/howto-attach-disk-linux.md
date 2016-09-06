---
ms.openlocfilehash: f3fff07fd206cd1fecadf6a676fd18e2207724e5
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

有关磁盘的详细信息，请参阅[有关磁盘和虚拟机的 Vhd](../articles/virtual-machines-disks-vhds.md)。

<a id="attachempty"></a>
## 如何︰ 附加一个空的磁盘
附加一个空的磁盘是更简单的方法添加数据磁盘，因为 Azure 将.vhd 文件为您创建并将其存储在存储帐户。

1.  打开 Mac、 Linux、 Windows Azure CLI 并连接到 Azure 订购。 有关详细信息，请参阅[连接到 Azure CLI 从 Azure](../articles/xplat-cli-connect.md) 。

2.  请确保位于 Azure 服务管理模式，即通过键入默认的`azure config
    mode asm`。

3.  使用命令`azure vm disk attach-new`创建并附加新的磁盘，如下所示。 请注意名称的 Linux 创建虚拟机，您必须在您的订阅将替换_ubuntuVMasm_ 。 30 号是以 gb 为单位在此示例中，磁盘的大小。

        azure vm disk attach-new ubuntuVMasm 30

4.  创建并附加数据磁盘后，它将列在输出的`azure vm disk list
    <virtual-machine-name>`如下所示︰

        $ azure vm disk list ubuntuVMasm
        info:    Executing command vm disk list
        + Fetching disk images
        + Getting virtual machines
        + Getting VM disks
        data:    Lun  Size(GB)  Blob-Name                         OS
        data:    ---  --------  --------------------------------  -----
        data:         30        ubuntuVMasm-2645b8030676c8f8.vhd  Linux
        data:    0    30        ubuntuVMasm-76f7ee1ef0f6dddc.vhd
        info:    vm disk list command OK

<a id="attachexisting"></a>
## 如何︰ 附加现有的磁盘

附加一个现有磁盘需要您有存储帐户中.vhd。

1.  打开 Mac、 Linux、 Windows Azure CLI 并连接到 Azure 订购。 有关详细信息，请参阅[连接到 Azure CLI 从 Azure](../articles/xplat-cli-connect.md) 。

2.  请确保您在 Azure 服务管理模式，这是默认值。 如果您已更改为资源管理的模式，只需恢复通过键入`azure config mode asm`。

3.  如果您要附加的 VHD 已上载到 Azure 订购通过查明︰

        $azure vm disk list
        info:    Executing command vm disk list
        + Fetching disk images
        data:    Name                                          OS
        data:    --------------------------------------------  -----
        data:    myTestVhd                                     Linux
        data:    ubuntuVMasm-ubuntuVMasm-0-201508060029150744  Linux
        data:    ubuntuVMasm-ubuntuVMasm-0-201508060040530369
        info:    vm disk list command OK

4.  如果您没有找到您想要使用的磁盘，则可能上载本地 VHD 订阅使用`azure vm disk create`或`azure vm disk upload`。 就是这样一个示例︰

        $azure vm disk create myTestVhd2 .\TempDisk\test.VHD -l "East US" -o Linux
        info:    Executing command vm disk create
        + Retrieving storage accounts
        info:    VHD size : 10 GB
        info:    Uploading 10485760.5 KB
        Requested:100.0% Completed:100.0% Running:   0 Time:   25s Speed:    82 KB/s
        info:    Finishing computing MD5 hash, 16% is complete.
        info:    https://portalvhdsq1s6mc7mqf4gn.blob.core.windows.net/disks/test.VHD was
        uploaded successfully
        info:    vm disk create command OK

    您也可以使用`azure vm disk upload`命令将 VHD 上传到一个特定的存储帐户。 有关的命令来管理您 Azure 的虚拟机的数据磁盘[在这里](../virtual-machines-command-line-tools.md#commands-to-manage-your-azure-virtual-machine-data-disks)阅读的详细信息。

5.  键入以下命令，以将所需上载的 VHD 连接到您的虚拟机︰

        $azure vm disk attach ubuntuVMasm myTestVhd
        info:    Executing command vm disk attach
        + Getting virtual machines
        + Adding Data-Disk
        info:    vm disk attach command OK

    请确保您的虚拟机和_myTestVhd_与您所需的 VHD 的名称替换为_ubuntuVMasm_ 。

6.  您可以验证是否将磁盘连接到虚拟机的命令`azure vm disk list
    <virtual-machine-name>`为︰

        $azure vm disk list ubuntuVMasm
        info:    Executing command vm disk list
        + Fetching disk images
        + Getting virtual machines
        + Getting VM disks
        data:    Lun  Size(GB)  Blob-Name                         OS
        data:    ---  --------  --------------------------------  -----
        data:         30        ubuntuVMasm-2645b8030676c8f8.vhd  Linux
        data:    1    10        test.VHD
        data:    0    30        ubuntuVMasm-76f7ee1ef0f6dddc.vhd
        info:    vm disk list command OK


> [AZURE.NOTE]
> 添加数据磁盘后，您将需要登录到虚拟机并初始化该磁盘，以便虚拟机可以使用该磁盘的存储。

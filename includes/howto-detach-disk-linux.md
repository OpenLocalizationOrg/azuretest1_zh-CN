---
ms.openlocfilehash: 4fb7812e6651197d0cb29fd5e7a138c8dde3d480
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties writer="kathydav" editor="tysonn" manager="timlt" />


当不再需要连接到虚拟机的数据磁盘时，您可以很容易地进行分离。 这从虚拟机中，删除磁盘，但不将其从存储中删除。 如果要再次使用磁盘上的现有数据，可以将它重新连接到同一个虚拟机或另一个。  

> [AZURE.NOTE] Azure 中的虚拟机使用不同类型的磁盘-操作系统磁盘、 本地临时磁盘和可选的数据磁盘。 数据磁盘是推荐的方式来存储虚拟机的数据。 有关详细信息，请参阅[有关磁盘和虚拟机的 Vhd](../../virtual-machines-disks-vhds.md)。 不能分离操作系统磁盘，除非您还删除虚拟机。

## 找到磁盘

您可以分离从虚拟机磁盘之前，您需要了解 LUN 编号，是可以分离的磁盘的标识符。 为此，请执行以下步骤︰

1.  打开 Mac、 Linux、 Windows Azure CLI 并连接到 Azure 订购。 有关详细信息，请参阅[连接到 Azure CLI 从 Azure](../articles/xplat-cli-connect.md) 。

2.  请确保位于 Azure 服务管理模式，即通过键入默认的`azure config
    mode asm`。

3.  找出哪些磁盘附加到虚拟机的方法使用`azure vm disk list
    <virtual-machine-name>`，如下所示︰

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

4.  请注意该 LUN 或要取消该磁盘的**逻辑单元号**。


## 分离磁盘

找到磁盘的 LUN 编号之后，就可以将其分离︰

1.  通过运行命令分离从虚拟机所选的磁盘`azure vm disk detach
    <virtual-machine-name> <LUN>`如下所示︰

        $azure vm disk detach ubuntuVMasm 0
        info:    Executing command vm disk detach
        + Getting virtual machines
        + Removing Data-Disk
        info:    vm disk detach command OK

2.  您可以检查该磁盘有分离运行此命令︰

        $azure vm disk list ubuntuVMasm
        info:    Executing command vm disk list
        + Fetching disk images
        + Getting virtual machines
        + Getting VM disks
        data:    Lun  Size(GB)  Blob-Name                         OS
        data:    ---  --------  --------------------------------  -----
        data:         30        ubuntuVMasm-2645b8030676c8f8.vhd  Linux
        data:    1    10        test.VHD
        info:    vm disk list command OK

分离的磁盘存储中仍保留，但不再连接到虚拟机。

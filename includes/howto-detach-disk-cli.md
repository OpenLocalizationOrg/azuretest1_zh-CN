---
ms.openlocfilehash: fcef70f312d370e6e45958c35239087a9d339f95
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties writer="kathydav" editor="tysonn" manager="timlt" />



#如何分离从 CLI 使用的虚拟机的数据磁盘

当不再需要连接到虚拟机的数据磁盘时，您可以很容易地进行分离。 这从虚拟机中，删除磁盘，但不将其从存储中删除。 如果要再次使用磁盘上的现有数据，可以将它重新连接到同一个虚拟机或另一个。

> [AZURE.NOTE] Azure 中的虚拟机使用不同类型的磁盘︰ 操作系统磁盘、 本地临时磁盘和可选的数据磁盘。 数据磁盘是推荐的方式来存储虚拟机的数据。 有关磁盘的详细信息，请参阅[关于磁盘和图像](http://go.microsoft.com/fwlink/p/?LinkId=263439)。 不是当前可以分离操作系统磁盘。


1. 获取附加到您的虚拟机的磁盘的列表︰

        vm disk list <vm-name>

    如果省略`<vm-name>`，在您的订购将出现的所有磁盘的列表。


2. 分离一个磁盘︰

        vm disk detach <vm-name> <lun>

    `lun` 标识磁盘可以分离，并将为一个数字，可以在虚拟机的磁盘列表中找到。

示例演练包括终端的输出︰

    ~$ azure vm disk list kmlinux
    info:    Executing command vm disk list
    + Fetching disk images
    + Getting virtual machines
    + Getting VM disks
    data:    Lun  Size(GB)  Blob-Name                               OS
    data:    ---  --------  --------------------------------------  -----
    data:         30        kmlinux-kmlinux-2015-02-05.vhd          Linux
    data:    1    5         kmlinux-f8ef0006ab182209.vhd
    data:    2    7         kmlinux-602362868dbb7439.vhd
    info:    vm disk list command OK
    ~$ azure vm disk detach kmlinux 2
    info:    Executing command vm disk detach
    + Getting virtual machines
    + Removing Data-Disk
    info:    vm disk detach command OK
    ~$ azure vm disk list kmlinux
    info:    Executing command vm disk list
    + Fetching disk images
    + Getting virtual machines
    + Getting VM disks
    data:    Lun  Size(GB)  Blob-Name                               OS
    data:    ---  --------  --------------------------------------  -----
    data:         30        kmlinux-kmlinux-2015-02-05.vhd          Linux
    data:    1    5         kmlinux-f8ef0006ab182209.vhd
    info:    vm disk list command OK

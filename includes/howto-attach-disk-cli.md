---
ms.openlocfilehash: 93ea0dc6844ca6b4adb8c74dfbe62063d051fd8d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

有关磁盘的详细信息，请参阅[关于虚拟机磁盘在 Azure 中](http://go.microsoft.com/fwlink/p/?LinkId=403697)。

##<a id="cliattachempty"></a>如何︰ 附加一个空的磁盘
附加一个空的磁盘是更简单的方法添加数据磁盘。 运行以下命令，以添加新的空磁盘︰

    vm disk attach-new <vm-name> <size-in-gb> [blob-url]

更换`vm-name`您的虚拟机的名称和`size-in-gb`的新磁盘的大小。 您可以选择使用 blob URL 作为最后一个参数显式指定要创建目标斑点。 如果您不指定一个 blob 的 URL，将自动生成一个 blob 对象。  

运行以下命令来验证您的磁盘已创建︰

    vm disk list <vm-name>

以下是上述命令包括终端输出示例演练︰

    ~$ azure vm disk attach-new pinkylinux 20 http://pinkylinux.blob.core.windows.net/vhds/pinkydisk1.vhd
    info:   Executing command vm disk attach-new
    + Getting virtual machines
    + Adding Data-Disk
    info:   vm disk attach-new command OK
    ~$ azure vm disk list pinkylinux
    info:    Executing command vm disk list
    + Fetching disk images
    + Getting virtual machines
    + Getting VM disks
    data:    Lun  Size(GB)  Blob-Name                               OS
    data:    ---  --------  --------------------------------------  -----
    data:         30        pinkylinux-pinkylinux-2015-02-05.vhd    Linux
    data:    0    5         pinkydisk1.vhd
    data:    1    20        pinkylinux-f8ef0006ab182209.vhd
    info:    vm disk list command OK

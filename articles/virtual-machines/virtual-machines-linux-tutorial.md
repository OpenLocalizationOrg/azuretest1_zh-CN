---
ms.openlocfilehash: a5632643a7cf6ecf2d702bcb5fd0ff8041a5cbce
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="创建运行 Linux 在 Azure 中的虚拟机"
    description="了解如何创建使用图像从 Azure 运行 Linux 的 Azure 的虚拟机 (VM)。"
    services="virtual-machines"
    documentationCenter=""
    authors="squillace"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-management" />

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="07/13/2015"
    ms.author="rasquill"/>

# 创建一个运行 Linux 虚拟机

> [AZURE.SELECTOR]
- [Azure 门户](virtual-machines-linux-tutorial-portal-rm.md)
- [Azure CLI](virtual-machines-linux-tutorial.md)

创建运行 Linux Azure 虚拟机 (VM) 可以很容易地执行从命令行或从门户。 本教程展示如何使用 Mac、 Linux、 和 (Azure CLI) Windows Azure 命令行界面来运行在 Azure Ubuntu 服务器虚拟机快速创建、 连接到使用**ssh**，以及如何创建和挂载新磁盘。 （本主题使用 Ubuntu 服务器 VM，但您也可以创建[您自己的图像作为模板](virtual-machines-linux-create-upload-vhd.md)使用的 Linux 虚拟机）。

[AZURE.INCLUDE [free-trial-note](../../includes/free-trial-note.md)]

## 视频的演练

这是本教程的演练。

[AZURE.VIDEO building-a-linux-virtual-machine-tutorial]

## 安装 Azure CLI

第一步是[安装 Azure CLI](../xplat-cli-install.md)。

很好。 现在请确保您在资源管理模式通过键入`azure config mode arm`。

甚至更好。 现在通过键入登录您的工作或学校 id`azure login`并按照提示进行操作。

> [AZURE.NOTE] 如果您收到错误日志，您可能需要[创建工作或学校从您的 Microsoft 个人帐户的 id](resource-group-create-work-id-from-personal.md)。

## 创建 Azure VM

类型`azure group create <my-group-name> westus`替换_&lt;我组名称&gt;_组的名称都是唯一的 （如果您希望可以使用不同的地区）。 您应该看到类似以下内容︰

    azure group create myuniquegroupname westus
    info:    Executing command group create
    + Getting resource group myuniquegroupname
    + Creating resource group myuniquegroupname
    info:    Created resource group myuniquegroupname
    data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myuniquegroupname
    data:    Name:                myuniquegroupname
    data:    Location:            westus
    data:    Provisioning State:  Succeeded
    data:    Tags:
    data:
    info:    group create command OK

现在，通过键入创建 VM `azure vm quick-create`，并且您将收到提示输入其余的参数。 上例中，使用您刚才创建的资源组的名称和有关的**ImageURN**值，使用`canonical:ubuntuserver:14.04.2-LTS:latest`，以便您的经验如下所示︰

    azure vm quick-create
    info:    Executing command vm quick-create
    Resource group name: myuniquegroupname
    Virtual machine name: myuniquevmname
    Location name: westus
    Operating system Type [Windows, Linux]: Linux
    ImageURN (format: "publisherName:offer:skus:version"): canonical:ubuntuserver:14.04.2-LTS:latest
    User name: ops
    Password: *********
    Confirm password: *********
    + Looking up the VM "myuniquevmname"
    info:    Using the VM Size "Standard_D1"
    info:    The [OS, Data] Disk or image configuration requires storage account
    + Retrieving storage accounts
    info:    Could not find any storage accounts in the region "westus", trying to create new one
    + Creating storage account "cli3c0464f24f1bf4f014323" in "westus"
    + Looking up the storage account cli3c0464f24f1bf4f014323
    + Looking up the NIC "myuni-westu-1432328437727-nic"
    info:    An nic with given name "myuni-westu-1432328437727-nic" not found, creating a new one
    + Looking up the virtual network "myuni-westu-1432328437727-vnet"
    info:    Preparing to create new virtual network and subnet
    / Creating a new virtual network "myuni-westu-1432328437727-vnet" [address prefix: "10.0.0.0/16"] with subnet "myuni-westu-1432328437727-snet"+[address prefix: "10.0.1.0/24"]
    + Looking up the virtual network "myuni-westu-1432328437727-vnet"
    + Looking up the subnet "myuni-westu-1432328437727-snet" under the virtual network "myuni-westu-1432328437727-vnet"
    info:    Found public ip parameters, trying to setup PublicIP profile
    + Looking up the public ip "myuni-westu-1432328437727-pip"
    info:    PublicIP with given name "myuni-westu-1432328437727-pip" not found, creating a new one
    + Creating public ip "myuni-westu-1432328437727-pip"
    + Looking up the public ip "myuni-westu-1432328437727-pip"
    + Creating NIC "myuni-westu-1432328437727-nic"
    + Looking up the NIC "myuni-westu-1432328437727-nic"
    + Creating VM "myuniquevmname"
    + Looking up the VM "myuniquevmname"
    + Looking up the NIC "myuni-westu-1432328437727-nic"
    + Looking up the public ip "myuni-westu-1432328437727-pip"
    data:    Id                              :/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myuniquegroupname/providers/Microsoft.Compute/virtualMachines/myuniquevmname
    data:    ProvisioningState               :Succeeded
    data:    Name                            :myuniquevmname
    data:    Location                        :westus
    data:    FQDN                            :myuni-westu-1432328437727-pip.westus.cloudapp.azure.com
    data:    Type                            :Microsoft.Compute/virtualMachines
    data:
    data:    Hardware Profile:
    data:      Size                          :Standard_D1
    data:
    data:    Storage Profile:
    data:      Image reference:
    data:        Publisher                   :canonical
    data:        Offer                       :ubuntuserver
    data:        Sku                         :14.04.2-LTS
    data:        Version                     :latest
    data:
    data:      OS Disk:
    data:        OSType                      :Linux
    data:        Name                        :cli3c0464f24f1bf4f0-os-1432328438224
    data:        Caching                     :ReadWrite
    data:        CreateOption                :FromImage
    data:        Vhd:
    data:          Uri                       :https://cli3c0464f24f1bf4f014323.blob.core.windows.net/vhds/cli3c0464f24f1bf4f0-os-1432328438224.vhd
    data:
    data:    OS Profile:
    data:      Computer Name                 :myuniquevmname
    data:      User Name                     :ops
    data:      Linux Configuration:
    data:        Disable Password Auth       :false
    data:
    data:    Network Profile:
    data:      Network Interfaces:
    data:        Network Interface #1:
    data:          Id                        :/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myuniquegroupname/providers/Microsoft.Network/networkInterfaces/myuni-westu-1432328437727-nic
    data:          Primary                   :true
    data:          MAC Address               :00-0D-3A-31-55-31
    data:          Provisioning State        :Succeeded
    data:          Name                      :myuni-westu-1432328437727-nic
    data:          Location                  :westus
    data:            Private IP alloc-method :Dynamic
    data:            Private IP address      :10.0.1.4
    data:            Public IP address       :191.239.51.1
    data:            FQDN                    :myuni-westu-1432328437727-pip.westus.cloudapp.azure.com
    info:    vm quick-create command OK

您的虚拟机已启动并运行，等待您连接。

## 连接到您的虚拟机

与 Linux 虚拟机，您通常连接使用**ssh**。 本主题将连接到虚拟机使用的用户名和密码;若要使用公共和私有密钥对与您的虚拟机进行通信，请参阅[如何使用 SSH 使用 Linux 在 Azure 上](virtual-machines-linux-use-ssh-key.md)。

如果你不熟悉使用**ssh**连接，此命令采用`ssh <username>@<publicdnsaddress> -p <the ssh port>`。 在这种情况下，我们使用的用户名和密码从上一步和是默认**ssh**端口的端口 22 日。

    ssh ops@myuni-westu-1432328437727-pip.westus.cloudapp.azure.com -p 22
    The authenticity of host 'myuni-westu-1432328437727-pip.westus.cloudapp.azure.com (191.239.51.1)' can't be established.
    ECDSA key fingerprint is bx:xx:xx:xx:xx:xx:xx:xx:xx:x:x:x:x:x:x:xx.
    Are you sure you want to continue connecting (yes/no)? yes
    Warning: Permanently added 'myuni-westu-1432328437727-pip.westus.cloudapp.azure.com,191.239.51.1' (ECDSA) to the list of known hosts.
    ops@myuni-westu-1432328437727-pip.westus.cloudapp.azure.com's password:
    Welcome to Ubuntu 14.04.2 LTS (GNU/Linux 3.16.0-37-generic x86_64)

     * Documentation:  https://help.ubuntu.com/

      System information as of Fri May 22 21:02:32 UTC 2015

      System load: 0.37              Memory usage: 2%   Processes:       207
      Usage of /:  41.4% of 1.94GB   Swap usage:   0%   Users logged in: 0

      Graph this data and manage this system at:
        https://landscape.canonical.com/

      Get cloud support with Ubuntu Advantage Cloud Guest:
        http://www.ubuntu.com/business/services/cloud

    0 packages can be updated.
    0 updates are security updates.



    The programs included with the Ubuntu system are free software;
    the exact distribution terms for each program are described in the
    individual files in /usr/share/doc/*/copyright.

    Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
    applicable law.

    ops@myuniquevmname:~$

现在，您可以访问您的虚拟机，您就可以附加磁盘。

## 连接和安装磁盘

附加新的磁盘时，会比较快。 只需键入`azure vm disk attach-new <myuniquegroupname> <myuniquevmname> <size-in-GB>`创建并附加新的 GB 磁盘的 VM。 它应该看起来如下︰

    azure vm disk attach-new myuniquegroupname myuniquevmname 5
    info:    Executing command vm disk attach-new
    + Looking up the VM "myuniquevmname"
    info:    New data disk location: https://cliexxx.blob.core.windows.net/vhds/myuniquevmname-20150526-0xxxxxxx43.vhd
    + Updating VM "myuniquevmname"
    info:    vm disk attach-new command OK


现在让我们来了解该磁盘，使用`dmesg | grep SCSI`（以发现新磁盘所使用的方法可能会有所不同）。 在这种情况下，它如下所示︰

    dmesg | grep SCSI
    [    0.294784] SCSI subsystem initialized
    [    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
    [    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
    [    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
    [ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk

对于本主题中，`sdc`磁盘是我们想要的那个。 现在与该磁盘进行分区`sudo fdisk /dev/sdc`-假定在您的案例是磁盘`sdc`，和使其主磁盘分区 1，以及接受其他默认设置。

    sudo fdisk /dev/sdc
    Device contains neither a valid DOS partition table, nor Sun, SGI or OSF disklabel
    Building a new DOS disklabel with disk identifier 0x2a59b123.
    Changes will remain in memory only, until you decide to write them.
    After that, of course, the previous content won't be recoverable.

    Warning: invalid flag 0x0000 of partition table 4 will be corrected by w(rite)

    Command (m for help): n
    Partition type:
       p   primary (0 primary, 0 extended, 4 free)
       e   extended
    Select (default p): p
    Partition number (1-4, default 1): 1
    First sector (2048-10485759, default 2048):
    Using default value 2048
    Last sector, +sectors or +size{K,M,G} (2048-10485759, default 10485759):
    Using default value 10485759

通过键入来创建分区`p`在提示符处︰

    Command (m for help): p

    Disk /dev/sdc: 5368 MB, 5368709120 bytes
    255 heads, 63 sectors/track, 652 cylinders, total 10485760 sectors
    Units = sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes
    Disk identifier: 0x2a59b123

       Device Boot      Start         End      Blocks   Id  System
    /dev/sdc1            2048    10485759     5241856   83  Linux

    Command (m for help): w
    The partition table has been altered!

    Calling ioctl() to re-read partition table.
    Syncing disks.

并通过使用**mkfs**命令并指定文件系统类型和设备名称写入分区的文件系统。 在本主题中，我们使用`ext4`和`/dev/sdc1`从上面︰

    sudo mkfs -t ext4 /dev/sdc1
    mke2fs 1.42.9 (4-Feb-2014)
    Discarding device blocks: done
    Filesystem label=
    OS type: Linux
    Block size=4096 (log=2)
    Fragment size=4096 (log=2)
    Stride=0 blocks, Stripe width=0 blocks
    327680 inodes, 1310464 blocks
    65523 blocks (5.00%) reserved for the super user
    First data block=0
    Maximum filesystem blocks=1342177280
    40 block groups
    32768 blocks per group, 32768 fragments per group
    8192 inodes per group
    Superblock backups stored on blocks:
        32768, 98304, 163840, 229376, 294912, 819200, 884736

    Allocating group tables: done
    Writing inode tables: done
    Creating journal (32768 blocks): done
    Writing superblocks and filesystem accounting information: done

现在我们创建一个目录来挂载文件系统使用`mkdir`:

    sudo mkdir /datadrive

和您装载目录使用`mount`:

    sudo mount /dev/sdc1 /datadrive

现在可以使用作为数据磁盘了`/datadrive`。

    ls
    bin   datadrive  etc   initrd.img  lib64       media  opt   root  sbin  sys  usr  vmlinuz
    boot  dev        home  lib         lost+found  mnt    proc  run   srv   tmp  var

> [AZURE.NOTE] 您还可以连接到您的 Linux 虚拟机的标识使用 SSH 密钥。 有关详细信息，请参阅[如何使用 SSH 使用 Linux 在 Azure 上](virtual-machines-linux-use-ssh-key.md)。

## 下一步行动

请记住，这些新磁盘将通常不提供给 VM 是否[fstab](http://en.wikipedia.org/wiki/Fstab)文件中写入信息，除非重新启动。

若要了解有关 Linux 在 Azure 上的详细信息，请参阅︰

- [Linux 和开源在 Azure 上计算](virtual-machines-linux-opensource.md)

- [如何使用 Azure 命令行界面](../virtual-machines-command-line-tools.md)

- [将用于 Linux 使用 Azure CustomScript 扩展一个灯应用程序部署](virtual-machines-linux-script-lamp.md)

- [关于 Azure VM 配置设置](http://msdn.microsoft.com/library/azure/dn763935.aspx)

- [在 Azure 上 Linux Docker 虚拟机扩展](virtual-machines-docker-vm-extension.md)
 

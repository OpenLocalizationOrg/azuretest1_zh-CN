---
ms.openlocfilehash: 61c37b321c6fd18fab1289aacd12f82fad252aad
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="创建并上载 VHD Linux 在 Azure 中"
    description="了解如何创建并上载 Azure 虚拟硬盘 (VHD) 包含 Linux 操作系统的系统。"
    services="virtual-machines"
    documentationCenter=""
    authors="szarkos"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/15/2015"
    ms.author="szarkos"/>


# <a id="nonendorsed"> </a>对于非认可的版本信息 #

**重要提示**︰ Azure 平台 SLA 适用于仅在其中一个[认可分配](virtual-machines-../linux-endorsed-distributions.md)使用时运行 Linux 操作系统的虚拟机。 在 Azure 图像库中提供的所有 linux 都是具有所需配置的背书的分布。

- [Linux 在 Azure 上的认可分配](virtual-machines-../linux-endorsed-distributions.md)
- [在 Microsoft Azure 中的 Linux 映像的支持](http://support2.microsoft.com/kb/2941892)

在 Azure 上运行的所有分配都需要满足大量有机会在平台上正常运行的先决条件。  这篇文章并不意味着全面按照每个分布是不同的;并很有可能，即使满足下列所有条件仍然需要大大改良自己的 Linux 系统，以确保它正确运行的平台上。

正是由于这个原因，我们建议您与我们[在 Azure 认可分配上的 Linux](../linux-endorsed-distributions.md)在可能的情况之一启动。 下面的文章将带您了解如何准备在 Azure 受支持的各种背书的 Linux 发行版本︰

- **[基于 centOS 的分配](virtual-machines-linux-create-upload-vhd-centos.md)**
- **[Oracle Linux](virtual-machines-linux-create-upload-vhd-oracle.md)**
- **[SLES openSUSE &](../virtual-machines-linux-create-upload-vhd-suse)**
- **[Ubuntu](virtual-machines-linux-create-upload-vhd-ubuntu.md)**

本文的其余部分将着重在 Azure 上运行您的 Linux 发行版的一般准则。


## <a id="linuxinstall"> </a>常规的 Linux 安装说明 ##

- 在 Azure 中不支持较新的 VHDX 格式。 您可以将磁盘转换为使用 Hyper-V 管理器或转换 vhd cmdlet 的 VHD 格式。

- 在安装 Linux 系统时建议使用标准分区，而不是 LVM （通常为许多安装默认值）。 尤其是以往任何时候都需要附加到另一个虚拟机的故障诊断操作系统磁盘，这样可以避免与克隆虚拟机，LVM 名称冲突。  如果愿意，可能在数据磁盘中使用的 LVM 或[RAID](virtual-machines-linux-configure-raid.md) 。

- NUMA 不支持更大的 VM 大小，由于 2.6.37 以下的 Linux 内核版本中的错误。 此问题主要影响分配使用上游红帽 2.6.32 内核。 Azure Linux 代理 (waagent) 的手动安装将自动禁用 NUMA，Linux 内核的 GRUB 配置中。

- OS 磁盘上未配置一个交换分区。 可以配置 Linux 代理来创建临时的资源磁盘上的交换文件。  下面的步骤中，可以找到相关详细信息。

- 所有 Vhd 必须使用大小为 1 MB 的倍数。


### 而 Hyper-V 不安装 Linux ###

在某些情况下，Linux 安装程序可能无法包含驱动程序的 Hyper-V 中初始 ramdisk 磁盘 （initrd 或 initramfs） 除非它检测到它正在运行 Hyper-V 环境。  当使用不同的虚拟化系统 （即 Virtualbox，KVM，等等） 来准备您的 Linux 映像，您可能需要重新生成 initrd 以确保至少`hv_vmbus`和`hv_storvsc`内核模块位于初始 ramdisk。  这至少是一个已知的问题，在基于上游的 Red Hat 通讯系统。

Initrd 或 initramfs 图像重建的机制可能因分布。 请查阅您的发行版的文档或支持正确的过程。  下面是一个示例，对于如何重建 initrd 使用`mkinitrd`实用程序︰

首先，备份现有的 initrd 映像︰

    # cd /boot
    # sudo cp initrd-`uname -r`.img  initrd-`uname -r`.img.bak

下一步，重建与 initrd`hv_vmbus`和`hv_storvsc`内核模块︰

    # sudo mkinitrd --preload=hv_storvsc --preload=hv_vmbus -v -f initrd-`uname -r`.img `uname -r`


### 调整大小 Vhd ###

在 Azure 上的 VHD 映像必须对齐到 1 MB 虚拟大小。  通常情况下，使用 Hyper-V 创建 Vhd 应已正确对齐。  如果没有正确对齐 VHD 然后当您尝试创建您的 VHD*映像*时可能会收到与以下内容类似的错误信息︰

    "The VHD http://<mystorageaccount>.blob.core.windows.net/vhds/MyLinuxVM.vhd has an unsupported virtual size of 21475270656 bytes. The size must be a whole number (in MBs).”

若要解决此问题，可以调整 VM 使用 Hyper-V 管理器控制台或[调整 VHD](http://technet.microsoft.com/library/hh848535.aspx) Powershell cmdlet。

如果您不运行在 Windows 环境中然后建议使用 qemu img 转换 （如果需要），并调整其大小 VHD:

 1. 调整大小直接使用工具如 VHD`qemu-img`或`vbox-manage`可能会导致无法引导 VHD。  因此建议对第一个转换到原始磁盘映像 VHD。  如果原始磁盘映像 （例如 KVM 某些虚拟机管理程序的默认值） 作为已创建虚拟机映像然后可以跳过此步骤︰

        # qemu-img convert -f vpc -O raw MyLinuxVM.vhd MyLinuxVM.raw

 2. 计算所需的磁盘映像以确保虚拟大小已对齐到 1 MB 大小。  下面的 bash shell 脚本可以协助这项。  该脚本使用"`qemu-img info`"确定虚拟磁盘映像的大小，然后计算到下一步的 1 MB 的大小︰

        rawdisk="MyLinuxVM.raw"
        vhddisk="MyLinuxVM.vhd"

        MB=$((1024*1024))
        size=$(qemu-img info -f raw --output json "$rawdisk" | \
               gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

        rounded_size=$((($size/$MB + 1)*$MB))
        echo "Rounded Size = $rounded_size"

 3. 调整大小设置在上述脚本中使用 rounded_size 的原始磁盘︰

        # qemu-img resize MyLinuxVM.raw $rounded_size

 4. 现在，将转换为固定大小的 VHD 的原始磁盘︰

        # qemu-img convert -f raw -o subformat=fixed -O vpc MyLinuxVM.raw MyLinuxVM.vhd



## Linux 内核的要求 ##

Hyper-V 和 Azure 的 Linux 集成服务 (LIS) 驱动程序提供直接到上游 Linux 内核。 许多分发方案，以包括最新的 Linux 内核版本 (即 3.x) 将已经使用这些驱动程序，或否则其内核提供的这些驱动程序的 backported 版本。  这些驱动程序会不断更新中上游内核的新修补程序和功能，以便在可能的情况，建议运行[分发的认可](../linux-endorsed-distributions.md)，将包括这些修补程序和更新。

如果您运行的一种形式的 Red Hat 企业 Linux 版本**6.0 6.3**，您将需要为 Hyper-V 安装最新的 LIS 驱动程序。 [在此位置](http://go.microsoft.com/fwlink/p/?LinkID=254263&clcid=0x409)找不到驱动程序。 RHEL **6.4 +** （和衍生品） 截至 LIS 驱动程序已包含的内核，因此没有额外安装软件包所需在 Azure 上运行这些系统。

如果需要自定义的内核，则建议使用较新的内核版本 （即**3.8 +**）。 一些努力将这些分配或供应商维护他们自己的内核，对所需定期 backport LIS 驱动程序从上游内核对您自定义的内核。  即使您已经在运行的相对较新的内核版本，它是强烈建议来跟踪在 LIS 的驱动程序和 backport 任何上游修复那些根据需要。 LIS 驱动程序源文件的位置是在 Linux 内核源代码树中[维护](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/tree/MAINTAINERS)文件中︰

    F:  arch/x86/include/asm/mshyperv.h
    F:  arch/x86/include/uapi/asm/hyperv.h
    F:  arch/x86/kernel/cpu/mshyperv.c
    F:  drivers/hid/hid-hyperv.c
    F:  drivers/hv/
    F:  drivers/input/serio/hyperv-keyboard.c
    F:  drivers/net/hyperv/
    F:  drivers/scsi/storvsc_drv.c
    F:  drivers/video/fbdev/hyperv_fb.c
    F:  include/linux/hyperv.h
    F:  tools/hv/

在非常小的据悉缺少以下修补程序会在 Azure 上引起问题，因此必须将这些包含在内核中。 此列表绝不是详尽的或完成所有分配︰

- [ata_piix︰ 默认情况下推迟到 Hyper-V 驱动程序磁盘](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/drivers/ata/ata_piix.c?id=cd006086fa5d91414d8ff9ff2b78fbb593878e3c)
- [storvsc︰ 用于传输过程中数据包的重设路径中的帐户](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/drivers/scsi/storvsc_drv.c?id=5c1b10ab7f93d24f29b5630286e323d1c5802d5c)
- [storvsc︰ 避免使用 WRITE_SAME](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=3e8f4f4065901c8dfc51407e1984495e1748c090)
- [storvsc︰ 禁用写入同一个 RAID 和虚拟主机适配器驱动程序](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=54b2b50c20a61b51199bedb6e5d2f8ec2568fb43)
- [storvsc︰ 空指针取消引用的修补程序](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=b12bb60d6c350b348a4e1460cd68f97ccae9822e)
- [storvsc︰ 环形缓冲区故障可能会导致 I/O 冻结](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=e86fb5e8ab95f10ec5f2e9430119d5d35020c951)


## Azure 的 Linux 代理 ##

[Azure Linux 代理](virtual-machines-linux-agent-user-guide.md)(waagent) 需要正确设置 Azure 中的 Linux 虚拟机。 您可以获取最新版本、 文件问题或提交在[Linux 代理 GitHub repo](https://github.com/Azure/WALinuxAgent)拉请求。

- Linux 代理是 Apache 2.0 许可证下发布的。 许多分配已经提供了 RPM 或 deb 包的代理，并因此在某些情况下这可以安装和更新很少的工作。

- Azure Linux 代理要求 Python v2.6 内部 +。

- 代理还需要 python pyasn1 模块。 大多数版本提供这作为一个单独的软件包，可以安装。

- 在某些情况下 Azure Linux 代理不可能与 NetworkManager 兼容。 许多由分配的 RPM/Deb 包配置 NetworkManager 到 waagent 包中，冲突，当安装 Linux 代理包，这样就将卸载 NetworkManager。


## 常规 Linux 系统要求 ##

- 修改 GRUB 或 GRUB2 包含以下参数中的内核启动行。 这还将确保所有控制台消息被都发送到第一个串行端口，可以帮助 Azure 支持调试的问题︰

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300

    这还将确保所有控制台消息被都发送到第一个串行端口，可以帮助 Azure 支持调试问题。

    另外，建议*删除*以下参数如果存在︰

        rhgb quiet crashkernel=auto

    图形和安静的引导是没有用的在云环境中我们希望所有日志发送到串行端口的位置。

    `crashkernel`选项可能左如果需要，配置，但是请注意，此参数将减少 128 MB 或更多，由 VM 中的可用内存量可能较小的虚拟机大小上有问题。

- 安装 Azure 的 Linux 代理

    Azure Linux 代理程序需要资源调配上 Azure 的 Linux 映像。  许多分配作为 RPM 或 Deb 的包 （包通常称为 WALinuxAgent 或 walinuxagent） 提供代理。  此外可以在[Linux 代理指南](virtual-machines-linux-agent-user-guide.md)中的步骤手动安装代理。

- 确保安装和配置为在系统启动时启动 SSH 服务器。  这通常是默认值。

- OS 磁盘上不创建交换空间

    Azure Linux 代理可以自动配置使用本地资源磁盘设置在 Azure 上后附加到虚拟机的交换空间。 请注意，本地资源磁盘是一个*临时*磁盘时 VM deprovisioned 可能清空。 Azure Linux 代理在安装之后 （请参阅上一步），适当地修改 /etc/waagent.conf 中的下列参数︰

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

- 在"/ 等/sudoers"，必须删除或注释掉下面的行中，如果它们存在︰

        Defaults targetpw
        ALL    ALL=(ALL) ALL

- 作为最后一步，运行以下命令以取消设置虚拟机︰

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

- 然后需要关闭虚拟机，然后到 Azure 上载 VHD。

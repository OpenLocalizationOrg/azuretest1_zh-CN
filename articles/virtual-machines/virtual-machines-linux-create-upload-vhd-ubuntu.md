---
ms.openlocfilehash: 4e6f87277ca91835da36111015dd3d737e62c952
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="创建并上载在 Azure Ubuntu Linux VHD" 
    description="了解如何创建并上载 Azure 虚拟硬盘 (VHD) 包含 Ubuntu Linux 操作系统的系统。" 
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


# 准备 Azure 的 Ubuntu 的虚拟机

##先决条件##

本文假定您已经为虚拟硬盘中安装了 Ubuntu Linux 操作系统的系统。 多个工具可创建.vhd 文件的形式，例如如 Hyper-V 虚拟化解决方案。 有关说明，请参阅[安装 Hyper-V 角色并配置虚拟机](http://technet.microsoft.com/library/hh846766.aspx)。 

**Ubuntu 的安装说明**

- 在 Azure 中不支持较新的 VHDX 格式。 您可以将磁盘转换为使用 Hyper-V 管理器或转换 vhd cmdlet 的 VHD 格式。

- 在安装 Linux 系统时建议使用标准分区，而不是 LVM （通常为许多安装默认值）。 尤其是以往任何时候都需要附加到另一个虚拟机的故障诊断操作系统磁盘，这样可以避免与克隆虚拟机，LVM 名称冲突。  如果愿意，可能在数据磁盘中使用的 LVM 或[RAID](virtual-machines-linux-configure-raid.md) 。

- OS 磁盘上未配置一个交换分区。 可以配置 Linux 代理来创建临时的资源磁盘上的交换文件。  下面的步骤中，可以找到相关详细信息。

- 所有 Vhd 必须使用大小为 1 MB 的倍数。


## <a id="ubuntu"> </a>Ubuntu 12.04 + ##

1. 在 Hyper-V 管理器的中央窗格中，选择虚拟机。

2. 单击**连接**以打开虚拟机窗口。

3.  替换为要使用 Ubuntu 的 Azure repos 图像中当前存储库。 根据 Ubuntu 版本，步骤略有不同。

    在编辑 /etc/apt/sources.list 之前, 建议进行备份

        # sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak

    12.04 Ubuntu:

        # sudo sed -i "s/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g" /etc/apt/sources.list
        # sudo apt-add-repository 'http://archive.canonical.com/ubuntu precise-backports main'
        # sudo apt-get update

    12.10 Ubuntu:

        # sudo sed -i "s/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g" /etc/apt/sources.list
        # sudo apt-add-repository 'http://archive.canonical.com/ubuntu quantal-backports main'
        # sudo apt-get update

    Ubuntu 14.04 +:

        # sudo sed -i "s/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g" /etc/apt/sources.list
        # sudo apt-get update

4. 更新到最新版本内核的操作系统，通过运行以下命令︰ 

    12.04 Ubuntu:

        # sudo apt-get update
        # sudo apt-get install hv-kvp-daemon-init linux-backports-modules-hv-precise-virtual
        (recommended) sudo apt-get dist-upgrade

        # sudo reboot

    12.10 Ubuntu:

        # sudo apt-get update
        # sudo apt-get install hv-kvp-daemon-init linux-backports-modules-hv-quantal-virtual
        (recommended) sudo apt-get dist-upgrade

        # sudo reboot
    
    Ubuntu 14.04 +:

        # sudo apt-get update
        # sudo apt-get install hv-kvp-daemon-init
        (recommended) sudo apt-get dist-upgrade

        # sudo reboot

5.  （可选）如果 Ubuntu 系统遇到一个错误并重新启动，然后它将通常一直等待在 grub 引导提示符下为阻止系统引导正确的用户输入。 若要防止出现这种情况，请完成以下步骤︰

    a） 打开 /etc/grub.d/00_header 文件。

    b） 在函数**make_timeout()**中，搜索**如果 ["\${recordfail}"= 1]; 然后**

    c） 更改到这条线以下语句**设置超时 = 5**。

    d） 运行 sudo 更新-grub。

6. 修改 Grub Azure 中包含其他内核参数的内核引导线。 若要执行此操作打开"/ 等/默认/grub"在文本编辑器中，找到名为`GRUB_CMDLINE_LINUX_DEFAULT`（或如果需要添加它） 并编辑它包含以下参数︰

        GRUB_CMDLINE_LINUX_DEFAULT="console=tty1 console=ttyS0 earlyprintk=ttyS0 rootdelay=300"

    保存并关闭此文件，然后再运行 '`sudo update-grub`'。 这将确保所有控制台消息被都发送到第一个串行端口，可帮助调试问题的 Azure 的技术支持。 

8.  确保安装和配置为在系统启动时启动 SSH 服务器。  这通常是默认值。

9.  安装 Azure 的 Linux 代理︰

        # sudo apt-get update
        # sudo apt-get install walinuxagent

    请注意，安装`walinuxagent`软件包将删除`NetworkManager`和`NetworkManager-gnome`包，如果它们安装。

10. 运行以下命令以取消设置虚拟机，并准备将它在 Azure 上资源调配︰

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

11. 单击**操作-> 关闭关闭**Hyper-V 管理器中。 Linux VHD 现在就可以上载到 Azure。


 
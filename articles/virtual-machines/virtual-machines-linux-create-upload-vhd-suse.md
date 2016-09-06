---
ms.openlocfilehash: f9b5518e330791aeb25027baad3c0b365f1bbb61
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="创建并上载在 Azure SUSE Linux VHD" 
    description="了解如何创建并上载 Azure 虚拟硬盘 (VHD) 包含 SUSE Linux 操作系统的系统。" 
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


# 准备 Azure SLES 或 openSUSE 的虚拟机

- [Azure 准备 SLES 11 SP3 虚拟机](#sles11)
- [准备 openSUSE 13.1 + Azure 的虚拟机](#osuse)

##先决条件##

本文假定已安装 SUSE 或 openSUSE Linux 操作系统的虚拟硬盘。 多个工具可创建.vhd 文件的形式，例如如 Hyper-V 虚拟化解决方案。 有关说明，请参阅[安装 Hyper-V 角色并配置虚拟机](http://technet.microsoft.com/library/hh846766.aspx)。 


**SLES / openSUSE 安装注意事项**

 - [SUSE Studio](http://www.susestudio.com)可以轻松地创建和管理您 SLES / openSUSE Azure 和 Hyper-V 映像。 这是推荐用于自定义您自己的 SUSE 和 openSUSE 图像的方法。 可以下载或复制到您自己的 SUSE Studio 官方图像 SUSE Studio 库中︰

  - [SLES 11 SUSE Studio 库在 Azure 的 SP3](http://susestudio.com/a/02kbT4/sles-11-sp3-for-windows-azure)
  - [SUSE Studio 库在 Azure openSUSE 13.1](https://susestudio.com/a/02kbT4/opensuse-13-1-for-windows-azure)

- 在 Azure 中不支持较新的 VHDX 格式。 您可以将磁盘转换为使用 Hyper-V 管理器或转换 vhd cmdlet 的 VHD 格式。

- 在安装 Linux 系统时建议使用标准分区，而不是 LVM （通常为许多安装默认值）。 尤其是以往任何时候都需要附加到另一个虚拟机的故障诊断操作系统磁盘，这样可以避免与克隆虚拟机，LVM 名称冲突。  如果愿意，可能在数据磁盘中使用的 LVM 或[RAID](virtual-machines-linux-configure-raid.md) 。

- OS 磁盘上未配置一个交换分区。 可以配置 Linux 代理来创建临时的资源磁盘上的交换文件。  下面的步骤中，可以找到相关详细信息。

- 所有 Vhd 必须使用大小为 1 MB 的倍数。


## <a id="sles11"> </a>准备 SUSE Linux 企业服务器 11 SP3 ##

1. 在 Hyper-V 管理器的中央窗格中，选择虚拟机。

2. 单击**连接**以打开虚拟机窗口。

3. 注册 SUSE Linux 企业系统以便进行更新的下载和安装程序包。

4. 使用最新的修补程序来更新系统︰

        # sudo zypper update

5. 从 SLES 存储库安装 Azure Linux 代理︰

        # sudo zypper install WALinuxAgent

6. 修改 grub 配置 Azure 中包含其他内核参数中的内核启动行。 为此打开"/ boot/grub/menu.lst"在文本编辑器中，并确保默认内核包含以下参数︰

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300

    这将确保所有控制台消息被都发送到第一个串行端口，可以帮助 Azure 支持调试问题。

7.  它是建议对文件进行编辑"/ 等/sysconfig/网络/dhcp"并更改`DHCLIENT_SET_HOSTNAME`与下面的参数︰

        DHCLIENT_SET_HOSTNAME="no"

8.  在"/ 等/sudoers"，注释掉或删除以下各行，如果它们存在︰

        Defaults targetpw   # ask for the password of the target user i.e. root
        ALL    ALL=(ALL) ALL   # WARNING! Only use this together with 'Defaults targetpw'!

9.  确保安装和配置为在系统启动时启动 SSH 服务器。  这通常是默认值。

10. OS 磁盘上不创建交换空间

    Azure Linux 代理可以自动配置使用本地资源磁盘设置在 Azure 上后附加到虚拟机的交换空间。 请注意，本地资源磁盘是一个*临时*磁盘时 VM deprovisioned 可能清空。 Azure Linux 代理在安装之后 （请参阅上一步），适当地修改 /etc/waagent.conf 中的下列参数︰

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

11. 运行以下命令以取消设置虚拟机，并准备将它在 Azure 上资源调配︰

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

12. 单击**操作-> 关闭关闭**Hyper-V 管理器中。 Linux VHD 现在就可以上载到 Azure。


----------

## <a id="osuse"> </a>准备 openSUSE 13.1 + ##

1. 在 Hyper-V 管理器的中央窗格中，选择虚拟机

2. 单击**连接**以打开虚拟机窗口

3. 在 shell 中运行命令`zypper lr`。 如果该命令返回的输出类似于以下 （请注意，数字可能会有所不同）︰

        # | Alias                 | Name                  | Enabled | Refresh
        --+-----------------------+-----------------------+---------+--------
        1 | Cloud:Tools_13.1      | Cloud:Tools_13.1      | Yes     | Yes
        2 | openSUSE_13.1_OSS     | openSUSE_13.1_OSS     | Yes     | Yes
        3 | openSUSE_13.1_Updates | openSUSE_13.1_Updates | Yes     | Yes

    然后像预期的那样，任何调整都需要配置存储库。

    如果该命令返回"没有资料库定义..."然后使用以下命令来添加这些 repos:

        # sudo zypper ar -f http://download.opensuse.org/repositories/Cloud:Tools/openSUSE_13.1 Cloud:Tools_13.1 
        # sudo zypper ar -f http://download.opensuse.org/distribution/13.1/repo/oss openSUSE_13.1_OSS
        # sudo zypper ar -f http://download.opensuse.org/update/13.1 openSUSE_13.1_Updates

    然后可以验证已通过运行命令添加存储库`zypper lr`再次。 在的情况下不启用相关的更新资料库之一，请使用以下命令启用它︰

        # sudo zypper mr -e [NUMBER OF REPOSITORY]


4. 请更新至最新版本的内核︰

        # sudo zypper up kernel-default

    或使用所有最新的修补程序来更新系统︰

        # sudo zypper update

5.  安装 Azure 的 Linux 代理

        # sudo zypper install WALinuxAgent

6.  修改 grub 配置 Azure 中包含其他内核参数中的内核启动行。 为此打开"/ boot/grub/menu.lst"在文本编辑器中，并确保默认内核包含以下参数︰

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300

    这将确保所有控制台消息被都发送到第一个串行端口，可以帮助 Azure 支持调试问题。 此外，从内核引导行删除以下参数如果存在︰

        libata.atapi_enabled=0 reserve=0x1f0,0x8

7.  它是建议对文件进行编辑"/ 等/sysconfig/网络/dhcp"并更改`DHCLIENT_SET_HOSTNAME`与下面的参数︰

        DHCLIENT_SET_HOSTNAME="no"

8.  **重要︰**在"/ 等/sudoers"，注释掉或删除以下各行，如果它们存在︰

        Defaults targetpw   # ask for the password of the target user i.e. root
        ALL    ALL=(ALL) ALL   # WARNING! Only use this together with 'Defaults targetpw'!

9.  确保安装和配置为在系统启动时启动 SSH 服务器。  这通常是默认值。

10. OS 磁盘上不创建交换空间

    Azure Linux 代理可以自动配置使用本地资源磁盘设置在 Azure 上后附加到虚拟机的交换空间。 请注意，本地资源磁盘是一个*临时*磁盘时 VM deprovisioned 可能清空。 Azure Linux 代理在安装之后 （请参阅上一步），适当地修改 /etc/waagent.conf 中的下列参数︰

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

11. 运行以下命令以取消设置虚拟机，并准备将它在 Azure 上资源调配︰

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

12. 确保在启动时运行的 Azure Linux 代理︰

        # sudo systemctl enable waagent.service

13. 单击**操作-> 关闭关闭**Hyper-V 管理器中。 Linux VHD 现在就可以上载到 Azure。


 
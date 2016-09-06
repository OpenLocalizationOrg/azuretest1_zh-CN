---
ms.openlocfilehash: ea7ab1359d1f2883b24573cb73c70e20fdd6d715
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="在 Azure 中的 Linux 简介 |Microsoft Azure"
    description="了解如何在 Azure 上使用 Linux 虚拟机。"
    services="virtual-machines"
    documentationCenter="python"
    authors="szarkos"
    manager="timlt"
    editor=""/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/11/2015"
    ms.author="szark"/>
#在 Azure 上 Linux 简介

本主题概述了在 Azure 的云环境中使用 Linux 虚拟机的某些方面。 部署 Linux 虚拟机是一个简单的过程，使用库中的图像。

## 身份验证︰ 用户名、 密码和 SSH 密钥

在创建 Linux 虚拟机使用 Azure 管理门户时，您需要提供用户名、 密码或 SSH 公钥。 部署 Linux 虚拟机在 Azure 上的用户名的选择受到以下限制︰ 系统帐户名称 (UID < 100) 已经存在于虚拟机都不允许，根，例如。


 - 请参阅[创建运行 Linux 虚拟机](virtual-machines-linux-tutorial.md)
 - 了解[如何使用 SSH 使用 Linux 在 Azure 上](../linux-use-ssh-key.md)


## 获取使用超级用户权限 `sudo`

在 Azure 上的虚拟机实例部署期间指定的用户帐户是特权的帐户。 Azure Linux 代理能够到根 （超级用户帐户） 使用提升的权限配置此帐户`sudo`实用程序。 一旦使用此用户帐户登录，您将能够运行命令以使用该命令语法的根

    # sudo <COMMAND>

（可选） 您可以获取根外壳使用**sudo s**。

- [在 Azure 的 Linux 虚拟机上使用超级用户权限](virtual-machines-linux-use-root-privileges.md)，请参阅


## 防火墙配置

Azure 提供将连接限制为管理门户中指定的端口的入站的数据包筛选器。 默认情况下，唯一的允许的端口是 SSH。 您可能打开其他端口访问 Linux 虚拟机上通过管理门户中配置终结点︰

 - 请参见︰[如何设置终结点的虚拟机](virtual-machines-set-up-endpoints.md)

Linux 映像在 Azure 库默认情况下不启用*iptables*防火墙。 如果需要，可以将防火墙配置来提供更多筛选。


## 主机名更改

在最初部署 Linux 映像实例时，需要提供用于虚拟机的主机名。 一旦虚拟机正在运行，此主机名发布到平台的 DNS 服务器，以便多个相互连接的虚拟机可以执行使用主机名的 IP 地址查找。

如果需要更改主机名后已部署虚拟机，请使用命令

    # sudo hostname <newname>

Azure Linux 代理包括自动检测此名称更改，并相应地配置该虚拟机保存此更改并将更改发布到平台的 DNS 服务器的功能。

 - [Azure Linux 代理用户指南](virtual-machines-linux-agent-user-guide.md)

### 云-初始化
**Ubuntu**和**CoreOS**图像利用云 init pn Azure，引导虚拟机提供附加功能。

 - [如何将自定义数据](virtual-machines-how-to-inject-custom-data.md)
 - [自定义数据并在 Microsoft Azure 的云初始化](http://azure.microsoft.com/blog/2014/04/21/custom-data-and-cloud-init-on-windows-azure/)
 - [创建使用云 Init 的 Azure 的交换分区](https://wiki.ubuntu.com/AzureSwapPartitions)
 - [如何在 Azure 上使用 CoreOS](virtual-machines-linux-coreos-how-to.md)


## 虚拟机映像捕获

Azure 提供随后可用于部署额外的虚拟机实例为图像捕获的现有虚拟机状态的能力。 Azure Linux 代理可能会回滚使用一些自定义设置过程中执行。 您可以按照下面的步骤来捕获映像作为虚拟机︰

1. 运行**waagent-取消**撤消配置自定义项。 或者**waagent-取消 + 用户**（可选） 删除资源调配和所有相关的数据期间指定的用户帐户。

2. 关机/电源关闭虚拟机。

3. 单击管理门户中的*捕获*或使用 Powershell 或 CLI 工具来捕获映像作为虚拟机。

 - 请参见︰[如何捕获 Linux 虚拟机作为模板使用](virtual-machines-linux-capture-image.md)


## 附加的磁盘

每个虚拟机都具有临时、 本地连接的*磁盘资源*。 因为在重新引导后，资源磁盘上的数据不可能会持久，它通常使用应用程序和数据的瞬时和**临时**存储的虚拟机中运行的进程。 它还用于存储页面或交换的操作系统的文件。

在 Linux 上，资源磁盘是通常由 Azure Linux 代理管理，自动装载到**/mnt/resource** （或**/mnt**上 Ubuntu 的图像）。


    >[AZURE.NOTE] 请注意资源磁盘是**临时**磁盘，并可能被删除和重新格式化时重新启动虚拟机。

在 Linux 上数据磁盘可能命名为内核的`/dev/sdc`，和用户都需要进行分区、 格式化并装载该资源。 这涉及分步教程中︰[如何附加到虚拟机的数据磁盘](virtual-machines-linux-how-to-attach-disk.md)。

 - **另请参见︰**[配置软件 RAID 在 Linux 上](virtual-machines-linux-configure-raid.md)
 

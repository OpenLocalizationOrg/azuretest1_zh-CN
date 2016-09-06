---
ms.openlocfilehash: 986edaf0bac2660fa111bcf720900d9e66b7d18c
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="Azure 的 Linux 代理用户指南" 
    description="了解如何安装和配置 Linux 代理来管理您的虚拟机交互使用 Azure 结构控制器 (waagent)。" 
    services="virtual-machines" 
    documentationCenter="" 
    authors="szarkos" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="virtual-machines" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/30/2015" 
    ms.author="szark"/>



#Azure Linux 代理用户指南

##简介

Azure Linux 代理 （/usr/sbin/waagent） 管理虚拟机和 Azure 结构控制器之间的交互。 它执行以下任务︰

* **图像资源调配**
  - 创建用户帐户
  - 配置 SSH 的身份验证类型
  - 部署 SSH 公钥和密钥对
  - 设置主机名
  - 发布平台 DNS 主机名
  - 报告 SSH 主机密钥指纹到平台
  - 管理资源磁盘
  - 设置格式并装入资源磁盘
  - 配置交换空间
* **网络连接**
  - 管理路由来提高与 DHCP 服务器平台的兼容性
  - 确保网络接口名称的稳定性
* **内核**
  - 配置虚拟 NUMA
  - 使用 /dev/random 的 Hyper-V 熵
  - 配置的根设备 （它可以是远程的） 的 SCSI 超时
* **Diagnostics**
  - 将控制台重定向到串行端口
* **SCVMM 部署**
    - 检测并引导 Linux 的 VMM 代理系统中心 Virtual Machine Manager 2012 R2 环境中运行时
* **VM 扩展**
    - 插入由 Microsoft 和合作伙伴编写到 Linux 虚拟机 (IaaS) 启用软件和配置自动化的组件
    - 在[https://github.com/Azure/azure-linux-extensions](https://github.com/Azure/azure-linux-extensions)上的 VM 扩展引用实现


##通信

信息流从平台到代理是通过两个渠道︰

* 启动时连接 DVD IaaS 部署。 此 DVD 包括包括实际的 SSH 密钥之外的所有资源调配信息 OVF 标准配置文件。

* TCP 终结点公开 REST API 用于获取部署和拓扑配置。

###获得 Linux 代理
您可以直接从最新的 Linux 代理︰

- [Linux 在 Azure 上的签署不同的通讯组提供程序](http://support.microsoft.com/kb/2805216)
- 或从[Azure 的 Linux 代理 GitHub 打开来源资料库](https://github.com/Azure/WALinuxAgent)


## 要求
以下系统已经过测试并与 Azure Linux 代理工作。 **请注意，此列表可能有所不同，从官方的列表支持微软 Azure 平台上的系统**，如下所述︰ [http://support.microsoft.com/kb/2805216](http://support.microsoft.com/kb/2805216)

###受支持的 Linux 版本

* CoreOS
* CentOS 6.2 +
* Debian 7.0 +
* Ubuntu 12.04 +
* openSUSE 12.3 +
* SLES 11 SP2 +
* Oracle Linux 6.4 +

其他受支持的系统︰

* FreeBSD 9 + （Azure Linux 代理 v2.0.10 +）


Linux 代理依赖某些系统包才能正常工作︰

* Python 2.6 +
* Openssl 1.0 +
* Openssh 5.3 +
* 文件系统实用程序︰ sfdisk，fdisk，mkfs，parted
* 密码工具︰ chpasswd，sudo
* 文本处理工具︰ sed，grep
* 网络工具︰ ip 路由


##安装

安装您的发行版软件包存储库，使用 RPM 或 DEB 包已安装和升级 Azure Linux 代理的首选的方法。

如果手动安装，应复制到 /usr/sbin/waagent waagent 脚本，然后通过运行安装︰ 

    # sudo chmod 755 /usr/sbin/waagent
    # sudo /usr/sbin/waagent -install -verbose

代理的日志文件保留在 /var/log/waagent.log。


##命令行选项

###标志

- 详细︰ 增加指定命令的详细程度
- 强制︰ 跳过某些命令的交互式确认

###命令

- 帮助︰ 列出了支持的命令和标志。

- 安装︰ 手动安装代理
 * 检查系统中所需的依赖项

 * 创建 SysV init 脚本 (/ etc/init.d/waagent)，logrotate 配置文件 (/etc/logrotate.d/waagent 和配置映像在引导上运行 init 脚本

 * 写入到 /etc/waagent.conf 的示例配置文件

 * 任何现有的配置文件被移动到 /etc/waagent.conf.old

 * 检测到内核版本并应用该 VNUMA 替代方法，如有必要

 * 可能会影响网络的移动 udev 规则 (/ lib/udev/rules.d/75-persistent-net-generator.rules、 /etc/udev/rules.d/70-persistent-net.rules) 到/var/lib/waagent /  

- 卸载︰ 删除 waagent 和相关的文件
 * 注销系统中的 init 脚本并将其删除

 * 删除 logrotate 配置，并在 /etc/waagent.conf waagent 配置文件

 * 还原已在安装过程中移动的任何移动的 udev 规则

 * 不支持自动恢复的 VNUMA 替代方法，请编辑 GRUB 配置文件手动重新启用 NUMA，如果需要。

- 取消︰ 尝试清理系统并使其适用于重新调配。 此操作将删除下列︰
 * 所有 SSH 主机键 （如果 Provisioning.RegenerateSshHostKeyPair 是在配置文件中的 y）

 * 在 /etc/resolv.conf 中的名称服务器配置

 * 超级用户口令从 /etc/shadow （如果 Provisioning.DeleteRootPassword 是在配置文件中的 y）

 * 缓存的 DHCP 客户端租约

 * 重置为 localhost.localdomain 的主机名

 **警告︰**取消并不保证该图像是清除所有的敏感信息，适合于重新分发。

- 取消 + 用户︰ 执行下面的取消 （上图） 的所有项目，并还删除最后一个配置的用户帐户 （来自 /var/lib/waagent） 和相关联的数据。 消除资源调配使它可以捕获并重新使用以前在 Azure 中调配的图像时，此参数。

- 版本︰ 显示的 waagent 版本

- serialconsole︰ 配置 GRUB 标记 ttyS0 （第一个串行端口） 作为启动控制台。 这将确保，发送到串行端口，可用于调试内核启动日志。

- 守护进程︰ 作为守护程序来管理平台的交互运行 waagent。
   对 waagent waagent 的初始化脚本中指定了此参数。

##配置

配置文件 (/ etc/waagent.conf) 控制 waagent 的操作。 下面显示了一个示例配置文件︰
    
    #
    # Azure Linux Agent Configuration   
    #
    Role.StateConsumer=None 
    Role.ConfigurationConsumer=None 
    Role.TopologyConsumer=None
    Provisioning.Enabled=y
    Provisioning.DeleteRootPassword=n
    Provisioning.RegenerateSshHostKeyPair=y
    Provisioning.SshHostKeyPairType=rsa
    Provisioning.MonitorHostName=y
    ResourceDisk.Format=y
    ResourceDisk.Filesystem=ext4
    ResourceDisk.MountPoint=/mnt/resource 
    ResourceDisk.EnableSwap=n 
    ResourceDisk.SwapSizeMB=0
    LBProbeResponder=y
    Logs.Verbose=n
    OS.RootDeviceScsiTimeout=300
    OS.OpensslPath=None

下面将详细介绍的各种配置选项。 配置选项分为三种类型;布尔值、 字符串或整数。 布尔值的配置选项可以指定为"y"或"n"。 "无"可用于某些字符串类型配置项所述以下特殊关键字。

**Role.StateConsumer:**

类型︰ 字符串  
默认值︰ 无

如果指定一个可执行程序的路径，则它会调用 waagent 已设置图像和"就绪"状态将汇报给结构。 指定对该程序的参数将为"就绪"。 代理不会等待程序继续之前返回。

**Role.ConfigurationConsumer:**

类型︰ 字符串  
默认值︰ 无

如果指定一个可执行程序的路径，则程序被当结构指示配置文件是可用的虚拟机。 作为可执行文件的参数提供 XML 配置文件的路径。 这可能会多次调用配置文件发生更改时。 附录中提供了一个示例文件。 该文件的当前路径是 /var/lib/waagent/HostingEnvironmentConfig.xml。

**Role.TopologyConsumer:**

类型︰ 字符串  
默认值︰ 无

如果指定一个可执行程序的路径，则程序被当结构指示新的网络拓扑布局是可用的虚拟机。作为可执行文件的参数提供 XML 配置文件的路径。 这可能会多次调用只要网络拓扑发生变化 （例如，由于服务例如康复）。 附录中提供了一个示例文件。 此文件的当前位置是 /var/lib/waagent/SharedConfig.xml。

**Provisioning.Enabled:**

类型︰ 布尔型  
默认值︰ y

这样，用户可以启用或禁用代理中的资源调配功能。 有效值为"y"或"n"。 如果禁用资源调配，则保留图像中的 SSH 主机和用户密钥，并指定在资源调配 API Azure 中的任何配置被忽略。

**注意︰**此参数默认为"n"调配使用云 init 的 Ubuntu 云图像。

**Provisioning.DeleteRootPassword:**

类型︰ 布尔型  
默认值︰ n

如果设置，阴影文件中的根密码将清除设置过程中。

**Provisioning.RegenerateSshHostKeyPair:**

类型︰ 布尔型  
默认值︰ y

如果在资源调配过程从 /etc/ssh/删除组，所有 SSH 主机密钥对 （ecdsa、 dsa 和 rsa）。 并生成一个新密钥对。

新的密钥对的加密类型进行配置的 Provisioning.SshHostKeyPairType 项。 请注意一些分布在 SSH 守护程序 （例如，在重新启动时） 重新启动时，将重新创建 SSH 密钥对的任何丢失的加密类型。

**Provisioning.SshHostKeyPairType:**

类型︰ 字符串  
默认值︰ rsa

这可设为支持 SSH 守护程序在虚拟机上的一个加密算法类型。 通常受支持的值是"rsa"、"dsa"和"ecdsa"。 请注意，在 Windows 上的"putty.exe"不支持"ecdsa"。 因此，如果打算在 Windows 上使用 putty.exe 连接到 Linux 部署，请使用"rsa"或"dsa"。

**Provisioning.MonitorHostName:**

类型︰ 布尔型  
默认值︰ y

如果设置，waagent 将监视 Linux 虚拟机的主机名更改 （如"主机名"命令返回） 并自动更新以反映更改图像中的网络配置。 要将更改后的名称推送到 DNS 服务器，网络将重新启动虚拟机中。 这样会简单地说互联网连接中断。

**ResourceDisk.Format:**

类型︰ 布尔型  
默认值︰ y

如果设置，平台提供的资源磁盘格式化并装入 waagent 如果要求用户在"ResourceDisk.Filesystem"的文件系统类型是"ntfs"之外的任何内容。 Linux (83) 类型的单个分区将可在磁盘上。 请注意是否成功装载不会格式化这个磁盘分区。

**ResourceDisk.Filesystem:**

类型︰ 字符串  
默认值︰ ext4

该选项用于指定资源磁盘的文件系统类型。 受支持的值不同的 Linux 分发。 如果字符串不是 X，然后 mkfs。X 应该存在于 Linux 映像。 SLES 11 图像通常应使用 ext3'。 FreeBSD 图像应在此处使用 ufs2。

**ResourceDisk.MountPoint:**

类型︰ 字符串  
默认值: / mnt/资源 

此参数指定在装载资源磁盘的路径。 请注意，资源磁盘是一个*临时*磁盘时 VM deprovisioned 可能清空。

**ResourceDisk.EnableSwap:**

类型︰ 布尔型  
默认值︰ n 

如果设置，交换文件 (/ 交换文件) 的资源磁盘上创建并添加到系统的交换空间。

**ResourceDisk.SwapSizeMB:**

类型︰ 整型  
默认值︰ 0

交换文件大小以兆字节为单位。

**LBProbeResponder:**

类型︰ 布尔型  
默认值︰ y

如果设置，waagent 将在响应以负载平衡器探测从平台 （如果存在）。

**Logs.Verbose:**

类型︰ 布尔型  
默认值︰ n

如果集合日志的详细程度放大的。 Waagent 将记录到 /var/log/waagent.log，并利用旋转日志的系统 logrotate 功能。

**操作系统。RootDeviceScsiTimeout:**

类型︰ 整型  
默认︰ 300

这将以秒为单位的 OS 磁盘和数据驱动器上配置 SCSI 超时。 如果未设置的系统将使用默认值。

**操作系统。OpensslPath:**

类型︰ 字符串  
默认值︰ 无

这可以用于指定二进制加密操作使用 openssl 的可选路径。



##Ubuntu 云图像

请注意 Ubuntu 云图像利用[云初始化](https://launchpad.net/ubuntu/+source/cloud-init)执行很多否则为由 Azure Linux 代理的配置任务。  请注意以下几点差异︰


- **Provisioning.Enabled**默认为"n"云初始化用于执行资源调配任务的 Ubuntu 云图像。

- 以下配置参数对使用云初始化管理资源磁盘和交换空间的 Ubuntu 云图像无效︰

 - **ResourceDisk.Format**
 - **ResourceDisk.Filesystem**
 - **ResourceDisk.MountPoint**
 - **ResourceDisk.EnableSwap**
 - **ResourceDisk.SwapSizeMB**

- 请参阅以下资源配置资源磁盘挂载点并在资源调配过程中换上 Ubuntu 云图像的空间︰

 - [Ubuntu Wiki︰ 配置交换分区](http://go.microsoft.com/fwlink/?LinkID=532955&clcid=0x409)
 - [注入到 Azure 的虚拟机中的自定义数据](virtual-machines-how-to-inject-custom-data.md)

 
---
ms.openlocfilehash: bbe393eadaf70a4be75048e416493a9cb94c6d53
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="从 Github 的 Azure Linux 代理更新到最新版本"
    description="了解如何从 Github Azure Linux 代理更新为您在 Azure 中的 Linux 虚拟机。"
    services="virtual-machines"
    documentationCenter=""
    authors="SuperScottz"
    manager="timlt"
    editor=""/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/16/2015"
    ms.author="mingzhan"/>


# 从 Github 的 Azure Linux 代理更新到最新版本

若要更新您的[Azure Linux 代理](https://github.com/Azure/WALinuxAgent)，您必须已经︰

1. 在 Azure 中运行 Linux 虚拟机
2. 您连接到使用 SSH 的 Linux 虚拟机

> [AZURE.NOTE] 如果从 Windows 计算机，您将执行此任务，可以使用到您的 Linux 机器 Putty ssh。 有关详细信息，请参阅[如何登录到虚拟机运行 Linux](virtual-machines-linux-how-to-log-on.md)。

Azure 认可 Linux distros 已将 Azure Linux 代理包放在其资料库、 所以请检查和安装最新版本的 Distro 存储库从第一次如果可能的话。  

Ubuntu，只需键入︰
     
    #sudo apt-get install walinuxagent

然后在 CentOS，键入︰

    #sudo yum install waagent

对于 Oracle Linux，请确保加载项资源库文件中启用`/etc/yum.repo.d/public-yum-ol6.repo`或`/etc/yum.repo.d/public-yum-ol7.repo`，然后键入︰

    #sudo yum install WALinuxAgent

通常这是所有需要但如果由于某种原因您需要直接从 https://github.com 安装使用以下步骤。 


## 安装 wget

VM 使用 SSH 登录。 

通过键入安装的 wget （有一些，不要安装它默认情况下，如 Redhat、 CentOS 和 Oracle Linux 版本 6.4 和 6.5 中的 distros）`#sudo yum install wget`在命令行上。


## 下载最新版本

在 web 页中，打开[在 Github 的 Azure Linux 代理发行](https://github.com/Azure/WALinuxAgent/releases)，了解最新的版本数。 (您可以通过键入来找到您的当前版本`#waagent --version`。)

###版本 2.0.x，请键入︰

    #wget https://raw.githubusercontent.com/Azure/WALinuxAgent/WALinuxAgent-[version]/waagent  

   下面的行作为示例使用版本 2.0.14:

    #wget https://raw.githubusercontent.com/Azure/WALinuxAgent/WALinuxAgent-2.0.14/waagent  

###版本 2.1.x 或更高版本，键入︰
  
    #wget https://github.com/Azure/WALinuxAgent/archive/WALinuxAgent-[version].zip 
    #unzip WALinuxAgent-[version].zip
    #cd WALinuxAgent-[version]

   下面的行作为示例使用 2.1.0 版︰

    #wget https://github.com/Azure/WALinuxAgent/archive/WALinuxAgent-2.1.0.zip
    #unzip WALinuxAgent-2.1.0.zip  
    #cd WALinuxAgent-2.1.0

##安装 Linux 代理

###版本 2.0.x，使用︰

 使 waagent 可执行文件

    #chmod +x waagent

 将新的可执行文件复制到 /usr/sbin /
   
  对于大多数 Linux，使用
         
      #sudo cp waagent /usr/sbin

  对于 CoreOS，使用︰

    #sudo cp waagent /usr/share/oem/bin/
 
###版本 2.1.x，使用︰

您可能需要安装包`setuptools`第一次，看到[这里](https://pypi.python.org/pypi/setuptools)。 然后运行下面︰

    #sudo python setup.py install

## 重新启动 waagent 服务

对于大多数的 linux Distros:

    #sudo service waagent restart

对于 Ubuntu，使用︰

    #sudo service walinuxagent restart

对于 CoreOS，使用︰

    #sudo systemctl restart waagent 

## 确认的 Azure Linux 代理版本
   
    #waagent -version

对于 CoreOS，上面命令可能无法工作。

您将看到 Linux 代理版本已更新为新版本。

关于 Azure Linux 代理的详细信息，请参阅[Azure Linux 代理程序自述文件](https://github.com/Azure/WALinuxAgent)。



 
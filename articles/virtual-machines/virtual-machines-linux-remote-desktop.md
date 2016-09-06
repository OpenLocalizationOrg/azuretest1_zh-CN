---
ms.openlocfilehash: d0aa9577d17c6070c4fe1ed50bc83018893e880b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="通过 xrdp 使用远程桌面连接 Microsoft Azure Linux 虚拟机。"
    description="了解如何安装和配置远程桌面在 Microsoft Azure Linux 虚拟机上。"
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
    ms.date="08/31/2015"
    ms.author="mingzhan"/>


#通过 xrdp 使用远程桌面连接 Microsoft Azure Linux 虚拟机

##概述

RDP （远程桌面协议） 是我们使用 RDP 远程连接 Linux 虚拟机但是对于 Windows，使用一种专用协议如何？

本指南将为您提供了答案 ！ 它将帮助您安装和配置在您 Microsoft Azure Linux VM(virtual machine) 的 xrdp，您将能够从 Windows 计算机连接与远程桌面。

Xrdp 是一个开源 RDP 服务器上，以便您可以从 Windows 计算机与远程桌面连接您的 Linux 服务器。 它执行 VNC （虚拟网络计算） 比更好些。 VNC 有此连续"JPEG"质量和速度慢的问题，而 RDP 是快速且非常明晰。
 

> [AZURE.NOTE] 您必须已经运行 Linux Microsoft Azure VM。 若要创建和设置 Linux 虚拟机，请参阅[Azure Linux 虚拟机教程](virtual-machines-linux-tutorial.md)。


##为远程桌面创建终结点
我们将在本文档中使用的远程桌面默认端点 3389。 因此为与下面类似 Linux VM 远程桌面设置 3389 终结点︰


![image](./media/virtual-machines-linux-remote-desktop/no1.png)


如果您不知道如何设置到 VM 的终结点，请参见[指南](virtual-machines-set-up-endpoints.md)。


##Gnome 桌面安装

连接到 Linux VM 通过 putty，并安装`Gnome Desktop`。

Red Hat 系列 Linux，请使用︰

    #sudo yum install gnome* "xorg*" -y

Debian 和 Ubuntu，请使用︰

    #sudo apt-get update
    #sudo apt-get install ubuntu-desktop


对于 OpenSUSE，使用︰

    #sudo zypper -y install gnome-session


##安装 xrdp

Red Hat 系列 Linux，您需要添加 EPEL 存储库 Linux VM 中第一次安装 xrdp 软件包通过`yum`，使用︰

    #sudo rpm -ivh http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
    #sudo yum -y install xrdp tigervnc-server tigervnc-server-module xterm

Debian 和 Ubuntu Linux，请使用︰

    #sudo apt-get install xrdp


对于 OpenSUSE，使用︰

> [AZURE.NOTE] OpenSUSE 版本，且您使用的版本是到下命令，下面是示例命令更新`OpenSUSE 13.2`。

    #sudo zypper in http://download.opensuse.org/repositories/X11:/RemoteDesktop/openSUSE_13.2/x86_64/xrdp-0.9.0git.1401423964-2.1.x86_64.rpm
    #sudo zypper install tigervnc xorg-x11-Xvnc xterm remmina-plugin-vnc


##启动 xrdp 和 xdrp 服务在启动设置

Red Hat 系列 Linux，请使用︰

    #sudo service xrdp start
    #sudo chkconfig xrdp on


对于 OpenSUSE，使用︰

    #sudo systemctl start xrdp
    #sudo systemctl enable xrdp
 

##如果您使用的 Red Hat Linux 家族，禁用 iptables 

使用︰

    #sudo service iptables stop


##如果您正在使用 Ubuntu 版本高于 Ubuntu 12.04LTS，使用 xfce

因为当前的 xrop 不能支持从 Ubuntu 版本比 Ubuntu 12.04LTS Gnome 桌面，我们将使用`xfce`桌面相反。

安装`xfce`，使用︰

    #sudo apt-get install xubuntu-desktop

然后启用`xfce`，使用︰
    
    #echo xfce4-session >~/.xsession

编辑配置文件`/etc/xrdp/startwm.sh`，使用︰

    #sudo vi /etc/xrdp/startwm.sh   

添加行`xfce4-session`的行之前`/etc/X11/Xsession`。

重新启动 xrdp 服务，请使用︰

    #sudo service xrdp restart


##连接您的 Linux 虚拟机从一台 Windows 计算机
Windows 计算机，在启动远程桌面客户端、 输入您的 Linux 虚拟机的 DNS 名称，或转至`Dashboard`Azure 门户，单击 VM 的`Connect`，您将会看到登录窗口的下方︰

![image](./media/virtual-machines-linux-remote-desktop/no2.png)

使用登录`user` & `password`为您的 Linux 虚拟机，并立即享受从 Microsoft Azure Linux VM 远程桌面 ！


##下一步 
使用 xrdp 的详细信息，您可以查阅[此处](http://www.xrdp.org/)。





 







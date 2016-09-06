---
ms.openlocfilehash: d05d8c54908824919a77fcfc559c94eea6dee63c
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="登录到运行 Linux 在 Azure 中的虚拟机"
    description="了解如何登录到使用安全外壳协议 (SSH) 客户端运行 Linux Azure 的虚拟机。"
    services="virtual-machines"
    documentationCenter=""
    authors="squillace"
    manager="timlt"
    editor=""/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/25/2015"
    ms.author="rasquill"/>




#如何登录到运行 Linux 的虚拟机 #

对于运行 Linux 操作系统的虚拟机，您可以使用安全外壳协议 (SSH) 客户端登录。

您将需要在您想要用来登录到虚拟机的计算机上安装 SSH 客户端。 有许多可供选择的 SSH 客户端程序。 以下是可能的选项︰

- 在计算机上运行 Windows 操作系统的系统，您可以使用如 PuTTY SSH 客户端。 有关详细信息，请参阅[PuTTY 下载页面](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)。
- 运行 Linux 操作系统的计算机，您可能需要使用 OpenSSH 如 SSH 客户端。 有关详细信息，请参阅[OpenSSH](http://www.openssh.org/)。

>[AZURE.NOTE] 有关要求和故障排除技巧，请参阅[连接到 RDP 或 SSH Azure 的虚拟机](http://go.microsoft.com/fwlink/p/?LinkId=398294)。

此过程说明了如何使用 PuTTY 程序来访问虚拟机。

1. 从[管理门户](http://manage.windowsazure.com)中找到的**主机名**和**端口信息**。 您可以找到您需要从虚拟机的仪表板的信息。 单击虚拟机名称，查找中的仪表板的**快速概览**部分的**SSH 详细信息**。

    ![获取 SSH 的详细信息](./media/virtual-machines-linux-how-to-log-on/sshdetails.png)

2. 打开的 PuTTY 程序。

3. 输入主机名和端口信息从控制板，收集，然后单击**打开**。

    ![打开 PuTTY](./media/virtual-machines-linux-how-to-log-on/putty.png)

4. 登录到虚拟机使用计算机创建时所指定的帐户上。 有关如何创建虚拟机时使用用户名和密码的详细信息，请参阅[创建虚拟机运行 Linux](virtual-machines-linux-tutorial.md)。

    ![登录到虚拟机](./media/virtual-machines-linux-how-to-log-on/sshlogin.png)

>[AZURE.NOTE] VMAccess 扩展可以帮助您重新设置密码的 SSH 密钥如果您忘记了它。 如果您忘记了用户名，您可以使用扩展来创建一个新使用 sudo 颁发机构。 有关说明，请参阅[如何重置密码或 SSH 的 Linux 虚拟机]。

您现在可以使用虚拟机就像使用任何其他服务器一样。

<!-- LINKS -->
[如何重置密码或 SSH 的 Linux 虚拟机]: http://go.microsoft.com/fwlink/p/?LinkId=512138

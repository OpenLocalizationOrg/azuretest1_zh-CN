---
ms.openlocfilehash: 39a93536bb804dedc088bd1e51564dd2761537e6
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties writer="kathydav" editor="tysonn" manager="timlt" />

1. 登录到[Azure 的门户](http://manage.windowsazure.com)。 如果您没有订阅尚未，检出[免费试用](http://azure.microsoft.com/pricing/free-trial/)优惠。

2. 在窗口底部的命令栏中，单击**新建**。

3. 在**计算**，单击**虚拟机**，，然后单击**剪辑库中**。

    ![创建新的虚拟机][Image1]

4. 在**SUSE**组下，选择 OpenSUSE 虚拟机映像，然后单击箭头，以继续。

5. 在第一个**虚拟机配置**页上︰

    - 键入一个**虚拟机的名称**，例如"testlinuxvm"。
    - 验证**层**，并选取一个**尺寸**。 将层确定可以从选择的大小。 大小将影响使用它，也如多少数据磁盘的配置选项可以将附加的成本。 有关详细信息，请参阅[虚拟机大小](../articles/virtual-machines-size-specs.md)。
    - 键入**新用户的名称**，或接受默认设置，即 $ **azureuser**。 该名称将添加到列表 Sudoers 文件。
    - 决定哪种类型的**身份验证**使用。 常规密码指南，请参阅[强密码](http://msdn.microsoft.com/library/ms161962.aspx)。

6. 在下一次**虚拟机配置**页上︰

    - 使用默认设置**创建新的云服务**。
    - 在**DNS 名称**框中，键入一个有效的 DNS 名称要使用的地址，如"testlinuxvm"的一部分。
    - 在**区域中的相似性组/虚拟网络**框中，选择该虚拟映像所在的区域。
    - 在**终结点**，请 SSH 终结点。 您可以立即将其他人添加或添加、 更改或创建虚拟机后将其删除。

    >[AZURE.NOTE] 创建虚拟机时，如果您希望虚拟机使用的虚拟网络，您**必须**指定虚拟网络。 创建虚拟机后，无法添加虚拟机与虚拟网。 有关详细信息，请参阅[虚拟网络概述](virtual-networks-overview.md)。

7.  在最后一个**虚拟机配置**页上，保留默认设置，然后单击复选标记来完成。

门户网站列出**虚拟机**下新的虚拟机。 状态报告**（资源调配）**，而是设置虚拟机。 **运行**报告状态，您可以继续下一步。

##连接到虚拟机

您将使用 SSH 或 PuTTY 来连接到虚拟机，具体取决于您将从连接的计算机上的操作系统︰

- 从运行 Linux 的计算机，使用 SSH。 在命令提示符下，键入︰

    `$ ssh newuser@testlinuxvm.cloudapp.net -o ServerAliveInterval=180`

    键入用户的密码。

- 从运行 Windows 的计算机，使用 PuTTY。 如果您没有安装它，从[PuTTY 下载网页][PuTTYDownload]下载。

    在您的计算机上保存到目录的**putty.exe** 。 打开命令提示窗口，定位到该文件夹中，并运行**putty.exe**。

    键入的主机名，如"testlinuxvm.cloudapp.net"，并键入"22"**端口**。

    ![PuTTY 屏幕][Image6]  

##更新虚拟机 （可选）

1. 正在连接到虚拟机后，您可以选择安装系统更新和修补程序。 若要运行此更新，请键入︰

    `$ sudo zypper update`

2. 选择**软件**，然后**在线更新**，以列出可用的更新。 选择**接受**以开始安装和应用 （除了可选的属性中） 的所有新补丁。

3. 安装完成后，选择**完成**。  现在，您的系统是最新的。

[PuTTYDownload]: http://www.puttyssh.org/download.html

[Image1]: ./media/create-and-configure-opensuse-vm-in-portal/CreateVM.png

[Image6]: ./media/create-and-configure-opensuse-vm-in-portal/putty.png

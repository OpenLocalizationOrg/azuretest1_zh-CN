---
ms.openlocfilehash: 46d04ad6db5060305195f153e9c5b4760555fae6
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties writer="kathydav" editor="tysonn" manager="jeffreyg" /> 

**注意**︰ 本文建立虚拟机未连接到虚拟网络。 如果您希望您的虚拟机使用虚拟网络，以便您可以直接通过主机名连接到您的虚拟机或设置跨内部连接，请使用**剪辑库中的**方法和创建虚拟机时指定虚拟网络。 有关虚拟网络的详细信息，请参阅[Azure 虚拟网络概述](http://go.microsoft.com/fwlink/p/?LinkID=294063)。

1. 登录到 Azure 管理门户使用 Azure 帐户。
2. 在管理门户中，在左下角的网页上，单击**+ 新建**，单击**虚拟机**，，然后单击**剪辑库中的**。

    ![创建新的虚拟机][Image1]

3. 从**平台的图像**，选择 CentOS 虚拟机映像，然后单击页的底部右侧的下一个箭头。
    
4. 在**虚拟机配置**页上，提供了以下信息︰
    - 提供了一个**虚拟机的名称**，例如"testlinuxvm"。
    - 指定一个**新的用户名称**，如"新"，将被添加到列表 Sudoers 文件。
    - 在**新密码**框中，键入一个[强密码](http://msdn.microsoft.com/library/ms161962.aspx)。
    - 在**确认密码**框中，重新键入密码。
    - 从下拉列表中选择适当的**大小**。

    单击下一步以继续。
    
5. 在**虚拟机模式**页中，提供以下信息︰
    - 选择**独立的虚拟机**。
    - 在**DNS 名称**框中，键入一个有效的 DNS 地址。  例如，"testlinuxvm"
    - 在**存储帐户**框中，选中**使用自动生成的存储帐户**。
    - 在**区域中的相似性组/虚拟网络**框中，选择该虚拟映像所在的区域。

    单击下一步以继续。

6. 在**虚拟机选项**页中，在**可用性设置**框中选择**（无）** 。

    单击复选标记以继续。
    
7. 请等待 Azure 准备您的虚拟机。

##配置终结点
创建虚拟机后，为了远程连接配置终结点。

1. 在管理门户中，单击**虚拟机**，单击您的新虚拟机的名称然后单击**终结点**。

2. 单击底部的页，**编辑终结点**并编辑 SSH 终结点，以便其**公用端口**是 22。

##连接到虚拟机
虚拟机已设置和配置终结点时您可以连接到使用 SSH 或 PuTTY。

###使用 SSH 连接
如果您正在使用 Linux，连接到虚拟机使用 SSH。  在命令提示符下运行︰

    $ ssh newuser@testlinuxvm.cloudapp.net -o ServerAliveInterval=180

输入用户的密码。

###使用 PuTTY 连接
如果您使用的 Windows 计算机，连接到 VM 使用 PuTTY。 PuTTY 可以从[下载页面下载 PuTTY][PuTTYDownLoad]下载。 

1. 下载并保存到目录的**putty.exe** ，您的计算机上。 打开命令提示窗口，定位到该文件夹，并执行**putty.exe**。

2. 输入**端口**的**主机名**和"22"的"testlinuxvm.cloudapp.net"。
![PuTTY 屏幕][Image6]  

##更新虚拟机 （可选）
一旦连接到虚拟机，您可以选择安装更新。 运行︰

    $ sudo yum update

请再次输入密码。  更新安装，请稍候。


[PuTTYDownload]: http://www.puttyssh.org/download.html

[Image1]: ./media/create-and-configure-centos-vm-in-portal/CreateVM.png

[Image6]: ./media/create-and-configure-centos-vm-in-portal/putty.png

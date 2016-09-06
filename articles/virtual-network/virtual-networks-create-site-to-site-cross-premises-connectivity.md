---
ms.openlocfilehash: 62033c9aafbabed11d95fe71ef941efa65ca6eca
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="教程︰ 创建站点到站点连接的跨部署虚拟网络" 
    description="了解本教程中创建具有跨内部连接的 Azure 虚拟网络。" 
    services="virtual-network" 
    documentationCenter="" 
    authors="cherylmc" 
    manager="adinah" 
    editor="tysonn"/>

<tags 
    ms.service="virtual-network" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/17/2015" 
    ms.author="cherylmc"/>



# 教程︰ 创建站点到站点连接的跨部署虚拟网络

本教程将带领您一步步地创建一个示例跨部署虚拟网络站点到站点连接。 

如果您想要创建一个仅云的虚拟网络，请参阅[教程︰ 在 Azure 创建 Cloud-Only 虚拟网络](../virtual-machines/create-virtual-network.md)。 如果您想要使用的证书和 VPN 客户端创建点到站点 VPN，请参阅[配置点到站点 VPN 管理门户中](http://go.microsoft.com/fwlink/p/?LinkId=296653)。

本教程假定您已经使用 Azure 没有经验。 它应帮助您熟悉创建虚拟网络示例跨部署所需的步骤。 如果您正在寻找设计方案和有关虚拟网络的高级的信息，请参阅[Azure 虚拟网络概述](../virtual-network/virtual-networks-overview.md)。

在完成本教程之后, 将有示例跨部署虚拟网络。 下图显示的详细信息，根据在本教程中的示例设置。

![](./media/virtual-networks-create-site-to-site-cross-premises-connectivity/CreateCrossVnet_12_TutorialCrossPremVNet.png)

此图，另一个的副本可用于描述自己的跨部署虚拟网络，请参阅[示例跨内部教程主题从虚拟网络图](http://gallery.technet.microsoft.com/Example-cross-premises-e5ecb8bb)。

请注意，使用在本教程中的示例配置设置未自定义您的组织的网络。 如果您将虚拟网络配置并使用本主题中介绍的示例配置设置的站点到站点连接，它将不工作。 要配置跨部署虚拟网络工作，您必须与您的 IT 部门和网络管理员联系，以获得正确的设置工作。 有关详细信息，请参阅本主题的**先决条件**部分。

有关添加虚拟机和扩展到 Azure 虚拟网络的内部活动目录的信息，请参阅以下资源︰

-  [如何向自定义创建的虚拟机](http://go.microsoft.com/fwlink/p/?LinkID=294356)

-  [安装在 Azure 的虚拟网络中复制 Active Directory 域控制器](http://go.microsoft.com/fwlink/p/?LinkId=299877)

有关部署 AD DS 在 Azure 的虚拟机的指南，请参阅[部署 Windows 服务器 Active Directory 在 Azure 的虚拟机的准则](http://msdn.microsoft.com/library/windowsazure/jj156090.aspx)。

其他的虚拟网络配置过程和设置，请参阅[Azure 虚拟网络配置任务](http://go.microsoft.com/fwlink/p/?LinkId=296652)。

##  目标

在本教程中，您将了解︰

-  如何设置一个示例跨内部 Azure 的虚拟网络，可以向其中添加 Azure 服务。

-  如何配置虚拟网络与组织的网络进行通信。

##  先决条件

-  至少一个有效的、 活动的 Azure 订阅 Microsoft 帐户。  如果您已经没有 Azure 的订阅，您可以注册在[尝试 Azure](http://azure.microsoft.com/pricing/free-trial/)的免费试用版。 如果您订阅了 MSDN，请参阅[Microsoft Azure 特殊定价︰ MSDN、 MPN 和 Bizspark 好处](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)。

如果您使用本教程为您的组织配置自定义的工作跨部署虚拟网络，则需要以下︰

-  专用 IPv4 地址中的空格 （CIDR 表示法） 的虚拟网络和它的子网。

-  名称和内部 DNS 服务器的 IP 地址。

-  VPN 设备与公用 IPv4 地址。 若要完成此向导，您将需要的 IP 地址。 VPN 设备不能位于网络地址转换器 (NAT) 的后面，并且必须满足最基本的设备标准。 有关更多信息，请参见[关于 VPN 设备的虚拟网络](http://go.microsoft.com/fwlink/p/?LinkID=248098)。 

    注意︰ 您可以使用路由和远程访问服务 (RRAS) 中 Windows Server 作为 VPN 解决方案的一部分。 但是，本教程不会引导您完成 RRAS 配置步骤。 
    RRAS 配置信息，请参阅[路由和远程访问服务的模板](http://msdn.microsoft.com/library/windowsazure/dn133801.aspx)。 

-  遇到与配置路由器，实现 IPsec 隧道模式连接或人，可帮助您使用此步骤。

-  总结您的内部网络 （也称作本地网络） 可到达位置的地址空间 （在 CIDR 表示法） 的集合。


## 高级别的步骤

1.  [创建虚拟网络](#CreateVN)

2.  [启动网关并收集有关您的网络管理员的信息](#StartGateway)

3.  [配置 VPN 设备](#ConfigVPN)

##  <a name="CreateVN">创建虚拟网络</a>

若要创建一个示例虚拟网络连接到公司网络︰

1.  登录到[Azure 的管理门户](http://manage.windowsazure.com/)。

2.  在屏幕的左下角，单击**新建**。 在导航窗格中，单击**网络**，，然后单击**虚拟网络**。 单击**创建自定义**启动配置向导。 

    ![](./media/virtual-networks-create-site-to-site-cross-premises-connectivity/CreateCrossVnet_01_OpenVirtualNetworkWizard.png)

3.  在**虚拟的网络详细信息**页上，输入以下信息，然后单击右下角上的下箭头。 在详细信息页上的设置的详细信息，请参阅[关于配置虚拟网络使用管理门户](http://go.microsoft.com/fwlink/p/?LinkID=248092)中的**虚拟网络详细信息**一节。

    -  **名称︰**命名您的虚拟网络。 在本教程中的示例，请键入**YourVirtualNetwork**。

    -  **位置︰**从下拉列表中，选择所需的区域。 在 Azure 数据中心位于指定的区域中，将创建虚拟网络。

    
4.  在**DNS 服务器和 VPN 连接**页面上，输入以下信息，然后单击右下方向前箭头。 

> [AZURE.NOTE] 可以选择两个**点到站点**和并发此页上的**站点对站点**配置。 对于本教程，我们将选择配置仅**站点到站点**。 在此页上的设置的详细信息，请参阅[关于配置虚拟网络使用管理门户](http://go.microsoft.com/fwlink/p/?LinkID=248092)中的**DNS 服务器和 VPN 连接**页。

    -  **DNS SERVERS:** Enter the DNS server name and IP address that you want to use for name resolution. Typically this would be a DNS server that you use for on-premises name resolution. This setting does not create a DNS server. For the example in this tutorial, type **YourDNS** for the name and **10.1.0.4** for the IP address.
    -  **Configure Point-To-Site VPN:** Leave this field blank. 
    -  **Configure Site-To-Site VPN:** Select checkbox.
    -  **LOCAL NETWORK:** Select **Specify a New Local Network** from the drop-down list.
 
    ![](./media/virtual-networks-create-site-to-site-cross-premises-connectivity/CreateCrossVNet_03_DNSServersandVPNConnectivity.png)

5.  在**站点对站点连接**页中，输入下面的信息，然后单击右下角的页中的复选标记。 在此页上的设置的详细信息，请参阅[关于配置虚拟网络使用管理门户](http://go.microsoft.com/fwlink/p/?LinkID=248092)的**站点对站点连接**页面部分。 

    -  **名称︰**在本教程中的示例，请键入**YourCorpHQ**。

    -  **VPN 设备 IP 地址︰**在本教程中的示例，请键入**3.2.1.1**。 否则，输入您的 VPN 设备的公用 IP 地址。 如果您没有此信息，您需要在今后使用向导中的下一个步骤之前获取它。 请注意，您的 VPN 设备不能背后 nat。 VPN 设备的详细信息，请参阅[关于 VPN 设备的虚拟网络](http://msdn.microsoft.com/library/windowsazure/jj156075.aspx)。

    -  **地址空间︰**在本教程中的示例，请键入**10.1.0.0/16**。
    -  **将地址空间添加︰**本教程不需要额外地址空间。

    ![](./media/virtual-networks-create-site-to-site-cross-premises-connectivity/CreateCrossVnet_04_SitetoSite.png)

6.  在**虚拟的网络地址空间**页上，输入下面的信息，然后单击右下方以配置您的网络上的选中标记。 

    地址空间必须是专用地址范围内，指定 CIDR 表示法 （按照指定的 RFC 1918） 10.0.0.0/8、 172.16.0.0/12 或 192.168.0.0/16 地址空间中。 在此页上的设置的详细信息，请参阅[关于配置虚拟网络使用管理门户](http://go.microsoft.com/fwlink/?LinkID=248092)中的**虚拟网络地址空间页**。

    -  **的地址空间︰**在本教程中的示例中，单击右上角的**CIDR**然后输入以下命令︰
        -  **开始 IP:** 10.4.0.0
        -  **CIDR:** /16
    -  **中添加子网︰**对于在本教程中的示例，输入以下命令︰
        -  重命名为**FrontEndSubnet**与起始 IP **10.4.2.0/24**的**子网 1** 。
        -  添加子网使用起始 IP **10.4.3.0/24**调用**BackEndSubnet** 。
        -  添加子网使用起始 IP **10.4.4.0/24**调用**ADDNSSubnet** 。
        -  添加与起始 IP **10.4.1.0/24**网关子网。
    -  在本教程中的示例中，请验证您现在具有三个子网和一个网关的子网创建的并单击右下方创建虚拟网络的复选标记。

    ![](./media/virtual-networks-create-site-to-site-cross-premises-connectivity/CreateCrossVnet_05_VirtualNetworkAddressSpaces.png)

7.  单击复选标记，经过虚拟网络将开始创建。 当创建虚拟网络后时，您将看到**创建**管理门户中的网络页上列出**状态**下。 

    ![](./media/virtual-networks-create-site-to-site-cross-premises-connectivity/CreateCrossVNet_06_VirtualNetworkCreatedStatus.png)

##  <a name="StartGateway">启动该网关</a>

创建后 Azure 虚拟网络，使用以下过程来创建站点到站点 VPN 配置虚拟网络网关。 此过程要求必须满足的最低要求的 VPN 设备。 关于 VPN 设备和设备配置的详细信息，请参阅[关于 VPN 设备的虚拟网络](http://go.microsoft.com/fwlink/p/?LinkID=248098)。

**若要启动该网关︰**

1.  当创建虚拟网络后时，**网络**页面会显示**创建**虚拟网络的状态。

    在**名称**列中，单击**YourVirtualNetwork** （用于本教程中创建的示例） 来打开仪表板。
 
    ![](./media/virtual-networks-create-site-to-site-cross-premises-connectivity/CreateCrossVNet_07_ClickYourVirtualNetwork.png)

2.  单击页面顶部的**仪表板**。 在仪表板页面，在页面的底部，单击**创建网关**。 选择**动态路由**或**静态路由**的网关，您想要创建的类型。 

    请注意，是否您想要使用此虚拟网络进行点到站点除了站点到站点的连接，您必须选择**动态路由**网关类型。 创建网关之前, 验证您的 VPN 设备将支持您想要创建网关类型。 请参阅[关于虚拟网络的 VPN 设备](http://go.microsoft.com/fwlink/p/?LinkID=248098)。 当系统提示您确认您想要创建的网关时，请单击**是**。

    ![](./media/virtual-networks-create-site-to-site-cross-premises-connectivity/CreateCrossVnet_08_CreateGateway.png)

3.  网关创建启动时，您将看到消息让您知道网关已启动。

    可能需要 15 分钟的时间来创建网关。

4.  在创建网关后，您将需要收集用于配置 VPN 设备的以下信息。 

    -  网关 IP 地址
    -  共享的密钥
    -  VPN 设备配置脚本模板

    接下来的步骤将引导您完成此过程。

5.  若要找到网关 IP 地址︰ 网关 IP 地址位于虚拟网络**仪表板**页面。 下面是一个示例︰

    ![](./media/virtual-networks-create-site-to-site-cross-premises-connectivity/CreateCrossVnet_09_GatewayIP.png)

6.  获取共享密钥︰ 共享的密钥位于虚拟网络**仪表板**页面。 单击**管理密钥**在屏幕的底部，然后复制对话框中显示的项。 您将需要此参数来配置 IPsec 隧道公司的 VPN 设备上。

    ![](./media/virtual-networks-create-site-to-site-cross-premises-connectivity/CreateCrossVNet_10_ManageSharedKey.png)

7.  若要下载 VPN 设备配置脚本模板︰ 在仪表板，请单击**下载 VPN 设备脚本**。

8.  在**下载一个 VPN 设备配置脚本**对话框中，选择供应商、 平台和操作系统为您公司的 VPN 设备。 单击复选标记按钮并保存该文件。 

    ![](./media/virtual-networks-create-site-to-site-cross-premises-connectivity/CreateCrossVnet_11_DownloadVPNDeviceScript.png)

如果您看不到 VPN 设备上的下拉列表中，请参阅 MSDN 库中的其他脚本模板中[关于虚拟网络 VPN 设备](http://go.microsoft.com/fwlink/p/?LinkID=248098)。


##  <a name="ConfigVPN">配置 VPN 设备 （网络管理员）</a>

由于每个 VPN 设备不同，这是高级过程。 此过程应该通过您的网络管理员。

您可以从管理门户或[VPN 设备的虚拟网络有关](http://go.microsoft.com/fwlink/p/?LinkId=248098)，其中还说明了路由类型和您选择要使用的路由配置与兼容的设备获取 VPN 配置脚本。

有关配置虚拟网络网关的其他信息，请参阅[配置管理门户中的虚拟网络网关](http://go.microsoft.com/fwlink/p/?LinkId=299878)，请咨询您的 VPN 设备文档。

此过程假定下列情况︰

-  配置 VPN 设备的人员是精通于已选择的设备配置。 由于与虚拟网络和配置特定于每个设备系列兼容的设备的数量，这些步骤不逐步细致的设备配置。 因此，它是重要配置设备的人是熟悉设备和配置设置。 

-  选择了要使用的设备是与虚拟网络兼容。 检查[以下](http://go.microsoft.com/fwlink/p/?LinkID=248098)有关设备的兼容性。


**若要配置 VPN 设备︰**

1.  修改 VPN 配置脚本。 将配置以下各项︰

    一。  安全策略

    b。  传入的隧道

    c。  传出的隧道

2.  运行修改后的 VPN 配置脚本来配置 VPN 设备。

3.  通过运行以下命令之一来测试您的连接︰

    <table border="1">
    <tr>
    <th>-</th>
    <th>ASA cisco</th>
    <th>Cisco ISR/ASR</th>
    <th>刺柏 SSG/ISG</th>
    <th>刺柏 SRX/J</th>
    </tr>
    
    <tr>
    <td><b>检查主模式 Sa</b></td>
    <td><FONT FACE="courier" SIZE="-1">显示加密的 isakmp sa</FONT></td>
    <td><FONT FACE="courier" SIZE="-1">显示加密的 isakmp sa</FONT></td>
    <td><FONT FACE="courier" SIZE="-1">获取 ike 的 cookie</FONT></td>
    <td><FONT FACE="courier" SIZE="-1">显示安全 ike 安全关联</FONT></td>
    </tr>
    
    <tr>
    <td><b>检查快速模式 Sa</b></td>
    <td><FONT FACE="courier" SIZE="-1">显示加密的 ipsec sa</FONT></td>
    <td><FONT FACE="courier" SIZE="-1">显示加密的 ipsec sa</FONT></td>
    <td><FONT FACE="courier" SIZE="-1">获取 sa</FONT></td>
    <td><FONT FACE="courier" SIZE="-1">显示安全 ipsec 安全关联</FONT></td>
    </tr>
    </table>


##  下一步行动
若要扩展到您刚创建的虚拟网络将内部部署 Active Directory，继续下面的教程︰

-  [如何向自定义创建的虚拟机](http://go.microsoft.com/fwlink/p/?LinkID=294356)

-  [安装在 Azure 的虚拟网络中复制 Active Directory 域控制器](http://go.microsoft.com/fwlink/p/?LinkId=299877)

如果您想要将虚拟网络设置导出到一个网络配置文件以便备份配置，或将其用作模板，请参阅[导出虚拟网络设置应用于网络配置文件](http://go.microsoft.com/fwlink/p/?LinkID=299880)。

## 请参见

-  [Azure 的虚拟网络技术概述](../virtual-network/virtual-networks-overview.md)

-  [虚拟网络常见问题解答](virtual-networks-faq.md)

-  [配置虚拟网络使用网络配置文件](virtual-networks-using-network-configuration-file.md)

-  [将虚拟机添加到虚拟网络](../virtual-machines/virtual-machines-create-custom.md)

-  [有关虚拟网络的 VPN 设备](http://msdn.microsoft.com/library/windowsazure/jj15] 75.aspx)

-  [虚拟机和角色实例名称解析](http://go.microsoft.com/fwlink/p/?LinkId=248097)
-  [设置用于测试的混合云环境](virtual-networks-setup-hybrid-cloud-environment-testing.md)





 

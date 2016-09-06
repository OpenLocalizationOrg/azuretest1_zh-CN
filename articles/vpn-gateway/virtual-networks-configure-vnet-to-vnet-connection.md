---
ms.openlocfilehash: 1b5ece756ff468642d4a69b2698bd86624616ed4
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="将 VNet 与 VNet 连接配置 |Microsoft Azure"
   description="Azure 一起在相同或不同的订阅或区域中的虚拟网络连接的方式。"
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carolz"
   editor=""/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/20/2015"
   ms.author="cherylmc"/>


# 在 Azure 门户配置 VNet 到 VNet 连接

> [AZURE.SELECTOR]
- [Azure 门户](virtual-networks-configure-vnet-to-vnet-connection.md)
- [PowerShell 的 Azure 的资源管理器](vpn-gateway-vnet-vnet-rm-ps.md)

本文将指导您完成使用 Azure 门户和 PowerShell 的组合连接在一起在典型部署模式的虚拟网络。 


将虚拟网络连接到另一个虚拟网络 (VNet Vnet) 是非常类似于将虚拟网络连接到内部网站的位置。 这两种连接类型使用的 VPN 网关来提供使用 IPsec/IKE 安全隧道。 在不同的订阅和不同地区可以连接 VNets。 您甚至可以到 VNet 通信具有多站点配置组合 VNet。 这样，如下图中所示建立组合的网络拓扑跨场所间虚拟网络连接的连接︰

![VNet 到 VNet 的连接关系图](./media/virtual-networks-configure-vnet-to-vnet-connection/IC727360.png)


>[AZURE.NOTE] Azure 目前有两种部署模式︰ 典型的部署模式中和 Azure 资源管理器部署模式。 两种部署模式不同的配置 cmdlet 和步骤。 本主题将指导您完成连接使用的典型部署模式创建的虚拟网络。 如果您想要将虚拟网络一起在 Azure 资源管理器模式中创建连接，请参阅[使用 Azure 资源管理器和 PowerShell 配置 VNet VNet 连接](vpn-gateway-vnet-vnet-rm-ps.md)。 如果您想要连接到 Azure 资源管理器中创建的虚拟网络的经典模式中创建的虚拟网络，请参阅[连接到新的 VNets 的经典 VNets](../virtual-network/virtual-networks-arm-asm-s2s.md)。

## 为什么将虚拟网络连接？

您可能想要将虚拟网络连接，原因如下︰

- **跨区域地理冗余和地理状态**
    - 您可以设置自己的 geo 复制或同步使用安全连接而不需要通过面向 internet 的终结点。
    - Azure 负载平衡器和 Microsoft 或第三方的群集技术，您可以设置 Azure 的多个区域间高度可用的工作负载进行地理冗余。 一个重要例子是，设置 SQL 始终在与可用性组多个 Azure 的区域间传播。

- **而强大的隔离能力的边界区域的多层应用程序**
    - 在相同区域中，可以使用多个虚拟的网络连接以及强大的隔离能力和层间的安全通信设置多层应用程序。

- **跨组织间通信在 Azure 中的订阅**
    - 如果您有多个 Azure 的订阅，您可以连接不同订阅的工作负载在一起安全地虚拟网络之间。
    - 对于企业或服务提供商，您可以启用安全 VPN 技术在 Azure 的跨组织通信。

## VNet 到 VNet 的常见问题解答

- 虚拟的网络可以在相同或不同的预订。

- 虚拟的网络可以在相同或不同 Azure 地区 （位置）。

- 即使连接在一起，不能在虚拟的网络，跨越云服务或负载平衡终结点。

- 将多个虚拟网络连接在一起不需要任何内部部署 VPN 网关，除非不需要跨内部连接。

- VNet 到 VNet 支持连接 Azure 的虚拟网络。 它不支持连接的虚拟机或云不在虚拟的网络服务。

- VNet 到 VNet 要求与动态路由 Vpn 的 Azure VPN 网关。 不支持 azure 静态路由 VPN 网关。

- 虚拟的网络连接可以同时使用多站点 Vpn，最多为 10 个 VPN 隧道连接到经不起考验的虚拟网络 VPN 网关与其它虚拟网络或本地站点。

- 虚拟的网络和内部部署本地网络站点的地址空间必须不重叠。 重叠的地址空间，则将导致创建虚拟网络或上载的 netcfg 配置文件失败。

- 不支持冗余虚拟网络对之间的隧道。

- 在虚拟的网络，包括 P2S Vpn 的所有 VPN 隧道都共享上的 Azure VPN 网关和同一 VPN 网关的正常运行时间 SLA Azure 中的可用带宽。

- VNet 到 VNet 流量穿越 Azure 的主干。

## 配置 VNet 连接到 VNet

在此过程中，我们将带领您通过连接两个虚拟网络，VNet1 和 VNet2。 您需要为要替换的 ip 地址与您的网络设计要求兼容的舒适带网络连接。 从 Azure 的虚拟网络连接到另一个 Azure 的虚拟网络并连接到内部网络通过 VPN 站点到站点 (S2S) 相同。

此过程主要是使用 Azure 门户，但是，您必须使用 Microsoft Azure PowerShell cmdlet 连接的 VPN 网关。

![连接到 VNet 的 VNet](./media/virtual-networks-configure-vnet-to-vnet-connection/IC727361.png)

有 5 节来规划和配置。 下面列出的顺序配置的每个节︰

1. [规划您的 IP 地址范围](#plan-your-ip-address-ranges)
2. [创建虚拟网络](#create-your-virtual-networks)
3. [添加本地网络](#add-local-networks)
4. [为每个 VNet 创建的动态路由网关](#create-the-dynamic-routing-gateways-for-each-vnet)
5. [连接的 VPN 网关](#connect-the-vpn-gateways)


## 规划您的 IP 地址范围

请务必确定将用来配置您的网络配置文件 (netcfg) 的范围。 从 VNet1 的角度来看，VNet2 是另一定义 Azure 平台中的 VPN 连接。 并从 VNet2，VNet1 将只是另一个 VPN 连接。 他们将两会确定彼此作为本地网络站点。 请记住，您必须确保以任何方式任何 VNet 区域或本地网络范围重叠。

表 1 显示了如何定义您的 VNets 的一个示例。 原则是只使用下面的范围。 记下区域您将虚拟的网络中使用。 您将需要此信息的后续步骤。

**表 1**

|虚拟网络  |虚拟网络站点定义 |本地网络的网站定义|
|:----------------|:-------------------------------|:----------------------------|
|VNet1            |VNet1 (10.1.0.0/16)             |VNet2 (10.2.0.0/16)          |
|VNet2            |VNet2 (10.2.0.0/16)             |VNet1 (10.1.0.0/16)          |

## 创建虚拟网络

对于本教程，我们将创建两个虚拟网络，VNet1 和 VNet2。 当创建您的 VNets 替换为您自己的值。 对于本教程，我们将使用以下值 VNets:

VNet1︰ 地址空间 = 10.1.0.0/16;区域 = 美国西

VNet2︰ 地址空间 = 10.2.0.0/16;区域 = 日本东

1. 登录到**管理门户。**

2. 在屏幕的左下角，单击**新建**。 在导航窗格中，单击**网络服务**，然后单击**虚拟网络**。 单击**创建自定义**启动配置向导。

**在虚拟网络详细信息页**上，输入以下信息。

  ![虚拟网络详细信息](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736055.png)

  - **名称**的虚拟网络名称。 例如，VNet1。
  - **位置**– 创建虚拟网络时，您将其与关联的 Azure 的位置 （区域）。 例如，如果您希望您的 Vm 部署到虚拟网络物理上位于美国西部，选择该位置。 您不能更改后创建该虚拟网络相关联的位置。



**在 DNS 服务器和 VPN 连接页**中，输入以下信息，然后单击右下方的下箭头。

  ![DNS 服务器和 VPN 连接](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736056.jpg)  


- **DNS 服务器**的 DNS 服务器的名称和 IP 地址，请输入或从下拉列表中选择以前已注册的 DNS 服务器。 此设置不会创建一个 DNS 服务器，它允许您指定要使用的虚拟网络名称解析的 DNS 服务器。 如果您希望让虚拟网络之间的名称解析，您需要配置自己的 DNS 服务器，而不是使用提供的 Azure 的名称解析。

  - 不选中任何复选框。 只需单击右下方移动到下一个屏幕上的箭头。

**在虚拟网络地址空间页面**上，指定要使用的虚拟网络地址范围。 这些都是动态的 IP 地址 (DIP) 将分配给虚拟机，该虚拟网络部署其他角色实例。 它是特别重要，要选择具有任何内部网络使用的范围不会重叠的范围。 您需要与您的网络管理员，他刻出内部网络地址空间供您使用的虚拟网络范围内的 IP 地址可能需要协调一致。


  ![虚拟网络地址空间页](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736057.jpg)

  **输入以下信息**，然后单击右下方以配置您的网络上的选中标记。

  - **地址空间**-包括起始 IP 和地址数。 请验证您指定的地址空间不覆盖任何对您的内部网络的地址空间。 对于本示例，我们将使用 10.1.0.0/16 VNet1。
  - **添加子网**-包括起始 IP 和地址数。 其他的子网不是必需的但要为将具有静态 DIP 的虚拟机创建一个单独的子网。 或者，您可能想要您的 Vm 中其他角色实例与不同的子网。

在右下角的页面，您的虚拟网络上**单击选中标记**将开始创建。 完成后，您将看到*创建*Azure 门户中的*网络*页上列出*状态*下。

## 创建另一个虚拟网络

接下来，重复以上步骤以创建其他的虚拟网络。 在本练习中，您以后将连接这两个虚拟网络。 请注意，它非常重要，没有重复或重叠的地址空间。 对于本教程，使用以下值︰ 

- **VNet2**
- **地址空间**= 10.2.0.0/16
- **区域**= 日本东

## 添加本地网络

当您创建一个 VNet 到 VNet 的配置时，您需要配置每个 VNet 相互识别为本地网络站点。 在此过程中，您将为本地网络配置每个 VNet。 如果已经具有配置 VNets，这是如何可将它们与本地网络在 Azure 门户。

1. 在屏幕的左下角，单击**新建**。 在导航窗格中，单击**网络服务**，然后单击**虚拟网络**。 单击**添加本地网络**

2. 在**指定您的本地网络的详细信息**页面上，对于**名称**输入要在 VNet 到 VNet 配置中使用的虚拟网络名称。 对于本示例，我们将使用 VNet 1，因为我们将指向 VNet2 此虚拟网络对于我们的配置。

  对于 VPN 设备的 IP 地址，使用任何 IP 地址。 通常情况下，您将使用 VPN 设备实际的外部 IP 地址。 对于 VNet 到 VNet 的配置，您将使用的网关 IP 地址。 但是，考虑到您没有创建网关，但在此处，我们使用您指定的 IP 地址作为占位符。 然后将这些设置都将恢复，并与相应的网关 IP 地址配置它们，一旦 Azure 生成它。

3. 在**指定地址页**中，将置于实际 IP 地址范围和地址数为 VNet1。 这必须完全对应 VNet1 为前面所指定的范围。

4. 为本地网络配置 VNet1 之后, 回过头来配置 VNet2 使用对应于该 VNet 的值。

5. 现在您将指向每个 VNet 将对方列为本地网络。 在管理门户中，转到**配置**网页 VNet1。 在**站点到站点连接**，选择**连接到本地网络**，然后选择**VNET2**作为本地网络。

  ![连接到本地网络](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736058.jpg)  

6. 在同一页上的**虚拟网络地址空间**部分中，单击**添加子网网关**，然后单击**保存**图标底部的页后，可以保存您的配置。

7. 重复指定为本地网络的 VNet1 的 VNet2 的一个步骤。

## 为每个 VNet 创建的动态路由网关

现在，您已经配置每个 VNet，您将配置您的 VNet 网关。

1. 在**网络**页面上，验证状态列中为您的虚拟网络时**创建**。

2. 在**名称**列中，单击虚拟网络的名称。

3. 在**仪表板**页中，请注意此 VNet 没有尚未配置的网关。 您将看到该状态更改所需完成的步骤来配置您的网关。

4. 在页面的底部，单击**创建网关**。

  您必须选择**动态路由**。 当系统提示您确认您想要创建的网关时，请单击是。

  ![网关类型](./media/virtual-networks-configure-vnet-to-vnet-connection/IC717026.png)  

5. 当创建您的网关时，请注意在页上的网关图形更改为黄色而称创建网关。 通常要花费大约 15 分钟来创建网关。

6. 重复相同的步骤为您其他 VNet，一定要选中**动态网关**。 您不需要第一个 VNet 网关以开始创建您其他 VNet 网关之前完成。

7. 当网关状态更改为连接时，每个网关的 IP 地址将显示在仪表板中。 每个 VNet，注意不要混淆了它们写下对应的 IP 地址。 这些都是您的**本地网络**中的 VPN 设备编辑占位符 IP 地址时，将使用的 IP 地址。

## 编辑本地网络

1. 在**本地网络**页面上，单击您想要编辑的本地网络名称的名称，然后单击页面底部的**编辑**。 对于**VPN 设备 IP 地址**，输入对应于 VNet 网关的 IP 地址。 例如，对于 VNet1，放在分配给 VNet1 网关 IP 地址。 然后单击页面底部的箭头。

2. 在**指定地址空间**页，单击右下方的复选标记而不进行任何更改。

## 连接的 VPN 网关

所有上述步骤完成后，您将设置 IPsec/IKE 预共享的密钥必须相同。 你可以使用 REST API 或 PowerShell cmdlet。 如果您使用 PowerShell，请验证您具有最新版本的 Microsoft Azure PowerShell cmdlet。 下面的示例使用 PowerShell cmdlet 将注册表项值设置为 A1b2C3D4。 请注意两者使用相同的密钥值。 编辑以下示例以反映您自己的值。

对于 VNet1

    PS C:\> Set-AzureVNetGatewayKey -VNetName VNet1 -LocalNetworkSiteName VNet2 -SharedKey A1b2C3D4

对于 VNet2

    PS C:\> Set-AzureVNetGatewayKey -VNetName VNet2 -LocalNetworkSiteName VNet1 -SharedKey A1b2C3D4

若要初始化的连接等。 一旦初始化具有网关，网关将类似于下面的图形，虚拟网络相互连接。

![网关状态的连接](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736059.jpg)  

## 下一步行动


如果您想要将虚拟机添加到虚拟网络，请参阅[如何创建一个自定义虚拟机](../virtual-machines/virtual-machines-create-custom.md)。

如果您想要配置使用 RRAS 的 VNet 连接，请参阅[配置站点到站点 VPN 使用 Windows Server 2012 路由和远程访问服务 (RRAS)](https://msdn.microsoft.com/library/dn636917.aspx)。

有关配置模式的信息，请参阅[Azure 虚拟网络配置架构](https://msdn.microsoft.com/library/azure/jj157100.aspx)。 


[1]: ../hdinsight-hbase-geo-replication-configure-vnets.md
[2]: http://channel9.msdn.com/Series/Getting-started-with-Windows-Azure-HDInsight-Service/Configure-the-VPN-connectivity-between-two-Azure-virtual-networks
 

---
ms.openlocfilehash: c6feebf09d6bde81fedeb78d50506b5ebc48111e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="使用站点到站点 VPN 连接使用 Azure 资源管理器和 PowerShell 创建虚拟网络 |Microsoft Azure"
   description="通过使用 Azure 资源管理器和 PowerShell 站点到站点 VPN 连接从虚拟网络创建到内部部署位置"
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carolz"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/21/2015"
   ms.author="cherylmc"/>

# 使用站点到站点 VPN 连接使用 Azure 资源管理器和 PowerShell 创建虚拟网络

> [AZURE.SELECTOR]
- [Azure 门户](vpn-gateway-site-to-site-create.md)
- [PowerShell 的资源管理器](vpn-gateway-create-site-to-site-rm-powershell.md)


本主题将指导您完成创建 Azure 资源管理器虚拟网络和站点到站点 VPN 连接到内部网络。 

Azure 目前有两种部署模型︰ 经典的部署模型和 Azure 资源管理器的部署模型。 站点的安装程序不同，具体取决于用于部署虚拟网络的模型。
这些说明将应用到资源管理器中。 如果您想要创建使用传统部署模型站点到站点 VPN 网关连接，请参阅[创建管理门户中的站点到站点 VPN 连接](vpn-gateway-site-to-site-create.md)。


## 在开始之前

在开始之前，请验证您具有以下︰

- 兼容的 VPN 设备 （和人员能够对其进行配置）。 请参阅[关于 VPN 设备](vpn-gateway-vpn-devices.md)。
- 面向外部的公共 IP 地址用于 VPN 设备。 此 IP 地址不能位于后面 nat。
- 最新版的 Azure PowerShell cmdlet。 您可以从下载并安装最新版本[下载页面](http://azure.microsoft.com/downloads/)的 Windows PowerShell 部分。 
- Azure 的订阅。 如果您没有订阅了 Azure，可以激活您的[MSDN 订户权益](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)或符号组成的[免费试用版](http://azure.microsoft.com/pricing/free-trial/)。
    

## 连接到您的订阅 


打开 PowerShell 控制台并切换到 Azure 资源管理器模式。 使用下面的示例可帮助您连接︰

        Add-AzureAccount

使用选择 AzureSubscription 连接到要使用的订阅。

        Select-AzureSubscription "yoursubscription"

接下来，切换到 ARM 模式。 这将切换模式允许您使用 ARM cmdlet。

        Switch-AzureMode -Name AzureResourceManager


## 创建虚拟网络和网关的子网

- 如果您已经使用网关网的虚拟网络，可以转跳到[添加本地站点](#add-your-local-site)。 
- 如果有一个虚拟的网络，您想要添加到您的 VNet 网关子网，请参阅[添加 VNet 网关网](#gatewaysubnet)。

### 若要创建虚拟网络和网关的子网

使用下面的示例创建一个虚拟网络和网关网。 替换为您自己的值。 

首先，创建资源组︰

    
        New-AzureResourceGroup -Name testrg -Location 'West US'

接下来，创建虚拟网络。 下面的示例创建一个名为*testvnet*和两个子网，一个称为*GatewaySubnet*的虚拟网络，另称为*Subnet1*。 若要创建一个名为特别是*GatewaySubnet*的子网至关重要。 如果您命名它停下手中的将失败，您的连接配置。

        $subnet1 = New-AzureVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.0.0/28
        $subnet2 = New-AzureVirtualNetworkSubnetConfig -Name 'Subnet1' -AddressPrefix '10.0.1.0/28'
        New-AzurevirtualNetwork -Name testvnet -ResourceGroupName testrg -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $subnet1, $subnet2


### <a name="gatewaysubnet"></a>若要将一个网关的子网添加到 VNet

如果您已经有一个现有的虚拟网络，您想要向其添加子网网关，您可以通过使用下面的示例创建网关网。 一定要命名为 GatewaySubnet 的网关子网。 如果您命名它停下手中的您的 VPN 配置不会像预期的那样。


    
        $vnet = Get-AzureVirtualNetwork -ResourceGroupName testrg -Name testvnet
        Add-AzureVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/28 -VirtualNetwork $vnet
        Set-AzureVirtualNetwork -VirtualNetwork $vnet

## 添加本地站点

在虚拟网络中，*本地站点*通常是指您的内部位置。 您将通过该网站依据 Azure 可以引用它的名称。 

您还将指定本地站点地址空间前缀。 Azure 将使用您指定标识哪些通信量发送到本地站点的 IP 地址前缀。 这意味着，您将需要指定每个您想要与本地的网站相关联的地址前缀。 如果更改了您的内部网络，可以轻松地更新这些前缀。 使用下面的 PowerShell 样本来指定本地站点。 

    
- *GatewayIPAddress*是内部部署 VPN 设备的 IP 地址。 VPN 设备不能位于后面 nat。 
- *AddressPrefix*为您的内部地址空间。

使用本示例添加一个地址前缀的本地网站。

        New-AzureLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'

如果您想要添加多个地址前缀的本地网站中，使用此示例。

        New-AzureLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix @('10.0.0.0/24','20.0.0.0/24')


若要将其他地址前缀添加到本地站点，您已经创建，使用下面的示例。

        $local = Get-AzureLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg
        Set-AzureLocalNetworkGateway -LocalNetworkGateway $local -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')


若要删除本地站点地址前缀，请使用下面的示例。 丢掉了不再需要的前缀。 在此示例中，我们不再需要前缀 20.0.0.0/24 （从上面的示例），因此我们将更新本地网站和排除该前缀。

        local = Get-AzureLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg
        Set-AzureLocalNetworkGateway -LocalNetworkGateway $local -AddressPrefix @('10.0.0.0/24','30.0.0.0/24')


## 请求公共 VNet 网关的 IP 地址

接下来，您将请求分配到 Azure VNet VPN 网关的公共 IP 地址。 这不是相同的 IP 地址分配给您的 VPN 设备，而不是分配到 Azure VPN 网关本身。 您不能指定的 IP 地址，您想要使用;它是动态地分配给您的网关。 您将使用此 IP 地址配置内部部署 VPN 设备时连接到网关。

使用下面的 PowerShell 示例。 此地址的分配方法必须是动态。 

        $gwpip= New-AzurePublicIpAddress -Name gwpip -ResourceGroupName testrg -Location 'West US' -AllocationMethod Dynamic

## 创建网关的 IP 地址配置

网关配置定义的子网和要使用的公用 IP 地址。 使用下面的示例创建网关配置。 


        $vnet = Get-AzureVirtualNetwork -Name testvnet -ResourceGroupName testrg
        $subnet = Get-AzureVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
        $gwipconfig = New-AzureVirtualNetworkGatewayIpConfig -Name gwipconfig1 -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id 


## 创建网关

在此步骤中，您将创建虚拟网络网关。 使用以下值︰

- 网关类型为*Vpn*。
- VpnType 可以是 RouteBased* （称为中一些文档的动态网关），或*策略基于 * （也称为某些文档中的静态网关）。 关于 VPN 网关类型的详细信息，请参阅[关于 VPN 网关](vpn-gateway-about-vpngateways.md)。     

        New-AzureVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg -Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased


## 配置 VPN 设备

在这种情况下，您需要的虚拟网络网关的公用 IP 地址配置内部部署 VPN 设备。 使用特定的配置信息与设备制造商联系。 此外，请参阅[VPN 设备](http://go.microsoft.com/fwlink/p/?linkid=615099)的详细信息。

若要查找虚拟网络网关的公用 IP 地址，请使用下面的示例︰

    Get-AzurePublicIpAddress -Name gwpip -ResourceGroupName testrg

## 创建 VPN 连接

接下来，您将创建虚拟网络网关和 VPN 设备之间的站点到站点 VPN 连接。 一定要替换为您自己的值。 共享的密钥必须符合用于 VPN 设备的配置的值。

        $gateway1 = Get-AzureVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
        $local = Get-AzureLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg

        New-AzureVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg -Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

几分钟后，应能建立连接。 到目前为止，使用资源管理器中创建站点到站点 VPN 连接是可见的门户。


## 下一步行动

将虚拟机添加到您的虚拟网络。 [创建虚拟机](../virtual-machines/virtual-machines-windows-tutorial.md)。
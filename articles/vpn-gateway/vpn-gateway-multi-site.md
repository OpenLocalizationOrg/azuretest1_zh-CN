---
ms.openlocfilehash: 1399e2f9d4bb53c9dde037822526905a47564691
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="将多个内部站点连接到虚拟网络"
   description="这篇文章将引导您完成将多个本地内部部署站点连接到虚拟网络。"
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carolz"
   editor="tysonn" />
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/04/2015"
   ms.author="cherylmc" />

# 将多个内部站点连接到虚拟网络

可以将多个内部站点连接到一个单独的虚拟网络。 这是一个用于构建混合云解决方案尤其好。 创建一个多站点连接到 Azure 的虚拟网络网关是非常类似于创建其他站点到站点连接。 事实上，可以使用现有的 Azure VPN 网关，前提是您具有一个基于路由 （或动态路由选择） 配置为虚拟网络的 VPN 网关。 

如果您的网关是基于策略的 （或静态路由选择），您可以随时更改而无需重新生成的虚拟网络，以适应多站点的网关类型，虽然您还需要确保内部部署 VPN 网关支持基于路由的 VPN 配置。 您将只是将配置设置添加到网络配置文件，然后从虚拟网络到其他网站中创建多个 VPN 连接。

![多站点 VPN](./media/vpn-gateway-multi-site/IC727363.png)

## 应考虑事项

**您不能使用管理门户网站来更改此虚拟网络。** 对于此版本，您需要更改网络配置文件，而不使用管理门户。 管理门户中进行更改时，如果他们将覆盖多站点引用设置为该虚拟网络。 您应该感觉非常舒适，您已经完成了多站点过程时使用的网络配置文件。 但是，如果您有多个用户使用您的网络配置，您需要确保每个人都知道关于这种限制。 这并不意味着不能在所有使用管理门户。 除非更改此特定的虚拟网络配置的其他所有内容，可以使用它。

## 在开始之前

在开始配置之前，请验证您具有以下︰

- Azure 的订阅。 如果您没有订阅了 Azure，可以激活您的[MSDN 订户权益](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)或符号组成的[免费试用版](http://azure.microsoft.com/pricing/free-trial/)。

- 每个硬件兼容的 VPN 内部部署的位置。 检查[有关虚拟网络连接 VPN 设备](http://go.microsoft.com/fwlink/p/?linkid=615099)以验证您要使用的设备是否是已知兼容。

- 面向外部公用 IPv4 IP 地址用于每个 VPN 设备。 IP 地址不能位于后面 nat。 这是要求。

-   最新版的 Azure PowerShell cmdlet。 您可以从下载并安装最新版本 Windows PowerShell 部分中的[下载](http://azure.microsoft.com/downloads/)页面。

- 有人是精通于配置 VPN 硬件。 您不能使用管理门户中的自动生成 VPN 脚本来配置 VPN 设备。 这意味着您必须有深刻的了解如何配置 VPN 设备，或与某人机构工作。

- 您想要使用的虚拟网络 （如果您还没有创建一个） 的 IP 地址范围。 

- 对于每个您将连接到本地网络站点的 IP 地址范围。 您将需要确保 IP 地址范围为每个您想要连接到不重叠的本地网络站点。 否则，管理门户或 REST API 将拒绝上载的配置。 例如，如果您有两个本地网络站点都包含 IP 地址范围 10.2.3.0/24，可以使用目标地址 10.2.3.3 包，Azure 岂不知道您想要发送到的包，因为重叠的地址范围的站点。 为了防止路由问题，Azure 不允许您上载有重叠区域的配置文件。

## 创建虚拟网络和网关

1. **创建站点到站点 VPN 与动态路由的网关。** 如果您已经拥有一个，好 ！ 您可以继续到[导出的虚拟网络配置设置](#export)。 如果不是，请执行下列操作︰

    **如果您已有站点到站点虚拟网络，但它具有静态路由网关︰****1.** 将网关类型更改为动态路由。 多站点 VPN 要求动态路由的网关。 若要更改您的网关类型，您将需要首先删除现有的网关，然后创建一个新。 有关说明，请参阅[更改一个 VPN 网关路由类型](vpn-gateway-configure-vpn-gateway-mp.md/#how-to-change-your-vpn-gateway-type)。 **2。** 配置您的新网关并创建 VPN 隧道。 有关说明，请参阅[配置 VPN 网关管理门户中](vpn-gateway-configure-vpn-gateway-mp.md)。 
    
    **如果您不具有站点到站点虚拟网络︰****1.** 创建站点到站点虚拟网络使用这些说明︰[创建站点到站点 VPN 连接管理门户中使用的虚拟网络](vpn-gateway-site-to-site-create.md)。 **2。** 配置动态路由网关使用这些说明︰[配置 VPN 网关管理门户中](vpn-gateway-configure-vpn-gateway-mp.md)。 请务必选择网关类型**动态路由**。



1. **<a name="export"></a>将导出的虚拟网络配置设置。** 若要导出网络配置文件，请参阅[导出您的网络设置](../virtual-network/virtual-networks-using-network-configuration-file.md#export-and-import-virtual-network-settings-using-the-management-portal)。 将导出的文件将用于配置新的多站点设置。

1. **打开您的网络配置文件。** 最后一步中打开下载的网络配置文件。 使用任何您喜欢的 xml 编辑器。 文件应该看起来类似于以下示例︰

        <NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
          <VirtualNetworkConfiguration>
            <LocalNetworkSites>
              <LocalNetworkSite name="Site1">
                <AddressSpace>
                  <AddressPrefix>10.0.0.0/16</AddressPrefix>
                  <AddressPrefix>10.1.0.0/16</AddressPrefix>
                </AddressSpace>
                <VPNGatewayAddress>131.2.3.4</VPNGatewayAddress>
              </LocalNetworkSite>
              <LocalNetworkSite name="Site2">
                <AddressSpace>
                  <AddressPrefix>10.2.0.0/16</AddressPrefix>
                  <AddressPrefix>10.3.0.0/16</AddressPrefix>
                </AddressSpace>
                <VPNGatewayAddress>131.4.5.6</VPNGatewayAddress>
              </LocalNetworkSite>
            </LocalNetworkSites>
            <VirtualNetworkSites>
              <VirtualNetworkSite name="VNet1" AffinityGroup="USWest">
                <AddressSpace>
                  <AddressPrefix>10.20.0.0/16</AddressPrefix>
                  <AddressPrefix>10.21.0.0/16</AddressPrefix>
                </AddressSpace>
                <Subnets>
                  <Subnet name="FE">
                    <AddressPrefix>10.20.0.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="BE">
                    <AddressPrefix>10.20.1.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="GatewaySubnet">
                    <AddressPrefix>10.20.2.0/29</AddressPrefix>
                  </Subnet>
                </Subnets>
                <Gateway>
                  <ConnectionsToLocalNetwork>
                    <LocalNetworkSiteRef name="Site1">
                      <Connection type="IPsec" />
                    </LocalNetworkSiteRef>
                  </ConnectionsToLocalNetwork>
                </Gateway>
              </VirtualNetworkSite>
            </VirtualNetworkSites>
          </VirtualNetworkConfiguration>
        </NetworkConfiguration>

1. 多个站点将引用添加到该网络的配置文件。 当您添加或删除站点的引用信息时，您将为 ConnectionsToLocalNetwork/LocalNetworkSiteRef 进行配置更改。 添加新的本地站点引用触发器 Azure 创建的新的隧道。 在下面的示例中，网络配置是单站点连接。

        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="Site1"><Connection type="IPsec" /></LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

    若要添加其他网站引用 （创建一个多站点配置），只需添加更多的"LocalNetworkSiteRef"行，如下面的示例所示︰ 

        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="Site1"><Connection type="IPsec" /></LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Site2"><Connection type="IPsec" /></LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

1. **保存网络配置文件并将其导入。** 要导入的网络配置文件，请参阅[导入您的网络设置](../virtual-network/../virtual-network/virtual-networks-using-network-configuration-file.md#export-and-import-virtual-network-settings-using-the-management-portal)。 导入此文件的更改时，将添加新的隧道。 隧道将使用您在前面创建的动态网关。



1. **下载 VPN 隧道的预共享的的密钥。** 一旦添加了您新的隧道，使用 PowerShell cmdlet Get AzureVNetGatewayKey 以获取每个隧道 IPsec/IKE 预先共享的密钥。

    例如︰

        PS C:\> Get-AzureVNetGatewayKey –VNetName "VNet1" –LocalNetworkSiteName "Site1"

        PS C:\> Get-AzureVNetGatewayKey –VNetName "VNet1" –LocalNetworkSiteName "Site2"

    如果您愿意，您可以使用*获取虚拟网络网关共享密钥*REST API 来获取预共享的密钥。

## 请验证连接

**检查多站点隧道状态。** 下载后每个隧道的密钥，您需要验证连接。 使用*Get AzureVnetConnection*以获取一份虚拟网络隧道，如下面的示例中所示。 VNet1 是 VNet 的名称。

        PS C:\Users\yushwang\Azure> Get-AzureVnetConnection -VNetName VNET1
        
        ConnectivityState         : Connected
        EgressBytesTransferred    : 661530
        IngressBytesTransferred   : 519207
        LastConnectionEstablished : 5/2/2014 2:51:40 PM
        LastEventID               : 23401
        LastEventMessage          : The connectivity state for the local network site 'Site1' changed from Not Connected to Connected.
        LastEventTimeStamp        : 5/2/2014 2:51:40 PM
        LocalNetworkSiteName      : Site1
        OperationDescription      : Get-AzureVNetConnection
        OperationId               : 7f68a8e6-51e9-9db4-88c2-16b8067fed7f
        OperationStatus           : Succeeded
        
        ConnectivityState         : Connected
        EgressBytesTransferred    : 789398
        IngressBytesTransferred   : 143908
        LastConnectionEstablished : 5/2/2014 3:20:40 PM
        LastEventID               : 23401
        LastEventMessage          : The connectivity state for the local network site 'Site2' changed from Not Connected to Connected.
        LastEventTimeStamp        : 5/2/2014 2:51:40 PM
        LocalNetworkSiteName      : Site2
        OperationDescription      : Get-AzureVNetConnection
        OperationId               : 7893b329-51e9-9db4-88c2-16b8067fed7f
        OperationStatus           : Succeeded

## 下一步行动

若要了解有关 VPN 网关的详细信息，请参阅[关于 VPN 网关](../vpn-gateway/vpn-gateway-about-vpngateways.md)。


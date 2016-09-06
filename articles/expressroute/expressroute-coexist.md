---
ms.openlocfilehash: 656d215055b6403214f7787c1c4fedd09d7fe4ca
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="配置可以共存的 Expressroute 和站点到站点 VPN 连接 |Microsoft Azure"
   description="本教程将指导您完成配置 ExpressRoute 和站点到站点 VPN 连接可以共存的。"
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carolz"
   editor="tysonn" />
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/05/2015"
   ms.author="cherylmc"/>

# 配置共存的 Azure ExpressRoute 和站点到站点 VPN 连接

现在将 ExpressRoute 和站点到站点 VPN 连接到相同的虚拟网络。
有两种不同的情况和可供选择的两个不同的配置过程。

## 方案

### 方案 1

在这种情况下，您有一个本地网络。 ExpressRoute 是该活动的链接，以及您的站点到站点 VPN 是备份。 如果 ExpressRoute 连接失败，站点到站点 VPN 连接就变成活动状态。 这种情况下是最合适的高可用性。

![共存](media/expressroute-coexist/scenario1.jpg)



### 方案 2

在这种情况下，您有两个本地网络。 可一到 Azure 通过 ExpressRoute，和其他通过站点到站点 VPN 连接。 在这种情况下，这两个连接在同一时间处于活动状态。

![共存](media/expressroute-coexist/scenario2.jpg)


## 创建和配置

有两组不同的过程，可供选择来配置连接共存。 您选择的配置过程取决于您是否有一个现有的虚拟网络，您想要连接，或者您想要创建新的虚拟网络。

- **创建新的虚拟网络和共存的连接︰**

    如果没有虚拟网络，则此过程将引导您完成创建新的虚拟网络和创建新的 ExpressRoute 和站点到站点 VPN 连接。 若要配置，请按照下列步骤[创建新的虚拟网络和连接](#create-a-new-virtual-network-and-connections-that-coexist)。

- **配置现有虚拟网络共存的连接︰**

    在与现有的站点到站点 VPN 连接或 ExpressRoute 连接的地方，可能已拥有虚拟网络。 [配置连接的现有虚拟网络共存](#configure-connections-that-coexist-for-your-existing-virtual-network)过程将引导您完成删除网关，然后再创建新的 ExpressRoute 和站点到站点 VPN 连接。 请注意，在创建新的连接时，必须完成步骤中非常特定的顺序。 不要使用说明在其他文章中来创建您的网关和连接。

    在此过程中，创建可以共存的连接将需要您删除您的网关，然后配置新的网关。 这意味着您必须删除并重新创建您的网关和连接，但您不需要将任何虚拟机或服务迁移到新的虚拟网络跨内部连接的停机时间。 您的虚拟机和服务仍然可以向外传播，通过负载平衡器时您配置您的网关，如果它们配置来执行此操作。


## 说明与限制

- 您不能 （通过 Azure) 路由通过站点到站点 VPN 连接本地网络和本地网络连接通过 ExpressRoute 之间。
- 您不能启用点到站点 VPN 连接到相同的 VNet 连接到 ExpressRoute。 点到站点 VPN 和 ExpressRoute 不能共存的同一个 VNet。
- ExpressRoute 网关和站点到站点 VPN 网关必须是标准或高性能网关的 SKU。 有关网关 Sku 的信息，请参阅[网关 Sku](../vpn-gateway/vpn-gateway-about-vpngateways.md)。
- 如果您的本地网络连接到 ExpressRoute 和站点到站点 VPN (方案 1)，应具有在您的本地网络路由到公用的 Internet 站点到站点 VPN 连接中配置的静态路由。
- 添加站点到站点 VPN 网关之前，必须先创建 ExpressRoute 网关。
- 这两个过程假定您已配置 ExpressRoute 电路。 如果不这样做，请参阅下列文章︰

    - [配置 ExpressRoute 连接到网络服务提供商 (NSP)](expressroute-configuring-nsps.md)
    - [ExpressRoute 通过配置连接的 exchange 提供 (EXP)](expressroute-configuring-exps.md)


## 创建新的虚拟网络和共存的连接

此过程将引导您完成创建虚拟网络，以及创建将共存的站点对站点和 ExpressRoute 连接。

1. 请确保您有最新版本的 PowerShell cmdlet。 您可以从下载并安装最新的 PowerShell cmdlet[下载页面](http://azure.microsoft.com/downloads/)的 PowerShell 部分。
2. 创建虚拟网络的架构。 有关如何使用该网络配置文件的详细信息，请参阅[配置虚拟网络使用网络配置文件](../virtual-network/virtual-networks-using-network-configuration-file.md)。 有关配置模式的详细信息，请参阅[Azure 虚拟网络配置架构](https://msdn.microsoft.com/library/azure/jj157100.aspx)。

    当您创建架构时，请确保使用以下值︰

    - 虚拟网络的网关子网必须是 /27 （或更短的前缀）。
    - 网关连接类型"专用"。

              <VirtualNetworkSite name="MyAzureVNET" Location="Central US">
                <AddressSpace>
                  <AddressPrefix>10.17.159.192/26</AddressPrefix>
                </AddressSpace>
                <Subnets>
                  <Subnet name="Subnet-1">
                    <AddressPrefix>10.17.159.192/27</AddressPrefix>
                  </Subnet>
                  <Subnet name="GatewaySubnet">
                    <AddressPrefix>10.17.159.224/27</AddressPrefix>
                  </Subnet>
                </Subnets>
                <Gateway>
                  <ConnectionsToLocalNetwork>
                    <LocalNetworkSiteRef name="MyLocalNetwork">
                      <Connection type="Dedicated" />
                    </LocalNetworkSiteRef>
                  </ConnectionsToLocalNetwork>
                </Gateway>
              </VirtualNetworkSite>



3. 在创建并配置您的 xml 架构文件之后, 上传文件。 这将创建虚拟网络。

    使用以下 cmdlet 上载您的文件替换为您自己的值。

    `Set-AzureVNetConfig -ConfigurationPath 'C:\NetworkConfig.xml'`
4. 创建的 ExpressRoute 网关。 一定要指定为*标准*或*高性能*和为*DynamicRouting*GatewayType 的 GatewaySKU。

    使用以下示例中，替换为您自己的值。

    `New-AzureVNetGateway -VNetName MyAzureVNET -GatewayType DynamicRouting -GatewaySKU HighPerformance`

5. 链接到 ExpressRoute 电路的 ExpressRoute 网关。 在完成此步骤后，被建立内部网络和 Azure，ExpressRoute，通过之间的连接。

    `New-AzureDedicatedCircuitLink -ServiceKey <service-key> -VNetName MyAzureVNET`

6. 接下来，创建您的站点到站点 VPN 网关。 GatewaySKU 必须是*标准*或*高性能*和 GatewayType 必须是*DynamicRouting*。

    `New-AzureVirtualNetworkGateway -VNetName MyAzureVNET -GatewayName S2SVPN -GatewayType DynamicRouting -GatewaySKU  HighPerformance`

    若要检索虚拟网络网关设置，包括网关 ID 和公用 IP，并使用`Get-AzureVirtualNetworkGateway`cmdlet。

        Get-AzureVirtualNetworkGateway

        GatewayId            : 348ae011-ffa9-4add-b530-7cb30010565e
        GatewayName          : S2SVPN
        LastEventData        :
        GatewayType          : DynamicRouting
        LastEventTimeStamp   : 5/29/2015 4:41:41 PM
        LastEventMessage     : Successfully created a gateway for the following virtual network: GNSDesMoines
        LastEventID          : 23002
        State                : Provisioned
        VIPAddress           : 104.43.x.y
        DefaultSite          :
        GatewaySKU           : HighPerformance
        Location             :
        VnetId               : 979aabcf-e47f-4136-ab9b-b4780c1e1bd5
        SubnetId             :
        EnableBgp            : False
        OperationDescription : Get-AzureVirtualNetworkGateway
        OperationId          : 42773656-85e1-a6b6-8705-35473f1e6f6a
        OperationStatus      : Succeeded

7. 创建一个本地站点的 VPN 网关实体。 此命令不会将内部部署 VPN 网关配置。 相反，它使您可以提供本地网关设置，如公用 IP 和本地地址空间，以便 Azure VPN 网关可以连接到它。

    > [AZURE.IMPORTANT] 在 netcfg 中未定义本地站点的站点到站点 vpn。 相反，您必须使用此 cmdlet 指定本地站点参数。 不能定义使用管理门户或 netcfg 文件。

    使用以下示例中，替换为您自己的值。

    `New-AzureLocalNetworkGateway -GatewayName MyLocalNetwork -IpAddress <local-network- gateway-public-IP> -AddressSpace <local-network-address-space>`

    若要检索虚拟网络网关设置，包括网关 ID 和公用 IP，并使用`Get-AzureVirtualNetworkGateway`cmdlet。 下面的示例，请参阅。

        Get-AzureLocalNetworkGateway

        GatewayId            : 532cb428-8c8c-4596-9a4f-7ae3a9fcd01b
        GatewayName          : MyLocalNetwork
        IpAddress            : 23.39.x.y
        AddressSpace         : {10.1.2.0/24}
        OperationDescription : Get-AzureLocalNetworkGateway
        OperationId          : ddc4bfae-502c-adc7-bd7d-1efbc00b3fe5
        OperationStatus      : Succeeded


8. 配置本地 VPN 设备要连接到的新网关。 使用您在步骤 6 中配置 VPN 设备时检索的信息。 VPN 设备配置有关的详细信息，请参阅[VPN 设备配置](http://go.microsoft.com/fwlink/p/?linkid=615099)。


9. 链接到本地网关在 Azure 上的站点到站点 VPN 网关。

    在此示例中，connectedEntityId 是本地网关 ID，您可以通过运行查找`Get-AzureLocalNetworkGateway`。 您可以通过查找 virtualNetworkGatewayId `Get-AzureVirtualNetworkGateway` cmdlet。 建立这一步之后, 通过站点到站点 VPN 连接本地网络和 Azure 之间的连接。


    `New-AzureVirtualNetworkGatewayConnection -connectedEntityId <local-network-gateway-id> -gatewayConnectionName Azure2Local -gatewayConnectionType IPsec -sharedKey abc123 -virtualNetworkGatewayId <azure-s2s-vpn-gateway-id>`


## 配置现有虚拟网络共存的连接

如果您有现有的虚拟网络连接通过 ExpressRoute 或站点对站点的 VPN 连接，以便使这两个连接以连接到现有的虚拟网络，必须先删除现有的网关。 这意味着您本地的场所将失去与虚拟网络的连接通过网关正在使用此配置。

**在开始配置之前︰**请确认您有足够留在虚拟网络中，以便您可以增大网关的子网的 IP 地址。


1. 下载最新版本的 PowerShell cmdlet。 您可以从下载并安装最新的 PowerShell cmdlet[下载页面](http://azure.microsoft.com/downloads/)的 PowerShell 部分。

2. 删除现有的站点到站点 VPN 网关。 使用以下 cmdlet，替换为您自己的值。

    `Remove-AzureVNetGateway –VnetName MyAzureVNET`

2. 导出虚拟网络架构。 使用以下 PowerShell cmdlet，替换为您自己的值。

    `Get-AzureVNetConfig –ExportToFile “C:\NetworkConfig.xml”`
3. 编辑网络配置文件架构，使网关网是 /27 （或更短的前缀）。 下面的示例，请参阅。 有关如何使用该网络配置文件的详细信息，请参阅[配置虚拟网络使用网络配置文件](../virtual-network/virtual-networks-using-network-configuration-file.md)。 有关配置模式的详细信息，请参阅[Azure 虚拟网络配置架构](https://msdn.microsoft.com/library/azure/jj157100.aspx)。


          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.17.159.224/27</AddressPrefix>
          </Subnet>
4. 如果您以前的网关站点到站点 VPN，您还必须**专用**于更改连接类型。

                 <Gateway>
                  <ConnectionsToLocalNetwork>
                    <LocalNetworkSiteRef name="MyLocalNetwork">
                      <Connection type="Dedicated" />
                    </LocalNetworkSiteRef>
                  </ConnectionsToLocalNetwork>
                </Gateway>
5. 在这种情况下，您将有没有网关 VNet。 您可以继续**第 3 步**在文章中，[创建新的虚拟网络和连接](#create-a-new-virtual-network-and-connections-that-coexist)，以便创建新网关并完成您的连接。



## 下一步行动

了解有关 ExpressRoute。 请参阅[ExpressRoute 概述](expressroute-introduction.md)。

了解更多有关 VPN 网关。 请参阅[关于 VPN 网关](../vpn-gateway/vpn-gateway-about-vpngateways.md)。

测试

---
ms.openlocfilehash: e32dc2273d176d9238fecfd8cf993d32d58cbcd0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="通过 Exchange 提供程序配置 ExpressRoute"
   description="本教程将指导您完成设置 ExpressRoute，通过 exchange 提供程序 (EXPs)。"
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carolz"
   editor="tysonn" />
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/28/2015"
   ms.author="cherylmc"/>

#  ExpressRoute 通过配置连接 Exchange 提供程序

要配置通过 exchange 提供商 ExpressRoute 连接，您将需要完成多个步骤，以正确的顺序。 以下说明将帮助您执行下列操作︰

- 创建和管理 ExpressRoute 电路
- 配置对等对 ExpressRoute 电路的 BGP
- 将虚拟网络链接到 ExpressRoute 电路

##  配置系统必备组件


在开始配置之前，请确保您满足以下先决条件︰

- Azure 的订阅
- Azure PowerShell 的最新版本
- 以下虚拟网络要求︰
    - 在 Azure 中的虚拟网络中使用的 IP 地址前缀的一组
    - 一套 IP 前缀场所 （可以包含公用 IP 地址）
    - 虚拟的网络网关必须使用 /28 创建子网。
    - 另一组 IP 前缀 （/ 28） 这就是在虚拟的网络之外。 这将用于配置对 BGP 等。
    - 为您的网络的数目。 与数字有关的详细信息，请参阅[自治系统 (AS) 编号](http://www.iana.org/assignments/as-numbers/as-numbers.xhtml)。
    - 如果需要身份验证的 BGP 会话的 MD5 哈希
    - 将会发送通信量的 VLAN Id。 必须为每个电路 2 VLAN Id︰ 一个用于虚拟网络，另一个用于承载在公用 IP 地址的服务。
    - 您的网络的的[自治系统 (AS) 编号](http://www.iana.org/assignments/as-numbers/as-numbers.xhtml)。
    - 两个 1 Gbps / 10 Gbps 交叉连接到 Exchange 提供的以太网交换。
    - 对能够支持 BGP 路由的路由器

##  部署工作流


![](./media/expressroute-configuring-exps/expressroute-exp-connectivity-workflow.png)

##  使用 PowerShell 的配置设置

Windows PowerShell 是功能强大的脚本编写环境，可用于控制和自动化的部署和管理您的工作负载在 Azure 中。 有关详细信息请参阅[MSDN](https://msdn.microsoft.com/library/windowsazure/jj156055.aspx)中的 PowerShell 文档。

1. **导入 ExpressRoute PowerShell 模块。**

        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

2. **获取提供程序、 位置和带宽支持列表。**

    创建电路之前您需要服务提供商、 支持的位置和带宽选项的列表的每个位置。 以下的 PowerShell cmdlet 返回在后续步骤中，您将使用此信息。

        PS C:\> Get-AzureDedicatedCircuitServiceProvider
        **The information returned will look similar to the example below:**


        Name                 DedicatedCircuitLocations      DedicatedCircuitBandwidths
        ----                 -------------------------      --------------------------
        AT&T                 Silicon Valley,Washington DC   10Mbps:10, 50Mbps:50, 100Mbps:100, 500Mbps:500, 1Gbps:1000
        British Telecom      London,Amsterdam               10Mbps:10, 50Mbps:50, 100Mbps:100, 500Mbps:500, 1Gbps:1000
        Equinix              Amsterdam,Atlanta,Chicago,Dall 200Mbps:200, 500Mbps:500, 1Gbps:1000, 10Gbps:10000
                             as,New York,Seattle,Silicon
                             Valley,Washington
                             DC,London,Hong
                             Kong,Singapore,Sydney,Tokyo
        IIJ                  Tokyo                          10Mbps:10, 50Mbps:50, 100Mbps:100, 500Mbps:500, 1Gbps:1000
        Level 3              London,Silicon                 200Mbps:200, 500Mbps:500, 1Gbps:1000
        Communications -     Valley,Washington DC
        Exchange
        Level 3              London,Silicon                 10Mbps:10, 50Mbps:50, 100Mbps:100, 500Mbps:500, 1Gbps:1000
        Communications -     Valley,Washington DC
        IPVPN
        Orange               Amsterdam,London               10Mbps:10, 50Mbps:50, 100Mbps:100, 500Mbps:500, 1Gbps:1000
        SingTel Domestic     Singapore                      10Mbps:10, 50Mbps:50, 100Mbps:100, 500Mbps:500, 1Gbps:1000
        SingTel              Singapore                      10Mbps:10, 50Mbps:50, 100Mbps:100, 500Mbps:500, 1Gbps:1000
        International
        TeleCity Group       Amsterdam,London               200Mbps:200, 500Mbps:500, 1Gbps:1000, 10Gbps:10000
        Telstra Corporation  Sydney                         10Mbps:10, 50Mbps:50, 100Mbps:100, 500Mbps:500, 1Gbps:1000
        Verizon              Silicon Valley,Washington DC   10Mbps:10, 50Mbps:50, 100Mbps:100, 500Mbps:500, 1Gbps:1000

3. **创建专用的电路**

    下面的示例演示如何创建 200 Mbps ExpressRoute 电路通过 Equinix 硅谷。 如果您使用的不同的提供程序和其他设置，替换该信息时您提出的请求。

    下面是示例请求为新的服务密钥︰

        #Creating a new circuit
        $Bandwidth = 200
        $CircuitName = "EquinixSVTest"
        $ServiceProvider = "Equinix"
        $Location = "Silicon Valley"

        New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location

        #Getting service key
        Get-AzureDedicatedCircuit

    响应将类似于下面的示例︰

        Bandwidth                        : 200
        CircuitName                      : EquinixSVTest
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : NotProvisioned
        Status                           : Enabled

    您可以随时使用 Get AzureCircuit cmdlet 的此信息。 调用不带任何参数，则将列出所有电路。 服务密钥将 ServiceKey 字段中列出。

        PS C:\> Get-AzureDedicatedCircuit

        Bandwidth                        : 200
        CircuitName                      : EquinixSVTest
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : NotProvisioned
        Status                           : Enabled


4. **服务密钥发送给您的 exchange 提供商。**

    Exchange 提供程序将使用服务键来启用连接其结束。 您将需要连接提供程序提供附加信息，才能启用连接。

5. **定期检查的状态和电路键的状态。**

    这样，如果您知道您的提供商已启用您的电路。 一旦启用了电路， *ServiceProviderProvisioningState*将显示为*Provisioned* ，如下面的示例中所示。

        PS C:\> Get-AzureDedicatedCircuit

        Bandwidth                        : 200
        CircuitName                      : EquinixSVTest
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled

6. **配置虚拟网络的路由。**

    我们使用 BGP 会话来交换路由，而且还要确保我们具有高可用性。 使用下面的示例创建对您电路的 BGP 会话。 在创建会话时替换为您自己的值。

        #Setting up a bgp session
        $ServiceKey = "<your key>"
        $PriSN = "<subnet/30 you use IP #1 and Azure uses IP #2>"
        $SecSN = "<subnet/30 use IP #1 and Azure uses IP #2>"
        $ASN = <your ASN>
        $VLAN = <your vlan ID>

        #Create a new bgp peering session
        New-AzureBGPPeering -ServiceKey $ServiceKey -PrimaryPeerSubnet $PriSN -SecondaryPeerSubnet $SecSN -PeerAsn $ASN VlanId $VLAN –AccessType Private
        #Get BGP parameters and Azure ASN
        Get-AzureBGPPeering -ServiceKey $ServiceKey –AccessType Private
        #Update BGP peering config
        Set-AzureBGPPeering  -ServiceKey $ServiceKey -PrimaryPeerSubnet $PriSN -SecondaryPeerSubnet $SecSN -PeerAsn $ASN -VlanId $VLAN –AccessType Private
        #Removing BGP peering config
        Remove-AzureBGPPeering -ServiceKey $ServiceKey –AccessType Private

    您可以通过提供服务密钥使用 Get AzureBGPPeering 的电路的路由信息。 此外可以使用一组 AzureBGPPeering 的 BGP 设置进行更新。 运行此命令时，将不提出 BGP 会话。 必须具有至少一个 VNet 以获取 BGP 会话链接电路。

    下面的响应将为您提供所需的下一步行动的信息。 使用对等 ASN 配置 BGP 路由器的 VRFs 上。

        PS C:\> New-AzureBGPPeering -ServiceKey $ServiceKey -PrimaryPeerSubnet $PriSN -SecondaryPeerSubnet $SecSN -PeerAsn $ASN -VlanId $VLAN –AccessType Private

        AzureAsn            : 12076
        PeerAsn             : 65001
        PrimaryAzurePort    : EQIX-SJC-06GMR-CIS-1-PRI-A
        PrimaryPeerSubnet   : 10.0.1.0/30
        SecondaryAzurePort  : EQIX-SJC-06GMR-CIS-2-SEC-A
        SecondaryPeerSubnet : 10.0.2.0/30
        State               : Enabled
        VlanId              : 100

7. **配置服务位于公用 IP 地址的路由。**

    我们使用 BGP 会话来交换路由，而且还要确保我们具有高可用性。 使用下面的示例创建对您电路的 BGP 会话。 在创建会话时替换为您自己的值。

        #Setting up a bgp session
        $ServiceKey = "<your key>"
        $PriSN = "<subnet/30 subnet you use IP #1 and Azure uses IP #2>"
        $SecSN = "< subnet/30 subnet you use IP #1 and Azure uses IP #2>"
        $ASN = <your ASN>
        $VLAN = <your vlan ID>

        #Create a new bgp peering session
        New-AzureBGPPeering -ServiceKey $ServiceKey -PrimaryPeerSubnet $PriSN -SecondaryPeerSubnet $SecSN -PeerAsn $ASN -VlanId $VLAN –AccessType Public
        #Get BGP parameters and Azure ASN
        Get-AzureBGPPeering -ServiceKey $ServiceKey –AccessType Public
        #Update BGP peering config
        Set-AzureBGPPeering  -ServiceKey $ServiceKey -PrimaryPeerSubnet $PriSN -SecondaryPeerSubnet $SecSN -PeerAsn $ASN -VlanId $VLAN –AccessType Public
        #Removing BGP peering config
        Remove-AzureBGPPeering -ServiceKey $ServiceKey –AccessType Public

    您可以通过提供服务密钥使用 Get AzureBGPPeering 的电路的路由信息。 此外可以使用一组 AzureBGPPeering 的 BGP 设置进行更新。 运行此命令时，将不提出 BGP 会话。 必须具有至少一个 VNet 以获取 BGP 会话链接电路。

    下面的响应将为您提供所需的下一步行动的信息。 使用对等 ASN 配置 BGP 路由器的 VRFs 上。

        PS C:\> New-AzureBGPPeering -ServiceKey $ServiceKey -PrimaryPeerSubnet $PriSN -SecondaryPeerSubnet $SecSN -PeerAsn $ASN -VlanId $VLAN –AccessType Private

        AzureAsn            : 12076
        PeerAsn             : 65001
        PrimaryAzurePort    : EQIX-SJC-06GMR-CIS-1-PRI-A
        PrimaryPeerSubnet   : 10.0.1.8/30
        SecondaryAzurePort  : EQIX-SJC-06GMR-CIS-2-SEC-A
        SecondaryPeerSubnet : 10.0.2.8/30
        State               : Enabled
        VlanId              : 101

8. **配置虚拟网络和网关。**

    请参阅[配置虚拟网络和网关 ExpressRoute](expressroute-configuring-vnet-gateway.md)。 请注意，网关网必须是 /28，为了使用 ExpressRoute 连接。

9. **将您的网络链接到电路。** 仅在您已确认您电路已移动到以下状态和状态后，才能继续下面的说明︰
    - ServiceProviderProvisioningState︰ 设置
    - 状态︰ 启用

            PS C:\> $Vnet = "MyTestVNet"
            New-AzureDedicatedCircuitLink -ServiceKey $ServiceKey -VNetName $Vnet
 
## 下一步行动

- 有关 ExpressRoute 的详细信息，请参阅[ExpressRoute 的常见问题解答](expressroute-faqs.md)。

测试

---
ms.openlocfilehash: 772b1b079ff12548ba4913acba537ab57e9743dc
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="使用 Nsp 配置 Expressroute"
   description="本教程将指导您完成设置通过 Nsp ExpressRoute"
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="adinah"
   editor="tysonn"/>

<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="06/29/2015"
   ms.author="cherylmc"/>

#  配置 ExpressRoute 连接到网络服务供应商

要配置 ExpressRoute 连接到网络服务供应商，您将需要完成多个步骤，以正确的顺序。 以下说明将帮助您︰

- 创建和管理 ExpressRoute 电路
- 将虚拟网络链接到 ExpressRoute 电路

##  配置系统必备组件


在开始配置之前，请确保您满足以下先决条件︰

- Azure 的订阅
- 最新版本的 Windows PowerShell
- 以下虚拟网络要求︰
    - 在 Azure 中的虚拟网络中使用的 IP 地址前缀的一组。
    - 一套 IP 前缀场所 （可以包含公用 IP 地址）。
    - 虚拟的网络网关必须使用 /28 创建子网。
    - 另一组 IP 前缀 （/ 28） 这就是在虚拟的网络之外。 这将用于配置路由。 – 您必须向网络服务提供商提供此。
    - 为您的网络的数目。 您将需要向网络服务提供商提供此。 您可以使用专用数字。 如果您选择这样做，它必须是 > 65000。 与数字有关的详细信息，请参阅自治系统 (AS) 编号。 - 

- 从网络服务提供商︰
    - 您必须具有与 ExpressRoute 支持 VPN 服务。 如果您现有的 VPN 服务有资格，请与网络服务供应商。

##  部署工作流

![网络服务提供商工作流](./media/expressroute-configuring-nsps/expressroute-nsp-connectivity-workflow.png)

##  使用 PowerShell 的配置设置
Windows PowerShell 是功能强大的脚本编写环境，可用于控制和自动化的部署和管理您的工作负载在 Azure 中。 有关详细信息请参阅[MSDN](https://msdn.microsoft.com/library/windowsazure/jj156055.aspx)中的 PowerShell 文档



1. **导入 ExpressRoute PowerShell 模块。**

        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1' 

2. **获取提供程序、 位置和带宽支持列表。**

    创建电路之前您需要服务提供商、 支持的位置和带宽选项的列表的每个位置。 以下的 PowerShell cmdlet 返回在后续步骤中，您将使用此信息。

        PS C:\> Get-AzureDedicatedCircuitServiceProvider

    返回的信息将类似于下面的示例︰

        PS C:\> Get-AzureDedicatedCircuitServiceProvider
    
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
        

3. **所提出的请求服务密钥并将其传递到网络服务供应商。** 

    您将使用 PowerShell cmdlet 发出此请求。 对于本示例，我们将使用 AT & T Netbond 作为服务提供商，并将指定的 50 Mbps ExpressRoute 电路在硅谷。 如果您使用的不同的提供程序和其他设置，替换该信息时您提出的请求。

    下面是示例请求为新的服务密钥︰

        ##Creating a new circuit
        $Bandwidth = 50
        $CircuitName = "NetBondSVTest"
        $ServiceProvider = "at&t"
        $Location = "Silicon Valley"
        
        New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location
        
        #Getting service key
        Get-AzureDedicatedCircuit

    响应将类似于下面的示例︰

        Bandwidth                        : 50
        CircuitName                      : NetBondSVTest
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : at&t
        ServiceProviderProvisioningState : NotProvisioned
        Status                           : Enabled

    您可以随时使用 Get AzureCircuit cmdlet 的此信息。 调用不带任何参数，则将列出所有电路。 服务密钥将 ServiceKey 字段中列出。

        PS C:\> Get-AzureDedicatedCircuit
        
        Bandwidth                        : 500
        CircuitName                      : NetBondSVTest
        Location                         : Silicon Valley
        ServiceKey                       : 00-0000-0000-0000-0000000000
        ServiceProviderName              : at&t
        ServiceProviderProvisioningState : NotProvisioned
        Status                           : Enabled

    一旦完成此步骤，您必须连接到 Azure 存储和其他服务。



4. **配置虚拟网络和网关。** 

    请参阅[配置虚拟网络和网关 ExpressRoute](../expressroute/expressroute-configuring-vnet-gateway.md)。 请注意，网关网必须是 /28，为了使用 ExpressRoute 连接。

5. **将您的网络链接到电路。** 

    继续下面的步骤仅在您已确认，之后您
 
    - ServiceProviderState︰ 设置
    - 状态︰ 启用

    请与网关创建确认有至少一个 Azure 的虚拟网络。 必须运行网关。

        PS C:\> $Vnet = "MyTestVNet"
        New-AzureDedicatedCircuitLink -ServiceKey $ServiceKey -VNetName $Vnet
        
        Provisioned 

测试

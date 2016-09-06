---
ms.openlocfilehash: 9e4111aa6631fb2b01578f97949f8e440e12664d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="在内部负载平衡器使用 Azure 资源管理器上开始 |Microsoft Azure "
   description="如何创建内部负载平衡器规则，NAT 规则探测器的 Azure 资源管理器中。 逐步显示端到端流程来创建一个内部负载平衡器 (ILB) 资源。"
   services="load-balancer"
   documentationCenter="na"
   authors="joaoma"
   manager="adinah"
   editor="tysonn" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/22/2015"
   ms.author="joaoma" />

# 开始配置内部负载平衡器使用 Azure 资源管理器


> [AZURE.SELECTOR]
- [Azure 经典步骤](load-balancer-internal-getstarted.md)
- [资源管理器 Powershell 的步骤](load-balancer-internal-arm-powershell.md)


下面的步骤将演示如何创建使用 PowerShell Azure 资源管理器内部负载平衡器。 使用 Azure 资源管理器中，项目以创建一个内部负载平衡器是单独配置，然后在一起以创建资源。 

我们将在此页中介绍各项任务已完成即可创建内部负载平衡器和详细说明正采取什么措施来实现目标，以创建一个负载平衡器的序列。


## 需要创建内部负载平衡器是什么？

需要创建内部负载平衡器之前，先配置以下各项︰

- 前结束 IP 配置-将配置为传入的网络通信的专用 IP 地址 

- 后端地址池-将配置的网络接口将接收来自前端 IP 池的负载平衡通信 

- 负载平衡规则-源和本地端口配置为负载平衡器。

- 探测器-配置虚拟机实例运行状况状态探测。

- 入站 NAT 规则-配置的端口规则直接访问虚拟机实例之一。

与在[Azure 资源管理器支持的负载平衡器](load-balancer-arm.md)的 Azure 的资源管理器中，获得有关负载的详细信息平衡器组件。

下面的步骤将演示如何配置以进行负载平衡 2 的虚拟机之间的负载平衡器。


## 逐步使用 powershell


### 负载平衡器创建资源组


### 第 1 步
请确保您切换使用 ARM cmdlet PowerShell 模式。 使用[Windows Powershell 与资源管理器](powershell-azure-resource-manager.md)提供了更多的信息。


    PS C:\> Switch-AzureMode -Name AzureResourceManager

### 第 2 步

登录到您的 Azure 帐户。


    PS C:\> Add-AzureAccount

您将被提示到身份验证使用您的凭据。


### 第 3 步

选择您要使用的 Azure 订阅。 

    PS C:\> Select-AzureSubscription -SubscriptionName "MySubscription"

若要查看可用的订阅列表，使用 Get AzureSubscription cmdlet。


### 第 4 步

创建新的资源组 （跳过此步骤如果使用现有资源组）

    PS C:\> New-AzureResourceGroup -Name NRP-RG -location "West US"

Azure 的资源管理器需要的所有资源组都指定一个位置。 这是该资源组中的资源用作默认位置。 请确保所有的命令，以创建一个负载平衡器将使用相同的资源组。

在上面的示例中，我们将创建资源组名为"NRP RG"和"西美国"的位置。 

## 创建虚拟网络和前端 IP 池的公用 IP 地址


### 第 1 步

创建虚拟网络︰

    $backendSubnet = New-AzureVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24

创建虚拟网络子网并将分配给变量 $backendSubnet

    $vnet= New-AzurevirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet

创建虚拟网络和将磅子网将子网添加到虚拟网络 NRPVNet，并将赋给变量 $vnet 



## 创建前结束 IP 库和后端地址池

前端 IP 池设置为接收负载平衡网络流量和后端地址的池来接收负载平衡通信。

### 第 1 步 

创建用于将传入的网络通信端点的子网 10.0.2.0/24 使用的专用 IP 地址 10.0.2.6 的前端 IP 池。

    $frontendIP = New-AzureLoadBalancerFrontendIpConfig -Name LB-Frontend -PrivateIpAddress 10.0.2.5 -SubnetId $backendSubnet.Id

### 第 2 步 

设置用于从前端 IP 池中接收传入通信的后端地址池︰

    $beaddresspool= New-AzureLoadBalancerBackendAddressPoolConfig -Name "LB-backend"


## 创建磅规则、 NAT 规则、 探测器和负载平衡器

创建前端 IP 池和后端地址池后，您将需要创建规则属于该负载平衡器资源︰

### 第 1 步

    $inboundNATRule1= New-AzureLoadBalancerInboundNatRuleConfig -Name "RDP1" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389

    $inboundNATRule2= New-AzureLoadBalancerInboundNatRuleConfig -Name "RDP2" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3442 -BackendPort 3389

    $healthProbe = New-AzureLoadBalancerProbeConfig -Name "HealthProbe" -RequestPath "HealthProbe.aspx" -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

    $lbrule = New-AzureLoadBalancerRuleConfig -Name "HTTP" -FrontendIpConfiguration $frontendIP -BackendAddressPool $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80


上面的示例将创建以下各项︰

- 所有传入通信端口 3441 将转到端口 3389 NAT 规则。
- 第二个 NAT 规则的所有传入通信端口 3442 将转到端口 3389。
- 这将加载一个负载平衡器规则平衡公共端口 80 后端地址池中的本地端口 80 上的所有传入通信。
- 它将检查路径"HealthProbe.aspx"的健康状态探测规则



### 第 2 步

创建负载平衡器将所有对象 （NAT 规则、 负载平衡器规则、 探测器的配置） 相加︰

    $NRPLB = New-AzureLoadBalancer -ResourceGroupName "NRP-RG" -Name "NRP-LB" -Location "West US" -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule1,$inboundNatRule2 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe 


## 创建网络接口

创建后内部负载平衡器，您需要定义哪些网络接口将接收传入的负载平衡网络流量、 NAT 规则和探测。 在这种情况下，网络接口单独配置，并可以在以后分配给虚拟机。 


### 第 1 步 


获取资源的虚拟网络和子网创建网络接口︰

    $vnet = Get-AzureVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG

    $backendSubnet = Get-AzureVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet 


在此步骤中，我们将创建一个网络接口将属于负载平衡器的后端池，并为该网络接口的 RDP 关联的第一个 NAT 规则︰
    
    $backendnic1= New-AzureNetworkInterface -ResourceGroupName "NRP-RG" -Name lb-nic1-be -Location "West US" -PrivateIpAddress 10.0.2.6 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[0]

### 第 2 步

创建第二个网络接口，称为磅 Nic2 将︰

在此步骤中，我们在创建第二个网络接口，将分配给同一个负载平衡器后端池并将 RDP 为创建第二个 NAT 规则︰ 

    $backendnic2= New-AzureNetworkInterface -ResourceGroupName "NRP-RG" -Name lb-nic2-be -Location "West US" -PrivateIpAddress 10.0.2.7 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[1]


最终结果会显示以下信息︰


PS c:\> backendnic1 美元


    Name                 : lb-nic1-be
    ResourceGroupName    : NRP-RG
    Location             : westus
    Id                   : /subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
    Etag                 : W/"d448256a-e1df-413a-9103-a137e07276d1"
    ProvisioningState    : Succeeded
    Tags                 :
    VirtualMachine       : null
    IpConfigurations     : [
                         {
                           "PrivateIpAddress": "10.0.2.6",
                           "PrivateIpAllocationMethod": "Static",
                           "Subnet": {
                             "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/virtualNetworks/NRPVNet/subnets/LB-Subnet-BE"
                           },
                           "PublicIpAddress": {
                             "Id": null
                           },
                           "LoadBalancerBackendAddressPools": [
                             {
                               "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/backendAddressPools/LB-backend"
                             }
                           ],
                           "LoadBalancerInboundNatRules": [
                             {
                               "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/inboundNatRules/RDP1"
                             }
                           ],
                           "ProvisioningState": "Succeeded",
                           "Name": "ipconfig1",
                           "Etag": "W/\"d448256a-e1df-413a-9103-a137e07276d1\"",
                           "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/ipconfig1"
                         }
                       ]
    DnsSettings          : {
                         "DnsServers": [],
                         "AppliedDnsServers": []
                       }
    AppliedDnsSettings   :
    NetworkSecurityGroup : null
    Primary              : False



### 第 3 步 

使用命令添加 AzureVMNetworkInterface 将 NIC 分配给虚拟机。

您可以查找循序渐进，若要创建虚拟机并按照文档的说明 NIC 分配[创建预配置 Windows 资源管理器和 Azure PowerShell 的虚拟机和](virtual-machines-ps-create-preconfigure-windows-resource-manager-vms.md#Example)


## 请参见

[配置负载平衡器分布模式](load-balancer-distribution-mode.md)

[配置负载平衡器的空闲 TCP 超时设置](load-balancer-tcp-idle-timeout.md)
 

测试

---
ms.openlocfilehash: 1bf11966d04ca40e44795b16aebe9bd0c77ac325
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="每个云服务的多个 Vip"
   description="MultiVIP 和如何设置多个 Vip 云服务概述"
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
   ms.date="07/23/2015"
   ms.author="joaoma" />

# 每个云服务的多个 Vip
通过使用 Azure 所提供的 IP 地址，可以通过公共互联网访问 Azure 的云服务。 此公用 IP 地址被称为 VIP (虚拟 IP) 因为它链接到 Azure 负载平衡器，并不是这样的 VM 实例内云服务。 可以通过使用一个 VIP 访问在云服务中的任何虚拟机实例。 

但是，有的方案中，您可能需要多个 VIP 作为条目指向同一个云服务。 例如，云服务可以承载多个需要使用默认的 SLSL 端口 443，另一个客户，或租户所驻留的每个站点的 SSL 连接的网站。 在这种情况下，您需要具有不同公共对开的 IP 地址的每个网站。 下图显示了典型的多租户 web 宿主具有相同公用端口上的多个 SSL 证书的需要。

![多 VIP SSL 方案](./media/load-balancer-multivip/Figure1.png)

在上述情形中，所有 Vip 都使用同一个公共端口 (443) 和通信被重定向到一个或多个负载平衡的虚拟机上承载的所有网站的云服务的内部 IP 地址的唯一专用端口。 

>[AZURE.NOTE] 使用多个 Vip 的另一种情况设立一组相同的虚拟机上的多个 SQL AlwaysOn 可用性组侦听器。

Vip 是默认情况下，这意味着实际的 IP 地址分配给云服务可能随时间变化动态。 若要防止这种情况发生，您可以保留您的服务 VIP。 若要了解有关保留 VIPs 的详细信息，请参阅[保留公用 IP](../virtual-networks-reserved-public-ip)。

>[AZURE.NOTE] 请参阅有关 vips 定价和保留的 Ip 信息[定价的 IP 地址](http://azure.microsoft.com/pricing/details/ip-addresses/)。

可以使用 PowerShell 验证使用云服务，Vip，以及添加和删除 Vip、 关联的终结点，VIP 和配置负载平衡上特定的 VIP。 

## 限制

到目前为止，多 VIP 功能仅限于以下情况︰

- **IaaS 只**。 您只能启用云服务，包含虚拟机的多个 VIP。 不能在 PaaS 方案，其中角色实例中使用多个 VIP。
- **仅 PowerShell**。 您只能通过使用 PowerShell 管理多 VIP。

>[AZURE.IMPORTANT] 这些限制是临时的可能会随时更改。 请确保重新访问此页后，可以验证以后的更改。


## 如何添加到云服务 VIP
若要添加到您的服务 VIP，运行下面的 PowerShell 命令︰

    Add-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService

上面的命令将显示类似于下面示例的结果︰

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Add-AzureVirtualIP   4bd7b638-d2e7-216f-ba38-5221233d70ce Succeeded

## 如何删除从云服务 VIP
若要删除添加到您在上面的示例服务 VIP，运行下面的 PowerShell 命令︰    

    Remove-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService

>[AZURE.IMPORTANT] 如果还没有与之关联的终结点，您可以只删除 VIP。

## 如何从云服务中检索 VIP 信息
若要检索与云服务 Vip，运行下面的 PowerShell 脚本︰

    $deployment = Get-AzureDeployment -ServiceName myService
    $deployment.VirtualIPs

上述脚本将显示类似于下面示例的结果︰

    Address         : 191.238.74.148
    IsDnsProgrammed : True
    Name            : Vip1
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip2
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip3
    ReservedIPName  :
    ExtensionData   :

在此示例中，云服务有 3 Vip:

- **Vip1**是默认 VIP，您知道，因为 IsDnsProgrammedName 的值设置为 true。
- 因为他们没有任何 IP 地址，不使用**Vip2**和**Vip3** 。 他们将只使用如果您关联到 VIP 的终结点。

>[AZURE.NOTE] 您的订购仅收取额外的 Vip 一旦他们与终结点关联。 有关定价的详细信息，请参阅[IP 地址定价](http://azure.microsoft.com/pricing/details/ip-addresses/)。

## 如何将关联的终结点 VIP
要关联 VIP 在云服务的终结点，请运行下面的 PowerShell 命令︰

    Get-AzureVM -ServiceName myService -Name myVM1 `
  	| Add-AzureEndpoint -Name myEndpoint -Protocol tcp -LocalPort 8080 -PublicPort 80 -VirtualIPName Vip2 `
  	| Update-AzureVM

上面的命令创建链接到端口*80*，和它给 VM 命名为*myVM1* ，在名为*myService*在端口*8080*上使用*TCP*的云服务的链接称为*Vip2* VIP 的终结点。

要验证配置，请运行下面的 PowerShell 命令︰

    $deployment = Get-AzureDeployment -ServiceName myService
    $deployment.VirtualIPs

然后输出将类似于下面的结果︰

    Address         : 191.238.74.148
    IsDnsProgrammed : True
    Name            : Vip1
    ReservedIPName  :
    ExtensionData   :

    Address         : 191.238.74.13
    IsDnsProgrammed :
    Name            : Vip2
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip3
    ReservedIPName  :
    ExtensionData   :

## 如何启用负载平衡上特定 VIP
您可以将单个 VIP 关联多个虚拟机的负载平衡的目的。 例如，假设您有云服务名为*myService*，并命名为*myVM1*和*myVM2*的两台虚拟机。 和云服务有多个 Vip，其中名为*Vip2*之一。 如果您想要确保所有通信发到端口*81*在*Vip2* *myVM1*和*myVM2*在端口*8181*，运行下面的脚本 PowerShell 之间平衡︰ 

    Get-AzureVM -ServiceName myService -Name myVM1 `
  	| Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet `
        -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2  -DefaultProbe `
  	| Update-AzureVM

    Get-AzureVM -ServiceName myService -Name myVM2 `
  	| Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet `
        -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2  -DefaultProbe `
  	| Update-AzureVM

您还可以更新您的负载平衡器使用不同的 VIP。 例如，以下 PowerShell 命令运行时，如果您将更改负载平衡要使用名为 Vip1 VIP 集︰

    Set-AzureLoadBalancedEndpoint -ServiceName myService -LBSetName myLBSet -VirtualIPName Vip1

## 请参见

[互联网面临的负载平衡器概述](load-balancer-internet-overview.md)

[面对负载平衡器的互联网开始](load-balancer-internet-getstarted.md)

[虚拟网络概述](../virtual-network/virtual-networks-overview.md)

[保留的 IP REST Api](https://msdn.microsoft.com/library/azure/dn722420.aspx)
 

测试

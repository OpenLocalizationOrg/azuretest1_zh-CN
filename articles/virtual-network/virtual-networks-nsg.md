---
ms.openlocfilehash: 827be6aed27163f65d7122da80a11d57f666a749
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="网络安全组 (NSG) 是什么"
   description="了解有关网络安全组 (NSG)"
   services="virtual-network"
   documentationCenter="na"
   authors="telmosampaio"
   manager="carolz"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/13/2015"
   ms.author="telmos" />

# 网络安全组 (NSG) 是什么？

在虚拟网络中，您可以使用来控制通信的一个或多个虚拟机 (VM) 实例 NSG。 网络安全组是 NSG 包含访问控制规则允许或拒绝通信量到 VM 实例订阅关联的顶级对象。 NSG 的规则可以随时更改，更改将应用于所有关联的实例。 若要使用 NSG，必须具有关联的区域 （位置） 与 VNet。 

>[AZURE.WARNING] 不兼容与相似性组相关联的 VNets 与 NSGs。 如果您不具有区域 VNet 并且想要控制流量到您的端点，请参阅[网络访问控制列表 (ACL) 是什么？](./virtual-networks-acl.md)。 您可以[迁移到区域 VNet 您 VNet](./virtual-networks-migrate-to-regional-vnet.md)。

您可以关联到一个虚拟机，或 VNet 中子 NSG。 当与虚拟机相关联，NSG 将应用于发送和接收由 VM 实例的所有通信。 应用于您的 VNet 中子时，它将适用于所有的通信量发送和接收到的子网中的所有虚拟机实例。 可以仅 1 NSG 和每个 NSG 可以包含最多 200 个规则与 VM 或子网。 您可以让每个订阅 100 NSGs。

>[AZURE.NOTE] 对同一个 VM 实例不支持基于端点的 Acl 和网络安全组。 如果您想要使用 NSG 和 ACL 的终结点已到位，先删除 ACL 的终结点。 有关如何执行此操作的信息，请参阅[管理访问控制列表 (Acl) 使用 PowerShell 的终结点](virtual-networks-acl-powershell.md)。

## 网络安全组是如何工作的？

网络安全组是不同于基于端点的 Acl。 终结点 Acl 仅适用于通过输入终结点公开的公共端口。 NSG 致力于一个或多个虚拟机实例并控制是入站和出站在虚拟机上的所有通信。

网络安全组都有一个*名称*，关联到某个*区域*，并且的描述性标签。 它包含两种类型的**入站**和**出站**规则。 入站的规则上的传入数据包应用于虚拟机和虚拟机从出站规则应用于出站数据包。 规则应用于虚拟机所在的主机。 传入或传出数据包必须与**允许**规则匹配的允许它，否则它将被丢弃。

按优先级的顺序处理规则。 例如，具有较低优先级编号 (例如 100) 的规则处理规则具有较高优先级数字 (如 200) 之前。 一旦找到匹配项，没有更多的规则进行处理。

规则有如下规定︰

- **名称︰**规则的唯一标识符

- **类型︰**入站/出站

- **优先级︰** < 您可以指定一个介于 100 和 4096 >

- **源 IP 地址︰**源 IP 范围的 CIDR

- **源端口范围︰** < 整数或范围为 0 到 65536 >

- **目标 IP 范围︰**CIDR IP 范围的目标

- **目标端口范围︰** < 整数或范围为 0 到 65536 >

- **协议︰** < TCP、 UDP 或 * 允许 >

- **的访问︰**允许/拒绝

### 默认规则

NSG 包含默认规则。 不能删除默认的规则，但由于它们分配优先级最低，它们可以重写由您创建的规则。 默认的规则描述了推荐的平台的默认设置。 如所示，下面的默认规则，同时在入站和出站方向允许通信起始和结束在 VNet 中。

为出站方向允许连接到 Internet，则这是默认情况下阻止入站方向。 还有一个默认规则来允许 Azure 的负载平衡器 (LB) 来探测虚拟机的运行状况。 如果 VM 或一组虚拟机在 NSG 下不参与负载平衡设置，您可以忽略此规则。

下面是默认规则︰

**入站**

| 名称                              | 优先级 | 源 IP          | 源端口 | 目标 IP  | 目标端口 | 协议 | Access |
|-----------------------------------|----------|--------------------|-------------|-----------------|------------------|----------|--------|
| 允许入站 VNET                | 65000    | VIRTUAL_NETWORK    | *           | VIRTUAL_NETWORK | *                | *        | 允许  |
| 允许入站 AZURE 负载平衡器 | 65001    | AZURE_LOADBALANCER | *           | *               | *                | *        | 允许  |
| 拒绝所有入站                  | 65500    | *                  | *           | *               | *                | *        | 拒绝   |

**出站**

| 名称                    | 优先级 | 源 IP       | 源端口 | 目标 IP  | 目标端口 | 协议 | Access |
|-------------------------|----------|-----------------|-------------|-----------------|------------------|----------|--------|
| 允许出站 VNET     | 65000    | VIRTUAL_NETWORK | *           | VIRTUAL_NETWORK | *                | *        | 允许  |
| 允许出站 INTERNET | 65001    | *               | *           | 互联网        | *                | *        | 允许  |
| 拒绝所有出站       | 65500    | *               | *           | *               | *                | *        | 拒绝   |

### 特殊的基础结构规则

NSG 规则是明确的。 允许或拒绝以外的 NSG 规则中指定任何通信。 但是，有两种类型的通信量始终允许而不考虑网络安全组规范。 这些规定是进行支持的基础结构。

- **的主机节点的虚拟 IP:**基本的基础结构服务，如为 DHCP、 DNS 和运行状况监视提供了通过虚拟化的主机 IP 地址 168.63.129.16。 此公用 IP 地址属于 Microsoft，将虚拟化的唯一 IP 地址用于在所有地区达到此目的。 此 IP 地址映射到托管虚拟机的物理服务器计算机 （主机节点） 的 IP 地址。 主机节点充当 DHCP 中继、 DNS 递归解析器，并为负载平衡器运行状况探测器和计算机运行状况探测器的探测源。 与此 IP 地址的通信不应视为攻击。

- **授权 （密钥管理服务）︰**应授权在虚拟机中运行的 Windows 映像。 要做到这一点，授权请求发送到处理此类查询的密钥管理服务主机服务器。 这始终将是出站端口 1688年上。

### 默认标签

默认标签是系统提供解决 IP 地址的类别的标识符。 客户定义规则中，可以指定默认的标记。 默认标记如下所示︰

- **-VIRTUAL_NETWORK**此默认标签表示所有的网络地址空间。 它包括虚拟网络地址空间 (IP CIDR Azure 中)，以及所有已连接的内部部署地址空间 （局域网）。 此外包括 VNet 到 VNet 的地址空间。

- **-AZURE_LOADBALANCER**此默认标记表示 Azure 的基础结构的负载平衡器。 这将转换到 Azure 的健康的探测位置的 IP 将源于 Azure 数据中心。 这需要 NSG 如果 VM 或一组虚拟机与参与负载平衡组只。

- **互联网的**此默认标记表示外部虚拟网络，并可由公共 Internet 访问的 IP 地址空间。 此区域包括 Azure 拥有公用 IP 空间。

### 端口和端口范围

网络安全组规则可以指定单个源/目标端口或端口范围。 这是在想要打开的应用程序，如 FTP 端口范围广泛的情况下特别有用。 该区域只能连续且不能混用单个端口规范。

若要指定的端口范围，请使用-签名，如下所示的*DestinationPortRange*参数中︰

    Get-AzureNetworkSecurityGroup -Name ApptierSG `
  	| Set-AzureNetworkSecurityRule -Name FTP -Type Inbound -Priority 600 -Action Allow `
        -SourceAddressPrefix INTERNET -SourcePortRange * `
        -DestinationAddressPrefix * -DestinationPortRange 100-500 -Protocol *

### ICMP 通信流

当前的 NSG 规则仅允许 TCP 或 UDP 的协议。 不是 ICMP 的特定标记。 默认情况下，允许通信量从/到任何协议和端口的入站 VNet 规则通过虚拟网络中允许 ICMP 通信流不过，' *' VNet 中。

## 将 NSGs 相关联

当 NSG 直接关联到 VM-NSG 与虚拟机相关联，在 NSG 的网络访问规则直接应用于发送到虚拟机的所有通信。 只要 NSG 将更新规则的更改，更改将反映在几分钟之内处理的通信。 NSG 不同从虚拟机相关联时，状态返回到 NSG 之前引入如果 NSG 将使用, 系统默认值的即之前的所有内容。

NSG 与子网相关联时，将子网-NSG 相关联，在 NSG 的网络访问规则应用于子网中的所有虚拟机。 每次更新都在 NSG 的访问规则时所做的更改应用到子网中的所有虚拟机在几分钟之内。

NSG 关联到一个子网和 VM-很可能您可以关联到一个虚拟机的 NSG 的子网不同 NSG VM 所处的位置。 这支持和 VM 在这种情况下获取两个图层的保护。 入站通讯数据包经过访问网跟规则在 VM 和出站的情况下它会经历 VM 中首先指定规则之前的规则中指定的规则中指定的子网，如下图所示。

![NSG 的 Acl](./media/virtual-networks-nsg/figure1.png)

NSG 的 VM 或子网相关联时，网络访问控制规则变得非常明确。 该平台将不插入任何隐式规则来允许对特定端口的通信。 在这种情况下，如果在 VM 创建终结点，也必须创建规则以允许来自 Internet 的通信。 如果您不执行此操作，请 VIP:<Port>将无法从外部访问。

例如︰ 创建新的虚拟机并创建新的 NSG。 将 vm NSG 的相关联。 VM 可以为通过允许 VNET 入站规则虚拟网络中的其他虚拟机进行通信。 虚拟机还可以进行出站连接到 Internet 使用允许互联网出站规则。 稍后，在端口 80 接收流量到您在虚拟机中运行的网站上创建终结点。 直到将类似于以下 （见下文） 的规则添加到 NSG，往 VIP （公用虚拟 IP 地址） 上的端口 80 从互联网的数据包将不到达 VM。

| 名称 | 优先级 | 源 IP | 源端口 | 目标 IP | 目标端口 | 协议 | Access |
|------|----------|-----------|-------------|----------------|------------------|----------|--------|
| 网站  | 100      | 互联网  | *           | *              | 80               | TCP      | 允许  |

## 设计考虑事项

您必须了解与基础结构服务和承载 PaaS 服务虚拟机的通信方式设计您的 NSGs 时，由 Azure 托管。 大多数的 Azure PaaS 服务，例如 SQL 数据库和存储，只能通过一个公共的面向 Internet 地址访问。 同样适用于负载平衡探测器。

在 Azure 中的一个常见方案是基于这些对象是否需要访问 internet，或不子网中的虚拟机和 PaaS 角色的职责划分。 在这种情况下，您可能必须使用虚拟机或角色实例，需要访问 Azure Paas 服务，例如 SQL 数据库和存储，但不是需要到公用互联网的任何入站或出站通信子网。 

想象一下以下的 NSG 规则，针对这一情况︰

| 名称 | 优先级 | 源 IP | 源端口 | 目标 IP | 目标端口 | 协议 | Access |
|------|----------|-----------|-------------|----------------|------------------|----------|--------|
|没有互联网|100| VIRTUAL_NETWORK|& #42;|互联网|& #42;|TCP|拒绝| 

由于该规则拒绝所有访问从虚拟网络到 Internet，虚拟机不能访问任何 Azure PaaS 服务需要一个公用的 Internet 端点，例如 SQL 数据库。 

而不是使用拒绝规则，请考虑使用一个规则允许来自互联网，在虚拟的网络访问，但拒绝访问从互联网到虚拟的网络中，如下所示︰

| 名称 | 优先级 | 源 IP | 源端口 | 目标 IP | 目标端口 | 协议 | Access |
|------|----------|-----------|-------------|----------------|------------------|----------|--------|
|到互联网|100| VIRTUAL_NETWORK|& #42;|互联网|& #42;|TCP|允许|
|来自互联网|110| 互联网|& #42;|VIRTUAL_NETWORK|& #42;|TCP|拒绝| 

>[AZURE.WARNING] Azure 使用特殊的子网，称为**网关**网来处理其他 VNets 到 VPN 网关和内部网络。 将关联到该子网的 NSG 将导致您的 VPN 网关不能按预期的方式工作。 不到网关的子网关联 NSGs ！

## 计划-网络安全组工作流

下面是基本的工作流步骤时使用网络安全组。

### 工作流 – 创建和关联 NSG

1. 创建网络安全组 (NSG)。

1. 添加网络安全规则，除非默认规则就足够了。

1. 将给 VM NSG 相关联。

1. 更新虚拟机。

1. 在更新后，NSG 规则将立即生效。

### 工作流 – 更新现有的 NSG

1. 添加、 删除或更新现有的 NSG 中的规则。

1. NSG 与相关联的所有虚拟机将获得在几分钟内的更新。 当 NSG 规则已与虚拟机相关联，则不需要 VM 更新。

### 工作流 – 更改 NSG 关联

1. 将新的 NSG 给 VM 已关联到另一个的 NSG 相关联。

1. 更新虚拟机。

1. 在几分钟之内，从新的 NSG 的规则将会生效。

## 如何创建、 配置和管理您的网络安全组

到目前为止，可以配置和修改使用 PowerShell cmdlet 和 REST Api 仅 NSGs。 您不能通过管理门户配置 NSGs。 下面的 PowerShell cmdlet 将帮助您创建、 配置和管理您的 NSGs。

**创建网络安全组**

    New-AzureNetworkSecurityGroup -Name "MyVNetSG" -Location uswest `
        -Label "Security group for my Vnet in West US"

**添加或更新规则**

    Get-AzureNetworkSecurityGroup -Name "MyVNetSG" `
  	| Set-AzureNetworkSecurityRule -Name WEB -Type Inbound -Priority 100 `
        -Action Allow -SourceAddressPrefix 'INTERNET'  -SourcePortRange '*' `
        -DestinationAddressPrefix '*' -DestinationPortRange '*' -Protocol TCP


**NSG 从删除规则**

    Get-AzureNetworkSecurityGroup -Name "MyVNetSG" `
  	| Remove-AzureNetworkSecurityRule -Name WEB

**将关联 NSG 给 VM**

    Get-AzureVM -ServiceName "MyWebsite" -Name "Instance1" `
  	| Set-AzureNetworkSecurityGroupConfig -NetworkSecurityGroupName "MyVNetSG" `
  	| Update-AzureVM

**与虚拟机相关联的视图 NSGs**

    Get-AzureVM -ServiceName "MyWebsite" -Name "Instance1" `
  	| Get-AzureNetworkSecurityGroupAssociation

**从虚拟机中删除 NSG**

    Get-AzureVM -ServiceName "MyWebsite" -Name "Instance1" `
  	| Remove-AzureNetworkSecurityGroupConfig -NetworkSecurityGroupName "MyVNetSG" `
  	| Update-AzureVM

**NSG 与子网关联**

    Get-AzureNetworkSecurityGroup -Name "MyVNetSG" `
  	| Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'VNetUSWest' `
        -SubnetName 'FrontEndSubnet'

**子网相关联的视图 NSGs**

    Get-AzureNetworkSecurityGroupForSubnet -SubnetName 'FrontEndSubnet' `
        -VirtualNetworkName 'VNetUSWest' 

**NSG 删除子网**

    Get-AzureNetworkSecurityGroup -Name "MyVNetSG" `
  	| Remove-AzureNetworkSecurityGroupFromSubnet -VirtualNetworkName 'VNetUSWest' `
        -SubnetName 'FrontEndSubnet'

**删除 NSG**

    Remove-AzureNetworkSecurityGroup -Name "MyVNetSG"

**获取规则以及 NSG 的详细信息**

    Get-AzureNetworkSecurityGroup -Name "MyVNetSG" -Detailed
 
**查看所有 Azure PowerShell cmdlet 相关的对 NSGs**

    Get-Command *azurenetworksecuritygroup*

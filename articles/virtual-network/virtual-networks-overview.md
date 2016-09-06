---
ms.openlocfilehash: 8b3053eed70dbdc90e64d9b2a93068bc04258309
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="Azure 虚拟网络 (VNet) 概述"
   description="了解有关在 Azure 中的虚拟网络 (VNets)"
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
   ms.date="08/05/2015"
   ms.author="telmos" />

# 虚拟网络概述

Azure 的虚拟网络 (VNet) 是在云网络的表示。 您可以控制 Azure 的网络设置，并定义 DHCP 地址块、 DNS 设置、 安全策略和路由。 此外进一步可以您 VNet 划分子网并部署 Azure IaaS 虚拟机 (Vm) 和 PaaS 角色实例中，在相同的方式，您可以部署物理和到您的内部数据中心的虚拟机。 从本质上讲，您可以扩展到 Azure，让您自己的 IP 地址块的网络。 

为了更好地了解 VNets，看一看图，它显示了一个简化的内部部署网络。

![内部网络](./media/virtual-networks-overview/figure01.png)

上图显示了连接到公用互联网通过路由器的内部网络。 您还可以查看路由器和 DMZ 主机的 DNS 服务器和 web 服务器场之间的防火墙。 Web 服务器场进行负载平衡使用硬件负载平衡器暴露给 Internet，而且消耗资源从内部子网。 内部子网分开的另一个防火墙，并且主机 Active Directory 域控制器服务器、 数据库服务器和应用程序服务器的 DMZ。

同一个网络可以承载 Azure 中，如下图中所示。

![Azure 的虚拟网络](./media/virtual-networks-overview/figure02.png)

注意到 Azure 的基础架构如何起作用的路由器，而无需任何配置公用的 Internet 从您 VNet 允许访问。 防火墙可以替换成网络安全组 (NSGs) 应用于每个单独的子网。 并通过 Azure 中的互联网对称和内部负载平衡器替换物理负载平衡器。

## 虚拟网络

VNets 为 IaaS Vm 和角色 PaaS 角色实例部署到它们提供以下服务︰

- **隔离**。 VNets 是从彼此完全独立。 它允许您创建用于开发、 测试和生产单独使用同一个 CIDR 地址块的 VNets。

- **包容**。 VNets 不能跨越多个 Azure 的区域。 

    >[AZURE.NOTE] 在 Azure 中有两种部署模式︰ 经典 （也称为服务管理） 和 Azure 资源管理器 (ARM)。 无法添加到关系组中，或创建作为区域 VNet 经典的 VNets。 如果关联组中有 VNet，建议到[迁移到区域 VNet](./virtual-networks-migrate-to-regional-vnet.md)。 

- **公共 Internet 访问权限**。 默认情况下，VNet 中的所有 IaaS Vm 和 PaaS 角色实例可以访问公用互联网。 您可以通过使用网络安全组 (NSGs) 来控制访问。

- **对 VNet 中的虚拟机的访问**。 IaaS 虚拟机和 PaaS 角色实例可以连接彼此相同的 VNet，即使他们在不同的子网，而无需配置一个网关或使用公用 IP 地址，汇集 PaaS 和 IaaS 环境。

- **名称解析**。 Azure IaaS Vm 和部署您的 VNet 在 PaaS 角色实例提供内部名称解析。 您还可以部署 DNS 服务器和配置 VNet 来使用它们。

- **连接性**。 VNets 可以通过站点到站点 VPN 连接或 ExpressRoute 连接到彼此，和甚至内部数据中心，连接。 若要了解有关 VPN 网关的详细信息，请访问[有关 VPN 网关](./vpn-gateway-about-vpngateways.md)。 若要了解有关 ExpressRoute 的详细信息，请访问[ExpressRoute 技术概述](./expressroute-introduction.md)。

    >[AZURE.NOTE] 请确保在部署到 Azure 环境的任意 IaaS Vm 或 PaaS 角色实例之前创建 VNet。 基于 ARM Vm 需要 VNet，如果您不指定现有 VNet，Azure 创建默认 VNet 可能具有与您的内部网络的 CIDR 地址块冲突。 使 ti 使您无法连接到内部网络的您的 VNet。

## 子网

您可以将您的 VNet 划分为多个子网的组织和安全。 VNet 内的子网可以进行通信，而无需任何额外的配置。 此外可以更改级别的子网的路由设置，然后将 NSGs 应用到的子网。

## IP 地址

有两种类型的 IP 地址分配给在 Azure 中的组件︰ 公钥和私钥。 IaaS 虚拟机和部署到 Azure 的子网的 PaaS 角色实例自动分配专用 IP 地址给每根据分配给您的子网的 CIDR 地址块的 Nic。 您可以分配您的 IaaS Vm 和 PaaS 角色实例的公共 IP 地址。 

这些 IP 地址是动态的这意味着他们可以在任何时间更改。 您可能希望确保 IP 地址对于某些服务保持不变，在所有的时间。 若要执行此操作，可以保留一个 IP 地址，使其静态。

## Azure 负载平衡器

您可以在 Azure 中使用两种类型的负载平衡器︰

- **外部负载平衡器**。 可以使用外部负载平衡器要从公共 Internet 访问的 IaaS 虚拟机和 PaaS 角色实例为提供高可用性。

- **内部负载平衡器**。 内部负载平衡器可用于为 IaaS Vm 并从您的 VNet 中的其他服务访问的 PaaS 角色实例提供高可用性。

若要了解有关负载平衡在 Azure 中的详细信息，请访问[负载平衡器概述](../load-balancer-overview.md)。

## 网络安全组 (NSG)

您可以创建可控制入站和出站访问网络接口 (Nic) 的 NSGs 虚拟机和子网。 每个 NSG 包含一个或多个规则，指定通信被批准或拒绝根据源 IP 地址、 源端口、 目标 IP 地址和目标端口。 若要了解有关 NSGs 的详细信息，请访问[什么是网络安全组](../virtual-networks-nsg.md)。

## 即用虚拟装置

虚拟设备是在您运行的软件装置功能，如防火墙、 WAN 优化或入侵检测的 VNet 只是另一个虚拟机。 在 Azure VNet 通信穿虚拟应用装置，使用它的功能，可以创建一个路由。

例如，可以使用 NSGs 在您 VNet 上提供安全。 但是，NSGs 提供第 4 层为传入和传出数据包的访问控制列表 (ACL)。 如果您想要使用一层 7 的安全模型，您需要使用一个防火墙设备。

即用虚拟装置取决于[用户定义的路由和 IP 转发](../virtual-networks-udr-overview.md)。

## 下一步行动

- [创建 VNet](../virtual-networks-create-a-vnet.md)和子网。
- [创建一个 VNet 中的虚拟机](../virtual-machines-windows-tutorial.md)。
- 请查阅[NSGs](../virtual-networks-nsg.md)。
- 了解[负载平衡器](../load-balancer-overview.md)。
- [保留的内部 IP 地址](../virtual-networks-reserved-private-ip.md)
- [保留的公共 IP 地址](../virtual-networks-reserved-public-ip.md)。
- 关于[用户定义的路由和 IP 转发](virtual-networks-udr-overview.md)。

---
ms.openlocfilehash: 51073da86ca4e14eedac00ae7da4f1b24da002cb
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="虚拟网络的 VPN 网关常见问题 |Microsoft Azure"
   description="VPN 网关的常见问题解答。 Microsoft Azure 虚拟网络跨内部连接、 混合配置连接和 VPN 网关的常见问题解答"
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carolz"
   editor="" />
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/20/2015"
   ms.author="cherylmc" />

# VPN 网关的常见问题解答

## 连接到虚拟网络

### 可以将连接虚拟网络中不同 Azure 的地区？

是。 事实上，没有任何区域约束。 一个虚拟的网络可以连接到另一个虚拟的网络，在同一个地区，或在其他 Azure 地区。

### 可以将连接的不同预订中的虚拟网络？

是。
### 可以连接到多个站点从一个虚拟网络？

您可以通过使用 Windows PowerShell 和 Azure REST Api 连接到多个站点。 请参阅[多站点和 VNet 到 VNet 连接](#multi-site-and-vnet-to-vnet-connectivity)常见问题解答部分。
## 我跨内部连接选项有哪些？

支持以下跨内部连接︰

- [站点到站点](vpn-gateway-site-to-site-create.md)— — （IKE v1 和 IKE v2） ipsec 的 VPN 连接。 这种类型的连接需要 VPN 设备或 RRAS。

- [网站点](vpn-gateway-point-to-site-create.md)– SSTP （安全套接字隧道协议） 通过 VPN 连接。 此连接不需要 VPN 设备。

- [VNet 与 VNet](virtual-networks-configure-vnet-to-vnet-connection.md) -这种类型是连接的站点的配置相同。 VNet 到 VNet 是通过 IPsec （IKE v1 和 IKE v2） 的 VPN 连接。 它不需要 VPN 设备。

- [多站点](vpn-gateway-multi-site.md)-这是另一种形式允许您连接到虚拟网络的多个内部站点的站点对站点配置。

- [ExpressRoute](../expressroute/expressroute-introduction.md) -ExpressRoute 是到 Azure 从您 WAN、 不在公共 Internet 的直接连接。 [ExpressRoute 技术概述](../expressroute/expressroute-introduction.md)和[ExpressRoute 常见问题](../expressroute/expressroute-faqs.md)的详细信息，请参阅。

### 站点到站点连接和点到站点之间的区别是什么？

**站点到站点**连接，可以连接的任何计算机上您的场所的任何虚拟机或虚拟网络，具体取决于您选择配置路由中的角色实例。 它是始终可用的跨内部连接的是好的选择，非常适合于混合配置。 必须在网络的边缘部署 IPsec VPN 装置 （硬件或软的装置），依赖于这种类型的连接。 若要创建此类型的连接，您必须具有所需的 VPN 硬件和面向外部的 IPv4 地址。

**网站点**连接让您从任何地方从一台计算机连接到位于虚拟网络中的任何内容。 它使用 Windows 内置的 VPN 客户端。 作为点为站点配置的一部分，您可以安装证书和 VPN 客户端配置软件包，其中包含的设置，使计算机能够连接到任何虚拟机或虚拟网络中的角色实例。 当您想连接到虚拟网络，但不位于内部部署是很好。 您不能访问 VPN 硬件或对外的 IPv4 地址，这两个所需的站点对站点连接时，它也是一个不错的选择。 

注意︰ 您可以配置虚拟网络使用站点对站点和点到站点同时，前提是您创建使用动态路由网关站点对站点连接。 

有关详细信息，请参阅[关于安全的跨内部连接的虚拟网络](vpn-gateway-cross-premises-options.md)。

### ExpressRoute 是什么？

ExpressRoute 允许您创建 Microsoft 数据中心和基础架构，以在您的场所或在主机托管环境之间的专用连接。 与 ExpressRoute，可以建立连接到 Microsoft 云服务，例如 Microsoft Azure 和 Office 365 在 ExpressRoute 合作伙伴同地区设施，也可以直接从现有的 WAN 网络 （例如，网络服务提供商提供了 MPLS VPN) 连接。

ExpressRoute 连接在 Internet 上提供更好的安全性、 更多可靠性、 更高的带宽和低延迟时间比典型的连接。 在某些情况下，使用 ExpressRoute 连接内部网络和 Azure 之间传输数据可以同时产生显著的成本优势。 如果您已经创建了跨内部连接从内部网络到 Azure，您可以迁移到 ExpressRoute 时保持不变的虚拟网络连接。

查看更多详细信息的[ExpressRoute 的常见问题解答](../expressroute/expressroute-faqs.md)。

## 站点到站点连接，VPN 设备

### 选择 VPN 设备时应考虑什么？

我们验证合作的标准站点到站点 VPN 设备一套设备供应商。 找不到的已知兼容的 VPN 设备、 它们的相应配置指导或示例和设备规格列表[在此处](vpn-gateway-about-vpn-devices.md)。 列出为已知的兼容设备系列中的所有设备应都使用虚拟网络。 要帮助配置 VPN 设备，请参阅设备配置一个或多个对应于相应设备系列的链接。

### 如果我不在已知兼容设备列表中的 VPN 设备，做什么？

如果您不是您列为已知兼容的 VPN 设备的设备，请参阅要用于您的 VPN 连接，您需要验证它支持 IPsec/IKE 配置选项和参数列出[以下](vpn-gateway-about-vpn-devices.md#devices-not-on-the-compatible-list)符合。 满足最低要求的设备应很好地使用 VPN 网关。 请有关其他支持和配置说明，与设备制造商联系。

### 可以使用 Vpn 软件连接到 Azure 吗？

我们支持站点对站点部署跨配置 Windows Server 2012 路由和远程访问 (RRAS) 的服务器。

我们的网关应处理其他软件 VPN 解决方案，只要它们符合行业标准的 IPsec 实现。 联系软件供应商的说明配置和支持。

## 点到站点的连接

### 与点到网站可以使用什么操作系统？

支持下列操作系统︰

- Windows 7 （仅适用于 64 位版本）

- Windows Server 2008 R2

- Windows 8 （仅适用于 64 位版本）

- Windows Server 2012

### 可以为点到站点支持 SSTP 使用任何软件 VPN 客户端？

No. 支持仅只限于上面列出的 Windows 操作系统版本。

### 我点到站点配置中可以有多少的 VPN 客户端终结点？

我们支持最多 128 个 VPN 客户端能够同时连接到虚拟网络。

### 我可以使用点到站点的连接我自己内部 PKI 根 CA 吗？

到目前为止，支持的只是自签名的根证书。

### 可以通过代理服务器和防火墙使用点到网站功能？

是。 我们到隧道穿越防火墙使用 SSTP （安全套接字隧道协议）。 该隧道将显示为 HTTPs 连接。

### 如果我重新启动客户端计算机配置为点到网站，将 VPN 自动重新连接？

默认情况下，客户端计算机将不重新建立 VPN 连接自动。

### 在 VPN 客户端上会点到站点支持自动重新连接和 DDNS？

自动重新连接和 DDNS 当前不支持点到站点 Vpn。

### 可以有站点对站点和点到站点配置为相同的虚拟网络共存？

是。 如果您有为您的虚拟网络的动态路由选择 VPN 网关，同时这些解决方案将起作用。 我们不支持点到网站中静态路由的 VPN 网关。

### 可以将配置点到站点客户端同时连接到多个虚拟网络？

是，它是可能的。 但虚拟网络不能有重叠的 IP 前缀和虚拟网络间点到站点地址空间不得重叠。

### 通过站点到站点或点到站点的连接可以期望多少吞吐量呢？

很难维护 VPN 隧道的实际吞吐量。 IPsec 与 SSTP 是大量加密的 VPN 协议。 吞吐量还受到延迟和您的场所和互联网之间的带宽。

## 网关

### 静态路由的网关是什么？

静态路由的网关实现基于策略的 Vpn。 基于策略的 Vpn 加密，并将数据包发送到基于地址前缀，您在部署网络和 Azure VNet 之间的组合的 IPsec 隧道。 策略 （或通信选择器） 通常被定义为在 VPN 配置访问列表中。

### 什么是动态路由的网关？

动态路由网关实现基于路由的 Vpn。 基于路由的 Vpn 使用 IP 转发或到直接数据包到其相应的隧道接口的路由表中的"路由"。 然后，隧道接口加密或解密数据包和隧道。 路由基于 Vpn 的通讯或策略选择器被配置为任意到任意 （或通配符）。

### 我在创建之前是否可以获得我 VPN 网关的 IP 地址？

No. 您必须创建您的网关，首先要获取 IP 地址。 如果您删除并重新创建您的 VPN 网关，则会更改 IP 地址。

### 我如何的 VPN 隧道进行在验证？

Azure VPN 使用 PSK （预共享密钥） 身份验证。 当我们创建 VPN 隧道，我们可以生成预共享的密钥 (PSK)。 为您自己设置预配置共享密钥 PowerShell cmdlet 或 REST API，您可以更改自动生成 PSK。

### 可以使用预共享密钥设置 API 来配置我静态路由网关 VPN？

是的设置预配置共享密钥 API 和 PowerShell cmdlet 可以用于配置 Azure 的静态路由选择 VPN 和动态路由的 Vpn。

### 可以使用其他身份验证选项吗？

我们被限制为使用预共享的密钥 (PSK) 进行身份验证。

### 什么是"网关"，以及为什么需要？

我们有我们运行才能启用跨内部连接的网关服务。 我们需要从我们启用您的场所和云之间路由的路由域 2 IP 地址。 我们要求您指定至少一个 /29，我们可以从中选择 IP 子网地址设置工艺路线。 即使您可以创建 /29 网，了解一些功能需要特定网关大小。 请按照您想要配置功能的网关网要求。

请注意，您必须部署虚拟机或网关网中的角色实例。

### 如何指定的流量所经历的 VPN 网关？

如果您正在使用 Azure 门户，添加所需的虚拟网络下的本地网络的网络页面上通过网关发送每个范围。

### 可以配置强制隧道？

是。 请参阅[配置强制隧道](vpn-gateway-about-forced-tunneling.md)。

### 可以设置我自己在 Azure 中的 VPN 服务器并使用它来连接到内部网络？

是的您可以部署自己的 VPN 网关或从 Azure 市场或创建 VPN 路由器的 Azure 中的服务器。 需要在您的虚拟网络，以确保您在部署网络与虚拟网络的子网之间正确路由通信配置用户定义路由。

### 有关网关类型、 要求和吞吐量的详细信息

有关详细信息，请参阅[关于 VPN 网关](vpn-gateway-about-vpngateways.md)。

## 多站点和 VNet 到 VNet 的连接

### 哪种类型的网关可以支持多站点和 VNet 到 VNet 的连接？

只有动态路由 Vpn。

### 可以将虚拟网络连接到另一个虚拟网络使用静态路由的 VPN 的动态路由的 VPN 与？

不，两个虚拟网络必须使用动态路由的 Vpn。

### VNet 到 VNet 通信是安全的？

是的它受 IPsec/IKE 加密保护。

### 没有通过 Azure 主干旅行 VNet 到 VNet 的通信？

是。

### 多少的内部网站和虚拟网络可以一个虚拟网络连接到？

最大值。 10 合并为基本和标准动态路由的网关;对于高性能 VPN 网关的 30。

### 可以使用虚拟网络与多个 VPN 隧道使用点到站点 Vpn？

是的点到网站 (P2S) Vpn 连接到多个在本地站点和其他虚拟网络的 VPN 网关使用。

### 可以将多个隧道配置之间我虚拟的网络和内部网站使用多站点 VPN？

否，不支持冗余 Azure 的虚拟网络和内部网站之间的隧道。

### 那里可以将重叠之间的连接的虚拟网络和内部部署本地站点地址空间吗？

No. Netcfg 文件上传或创建虚拟网络出现故障，将导致重叠的地址空间。

### 获取更多带宽和更多比的站点到站点 Vpn 的单个虚拟网络？

不是，所有 VPN 隧道，包括点到站点 Vpn，都共享相同的 Azure VPN 网关和可用带宽。

### 是否可以使用 Azure VPN 网关到我在的内部部署站点间传输通信量或另一个虚拟网络？

通过 Azure VPN 网关传输通信是可行的但依赖于 netcfg 配置文件中的静态定义的地址空间。 BGP 尚不支持使用 Azure 虚拟网络和 VPN 网关。 而 BGP，手动定义在 netcfg 的传输地址空间是非常错误容易出错，并且不建议这样做。

### Azure 是否生成相同的所有我的 VPN 连接的相同的虚拟网络 IPsec/IKE 预共享的密钥？

否，默认情况下的 Azure 生成不同的不同的 VPN 连接的预共享的密钥。 但是，可以使用设置 VPN 网关键 REST API 或 PowerShell cmdlet 来设置您喜欢的键值。 键必须是字母数字字符串，长度在 1 到 128 个字符之间。

### Azure 是否收取虚拟网络之间的通讯？

对于不同 Azure 的虚拟网络之间的通信，Azure 只对从一个 Azure 区域遍历到另一个的通信费用。 费率是 Azure [VPN 网关定价](https://azure.microsoft.com/pricing/details/vpn-gateway/)页上列出。


### 可以将连接虚拟网络与 IPsec Vpn 我 ExpressRoute 电路？

是的这被支持。 有关详细信息，请参阅[配置 ExpressRoute 和站点到站点 VPN 连接的共存](../expressroute/expressroute-coexist.md)。

## 连接和虚拟机

### 如果我的虚拟机是虚拟的网络中，我使用跨内部连接方式应该连接到虚拟机？

有几个选项。 如果已启用的 RDP，您可以创建一个终结点，则可以使用 VIP 连接到您的虚拟机。 在这种情况下，您会指定 VIP 和您想要连接到的端口。 您需要在您的通信的虚拟机上配置的端口。 通常情况下，您将转到管理门户并将 RDP 连接的设置保存到您的计算机。 这些设置将包含必须的连接信息。

如果您具有跨本地连接配置的虚拟网络，可以通过内部调节或专用 IP 地址连接到您的虚拟机。 此外可以从相同的虚拟网络上的其他虚拟机连接到虚拟机的内部调节。 如果外部虚拟网络连接从一个位置使用 DIP 不能到您的虚拟机的 RDP。 例如，如果您有点到站点虚拟网络配置，并且您没有从您的计算机建立连接，您无法连接到虚拟机通过 DIP。

### 我的虚拟机是使用跨内部连接的虚拟网络，如果没有来自我的虚拟机的所有通信都穿过该连接？

No. 有目标的通信包含在指定的虚拟网络本地网络 IP 地址范围的 IP 将会通过虚拟网络网关。 通讯具有虚拟的网络中找到 IP 将停留在虚拟网络中的目标。 其他通信是通过负载平衡器发送到公用网络，或者如果使用强制隧道，则发送通过 Azure VPN 网关。 如果您正在进行故障排除，很重要，以确保您有在您想要发送到网关的本地网络中列出的所有范围。 请验证本地网络地址范围不能重叠与任何虚拟的网络的地址范围。 此外，您需要验证您使用的 DNS 服务器将名称解析为正确的 IP 地址。

## 下一步行动

查看更多详细信息的多个网络常见问题解答︰

- [虚拟网络常见问题解答](../virtual-network/virtual-networks-faq.md)

- [ExpressRoute 的常见问题解答](../expressroute/expressroute-faqs.md)

 
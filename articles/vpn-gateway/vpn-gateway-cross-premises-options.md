---
ms.openlocfilehash: 854caa62c2f7658f39e62e79d262263be8af19d7
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="有关虚拟网络的安全的跨内部连接 |Microsoft Azure"
   description="了解的安全跨内部连接的虚拟网络，包括站点对站点、 站点点和 ExpressRoute 连接类型。"
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
   ms.date="08/07/2015"
   ms.author="cherylmc" />

# 有关虚拟网络的安全的跨内部连接

如果您想要到虚拟网络安全地连接您的内部网站，有三个选项可用︰[站点到站点](#site-to-site-connections)，点到网站[](#point-to-site-connections)，和[ExpressRoute](#expressroute-connections)。 

您选择的选项取决于多种因素，如︰


- 您的解决方案需要哪种类型的吞吐量？
- 通信通过安全 VPN，通过公用的 Internet 或专用连接吗？
- 您是否有可供使用的公共 IP 地址？
- 您是否计划使用 VPN 设备？ 如果是这样，是否兼容？
- 要连接几台计算机，或做为您的网站需要永久连接？
- 哪种类型的 VPN 网关是您想要创建的解决方案所需的？

下表可帮助您确定最佳的连接选项，为您的解决方案。

| -                            | **点到站点**                                                                   | **站点到站点**                                                                                              | **ExpressRoute 的 EXP**                                                                                                                      | **-NSP ExpressRoute**                                                                                                                      |
|------------------------------|---------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|
| **Azure 支持服务** | 云的虚拟机                                                  | 云服务的虚拟机                                                                           | [服务列表](../expressroute/expressroute-faqs.md#supported-azure-services)                                                                                                                | [服务列表](../expressroute/expressroute-faqs.md#supported-azure-services)                                                                                                                |
| **典型的带宽**       | 通常 < 100 Mbps 聚合                                                  | 通常 < 100 Mbps 聚合                                                                            | 200 Mbps、 500 Mbps、 1 Gbps 和 10 Gbps                                                                                                  | 10 Mbps、 50 Mbps、 100 Mbps、 500 Mbps、 1 Gbps                                                                                            |
| **受支持的协议**      | 安全套接字隧道协议 (SSTP)                                        | [IPsec](http://go.microsoft.com/fwlink/p/?LinkId=618592)                                                                       | 通过 Vlan 的直接连接                                                                                                            | NSP 的 VPN 技术 （MPLS，VPLS...）                                                                                                  |
| **路由选择**                  | 静态                                                                          | 静态-我们支持策略 — — 基于 （静态路由），并基于工艺路线 (动态路由 VPN)                   | BGP                                                                                                                                     | BGP                                                                                                                                     |
| **连接恢复**    | 主动-被动                                                                  | 主动-被动                                                                                            | 活动                                                                                                                           | 活动                                                                                                                           |
| **典型用例**         | 建立原型、 开发 / 测试 / 实验室方案，云服务和虚拟机 | 开发 / 测试 / 实验室方案和小型规模的云服务和虚拟机的生产工作负载 | 访问所有 Azure 服务 （验证列表），企业级和使命关键的工作负载，备份、 大数据、 Azure 作为灾难恢复站点 | 访问所有 Azure 服务 （验证列表），企业级和使命关键的工作负载，备份、 大数据、 Azure 作为灾难恢复站点 |
| **SLA**                      | [SLA](https://azure.microsoft.com/support/legal/sla/)                           | [SLA](https://azure.microsoft.com/support/legal/sla/)                                                     | [SLA](https://azure.microsoft.com/support/legal/sla/)                                                                                   | [SLA](https://azure.Microsoft.com/support/legal/sla/)                                                                                   |
| **定价**                  | [定价](http://azure.microsoft.com/pricing/details/vpn-gateway/)              | [定价](http://azure.microsoft.com/pricing/details/vpn-gateway/)                                             | [定价](http://azure.microsoft.com/pricing/details/expressroute/)                                                                           | [定价](http://azure.microsoft.com/pricing/details/expressroute/)                                                                           |
| **技术文档**  | [VPN 网关文档](https://azure.microsoft.com/documentation/services/vpn-gateway/)                                               | [VPN 网关文档](https://azure.microsoft.com/documentation/services/vpn-gateway/)                                                                         | [ExpressRoute 文档](https://azure.microsoft.com/documentation/services/expressroute/)                                                                                                      | [ExpressRoute 文档](https://azure.microsoft.com/documentation/services/expressroute/)                                                                                                      |
| **常见问题**                      | [VPN 网关的常见问题解答](vpn-gateway-vpn-faq.md)                                                         | [VPN 网关的常见问题解答](vpn-gateway-vpn-faq.md)                                                                                   | [ExpressRoute 的常见问题解答](../expressroute/expressroute-faqs.md)                                                                                                                | [ExpressRoute 的常见问题解答](../expressroute/expressroute-faqs.md)                                                                                                                |
                                                                                 



## 站点到站点连接

站点到站点 VPN 可以创建内部网站和虚拟网络之间的安全连接。 若要创建一个站点到站点连接，位于内部网络的 VPN 设备被配置为使用 Azure VPN 网关创建安全连接。 创建此连接后，您的本地网络上的资源和资源位于虚拟网络中可以进行直接和安全地通信。 站点到站点连接不需要您建立本地网络访问的虚拟网络中每台客户计算机一个单独的连接。

**使用站点对站点连接时︰**

- 您想要创建一个混合解决方案。
- 要连接之间内部位置和虚拟网络而无需客户端配置。
- 您想连接是持久性的。 

**要求**

- 内部部署 VPN 设备必须具有面向 Internet 的 IPv4 地址。 这不能是背后 nat。
- 您必须具有兼容的 VPN 设备。 请参阅[关于 VPN 设备](http://go.microsoft.com/fwlink/p/?LinkID=615099)。 
- 您使用的 VPN 设备必须是兼容与网关类型所需的解决方案。 请参阅[关于 VPN 网关](vpn-gateway-about-vpngateways.md)。
- 网关 SKU 也将影响聚合吞吐量。 [网关 Sku](vpn-gateway-about-vpngateways.md#gateway-skus)的详细信息，请参见 

有关配置站点到站点 VPN 网关连接的信息，请参阅[配置站点到站点 VPN 连接使用的虚拟网络](vpn-gateway-site-to-site-create.md)。 

如果您想要创建使用 RRAS 的站点到站点 VPN 网关连接，请参阅[配置站点到站点 VPN 使用 Windows Server 2012 路由和远程访问服务 (RRAS)](https://msdn.microsoft.com/library/dn636917.aspx)。


## 点到站点的连接

点到站点 VPN 还允许您创建一个安全连接到虚拟网络。 在点对站点配置中，连接每个您想要连接到虚拟网络的客户端计算机上单独配置。 点到站点的连接不需要 VPN 设备。 这种类型的连接将使用每台客户端计算机安装的 VPN 客户端。 通过手动启动来自内部客户端计算机的连接建立 VPN。

点到站点和站点的配置可同时，存在但与站点到站点连接，不同的是不能同时 ExpressRoute 连接到相同的虚拟网络配置点到站点的连接。

**使用点到站点的连接时︰**

- 只想将几个客户端连接到虚拟网络配置。

- 要从远程位置连接到虚拟网络。 例如，连接从咖啡店或大会会场。

- 有站点到站点连接，但有一些需要从远程位置连接的客户端。

- 没有满足最低要求为站点到站点连接的 VPN 设备访问。

- 您没有 Internet 面临的 VPN 设备的 IPv4 地址。

有关配置点到站点连接的详细信息，请参阅[配置点到站点 VPN 连接到虚拟网络](vpn-gateway-point-to-site-create.md)。

## ExpressRoute 连接

Azure ExpressRoute 允许您创建之间 Azure 数据中心和基础架构，以在您的场所或在主机托管环境中的专用连接。 ExpressRoute 连接不会通过公共互联网并通过互联网提供更多可靠性、 更快的速度、 更低的延迟和更高的安全性，比典型的连接。

在某些情况下，使用 ExpressRoute 连接内部和 Azure 之间传输的数据可以同时产生显著的成本优势。 使用 ExpressRoute，可以建立到 Azure ExpressRoute 位置 （Exchange 提供设备） 连接或直接从现有 WAN 网络 （如 MPLS VPN) 提供的网络服务提供商连接到 Azure。

有关 ExpressRoute 的详细信息，请参阅[ExpressRoute 技术概述](../expressroute/expressroute-introduction.md)。


## 下一步行动

请参阅[ExpressRoute 常见问题解答](../expressroute/expressroute-faqs.md)和[VPN 网关常见问题](vpn-gateway-vpn-faq.md)的详细信息。




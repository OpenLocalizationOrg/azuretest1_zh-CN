---
ms.openlocfilehash: 67aad6440b2ed5445d3bb0487f070ad838779998
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="负载平衡 Azure 的基础结构服务"
    description="介绍了两种不同类型的负载平衡支持的 Azure︰ 负载平衡器，以云服务和 Azure 流量管理器的客户端通信。"
    services="virtual-machines"
    documentationCenter=""
    authors="joaoma"
    manager="adinah"
    editor=""/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/21/2015"
    ms.author="joaoma"/>


# 负载平衡 Azure 的基础结构服务#

Azure 的基础结构服务有两个级别的负载平衡︰

- **DNS 级别**︰ 负载平衡对于不同的云服务的通信中心，到不同的 Azure 网站位于不同的数据位于不同的数据中心或外部端点。 这是使用 Azure 流量管理器和循环法负载平衡方法。
- **网络级别**︰ 负载平衡到不同的虚拟机的云服务，传入的 Internet 通讯或负载平衡的云服务或虚拟网络中的虚拟机之间的通信。 这是通过 Azure 负载平衡器。

## 通信管理器负载平衡以云服务和网站##

通信管理器允许您控制用户通信终结点，可以包括云服务、 网站、 外部网站和其他通信管理器配置文件的分发。 通信管理器的工作原理是将智能策略引擎应用到您的 Internet 资源的域名的域名系统 (DNS) 查询。 您的云服务或网站可以运行在不同数据中心遍及世界各地。

必须使用其余部分或 Windows PowerShell 为终结点配置外部端点或流量管理器配置文件。

通信管理器使用三种负载平衡方法来分配流量︰

- **故障转移**︰ 当您想要使用主终结点的所有通信，但在主变得不可用的情况下提供备份时使用此方法。
- **性能**︰ 具有终结点在不同的地理位置和所需请求的客户机使用以最低的延迟"最接近"的终结点时使用此方法。
- **循环︰** 当您想要在一组云服务在同一个数据中心或云服务或在不同数据中心的网站之间分布负载，请使用此方法。

有关详细信息，请参阅[有关通信管理器负载平衡方法](../traffic-manager/traffic-manager-load-balancing-methods.md)。

下图举例说明的循环负载平衡分配不同的云服务之间的通信方法。

![负载平衡](./media/virtual-machines-load-balance/TMSummary.png)

基本过程是如下︰

1.  Internet 客户端查询 web 服务所对应的域名。
2.  DNS 将名称查询请求转发到通讯管理器中。
3.  通信管理器选择循环列表中的下一步的云服务，并发送回的 DNS 名称。 互联网客户端的 DNS 服务器将名称解析为 IP 地址，并将其发送到 Internet 客户端。
4.  互联网客户端连接选择由通信管理器管理的云服务。

有关详细信息，请参阅[通信量管理器](../traffic-manager/traffic-manager-overview.md)。

## Azure 负载平衡的虚拟机 ##

在同一个虚拟机云服务或虚拟网络可以相互直接使用其专用的 IP 地址。 计算机和虚拟网络的云服务之外的服务只能与云服务或使用已配置的终结点的虚拟网络中的虚拟机通信。 终结点是公用 IP 地址和端口到该专用 IP 地址的映射和虚拟机或 web 角色在 Azure 的云服务的端口。

Azure 负载平衡器随机分布在多个虚拟机或服务，也就是负载平衡集配置中的特定类型的传入通信。 例如，您可以在多个 web 服务器或 web 角色分配 web 请求通信量的负载。

下图显示了负载平衡的公钥和私钥的 TCP 端口 80 的三个虚拟机之间共享的标准 （非加密） 的 web 通信的终结点。 这些三个虚拟机的负载平衡集中。

![负载平衡](./media/virtual-machines-load-balance/LoadBalancing.png)

有关详细信息，请参阅[Azure 负载平衡器](../load-balancer/load-balancer-overview.md)。 创建负载平衡集的步骤，请参阅[配置负载平衡设置](../load-balancer/load-balancer-internet-getstarted.md)。

Azure 也可以负载的云服务或虚拟网络中的平衡。 这被称为内部负载平衡，可以采用下列方式︰

- 在多层应用程序 （例如，在 web 和数据库层） 之间的不同层中的服务器之间进行负载平衡。
- 若要加载平衡的业务线 (LOB) 应用程序承载 Azure 中而不需要额外增加的负载平衡硬件或软件。
- 包括内部服务器中的一组计算机的通信是负载平衡。

类似于 Azure 负载平衡、 内部负载平衡推动通过配置内部负载平衡设置。

下图显示了一种业务 (LOB) 应用程序在虚拟网络中跨部署在三个虚拟机之间共享一套内部的负载平衡的端点。

![负载平衡](./media/virtual-machines-load-balance/LOBServers.png)

## 下一步行动

有关创建负载平衡集的步骤，请参阅[配置内部负载平衡设置](../load-balancer/load-balancer-internal-getstarted.md)。

有关负载平衡器的详细信息，请参阅[内部负载平衡](../load-balancer/load-balancer-internal-overview.md)。

<!-- LINKS -->

---
ms.openlocfilehash: 8bf9a2610e4adc3e76781ea35a7e6b76edd7a1fb
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

<properties 
   pageTitle="内部负载平衡器概述 |Microsoft Azure"
   description="对于内部负载平衡器和它的功能的概述。Azure 和可能的方案，来配置内部终结点的负载平衡器工作原理"
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
   ms.date="08/12/2015"
   ms.author="joaoma" />


# 内部负载平衡器概述

内部负载平衡器 (ILB) 通过当前互联网面临提供 Azure 中的负载平衡器是一种增强安全性。 对 ILB 的访问只能通过云服务或使用 VPN 访问的 Azure 的基础结构，以达到 ILB 内部的资源。
            
基础结构限制可访问性和创建到云服务或虚拟网络的负载平衡虚拟 IP 地址之间的信任边界将永远不会公开给互联网终结点直接。 这使内部业务线应用程序运行在 Azure 然后在云环境中或从内部进行访问。

## 方案内部负载平衡器

您可以使用 ILB 在许多新的配置，包括以下︰

Azure 内部负载平衡 (ILB) 提供驻留在云服务或使用区域范围的虚拟网络的虚拟机之间的负载平衡。 有关使用和配置的区域范围的虚拟网络的信息，请参阅在 Azure 的博客中的[区域的虚拟网络](http://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/)。 现有关系组已配置的虚拟网络不能使用 ILB。

ILB 使以下新类型的负载平衡︰

- 云服务，从一套驻留在同一个云服务的虚拟机的虚拟机中 （参见图 1）。

- 在虚拟网络上，一套驻留在相同的虚拟的云服务的虚拟机的虚拟网络中的虚拟机从网络 （请参见图 2）。

- 对于跨部署虚拟网络，从一组驻留在相同的虚拟的云服务的虚拟机的内部计算机网络 （请参见图 3）。

现有的 Azure 负载平衡只提供基于互联网的计算机和云服务中的虚拟机之间的负载平衡。 ILB 使承载虚拟机在 Azure 中的新功能。

- 面向 Internet 的、 多层应用程序的后端层不是面向 Internet 的但需要从面向 Internet 层通信的负载平衡。
- 负载平衡实现的业务线 (LOB) 应用程序承载 Azure 中而不需要额外增加的负载平衡硬件或软件。
包括内部服务器中的一组计算机的通信是负载平衡。 
- 以下各节描述了这些配置详细。

## 互联网面临多层应用程序


Web 层 Internet 客户端具有面向互联网的终结点，而且是负载平衡的组的一部分。 负载平衡器将 web 客户端的 TCP 端口 443 (HTTPS) 到 web 服务器的传入通信。

数据库服务器位于 ILB 终结点的 web 服务器上用于存储。 此数据库的服务负载平衡的端点，哪种通信进行负载平衡跨 ILB 组中的数据库服务器。

下图介绍了互联网面临着相同的云服务中的多层应用程序。 

图 1

![内部负载平衡一个云服务](./media/load-balancer-internal-overview/IC736321.png)

ILB 部署到不同的云服务使用的 ILB 服务比时多层应用程序的另一个可能的方案。

云服务使用相同的虚拟网络将能够访问到 ILB 的终结点。

在下面的图像中可以看到，前端 web 服务器位于不同的云服务从数据库后端和利用相同的虚拟网络中的 ILB 终结点。

图 2

![内部之间的负载平衡云服务](./media/load-balancer-internal-overview/IC744147.png)

## 内联网业务线 (LOB) 应用程序

从内部网络上的客户端通信获得跨的 LOB 服务器使用 VPN 连接到 Azure 的网络负载平衡。

客户端计算机都可以访问到的 IP 地址从 Azure VPN 服务到站点 VPN 使用点。它将允许使用 LOB 应用程序承载 ILB 终结点后面。


![到站点 VPN 使用点内部负载平衡](./media/load-balancer-internal-overview/IC744148.png)

LOB 的另一种情况是让站点到站点 VPN 虚拟网络配置 ILB 终结点的位置。这将允许内部网络通信被路由到 ILB 的终结点。

![使用站点到站点 VPN 内部负载平衡](./media/load-balancer-internal-overview/IC744150.png)


## 下一步行动

[开始配置 Internet 面临负载平衡器](load-balancer-internet-getstarted.md)

[开始配置内部负载平衡器](load-balancer-internal-getstarted.md)

[配置负载平衡器分布模式](load-balancer-distribution-mode.md)

[配置负载平衡器的空闲 TCP 超时设置](load-balancer-tcp-idle-timeout.md)

 
测试

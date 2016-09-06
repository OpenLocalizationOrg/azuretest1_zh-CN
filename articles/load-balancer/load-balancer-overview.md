---
ms.openlocfilehash: c1de0b5c08a933a9d9ae87463c8b0f03b07e7ac4
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="Azure 负载平衡器概述 |Microsoft Azure"
   description="概述 Azure 负载平衡器的功能、 体系结构和实现。 它有助于了解如何负载平衡器工作原理，以及利用它的云"
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
   ms.date="07/10/2015"
   ms.author="joaoma" />


# 负载平衡器概述 
Azure 负载平衡器为您的应用程序提供高可用性和网络性能。 它是-4 层 （TCP、 UDP） 键入分发传入的通信，在云服务或负载平衡器设置中定义的虚拟机中运行状况良好的服务实例之间的负载平衡器。
 
它可以配置为︰

- 通信执行负载平衡传入 Internet 虚拟机。 我们称它为[互联网面临的负载平衡](load-balancer-internet-overview.md)。
- 在虚拟网络中的虚拟机之间，在云服务中的虚拟机之间或内部计算机和跨部署虚拟网络中的虚拟机之间的负载平衡通信。 我们称它为[内部负载平衡 (ILB)](load-balancer-internal-overview.md)。
-   外部将通信转发到特定的虚拟机实例


## 负载平衡器功能

### 4 层负载平衡器，基于哈希的分布

Azure 负载平衡器使用哈希基于的分布算法。 默认情况下，它使用是 5 元 (源 IP，源端口目标 IP、 目标端口协议类型) 哈希将映射到可用的服务器的通信。 它只在传输会话中提供建立密切关系。 在同一会话中 TCP 或 UDP 数据包将被定向到负载平衡终结点后面的同一个数据中心 IP (DIP) 实例。 当客户端关闭并重新打开该连接或从相同的源 IP，源端口更改启动一个新会话。 这可能会导致可转到不同的 DIP 终结点的通信。


有关[负载平衡分布](load-balancer-distribution-mode.md)模式的详细信息

![希基于负载平衡器](./media/load-balancer-overview/load-balancer-distribution.png)

### 端口转发

Azure 负载平衡器提供控制入站通信，如从 Internet 主机或其他云服务或虚拟网络中的虚拟机启动的通信进行管理。 终结点 （也称为输入终结点） 表示此控件。

终结点在公共端口上侦听，并转发到内部端口的通信。  可以映射为内部或外部终结点相同的端口，也可以为它们使用不同的端口。 例如︰ 您可以配置 web 服务器侦听到端口 81 时公共端点映射为端口 80。 创建公共端点触发 Azure 负载平衡器的创建。

默认使用和使用 Azure 管理门户创建一个虚拟机上的终结点配置的远程桌面协议 (RDP) 和远程 Windows PowerShell 会话通信。 这些终结点使您可以远程管理 Internet 上的虚拟机。


### 温出/下自动重新配置

Azure 负载平衡器可立即重新配置本身当您放大或缩小 （由于增加 web/辅助角色实例计数，或由于放置在相同的其他虚拟机负载平衡的组） 的实例。


### 服务监视
Azure 负载平衡器提供的功能可以探测不同的服务器实例的运行状况。 当探测器无法响应时，Azure 负载平衡器将停止向不健康的实例发送新的连接。 现有的连接不会受到影响。 

有三种类型的支持的探测器︰
 
- 访客代理 （在 PaaS Vm) 的探测器。 Azure 负载平衡器利用虚拟机来宾代理、 侦听和作出仅当该实例处于就绪状态 （即 HTTP 200 确定的响应该实例不是忙，回收、 停止等指示）。 如果来宾代理没有响应 HTTP 200 确定，Azure 负载平衡器将标记为无响应的实例，并停止将流量发送到该实例。 Azure 负载平衡器将继续 ping 该实例，并如果访客的代理响应 HTTP 200，Azure 负载平衡器将通信发送到该实例再次。  使用网站的代码通常运行在 w3wp.exe 的 Azure 结构未被监视或来宾代理程序，这意味着 w3wp.exe 中的故障 （如 web 角色时 HTTP 500 响应) 将不会报告给来宾代理和负载平衡器将不知道要采取轮换该实例。

- HTTP 的自定义探测。 自定义负载平衡器探测重写默认来宾代理探测器，并使您可以创建您自己的自定义逻辑来确定角色实例的运行状况。  负载平衡器将定期探测终结点 （每隔 15 秒钟，默认情况下），该实例将循环中认为，如果在超时时间 （默认为 31 秒） 内响应 TCP ACK 或 HTTP 200。  这可用于实现您自己的逻辑，以从负载平衡器旋转，例如返回非 200 状态，如果高于 90 %cpu 实例删除实例。  使用的 w3wp.exe 这也意味着您的 web 角色获得自动监控您的网站因为网站的代码中的错误会返回非 200 状态到负载平衡器探测。  

- 自定义 TCP 探测。 TCP 探查依靠成功定义的探测器端口的 TCP 会话建立。

有关详细信息，请参阅[负载平衡器运行状况探测器](https://msdn.microsoft.com/library/azure/jj151530.aspx)。

### 源 NAT (SNAT)


所有出站通信到源自您的服务的互联网是源 NATed (SNAT) 使用相同的 VIP 地址用于传入通信。 SNAT 重要优点︰

- 这样可以方便地升级和灾难恢复的服务因为 VIP 可以动态映射到另一个实例的服务，

- 它可使 ACL 管理更容易因为 ACL 可以表现 Vip 因此向上或向下作为服务的比例不会更改或获得重新部署

Azure 负载平衡器配置支持 udp 完全圆锥 NAT。 完全圆锥 NAT 是的 NAT 端口 （在出站请求的响应） 的任何外部主机允许入站的连接的其中一种。

![snat](./media/load-balancer-overview/load-balancer-snat.png)


>[AZURE.NOTE]请注意，每个由 VM 启动新的出站连接，为出站端口还分配了 Azure 负载平衡器。 外部的主机就会看到为 VIP 通信︰ 分配端口。  如果您的方案需要大量的出站连接，建议 Vm 使用实例级公用 IPs，以便它有专门出站 IP 的源网络地址翻译 (SNAT)。 这将减少端口耗尽的风险。 
>
>VIP 或 ILPIP 可以使用的端口的最大数是 64k。 这是 TCP 标准限制。


**支持多个负载平衡的虚拟机的 IP**

您可以获取多个负载平衡公共 IP 地址分配给虚拟机的一组。 通过此功能可以承载多个 SSL 网站和/或多个 SQL 总是在相同的一组虚拟机的可用性组侦听器上。 在[多个 VIP 的每个云服务](load-balancer-multivip.md)的详细信息，请参阅

**基于模板的部署使用 Azure 资源管理器 （公共预览）**Azure 资源管理器 (ARM) 是 Azure 中的服务新的管理框架。 现在可以使用基于 Azure 资源管理器 Api 和工具来管理 azure 负载平衡器。 若要了解有关 Azure 资源管理器的详细信息，请参阅[Iaas 更加轻松使用 Azure 资源管理器](http://azure.microsoft.com/blog/2015/04/29/iaas-just-got-easier-again/)


## 下一步行动

[互联网面临的负载平衡器概述](load-balancer-internet-overview.md)

[内部负载平衡器概述](load-balancer-internal-overview.md)

[入门-互联网面临负载平衡器](load-balancer-internet-getstarted.md)
 

测试

---
ms.openlocfilehash: 1dda2b2d20a6e817fb6de871ae96b08f8a2930f1
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

<properties 
   pageTitle="负载平衡器概述面向 Internet |Microsoft Azure "
   description="负载平衡器和它的功能面向 Internet 的概述。 负载平衡器的工作原理的 Azure 将虚拟机和云服务。"
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
   ms.date="07/09/2015"
   ms.author="joaoma" />


# 多个虚拟机或服务之间面临互联网负载平衡器

为终结点的一个用途是通信的 Azure 负载平衡器来分发特定类型的多个虚拟机或服务之间配置。 例如，您可以在多个 web 服务器或 web 角色分配 web 请求通信量的负载。

从虚拟机，azure 负载平衡器将传入通信的公用 IP 地址和端口号映射到专用 IP 地址和端口号的虚拟机，反之亦然的响应通信量。

>[AZURE.NOTE] 在配置多个虚拟机或使用默认设置的服务之间的通信的负载平衡时，它将提供随机分布传入的通信量。 如果您正在寻找会话相关性，签出[负载平衡器分布模式](load-balancer-distribution-mode.md)

对于云服务包含的 web 角色或辅助角色实例，可以在服务定义 (.csdef) 中定义公用的终结点。
 
Servicedefinition.csdef 文件将包含终结点配置，对于 web 或辅助角色部署多个角色实例后，负载平衡器将对其进行安装。 将实例添加到您的云部署的方法更改服务配置文件 (.csfg) 上的实例计数。  

下图显示了负载平衡的公钥和私钥的 TCP 端口 443 的三个虚拟机之间共享的加密的 web 通信的终结点。 这些三个虚拟机的负载平衡集中。


![公共的负载平衡器示例](./media/load-balancer-internet-overview/IC727496.png))



当 Internet 客户端发送 web 页请求到云服务和 TCP 端口 443，Azure 负载平衡器的公用 IP 地址执行这些请求中的负载平衡设置的三个虚拟机之间随机平衡。


## 下一步行动

[内部负载平衡器概述](load-balancer-internal-overview.md)

[开始配置 Internet 面临负载平衡器](load-balancer-internet-getstarted.md)

[配置负载平衡器分布模式](load-balancer-distribution-mode.md)

[配置负载平衡器的空闲 TCP 超时设置](load-balancer-tcp-idle-timeout.md)


 
测试

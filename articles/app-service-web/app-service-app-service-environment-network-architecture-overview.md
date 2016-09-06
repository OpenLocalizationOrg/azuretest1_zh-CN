---
ms.openlocfilehash: 1a929e2f8e5b747d0675e9f86a876306e7cf37f4
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="应用程序服务环境的网络体系结构概述" 
    description="体系结构概述网络拓扑 ofApp 服务的环境。" 
    services="app-service\web" 
    documentationCenter="" 
    authors="stefsch" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/06/2015" 
    ms.author="stefsch"/>   

# 应用程序服务环境的网络体系结构概述

## 简介 ##
应用程序服务环境始终创建[虚拟网络][virtualnetwork]的子网中，可以与位于相同的虚拟网络拓扑的专用端点通信的应用程序服务环境中运行的应用程序。  因为客户可能会锁定其虚拟网络基础结构的部分，务必理解发生了一个应用程序服务环境的网络通信流的类型。

## 常规网络流 ##
 
应用程序服务环境始终具有一个公共的虚拟 IP 地址 (VIP)。  该 FTP、 远程调试功能，和 Azure 管理操作包括 HTTP 和 HTTPS 通信的应用程序，以及其他通信的公用 VIP 到达所有入站的通信。  特定的端口 （必需和可选） 的完整列表，可用于公用 VIP 请参见对应用程序服务环境的[控制入站的通信][controllinginboundtraffic]上的文章。 

下图显示了不同的入站和出站网络流量的概述︰

![常规网络流][GeneralNetworkFlows]

应用程序服务环境可以具有不同的专用客户终结点进行通信。  例如，应用程序服务环境中运行的应用程序可以连接到相同的虚拟网络拓扑中的 IaaS 虚拟机上运行的数据库服务器。  

应用程序服务的环境也与 Sql 数据库和通信 Azure 存储资源所需的管理和运行一个应用程序的服务环境。  Sql 和存储资源与进行通信的应用程序服务环境中的某些都位于同一区域作为应用程序服务环境中，而其他人都位于远程 Azure 的地区。  因此，出站连接到 Internet 是始终能够正常运行的应用程序服务环境要求。 

由于子网中部署的应用程序服务环境，网络安全组可用于控制到子网的入站的通信。  如何控制对应用程序服务环境的入站的通信的详细信息，请参阅以下[文章][controllinginboundtraffic]。

有关如何从应用程序服务环境允许出站 Internet 连接的详细信息，请参阅下文关于使用[快速通道][ExpressRoute]。  本文介绍的方法相同的方法应用在使用站点对站点连接时，使用强制隧道。

## 出站网络地址 ##
当一个应用程序服务环境，出站调用，IP 地址是始终与出站呼叫。  使用特定的 IP 地址取决于是否调用该终结点所在的虚拟网络拓扑结构，内部或外部虚拟网络拓扑结构。

如果**外部**虚拟网络拓扑的调用该终结点，则用于出站通讯 （亦即出站 NAT 地址） 是公共应用程序服务环境的 VIP。  可以为应用程序服务环境门户用户界面中找到此地址 (注︰ 用户体验挂起)。  

此地址还可以通过在应用程序服务环境中，创建一个应用程序，然后在应用程序的地址上执行*nslookup*来确定。 结果的 IP 地址是这两个公共 VIP，以及应用程序服务环境的出站 NAT 地址。

如果**在**虚拟的网络拓扑的调用该终结点，则将运行应用程序的单个计算资源的内部 IP 地址调用的应用程序的出站地址。  但是并不永久虚拟网络到应用程序的内部 IP 地址的映射。  应用程序可以跨不同的计算资源和可用的计算资源的应用程序服务环境中可以更改缩放操作由于池中移动。

但是，由于应用程序服务环境始终是位于一个子网内，在保证计算资源运行应用程序的内部 IP 地址将始终位于 CIDR 范围的子网。  因此，当细粒度的 Acl 或网络安全组用于安全访问的虚拟网络中的其他终结点，包含应用程序服务环境的子网范围需要被授予访问权限。

下图更详细地显示这些概念︰

![出站网络地址][OutboundNetworkAddresses]

在上面的图中︰

- 由于公共应用程序服务环境的 VIP 是 192.23.1.2，这就是对"Internet"终结点进行调用时使用的出站 IP 地址。
- 包含有关应用程序服务环境网 CIDR 范围是 10.0.1.0/26。  同一虚拟网络基础结构中的其他终结点将从某个地方发出此地址范围内看到来自应用程序的调用。

## 调用应用程序服务环境之间 ##
如果您部署多个应用程序服务环境中的相同的虚拟网络，并进行出站呼叫从一个应用程序服务环境到另一个应用程序服务环境，会出现更复杂的方案。  这种交叉调用也被视为"Internet"调用应用程序服务环境。  

作为 192.23.1.2 的出站 IP 地址使用上面的应用程序服务环境示例︰ 如果在应用程序服务环境上运行的应用程序可以对相同的虚拟网络中位于第二个应用程序服务环境上运行的应用程序的出站调用，对第二个到达的出站调用应用程序服务环境将显示来自 192.23.1.2 （即不子网地址范围的第一个应用程序服务环境）。

即使不同的应用程序服务环境间的调用视为"Internet"调用时这两种应用程序服务环境位于 Azure 的同一区域中的网络通信将保留在 Azure 的区域网络，通过公共互联网流动将不 phyically。  因此可用于网络安全组中的子网上的第二个应用程序服务环境从而确保应用程序服务环境之间的安全通信，则只允许来自 192.23.1.2，入站的调用。

## 附加的链接和信息 ##
详细信息在入站端口使用的应用程序服务的环境，使用网络安全组来控制入站的流量现[这里][controllinginboundtraffic]。

使用用户定义的路由可将出站 Internet 访问权限授予应用程序服务环境的详细信息将在本[文章][ExpressRoute]。 


<!-- LINKS -->
[virtualnetwork]: http://azure.microsoft.com/services/virtual-network/
[controllinginboundtraffic]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[ExpressRoute]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/

<!-- IMAGES -->
[GeneralNetworkFlows]: ./media/app-service-app-service-environment-network-architecture-overview/NetworkOverview-1.png
[OutboundNetworkAddresses]: ./media/app-service-app-service-environment-network-architecture-overview/OutboundNetworkAddresses-1.png


测试

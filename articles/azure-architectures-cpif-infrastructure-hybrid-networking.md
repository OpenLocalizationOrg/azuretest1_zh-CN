---
ms.openlocfilehash: e93cadc65448913f9b7d257a22db7db97758ef8f
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="混合网络 （Azure 的体系结构模式）" 
   description="混合网络模式是基础架构区域，CPIF 体系结构文档中广泛地介绍的一部分。" 
   services="" 
   documentationCenter="" 
   authors="arynes" 
   manager="fredhar" 
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple" 
   ms.date="03/25/2015"
   ms.author="arynes"/>

# 混合网络 （Azure 的体系结构模式）

[云平台的集成框架 (CPIF)](azure-architectures-cpif-overview.md)提供服务到 Microsoft 的云解决方案的应用程序工作负载集成指南。  

CPIF 介绍了如何组织、 客户和合作伙伴应设计和部署利用 Azure、 系统中心和 Windows 服务器混合云平台和管理功能为目标云的工作负载。 

**混合网络**模式是**基础架构**区域，CPIF 体系结构文档中广泛地介绍的一部分。 

##  混合网络

混合网络设计图案细节的 Azure 的功能并提供跨越地理边界可以提供可预知的性能和高可用性的网络功能所需的服务。  在 Microsoft Azure 文档站点提供了 Microsoft Azure 区域和其中每个可用服务的完整列表。  本文档提供了 Microsoft Azure 的混合环境的网络功能的概述。 Microsoft Azure 虚拟网络可以在 Azure 创建逻辑上独立的网络和安全地通过 Internet 或使用专用网络连接将它们连接到您的内部数据中心。  此外，个别客户端计算机可以连接到 Azure 隔离网络使用 IPsec VPN 连接。  

该混合网络体系结构模式包括以下重点领域︰ 

- 连接到 Azure 的内部网络上 
- 区域间扩展 Azure 的虚拟网络 
- 扩展的 Azure 虚拟网络订阅 
- 远程网络访问提供开发人员 

## 体系结构模式概述 

混合网络体系结构模式是可创建的方案的可能数目由于复杂。 此体系结构模式将重点关注以下四种情况︰ 

- 站点的混合与多级跳虚拟网络路由中单个订阅和地区网络 
- 站点的混合与多级跳虚拟网络路由通过订阅和区域网络 
- 使用 MPLS 连接 ExpressRoute 混合网络 
- ExpressRoute 混合网络使用 IXP 连接 

##  其他资源
[混合网络 (pdf)](https://gallery.technet.microsoft.com/Cloud-Platform-Integration-5e401f38)

## 请参见
[CPIF 的体系结构](https://gallery.technet.microsoft.com/Cloud-Platform-Integration-bd1e434a) 

[全局负载平衡的 Web 层](https://gallery.technet.microsoft.com/Cloud-Platform-Integration-2c3c663a) 

[负载平衡的数据层](https://gallery.technet.microsoft.com/Cloud-Platform-Integration-dfb09e41)

[Azure 搜索层](https://gallery.technet.microsoft.com/Cloud-Platform-Integration-e581d65d) 

[批次处理层](https://gallery.technet.microsoft.com/Cloud-Platform-Integration-0bc3f8b1)

测试

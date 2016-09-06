---
ms.openlocfilehash: 739c47590da7feaeb2e90a0960abbc7b102f7146
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="云平台的集成框架 |Microsoft Azure" 
   description="云平台的集成框架提供了工作负荷到 Microsoft 的云解决方案的服务应用程序的集成指南为 Microsoft Azure 包含的体系结构模式" 
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


# 云平台的集成框架 （Azure 的体系结构模式）

云平台的集成框架 (CPIF) 提供服务到 Microsoft 的云解决方案的应用程序工作负载集成指南。 

CPIF 介绍了如何组织、 客户和合作伙伴应设计和部署利用 Azure、 系统中心和 Windows 服务器混合云平台和管理功能为目标云的工作负载。 有了 CPIF 域分解为以下函数︰

![标记部分资源和资源组刀片](./media/azure-architecture-cpif-overview/overview.png)

##  CPIF 的体系结构

这两种公有和私有云环境提供公共元素，以支持复杂的工作负载运行。 尽管这些体系结构相对比较熟悉传统的内部部署物理和虚拟化环境中，Microsoft Azure 中的构造都要求额外的规划，以合理化基础架构和平台功能在公共云环境中找到。 若要支持在 Azure 中承载的应用程序或服务的开发，一系列模式，不需要大纲撰写给定工作负载的解决方案所需的各种组件。  

这些体系结构模式分为以下几类︰

- **基础结构**--Microsoft Azure 是一个基础结构和平台-与-服务解决方案组成的几个基本的服务和功能。  这些服务很大程度上可以分解为计算、 存储和网络服务，但也有一些可能超出这些定义的功能。  基础结构模式详细说明了 Microsoft Azure 它提供到一个或多个解决方案中给定的 Azure 订阅承载给定的服务所需的给定功能区。 
- **基础**– 当撰写的多层应用程序或服务，在 Azure，几个组件必须使用组合来提供合适的宿主环境。  基础图案构成从 Microsoft Azure 支持给定的层的应用程序中的功能的一个或多个服务。 这可能需要使用上面所述的基础结构模式中所述的一个或多个组件。 例如，表示层的多层应用程序要求计算、 网络和存储能力 Azure 中的才可用。  基础模式是为了与其他模式组合作为给定解决方案的一部分。
- **解决方案**-解决方案模式组成基础结构和/或基础表示结束应用程序或服务正在开发的模式。  它被设想复杂的解决方案不会开发独立于其他模式。  相反，他们应该利用组件和接口定义在每个上述的模式类别。    

## Azure 的体系结构模式的概念

为了支持在 Azure 的解决方案体系结构的开发，为常见方案提供了一系列模式指南。   而随着时间的推移，将构造这些方案指南，预想的模式包括︰

- 全局负载平衡的 Web 层 
- 负载平衡的数据层
- 批次处理层
- 混合网络
- Azure 搜索 

##  其他资源
[CPIF 的体系结构 (pdf)](https://gallery.technet.microsoft.com/Cloud-Platform-Integration-bd1e434a) 

## 请参见
[全局负载平衡的 Web 层](https://gallery.technet.microsoft.com/Cloud-Platform-Integration-2c3c663a) 

[负载平衡的数据层](https://gallery.technet.microsoft.com/Cloud-Platform-Integration-dfb09e41)

[批次处理层](https://gallery.technet.microsoft.com/Cloud-Platform-Integration-0bc3f8b1)

[Azure 网络](https://gallery.technet.microsoft.com/Cloud-Platform-Integration-5e401f38)

[Azure 搜索](https://gallery.technet.microsoft.com/Cloud-Platform-Integration-e581d65d)

测试

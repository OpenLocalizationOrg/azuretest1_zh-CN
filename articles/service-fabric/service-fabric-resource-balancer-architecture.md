---
ms.openlocfilehash: 7e3e9c1d2d0af9a9831b17f8e14b9acd9155b40e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="资源平衡器体系结构"
   description="服务结构资源平衡器体系结构概述"
   services="service-fabric"
   documentationCenter=".net"
   authors="abhic"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="04/27/2015"
   ms.author="abhic"/>

# 资源平衡器体系结构概述

![资源平衡器体系结构][Image1]

服务结构资源平衡器组成，在概念上分别搭配以服务结构故障转移管理器与本地服务结构节点，一个集中式的资源平衡服务和组件存在于每个节点上。

本地代理程序负责收集和缓冲从在本地节点运行的服务的负载报告、 将它们发送到该资源平衡器服务，并还报告故障转移管理器和资源平衡器 （1 和 2 以上） 的故障和其他事件。 资源平衡器服务和故障转移管理器进行协作来响应系统，例如复制副本或节点故障中加载事件和其他事件，加载报表，以及服务和应用程序的创建和删除。 例如，当复制副本失败，故障转移管理器请求服务结构资源平衡器建议根据从不同节点的负载数据替换的位置。 同样，当收到新的服务请求时，故障转移管理器从资源平衡器放置位置的服务请求的建议。 在所有这些情况下，资源平衡器响应更改各种服务配置 (3)，这再颁布通过故障转移管理器。 在前面的示例中，故障转移管理器节点，最佳的整体平衡 (4) 会导致创建新的副本。

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## 下一步行动

资源平衡的功能︰

- [描述了群集](service-fabric-resource-balancer-cluster-description.md)
- [描述服务](service-fabric-resource-balancer-service-description.md)
- [报告动态负载](service-fabric-resource-balancer-dynamic-load-reporting.md)
- [主动预防性的度量标准包装](service-fabric-resource-balancer-proactive-metric-packing.md)
- [节点缓冲区百分比](service-fabric-resource-balancer-node-buffer-percentage.md)

[Image1]: media/service-fabric-resource-balancer-architecture/Service-Fabric-Resource-Balancer-Architecture.png
 
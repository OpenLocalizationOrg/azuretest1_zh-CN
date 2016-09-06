---
ms.openlocfilehash: 14850eeec183e4ab224ccd641146386ad9a7cea2
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="动态负载报告"
   description="资源平衡器的动态加载报表概览"
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

# 动态负载报告概述

在运行时，有状态的和无状态服务对象可以报告通过 ReportLoad 方法 （IStatefulServicePartition 和 IStatelessServicePartition 接口的成员） 的负载。 报告运行时负载值很重要，因为它使服务的执行节点的准确装箱并有助于确保资源利用率由中央服务结构资源平衡器，准确地跟踪服务遇到它的节点上。

注意当副本报告负载，它是第一个批处理向上与本地其他加载报表，然后将一个报告发送到资源平衡。 这一过程意味着如果服务报告负载反复和速度非常快，仅最后一次报告实际获取发送到资源平衡。

服务结构资源平衡器在运行时，它会检查所有的负载信息的聚合中的所有节点，并执行一些检查。 这些检查包括群集清单中定义的平衡阈值和活动阈值的度量标准。 他们确定群集是否足够平衡运行资源平衡器以及任何特定节点是否上述任何有定义的产能的标准容量。 超容量的情况下，资源平衡器首先只考虑或多个节点上的服务通过共享的度量，在产能上的容量。 对于群集不平衡，资源平衡器认为指标与报告平衡指标的任何服务的所有服务。 如果服务结构资源平衡器确定发生了所有这些情况下，它运行模拟，试图找到更好的服务安排。

服务应该报告负载时负载更改并不会执行任何聚合或负载报告自己的进行平滑处理。

注意服务报告负载，这些加载报告此后替换默认主系统和辅助的负载值当已创建，并将成为新的负载值在服务结构资源平衡器必须作出平衡时, 使用服务定义的位置决定。



<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## 下一步行动

详细信息︰[资源平衡器体系结构](service-fabric-resource-balancer-architecture.md)
 
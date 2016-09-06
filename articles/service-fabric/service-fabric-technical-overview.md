---
ms.openlocfilehash: 1d044a944cd22eec9d21dacef653b6598debbe9a
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="技术概述"
   description="服务结构的技术概述。 讨论的主要概念和体系结构概述"
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor="chackdan;subramar"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/25/2015"
   ms.author="mfussell"/>

# 技术概述服务结构

服务结构是一种分布式的系统平台，可扩展、 可靠、 低延迟，可以轻松地构建和易于管理的云应用程序。 这意味着您可以专注于您的业务需求并让负责确保应用程序始终是可用的和可扩展的服务结构。

## 主要概念

**群集**-连接的网络设置的实例部署到的应用程序的虚拟或物理计算机。  群集可以扩展到数千台机器。

**节点**的群集中的可寻址单元。 节点具有唯一 Id 放置属性等特征。 节点可以加入群集，并与 Fabric.exe 运行与操作系统实例相关。

**应用程序 / 应用程序类型**-（微观） 服务的集合。 应用程序类型看作是一个或多个服务类型的容器。  请参阅以了解群集[应用程序模型](service-fabric-application-model.md)文章 （本身由多个节点） 可能包含多个 ApplicationTypes。

**服务 / 服务类型**的代码，并执行独立函数 （它可以启动和运行独立），例如，队列服务或数据库服务的配置。 ApplicationType 可能包含一个或多个 ServiceTypes。 有两种服务类型︰

- 无状态的服务︰ 有的状态，在状态被保持到外部存储，例如 Azure 数据库或表 Azure 存储服务。 如果此服务实例处于一个节点出现故障，另一个实例会自动启动另一个节点上。

- 有状态的服务︰ 一种有状态和实现可靠性通过在群集中其他节点上的副本之间的复制服务。 有状态服务具有主和多个辅助副本。 如果此服务的一个副本正在某个节点出现故障，另一个节点上启动一个新副本，如果是主副本，一个辅助副本将自动提升为新的主。

**应用程序实例**的群集中，可以创建多个应用程序实例的应用程序类型，每种都有特定的名称。 每个应用程序实例可以是独立托管和已进行版本管理的相同类型或不同类型的其它应用程序实例。 此外，它们定义资源和安全隔离能力。

**服务实例**的实例化服务类型的代码。 每个服务实例具有唯一的名称开头`fabric:/`和与特定命名的应用程序实例相关联。

某个特定应用程序组合**应用程序包**的服务代码包的集合和配置文件。 这些是物理文件，进行部署，并且只是一个文件和文件夹格式布局。 例如，电子邮件应用程序的应用软件包可以包含队列服务包、 前端服务包和数据库服务包。

**编程模型**的构建应用程序的服务结构中提供了两种编程模型︰

- 可靠的服务的 API 来构建基于无状态和有状态服务`StatelessService`和`StatefulService`.NET 类，.NET 可靠集合 （字典和队列） 中插入各种如 Web API 和 WCF 通信堆栈的能力与存储的状态。 此编程模型是状态的适用于应用程序，您需要跨多个设备执行计算。

- 可靠的演员-API 来构建通过虚拟主角编程模型是适用于应用程序使用多个独立的单元状态和计算的无状态和有状态的对象。

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## 下一步行动
若要了解有关服务结构的详细信息，请参阅︰

- [应用程序模型](service-fabric-application-model.md)
- [应用程序方案](service-fabric-application-scenarios.md)
 
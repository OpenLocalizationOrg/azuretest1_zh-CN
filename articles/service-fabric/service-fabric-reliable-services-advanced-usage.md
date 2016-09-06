---
ms.openlocfilehash: cdf834b5f310536faa7d54440eb89a92bdd95b05
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="编程模型的可靠服务的高级的用法"
   description="了解有关编程模型以增加灵活性，在您的服务的服务结构可靠服务的高级用法。"
   services="Service-Fabric"
   documentationCenter=".net"
   authors="jessebenson"
   manager="timlt"
   editor="masnider"/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/26/2015"
   ms.author="jesseb"/>

# 高级编程模型的可靠服务的使用情况
编写和管理可靠的无状态和有状态服务，简化了服务结构。 本指南将讨论高级用法的可靠服务编程模型来对您的服务中获得更多控制能力和灵活性。 在阅读本指南之前, 熟悉[编程模型可靠的服务](service-fabric-reliable-services-introduction.md)。

## 无状态服务基类
StatelessService 基类提供了 CreateCommunicationListener() 和 RunAsync()，这是足够的大部分无状态服务。 StatelessServiceBase 类基础 StatelessService 并公开其他服务生命周期事件。 如果您需要其他控件或灵活性，可以从 StatelessServiceBase。 有关详细信息，参阅[StatelessService](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.services.statelessservice.aspx)和[StatelessServiceBase](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.services.statelessservicebase.aspx)的开发人员参考文档。

- `void OnInitialize(StatelessServiceInitializiationParameters)`
    OnInitialize 是第一种方法调用的服务结构。 如服务名称、 分区 id、 实例 id 和代码封装信息提供服务的初始化信息。 没有复杂的处理应在此处完成。 在 OnOpenAsync 中应该进行冗长的初始化。

- `Task OnOpenAsync(IStatelessServicePartition, CancellationToken)`
    使用无状态的服务实例时调用 OnOpenAsync。 这一次还可以启动扩展的服务初始化任务。

- `Task OnCloseAsync(CancellationToken)`
    无状态的服务实例将正常关机时，将调用 OnCloseAsync。 正在升级服务代码、 负载平衡，由于正在移动服务实例或检测瞬时性故障时，发生这种。 OnCloseAsync 可用于安全地关闭任何资源，停止所有后台处理，完成保存外部状态，或关闭现有连接。

- `void OnAbort()`
    无状态的服务实例已强制关闭时，将调用 OnAbort。 这通常称为当永久性故障检测的节点上，或者当服务结构不能可靠地管理因内部故障而服务实例的生命周期。

## 有状态服务基类
StatefulService 的基类应足以满足最有状态的服务。 无状态的服务相似，StatefulServiceBase 类基础 StatefulService 并公开其他服务生命周期事件。 此外，它允许您提供自定义的稳定状态提供程序并有选择地支持辅助通信侦听器。 如果您需要其他控件或灵活性，可以从 StatefulServiceBase。 有关详细信息，参阅[StatefulService](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.services.statefulservice.aspx)和[StatefulServiceBase](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.services.statefulservicebase.aspx)的开发人员参考文档。

- `Task OnChangeRoleAsync(ReplicaRole, CancellationToken)`
    有状态的服务更改角色，例如为主要或辅助时，将调用 OnChangeRoleAsync。 主副本可写状态 （允许创建和写入的可靠的集合），同时给出辅助副本读取状态 （可以只读取现有可靠的集合）。 您可以启动或更新中的角色变化，如次一致性组上执行只读验证、 生成报告或数据挖掘的后台任务。

- `IStateProviderReplica CreateStateProviderReplica()`
    有状态服务需要有一个可靠的状态提供程序。 StatefulService 使用 ReliableStateManager 类，它提供了可靠的集合 （如字典和队列）。 您可能希望提供自己的提供程序，如果您想要自己管理的状态或扩展的内置状态提供程序的功能。

- `bool EnableCommunicationListenerOnSecondary { get; }`
    默认情况下，通信侦听器只在初级上创建。 StatefulService 和 StatefulServiceBase 允许您重写此属性以允许通信侦听器创建辅助区域。 您可能想要允许您那儿处理只读的请求，以提高读取密集型工作负载的吞吐量。

    > [AZURE.NOTE] 您有责任确保您的映像不尝试创建或写入可靠的集合。  在辅助上写入尝试将导致异常的如果未经处理的将导致复制副本必须关闭并重新打开。

StatefulServiceBase 还提供相同四个生命周期事件为 StatelessServiceBase，与相同的语义和用例︰

- `void OnInitialize(StatefulServiceInitializiationParameters)`
- `Task OnOpenAsync(IStatefulServicePartition, CancellationToken)`
- `Task OnCloseAsync(CancellationToken)`
- `void OnAbort()`

## 下一步行动
有关更高级的主题相关的服务结构，请参阅以下文章。

- [配置有状态的可靠服务](service-fabric-reliable-services-configuration.md)

- [服务结构状况简介](service-fabric-health-introduction.md)

- [用于故障排除的系统运行状况报告](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

- [放置约束概述](service-fabric-placement-constraint.md)

- [在 Azure 服务结构中的有状态服务安全复制流量](service-fabric-replication-security.md)
 

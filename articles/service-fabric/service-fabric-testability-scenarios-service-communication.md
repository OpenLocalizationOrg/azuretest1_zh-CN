---
ms.openlocfilehash: 25f6b816d5fa51d6bb60e97ca22bd8e38a8450bb
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="服务结构的可测试性方案︰ 服务通信" 
   description="服务与服务通信是一个服务结构的应用程序的关键集成点。 本文讨论了设计考虑因素和测试技术。" 
   services="service-fabric" 
   documentationCenter=".net" 
   authors="vturecek" 
   manager="timlt" 
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA" 
   ms.date="08/25/2015"
   ms.author="vturecek"/>

# 服务结构的可测试性方案︰ 服务通信

Microservices 和服务结构中自然的面向服务的体系结构样式面。 在这些类型的分布式体系结构中，组件化的 microservice 应用程序通常组成需要互相通信的多项服务。 即使在最简单的情况下，通常有至少一个无状态的 web 服务和状态数据存储服务，需要进行通信。

服务与服务通信是每项服务公开到其他服务的远程 API 的应用程序的关键集成点。 使用一组 API 边界涉及 I/O 通常需要一些医疗以及大量的测试和验证。 

有很多可以在分布式系统中的这些服务边界连接起来时的注意事项︰

 - **传输协议**。 它将为提高互操作性，HTTP 或自定义二进制协议的最大吞吐量？
 - **错误处理**。 如何处理永久性和暂时性错误和服务移动到另一个节点时，会发生什么情况？
 - **超时和滞后时间**。 在多层应用程序中，每个服务层如何处理整个堆栈，并由用户的延迟？

是否在使用内置服务通信组件提供的服务结构或构建您自己的测试您的服务之间的交互之一是确保应用程序中的可恢复性的一个关键部分。

## 其中，是我服务？

随着时间的推移中四处移动服务实例可能特别是在配置有定制的最佳资源平衡负载指标。 升级、 故障切换、 横向扩展和其他分布式系统的生命周期中发生的各种情况下的服务结构移动您要最大化可用性的服务实例的位置。

在群集中移动的服务，有两种情况下您的客户端和其他服务应准备好处理服务与交谈时︰

 + 自从上次您说，它已服务实例或分区副本。 这是服务生命周期的正常部分，应该预料到您的应用程序的生存期内发生这种情况。
 + 服务实例或分区副本是在移动的过程中。 尽管从一个节点到另一个服务的故障转移服务结构中很快发生，但可能有延迟的可用性通信组件您的服务是否启动缓慢。

妥善处理这些情况是很重要的是保持稳定的运行系统。 为此，我们需要作出一些考虑事项︰

+ 可以连接到每个服务都有 （HTTP、 Websocket 等） 它侦听*地址*。 服务实例或分区具有移动时，将更改终点地址 （它会被移动到不同的 IP 地址与另一个节点）。 如果使用内置的通信组件，它们将为您处理重新解析服务地址。 
+ 可能有其监听器在服务实例启动时服务滞后时间临时增加再次强调，这取决于如何快速移动后服务打开它。
+ 任何现有的连接将需要关闭并重新在新节点上打开该服务时再打开。 正常的节点关机或重新启动的时间可用于正常关闭现有连接。

### 对其进行测试︰ 移动服务实例

使用服务结构的可测试性工具，我们可以编写一个测试方案，测试这些情况下，以不同的方式。

1. 移动有状态服务的主副本。
 
    有状态的服务分区的主副本可以移用于任何数量的原因。 用于面向服务如何响应非常控制的方式移动，请参阅特定分区的主副本。

    ```powershell

    PS > Move-ServiceFabricPrimaryReplica -PartitionId 6faa4ffa-521a-44e9-8351-dfca0f7e0466 -ServiceName fabric:/MyApplication/MyService

    ```

2. 停止某个节点。

    当停止某个节点时，将移动服务结构，所有服务实例或在该节点与群集中的其他可用节点之一的分区。 用于测试 stituation 其中一个节点是丢失，请从您的群集和所有服务实例，对该节点的副本必须移动。

    可以使用 Stop ServiceFabricNode PowerShell cmdlet 停止节点︰

    ```powershell

    PS > Restart-ServiceFabricNode -NodeName Node.1

    ```

    
    


### 服务可用性

服务结构是一个平台，用于提供服务的高可用性。 但是，基本的基础结构问题仍可能在极端情况下导致不可用。 因此很重视还针对这一情况的测试。

有状态服务使用基于仲裁的系统复制的高可用性状态。 这意味着复制副本可用，才能执行写入操作需要仲裁。 在极少数情况下，普遍存在硬件故障，如仲裁的副本可能不可用。 在这种情况下，您将无法执行写入操作，但您将仍然能够执行读取操作。

### 对其进行测试︰ 写入操作不可用

服务结构的可测试性工具使您能够注入故障，则会引发仲裁丢失来测试这种类型的方案。 虽然少见，但很重要的客户端和有状态服务所依赖的服务正在准备处理它们能写到有状态服务的请求的情况。 同时它也是重要的有状态服务本身已意识到此 possibiliy，并且可以正常将它传达给调用方。 

可以使用调用 ServiceFabricPartitionQuorumLoss PowerShell cmdlet 引起仲裁丢失︰

```powershell

PS > Invoke-ServiceFabricPartitionQuorumLoss -ServiceName fabric:/Myapplication/MyService -QuorumLossMode PartialQuorumLoss -QuorumLossDurationInSeconds 20

```

在此示例中，我们将设置`QuorumLossMode`到`PartialQuorumLoss`表示我们希望无需停止所有副本中，促使仲裁丢失，以便读取的操作仍有可能。 若要测试的方案的整个分区不可，可以将此开关设置为`FullQuorumLoss`。

## 下一步行动

[了解更多有关可测试性操作](service-fabric-testability-actions.md)

[了解更多有关可测试性方案](service-fabric-testability-scenarios.md) 

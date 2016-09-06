---
ms.openlocfilehash: 3f6e9ea422e6c45d82cdd220f36b4fc8eb615018
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="开发服务结构服务"
   description="概念信息和教程，可帮助您了解如何开发服务结构服务使用的可靠的演员或可靠的服务的编程模型。"
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/20/2015"
   ms.author="ryanwi"/>

# 开发服务结构服务
此网页包含的概述和概念性的文章和教程，可帮助您了解如何开发服务结构服务的链接。 服务结构提供了用于构建服务的两个高级别编程模型︰ 可靠的主角 Api 和可靠的服务 Api。 虽然两者都生成相同的服务结构核心上，它们会使不同之间的折衷简洁性和并发、 分区和通信方面的灵活性。 它将有助于理解这两个模型，以便您可以在您的应用程序中选择特定服务的适当框架。

- [选择一种编程模型](service-fabric-choose-framework.md)
- [服务结构主角模型简介](service-fabric-reliable-actors-introduction.md)
- [可靠的服务编程模型简介](../Service-Fabric/service-fabric-reliable-services-introduction.md)

## 可靠的主角编程模型
 可靠的参与者提供了异步、 单线程使用者模型。 参与者表示状态和分布于整个群集以获得高可扩展性的计算的单位。 可靠的主角模型利用了分布式的存储基础服务结构平台，以提供高可用性和一致状态管理应用程序开发人员提供的。  若要了解详细信息，请阅读︰

- [开始使用可靠的演员](service-fabric-reliable-actors-get-started.md)
- [演员的生命周期和垃圾回收](service-fabric-reliable-actors-lifecycle.md)
- [结构操作者如何使用服务结构平台](service-fabric-reliable-actors-platform.md)
- [Azure 服务结构参与者上的注释类型的序列化](service-fabric-reliable-actors-notes-on-actor-type-serialization.md)

中所述与参与者进行通信︰

- [介绍服务结构使用者模型](service-fabric-reliable-actors-introduction.md#actor-communication)。
- [与服务通信](service-fabric-connect-and-communicate-with-services.md)

这些文章讨论了有用的设计模式和方案︰

- [主角模型设计模式](service-fabric-reliable-actors-patterns-introduction.md)  
- [图案︰ 智能高速缓存](service-fabric-reliable-actors-pattern-smart-cache.md)
- [图案︰ 分布式的网络和关系图](service-fabric-reliable-actors-pattern-distributed-networks-and-graphs.md)
- [图案︰ 资源管理](service-fabric-reliable-actors-pattern-resource-governance.md)
- [图案︰ 有状态的服务组合](service-fabric-reliable-actors-pattern-stateful-service-composition.md)
- [图案︰ 物联网](service-fabric-reliable-actors-pattern-internet-of-things.md)
- [图案︰ 分布式的计算](service-fabric-reliable-actors-pattern-distributed-computation.md)
- [一些反模式](service-fabric-reliable-actors-anti-patterns.md)

简单的基于打开的并发提供可靠的操作者的方法。 这些文章中介绍并发、 计时器和提醒和可重入性︰

- [并发](service-fabric-reliable-actors-introduction.md#concurrency)
- [事件和与并发相关的性能计数器](service-fabric-reliable-actors-diagnostics.md)
- [参与者可重入性](service-fabric-reliable-actors-reentrancy.md)
- [主角的计时器](service-fabric-reliable-actors-timers-reminders.md)

配置可靠参与者的信息将在以下位置找到︰

- [KVSActorStateProvider 配置](../Service-Fabric/service-fabric-reliable-actors-KVSActorstateprovider-configuration.md)  
- [配置可靠参与者-ReliableDictionaryActorStateProvider](../service-fabric-reliable-actors-reliabledictionarystateprovider-configuration.md)

可靠的操作者发出事件和性能计数器，可以用于诊断和监视您的服务︰

- [主角诊断](service-fabric-reliable-actors-diagnostics.md)
- [使用者事件](service-fabric-reliable-actors-events.md)


## 可靠的服务编程模型
可靠的服务为您提供了简单、 功能强大、 顶级的编程模型，可帮助您快速什么是对您的应用程序很重要。 若要了解详细信息，请阅读︰

- [开始使用可靠的服务](service-fabric-reliable-services-quick-start.md)
- [编程模型概述](../service-fabric-reliable-services-service-overview.md)  
- [体系结构](service-fabric-reliable-services-platform-architecture.md)
- [可靠的集合](service-fabric-reliable-services-reliable-collections.md)
- [配置有状态的可靠服务](../Service-Fabric/service-fabric-reliable-services-configuration.md)
- [可靠的服务编程模型高级的用法](../Service-Fabric/service-fabric-reliable-services-advanced-usage.md)

下面介绍了与可靠的服务和客户端可以使用它来发现并与服务终结点进行通信的抽象进行通信︰

- [与服务通信](service-fabric-connect-and-communicate-with-services.md)
- [服务的通信模型](service-fabric-reliable-services-communication.md)
- [默认通信栈提供可靠服务框架](service-fabric-reliable-services-communication-default.md)
- [WCF 服务可靠的基于通信栈](service-fabric-reliable-services-communication-wcf.md)
- [开始使用 Microsoft Azure 服务结构 Web API 服务 OWIN 和自托管 (VS 2015 RC)](service-fabric-reliable-services-communication-webapi.md)

可靠的服务发出事件和性能计数器，可以用于诊断和监视您的服务︰

- [有状态的可靠服务诊断](service-fabric-reliable-services-diagnostics.md)

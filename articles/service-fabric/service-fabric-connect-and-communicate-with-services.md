---
ms.openlocfilehash: b428646caca9b1266c405c19d5d40f01c3cb1447
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="Microsoft Azure 服务结构如何与服务进行通信"
   description="本文介绍了如何将连接到并与服务结构的应用程序中的服务进行通信。"
   services="service-fabric"
   documentationCenter=".net"
   authors="kunaldsingh"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/21/2015"
   ms.author="kunalds"/>


# 与服务通信
在 microservices 世界总体解决方案组成每个位置执行专项的任务的多种不同的服务。 这些 microservices 相互通信启用端端工作流。 也是客户端应用程序连接到并与服务进行通信。 本文讨论如何服务结构很容易为您与服务，用服务结构编写安装程序通信。

## 主要概念
这些是一些需要注意的安装与服务通信的重要概念。
### 表示作为客户端-服务器通信
服务结构通信 Api 表示客户端服务类型的交互，即使在这种情况，即两个在同一个群集中运行的服务。 将连接从客户端到目标服务或其他服务充当服务器并侦听有传入的请求。 可以在群集中的另一服务，客户端连接到服务器并对其进行调用。
### 移动服务的
在分布式系统中运行的服务实例可能移动从一台计算机到另一段时间由于各种原因，如负载平衡配置与资源平衡或由于升级、 故障转移、 向外扩展，以及其他各种情况下的负载指标时。 此方法的影响是可以更改某个服务实例的终结点地址，并设置下面的循环的服务实例与通信需要执行。 如果您使用通信 Api 提供的服务结构，这些详细信息都从您抽象。

* **解析**︰ 服务结构中的所有服务实例都有唯一的 Uri，例如"结构: / MyApplication/MyService"，而随着时间的推移这些 Uri 不更改。 每个服务实例还公开移动服务实例时可以更改该终结点。 这是类似于具有固定 Url 的网站，但 IP 地址可能会改变。 还有类似于在万维网网站 Url 会解析为 IP 地址的 DNS 服务结构平台还将 Uri 解析为终结点的系统服务。 此步骤涉及到的终结点解析的服务实例的 URI。

* **连接**︰ 服务 URI 解析为终结点地址后下一步是尝试与该服务的连接。 此连接可能会失败，如果终结点地址已更改，因为服务运动，其中，例如，可能是由机器出现故障或者由于负载平衡。

* **重试**︰ 如果需要重试连接尝试失败之前解决和连接步骤并重复此周期，直到连接成功。

## 决定要使用的通信 API
作为服务结构的一部分，我们提供通信 api 的几个不同的选项，并为其一个最适合您的决策取决于所选的编程模型、 通讯框架和编程语言中编写您的服务的。
### 可靠的操作者的通信
编写使用可靠者 API 的所有通信服务的详细信息都抽象和通信发生方法调用 ActorProxy，您可以停止阅读此处。

### 可靠的服务的通信选项
如果使用可靠服务 API 编写您的服务时，有几个不同的选项。 一种通信协议的选择将有关服务结构的通信 Api，您可以使用指导。

* **没有需要使用一个特定的通信协议，并且我想东西向上和快速运行**︰ 如果您没有选择特定的通信框架则为您的理想选择是使用[默认堆栈](service-fabric-reliable-services-communication-default.md)，以便作为主角的通信模型的相似模型。 这是最简单和最快的方式着手服务通信。 它提供强类型，以便调用您的代码中的远程服务的方法看起来像调用本地对象实例上的方法提取大部分通信详细信息消失的 RPC 通信。 这些类抽象的分辨率、 连接、 重试和错误处理设置通信通道时，允许方法调用基于的通信模型。 对此`ServiceCommunicationListener`类是用于服务器端和`ServiceProxy`类在客户端的通信。

* **HTTP**︰ 利用基于 HTTP 的通信提供了您可以使用服务结构通信 Api，允许通信机制，用于定义分辨率时的灵活性，连接，然后重试逻辑仍然从抽象出来您。 例如您可以使用 Web API 用于指定的通信机制和利用[`ICommunicationClient`和`ServicePartitionClient`类](service-fabric-reliable-services-communication.md)安装程序通信。
* **WCF**︰ 如果有使用 WCF 通信框架的现有代码，然后可以为客户端的服务器端，和 WcfCommunicationClient 和 ServicePartitionClient 类使用 WcfCommunicationListener。 有关更多详细信息请参阅[这篇文章](service-fabric-reliable-services-communication-wcf.md)。

* **其他包括自定义协议的通信框架**︰ 如果您计划使用任何其他包括有服务结构通信 Api 时所有的工作，以发现并连接抽象的您，您的通信堆叠成的插件，您可以自定义通信协议的通信框架。 请参见[这篇文章](service-fabric-reliable-services-communication.md)的更多详细信息。

### 用不同编程语言编写的服务之间的通信
所有 ServiceFabric 通信 Api 都目前仅在 C# 中提供因此，如果有一种服务，您必须编写自己的通信机制，然后用一些其他编程语言，如 Java 或 Node.JS 编写。

## 下一步行动
* [默认通信栈提供可靠服务框架 ](service-fabric-reliable-services-communication-default.md)
* [可靠的服务的通信模型](service-fabric-reliable-services-communication.md)
* [开始使用 Microsoft Azure 服务结构 Web API 服务 OWIN 和自托管](service-fabric-reliable-services-communication-webapi.md)
* [WCF 服务可靠的基于通信栈](service-fabric-reliable-services-communication-wcf.md)

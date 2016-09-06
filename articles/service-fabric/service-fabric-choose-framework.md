---
ms.openlocfilehash: 217fc4c6d0877704c1ae70e65b79af3c5a6514ff
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="选择一个框架"
   description="服务结构提供了两个高层框架的构建服务︰ 演员框架和服务框架。 了解每个值将帮助您为您的应用程序进行正确的体系结构决策。"
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/09/2015"
   ms.author="seanmck"/>

# 选择一个框架，用于您的服务

服务结构提供了两个高层框架的构建服务︰ 可靠的演员 Api 和可靠的服务 Api。 虽然两者都生成相同的服务结构核心上，它们会使不同权衡简洁性和并发、 分区和通信方面的灵活性。 它将有助于理解这两个模型，以便您可以在您的应用程序中选择特定服务的适当框架。

## 比较可靠的演员 Api 和可靠的服务 Api

|**可靠的演员 Api**|**可靠的服务的 Api**|
|-----------------------|--------------------------|
|问题空间涉及许多小的独立单元的状态和逻辑|您需要跨多个组件维护逻辑|
|所需工作与单线程对象，同时又能够扩展和维护一致性|您想要使用可靠的集合 （如.NET 字典和队列） 来存储和管理您的状态|
|所需的框架来管理并发性和粒度的状态|您想要控制的粒度和并发的州/省|
|所需的平台来管理您的通信|您想要管理的通信和控制您的服务的分区方案|

请记住它是完全合理，对不同的服务，在您的应用程序中使用不同的框架。 例如，您可能必须聚合生成数据，由多个参与者的有状态服务。

## 下一步行动

- [了解更多有关可靠的演员 Api](service-fabric-reliable-actors-introduction.md)
- [了解有关可靠的服务 Api 的详细信息](../Service-Fabric/service-fabric-reliable-services-introduction.md)

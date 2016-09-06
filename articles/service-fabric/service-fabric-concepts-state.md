---
ms.openlocfilehash: ba3de2bb30dc8e389fb97fad232b91fecff97ee8
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="定义和管理状态"
   description="如何定义和管理服务结构中的服务状态"
   services="service-fabric"
   documentationCenter=".net"
   authors="appi101"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/26/2015"
   ms.author="aprameyr"/>

# 服务状态
**服务状态**是服务正常工作所需的数据。 它是服务中读取，并将做工作的变量和数据结构。

例如︰ 考虑一个简单的计算器服务。 此服务使用两个数字并返回其总和。 这是纯粹的无状态服务具有与其关联的任何数据。

现在，考虑同一个计算器计算总和除了它还具有方法返回最后一个它必须计算的总和。 此服务是现在状态的--它包含 （计算新的总和） 时，它将写入的某些状态和读取 （当它返回最后一个计算的总和）。

在服务结构中，第一个服务称为无状态的服务。 第二个服务调用有状态服务。

## 存储服务的状态
状态可以外部化或已操作的状态代码与共存。 Externalization 通常是状态的通过使用外部数据库或存储区。 在计算器示例中，这可能是一个 SQL 数据库表中存储的当前结果。 若要进行求和计算的每个请求在此行上执行更新。

此外可以操纵此代码的代码与并存状态。 使用此模型构建服务结构中的有状态服务。 服务结构提供的基础架构，以确保这种状态是高可用性和容错能力出现了故障。

## 下一步行动

服务结构的概念信息，请参阅以下资源︰

- [服务结构服务的可用性](service-fabric-availability-services.md)

- [服务结构服务的可扩展性](service-fabric-concepts-scalability.md)

- [分区结构服务](service-fabric-concepts-partitioning.md)
 

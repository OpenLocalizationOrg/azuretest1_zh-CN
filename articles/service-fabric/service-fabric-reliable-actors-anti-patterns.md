---
ms.openlocfilehash: 60287c3498422ad923423c2338280e9ea17ad622
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="一些 Azure 服务结构参与者的反模式"
   description="对于客户来说学习 Azure 服务结构参与者的一些潜在缺陷"
   services="service-fabric"
   documentationCenter=".net"
   authors="jessebenson"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/11/2015"
   ms.author="claudioc"/>

# 可靠的操作者设计模式︰ 某些反模式

我们正在学习服务结构可靠参与者的客户标识潜在的陷阱︰

* 事务处理系统被视为可靠的演员。 服务结构可靠者不提供酸是一个两阶段提交基于系统。 如果我们不实现可选的持久性，而参与者的计算机运行瘫痪，其当前状态将随之转。 参与者将陆续推出速度非常快，另一个节点上，但除非我们已经实现支持持久性，则状态将不再存在。 但是，利用重试次数，重复筛选和/或幂等设计，可以获得高程度的可靠性和一致性。

* 块。 可靠的演员在我们做的一切应该是异步的。 这是通常很容易，因为异步 Api 是多产现在 Microsoft 平台中。 但是，如果由于某种原因，我们必须与系统只提供阻塞 API 进行交互，我们需要将它放在明确使用.NET 线程池的包装。

* 对架构师。 让工作环境。 它可以是硬的开发人员习惯于担心并发集合和锁，或使用工具进行编译对象从 XML，到只是代码执行简单操作，如将值赋给变量或计划工作的类。 计划的任务是内置的。 锁，则不需要。 状态不是可敌人。 这需要一段时间才能习惯为许多人所做的大量的大规模环境中的服务器端工作。

* 使单个演员瓶颈。 它通常是很容易被捕获此之一，拥有数以百万计的使用者 funnelling 到另一个参与者的单个实例。 使用我们在[分布式的计算的设计模式](service-fabric-reliable-actors-pattern-distributed-computation.md)中所示的聚合方法。

* 盲目地映射实体模型。 这对于开发人员来说来自关系世界是其中的问题来模拟使用实体和它们之间的关系。 对于理解主题域仍然有用这种方法时，它应结合面向服务的思想和行为与混合。

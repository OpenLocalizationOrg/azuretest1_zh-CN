---
ms.openlocfilehash: 6924033f054219f774cf7a57012b2ccd2ea65c2d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="服务总线津贴和标准消息层概述 |Microsoft Azure"
    description="服务总线津贴和标准消息"
    services="service-bus"
    documentationCenter=".net"
    authors="djrosanova"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="09/03/2015"
    ms.author="sethm"/>

# 服务总线津贴和标准的消息传递层 

服务总线通过代理进行消息传递，其中包括消息队列和主题等实体，与丰富的结合企业邮件服务功能发布-订阅在云规模较大的语义。 许多复杂的云解决方案可作为通信主干中转服务总线的消息传递。

*特优*服务总线的消息传递层的引入，我们在谈论常见的客户请求，围绕规模、 性能和任务关键型应用程序的可用性。 虽然几乎完全相同的功能集，这些两层的 Azure 服务总线的消息传递适用于不同的使用情形。

下表中突出显示一些高层次的差异。

| 高级                               | 标准                       |
|---------------------------------------|--------------------------------|
| 高吞吐量                       | 变量的吞吐量            |
| 可预知的性能               | 变量的滞后时间               |
| 可预测的定价                   | 收现付可变定价 |
| 向上和向下的工作负载扩展能力 | N/A                            |

**Azure 服务总线高级消息**提供资源隔离在 CPU 和内存层，以便每个客户工作负载以隔离方式运行。 此资源容器称为*消息服务单位*。 每个高级命名空间分配至少一个消息传递的单位。 您可以购买 1、 2 或 4 个邮件单位为每个服务总线特优的命名空间。 单个工作负载或实体可以跨越多个消息传递单元，虽然帐单是在 24 小时或每日费率的费用，可在会更改消息的单位数。 其结果是可预知而且可重复执行的性能，基于服务总线的解决方案。

不仅是这样的业绩，更可预知而且可用，而且速度也更快。 Azure 服务总线高级消息基于[Azure 事件集线器](http://azure.microsoft.com/services/event-hubs/)中引入的存储引擎。 使用高级消息，峰值性能为标准层比快得多。

## 高级消息传递技术差异

以下是一些津贴和标准的消息传递层之间的区别。

### 分区图元

分区的实体都支持高级消息，但它们不起作用的服务总线消息如下所示的标准和基本层一样。 高级消息不作为数据存储区中使用 SQL 和不再可能的资源竞争与共享的平台。 因此，分区不是必要的。 此外，已从 16 标准发送消息给津贴中的两个分区分区更改分区计数。 拥有 2 个分区可以确保可用性，更适合数字最优运行时环境。 有关详细信息，请参阅[分区消息处理实体](https://msdn.microsoft.com/library/dn520246.aspx)。

### 表示实体

因为它运行在一个完全独立的运行时环境中，将不再需要在高级消息明确实体。 因此，明确实体不支持在高级命名空间中。 有关明示功能的详细信息，请参阅[Microsoft.ServiceBus.Messaging.QueueDescription.EnableExpress](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enableexpress.aspx)属性。

## 下一步行动

若要了解有关服务总线消息，请参阅下列主题。

- [引入了 Azure 服务总线高级消息 （日志）](http://azure.microsoft.com/blog/introducing-azure-service-bus-premium-messaging/)
- [引入了 Azure 服务总线高级消息 (Channel9)](https://channel9.msdn.com/Blogs/Subscribe/Introducing-Azure-Service-Bus-Premium-Messaging)
- [服务总线消息概述](service-bus-messaging-overview.md)
- [Azure 服务总线体系结构概述](fundamentals-service-bus-hybrid-solutions.md)
- [如何使用服务总线队列](service-bus-dotnet-how-to-use-queues.md)

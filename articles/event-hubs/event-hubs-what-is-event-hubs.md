---
ms.openlocfilehash: f216583f6337d44db44033db1038dee8684195c7
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="什么是 Azure 事件集线器？"
    description="Azure 事件集线器的概述。"
    services="event-hubs"
    documentationCenter=".net"
    authors="nberdy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="07/15/2015"
    ms.author="sethm"/>

# 什么是 Azure 事件集线器？

Azure 事件集线器是一种高度可扩展的数据入口服务，可以摄取数以百万计，每秒钟的事件，以便您可以处理和分析大量数据连接的设备和应用程序所产生。 事件集线器充当事件管道，"前门"，一旦数据被收集在一个事件集线器，它可以转换并存储使用任何实时分析提供程序或批处理存储适配器。 这样的事件使用者可以访问他们自己的日程安排上的事件，事件集线器将生产从这些事件中，消耗的事件流的分离。

## 事件集线器功能

事件集线器是处理事件和遥测入口到在大规模，云提供低延迟和高可靠性的服务事件。 此服务是非常有用的︰

* 应用程序规范
* 用户体验或工作流处理
* 互联网内容 (IoT) 方案

一些其他关键事件集线器功能是跟踪在移动应用程序，从 web 场中，在控制台游戏中，游戏中事件捕获通信信息的行为或遥测数据收集从工业计算机或连接车辆。

与[服务总线队列和主题](../service-bus/service-bus-messaging-overview.md)，不同事件集线器致力于提供在规模较大的邮件流处理。 事件集线器功能与主题的不同，他们强烈偏爱高吞吐量和事件处理方案。 因此，事件集线器不实现某些消息传递功能所提供的[主题](service-bus/fundamentals-service-bus-hybrid-solutions.md#topics)。 如果您需要这些功能，主题仍然是最佳的选择。

## 下一步行动

有关事件集线器的详细信息，请参阅下列主题。

- [事件集线器概述](event-hubs-overview.md)
- [事件集线器编程指南](event-hubs-programming-guide.md)
- [事件集线器可用性和支持常见问题](event-hubs-availability-and-support-faq.md)
- 入门[教程事件集线器]
- 完整的[示例应用程序，它使用事件集线器]

[事件集线器教程]: service-bus-event-hubs-csharp-ephcs-getstarted.md
[示例应用程序使用事件集线器]: https://code.msdn.microsoft.com/windowsazure/Service-Bus-Event-Hub-286fd097
测试t

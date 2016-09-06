---
ms.openlocfilehash: 43d6277d80e15364c55316eb7cc3d2e0bb2f3f59
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="Azure 服务总线 |Microsoft Azure" 
    description="介绍不同的方式为您可以使用服务总线连接 Azure 应用程序与其他软件。" 
    services="service-bus" 
    documentationCenter=".net" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="07/25/2015" 
    ms.author="sethm"/>

# Azure 服务总线

无论应用程序或服务运行在云中或内部部署，它经常需要与其他应用程序或服务进行交互。 为了提供的广泛有用的方法来执行此操作，Azure 提供了服务总线。 本文采用一看这种技术，描述它是什么和为什么您可能希望使用它。

## 服务总线基础知识
不同的情况要求不同样式的通信。 有时，让应用程序发送和接收邮件通过简单队列是最好的解决办法。 在其他情况下，普通的队列是不够的;用发布和订阅机制队列是更好。 并在某些情况下，所有真正需要的是应用程序成本 / 收益分析之间的连接; 队列不是必需。 服务总线提供了所有三个选项，让您以多种不同的方式进行交互的应用程序。

服务总线是多租户的云服务，这意味着，由多个用户共享服务。 每个用户，应用程序开发人员，例如创建*命名空间*，然后定义该命名空间内她需要的通信机制。 图 1 显示效果。

![][1]
 
**图 1︰ 服务总线提供云通过连接应用程序的多租户服务。**

在命名空间中，可以使用一个或多个四种不同的通信机制，其中的每个连接的应用程序以不同的方式。 选项包括︰

- *队列*，它允许一个双向通信。 每个队列充当中介 （有时称为*经纪人*），用于存储已发送的邮件之前收到。 单个收件人接收每条消息。
- *主题*，它们提供了一个双向沟通使用的某个特定主题的*订阅*可以有多个订阅。 队列中，像一个主题作为中介，但每个订阅，可以选择使用筛选器来接收与特定条件匹配的邮件。
- *继电器*提供双向通信。 与不同的队列和主题，中继不存储未决消息-it 的不是一个代理。 相反，它只是将其传递到目标应用程序。
- *事件集线器*，到大规模，在云的事件和遥测入口提供低延迟和高可靠性。

创建队列、 主题、 中继或事件中心时，您为它指定一个名称。 此名称加上任何您调用您的名称空间，创建的对象的唯一标识符。 应用程序可以向服务总线提供此名称，然后使用该队列、 主题、 中继或事件中心之间的通信。 

若要使用这些对象，Windows 应用程序可以使用 Windows 通讯基础 (WCF)。 对于队列、 主题和事件集线器 Windows 应用程序还可以使用服务总线定义消息传递 Api。 若要使这些对象更容易地使用非 Windows 应用程序中，Microsoft 提供的 Sdk，为 Java，Node.js 和其他的语言。 您还可以访问队列、 主题和事件集线器通过 HTTP 使用 REST Api。 

务必要了解，即使服务总线本身运行在云中 (即，在微软 Azure 数据中心)，使用它的应用程序可以运行任意位置。 可以使用服务总线来连接上 Azure，例如，运行应用程序或应用程序运行在您自己的数据中心内。 您还可以使用它连接在 Azure 或另一个运行的应用程序与应用程序内部或平板电脑和手机的云平台。 它是甚至可能为中心的应用程序或其他连接家用电器、 传感器和其他设备。 服务总线是通用的通信机制，可以从几乎任何地方访问云中。 如何使用它取决于您的应用程序需要执行。

## 队列

假设您决定将两个应用程序使用服务总线队列连接。 图 2 显示了这种情况。

![][2]
 
**图 2︰ 服务总线队列提供单向异步队列。**

此过程非常简单︰ 发件人将消息发送到服务总线队列，并在以后的某个时间拾取该消息接收器。 队列可以只是单个接收器，如图 2 所示，也可以从同一个队列中读取多个应用程序。 只需一个接收器在后一种情况下，读取每个消息的多播服务您应改用一个主题。

每个消息包含两个部分︰ 一组属性，每个键/值对和二进制消息正文。 使用方式取决于应用程序要做什么。 例如，应用程序发送有关最近销售的一条消息可能包含属性*卖方 ="Ava"*和*量 = 10000*。 邮件正文可能包含已签署的销售合同的扫描的图像; 如果没有，只是保留为空。

接收器可以两种不同的方式，从服务总线队列读取消息。 第一个选项，称为*ReceiveAndDelete*，从队列中移除消息，并立即将其删除。 这很简单，但如果接收器崩溃之前处理该邮件，该邮件将会丢失。 它已从队列中移除，因为没有其他接收器可以访问它。 

第二个选项， *PeekLock*，是为了解决这个问题。 像 ReceiveAndDelete，PeekLock 读从队列中移除消息。 它不但是删除消息。 相反，它锁定的消息，使其不可见到其他收件人，然后等待这三个事件之一︰

- 如果接收方成功地处理邮件，它调用*完成*，并且队列中删除消息。 
- 如果接收方决定它不能成功地处理该消息，则调用*放弃*。 然后，队列从邮件中删除锁定，并使其他接收器。
- 如果接收器调用这两项都在一个可配置的时间段内 （默认为 60 秒），该队列假定接收器出现故障。 在这种情况下，其行为的放弃，使消息可供其他接收器调用接收方了。

请注意此处怎么办︰ 相同的消息可能在送达两次，可能是两个不同的接收器。 为此，必须准备使用服务总线队列的应用程序。 为便于重复检测，每条消息都有唯一的邮件 Id 属性，默认情况下，很多时候都保持不变，则无论从队列读取消息。 

队列是在很多情况下很有用。 它们允许应用程序通信甚至都不运行时在同一时间，这一点与批处理和移动应用程序特别有用。 具有多个接收器的队列还提供了自动负载平衡，因为发送的邮件散布在这些接收器。

## Topics

因为它们是很有用，队列并不总是正确的解决方案。 有时，服务总线主题是好的。 图 3 显示了这一思想。

![][3]
 
**图 3︰ 基于预订的应用程序指定的筛选器，它可以接收部分或全部发送到服务总线主题的消息。**

主题是在许多方面与队列相似。 发件人提交消息中相同的方式在提交到队列，消息以及这些消息的主题与队列一样具有相同的外观。 主要的差别是主题，让每个接收的应用程序定义*筛选器*来创建自己的订阅。 订阅服务器然后会看到与该筛选器匹配的邮件。 例如，图 3 显示发件人和主题三个订阅服务器，每个都有其自己的筛选器︰

- 订阅服务器 1 收到包含该属性的邮件*卖方 ="Ava"*。
- 订阅服务器 2 接收包含该属性的消息*卖方 ="Ruby"*和/或包含其值大于 100000*量*属性。 红宝石也许是销售经理，并因此她希望能看到她自己的销售和所有的大销售，而不考虑这些。
- 订阅服务器 3 已设置为*True*，这意味着它接收的所有邮件的筛选器。 例如，该应用程序可能是负责维护审计线索，因此需要查看所有邮件。

使用队列，如到某个主题的订阅服务器可以读取邮件使用 ReceiveAndDelete 或 PeekLock。 与不同的队列，但是，一条消息发送到一个主题可以接收由多个订阅服务器。 当多个应用程序可能有兴趣相同的邮件，这种方法，通常称为*发布和订阅*，非常有用。 通过定义适当的筛选器，每个订阅服务器可以领略只查看所需的消息流的一部分。

## 继电器

队列和主题提供单向异步通信通过一个代理程序。 通信排只是一个方向，并没有发件人和接收器之间的直接连接。 但是，如果您不希望这样呢？ 假设您的应用程序需要同时发送和接收消息，或可能希望它们之间的直接链接，不需要经纪人来存储邮件。 到此，通讯方案服务总线提供中继，如图 4 所示。

![][4]
 
**图 4︰ 服务总线中继提供应用程序之间的同步的双向通信。**

这是很明显的问题，提出有关继电器︰ 为什么使用其中一个？ 即使我不需要的队列，为什么使通讯通过云服务，而不是只是直接交互的应用程序？ 答案是，谈话直接可以比您想像的更难。

假设您想要连接两个内部应用程序，同时在企业数据中心内运行。 每个这些应用程序位于防火墙后面，并且每个数据中心可能使用网络地址转换 (NAT)。 防火墙会阻止所有而不是几个端口上的传入数据和 NAT 暗示运行每个应用程序的计算机不具有固定的 IP 地址，您可以直接从到达数据中心之外的。 而一些额外的帮助，通过公共互联网连接到这些应用程序是有问题的。

服务总线中继提供这种帮助。 通过中继的双向通信，每个应用程序建立与服务总线出站 TCP 连接，然后使其保持打开状态。 两个应用程序之间的所有通信将都旅行通过这些连接。 从数据中心内部建立每个连接，因为防火墙将允许每个应用程序的传入通讯，而无需打开新的端口。 这种方法还避开了 NAT 问题，由于每个应用程序在整个通信云一致的终结点。 通过交换通过中继数据，应用程序可以避免否则会使通信难的问题。 

若要使用服务总线继电器，应用程序依赖于 Windows 通讯基础 (WCF)。 服务总线提供更加直接地通过继电器进行交互的 Windows 应用程序的 WCF 绑定。 通常，已使用 WCF 应用程序可以只指定一个这些绑定，然后通过中继与彼此交谈。 与不同的队列和主题，但是，使用中继从非 Windows 应用程序，尽管可能，需要一些编程的努力;提供了无标准库。

与队列和主题，不同的应用程序不需要显式创建中继。 相反，当想要接收消息的应用程序建立与服务总线的 TCP 连接，则中继将自动创建。 当断开连接时，将删除中继。 若要允许应用程序查找特定侦听器创建中继，服务总线提供了使应用程序能够按名称查找特定中继的注册表。

继电器是合适的解决方案时所需的应用程序之间的直接通信。 例如，请考虑航空订票系统中必须从签入信息亭、 移动设备和其他计算机访问内部数据中心运行。 所有这些系统上运行的应用程序可能依赖服务总线继电器在云中进行通信，可能正在运行的任何地方。

## 事件集线器

事件集线器是一个高度可扩展的接收系统，可以处理数以百万计，每秒钟的事件，使您的应用程序处理和分析大量的数据连接的设备和应用程序所产生。 例如，可以使用事件中心来收集一队汽车实时引擎性能数据。 一旦收集到事件集线器，可以转换并使用任何实时的分析提供者或存储群集存储数据。 有关事件集线器的详细信息，请参阅[事件集线器概述](../event-hubs-overview.md)。

## 摘要

连接应用程序一直是构建完整的解决方案的部分需要相互通信的应用程序和服务的方案的区域设置以提高随着越来越多的应用程序和设备连接到互联网。 通过实现此队列、 主题、 继电器和事件集线器通过提供基于云技术，旨在使本很重要的功能，易于实施和更广泛地提供服务总线。

## 下一步行动

现在，您已经学习了 Azure 服务总线的基本原理，按照这些链接以了解详细信息。

- 如何使用[服务总线队列](service-bus-dotnet-how-to-use-queues.md)。
- 如何使用[服务总线主题](service-bus-dotnet-how-to-use-topics-subscriptions.md)。
- 如何使用[服务总线中继](service-bus-dotnet-how-to-use-relay.md)。
- 服务总线示例︰ 请参见[MSDN][]上的概述。 

[svc 总线]: ./media/fundamentals-service-bus-hybrid-solutions/SvcBus_01_architecture.png
[队列]: ./media/fundamentals-service-bus-hybrid-solutions/SvcBus_02_queues.png
[主题 sub]: ./media/fundamentals-service-bus-hybrid-solutions/SvcBus_03_topicsandsubscriptions.png
[中继]: ./media/fundamentals-service-bus-hybrid-solutions/SvcBus_04_relay.png
[MSDN]: https://msdn.microsoft.com/library/dn194201.aspx 

[1]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_01_architecture.png
[2]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_02_queues.png
[3]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_03_topicsandsubscriptions.png
[4]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_04_relay.png
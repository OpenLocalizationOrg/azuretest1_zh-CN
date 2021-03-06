---
ms.openlocfilehash: 91d933a47300745259cb1df519d29bc90689e564
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="开始使用事件集线器"
    description="按照本教程中若要开始使用 Azure 事件集线器;在 C# 和 Apache 风暴群集中接收它们发送事件。"
    services="event-hubs"
    documentationCenter=""
    authors="fsautomata"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="core"
    ms.tgt_pltfrm="csharp"
    ms.devlang="csharp"
    ms.topic="article" 
    ms.date="09/01/2015"
    ms.author="sethm"/>

# 开始使用事件集线器

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## 简介

事件集线器是可以摄入量数以百万计的具有高可扩展性的摄取系统每秒钟的事件，使应用程序可以处理和分析大量的数据产生的对连接的设备和应用程序。 一旦收集到事件集线器，可以转换并使用任何实时的分析提供者或存储群集存储数据。

有关详细信息，请参阅[事件集线器概述]。

在本教程中，您将学习如何到事件中心在 C# 中使用一个控制台应用程序接收消息并在并行使用 Apache 风暴中检索它们。

若要完成本教程中，您将需要以下︰

+ Microsoft Visual Studio 速成 2013年的窗口

+ Java 开发环境配置为运行[Maven](http://maven.apache.org/)。 对于本教程，我们将假定[Eclipse](https://www.eclipse.org/)

+ 活动的 Azure 帐户。 <br/>如果您没有帐户，您可以在几分钟创建免费的试用帐户。 有关详细信息，请参阅<a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Azure 免费试用版</a>。

## 创建事件中心

1. 登录到[Azure 的门户网站]，然后单击**新建**，在屏幕的底部。

2. 单击**应用程序服务**，再**服务总线**，然后**事件中心**，然后**快速创建**。

    ![][1]

3. 键入事件中心的名称、 选择您需要的区域，然后单击**创建新的事件中心**。

    ![][2]

4. 单击您刚创建的 (通常为***事件中心名称*-ns**) 的命名空间。

    ![][3]

5. 单击顶部的页的**事件集线器**选项卡，然后单击您刚刚创建的事件中心。

    ![][4]

6. 单击顶部的**配置**选项卡添加一个名为**SendRule** ，具有*发送*权限规则、 添加*聆听*的权利，名为**ReceiveRule**的另一个规则，然后单击**保存**。

    ![][5]

7. 在同一页上，记下生成的密钥为**ReceiveRule**。

    ![][6c]

8. 单击顶部，**仪表板**选项卡，然后单击**连接信息**。 请注意两个连接字符串。

    ![][6]

现在创建事件中心，并具有您需要发送和接收事件的连接字符串。

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-csharp](../../includes/service-bus-event-hubs-get-started-send-csharp.md)]


[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-storm](../../includes/service-bus-event-hubs-get-started-receive-storm.md)]

## 运行应用程序

现在，您就可以运行应用程序了。

1.  日蚀式，从运行的**LogTopology**类，然后等待其启动的所有分区接收器。

2.  运行该**发件人**项目，按**enter 键**在控制台窗口中，查看接收窗口中显示的事件。

    ![][22]

## 下一步行动

既然已经建立了有效的应用程序创建事件中心以及发送和接收数据，您可以将移动到以下情况︰

- 完整的[示例应用程序，它使用事件集线器]。
- 该[事件处理事件集线器扩展]示例。
- [排队的消息传送解决方案]使用服务总线队列。

<!-- Images. -->
[1]: ./media/event-hubs-csharp-storm-getstarted/create-event-hub1.png
[2]: ./media/event-hubs-csharp-storm-getstarted/create-event-hub2.png
[3]: ./media/event-hubs-csharp-storm-getstarted/create-event-hub3.png
[4]: ./media/event-hubs-csharp-storm-getstarted/create-event-hub4.png
[5]: ./media/event-hubs-csharp-storm-getstarted/create-event-hub5.png
[6]: ./media/event-hubs-csharp-storm-getstarted/create-event-hub6.png
[6 c]: ./media/event-hubs-csharp-storm-getstarted/create-event-hub6c.png

[22]: ./media/event-hubs-csharp-storm-getstarted/receive-storm1.png

<!-- Links -->
[Azure 门户]: https://manage.windowsazure.com/
[事件处理器主机]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[事件集线器概述]: event-hubs-overview.md
[示例应用程序使用事件集线器]: https://code.msdn.microsoft.com/windowsazure/Service-Bus-Event-Hub-286fd097
[扩展事件处理事件集线器]: https://code.msdn.microsoft.com/windowsazure/Service-Bus-Event-Hub-45f43fc3
[排队的消息传送解决方案]: ../service-bus-dotnet-multi-tier-app-using-service-bus-queues.md
 
测试

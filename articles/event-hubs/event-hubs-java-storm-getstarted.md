---
ms.openlocfilehash: a34221c0a2f92948a5f1d1f68883be5130ee722f
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="开始使用事件集线器"
    description="按照本教程中若要开始使用 Azure 事件集线器;发送使用 Java 和 Apache 风暴群集中接收其事件。"
    services="event-hubs"
    documentationCenter=""
    authors="fsautomata"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="core"
    ms.tgt_pltfrm="java"
    ms.devlang="java"
    ms.topic="article"
    ms.date="07/21/2015"
    ms.author="sethm"/>

# 开始使用事件集线器

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## 简介

事件集线器是可以摄入量数以百万计的具有高可扩展性的摄取系统每秒钟的事件，使应用程序可以处理和分析大量的数据产生的对连接的设备和应用程序。 一旦收集到事件集线器，可以转换并使用任何实时的分析提供者或存储群集存储数据。

有关详细信息，请参阅[事件集线器概述]。

在本教程中，您将学习如何收集到一个控制台应用程序，在 Java 中，使用事件中心的消息，并在并行使用 Apache 风暴中检索它们。

若要完成本教程中，您将需要以下︰

+ Java 开发环境配置为运行[Maven](http://maven.apache.org/)。 对于本教程，我们将假定[Eclipse](https://www.eclipse.org/)。

+ 活动的 Azure 帐户。 <br/>如果您没有帐户，您可以在几分钟创建免费的试用帐户。 有关详细信息，请参阅<a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Azure 免费试用版</a>。

## 创建事件中心

1. 登录到[Azure 的管理门户]，然后单击**新建**，在屏幕的底部。

2. 单击**应用程序服务**，然后**服务总线**，然后**事件集线器**，然后**快速创建**。

    ![][1]

3. 键入的名称甚至网络集线器，选择您需要的区域，然后单击**创建新的事件中心**。

    ![][2]

4. 单击您刚创建的 (通常为***事件中心名称*-ns**) 的命名空间。

    ![][3]

5. 单击顶部的页的**事件集线器**选项卡，然后单击您刚刚创建的事件中心。

    ![][4]

6. 单击顶部的页的**配置**选项卡、 添加*发送*权限与名为**SendRule**的规则、 添加其他规则调用**ReceiveRule**与*聆听*的权利，然后单击**保存**。

    ![][5]

7. 在同一页上，记下生成的密钥对**SendRule**和**ReceiveRule**。

    ![][6c]

现在创建事件中心，并具有您需要发送和接收事件的连接字符串。

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-java](../../includes/service-bus-event-hubs-get-started-send-java.md)]


[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-storm](../../includes/service-bus-event-hubs-get-started-receive-storm.md)]

## 运行应用程序

现在，您就可以运行应用程序了。

1.  日蚀式，从运行的**LogTopology**类，然后等待其启动的所有分区接收器。

2.  运行该**发件人**项目，按**enter 键**在控制台窗口中，查看接收窗口中显示的事件。

    ![][22]

> [AZURE.NOTE] 在本教程只风暴在本地模式下使用动向。 请参阅[HDInsight 风暴概述]和风暴部署和模式的详细信息的官方[Apache 风暴]文档。

## 下一步行动

以下资源都可用于开发应用程序集成事件集线器和风暴。

- [分析与风暴和 HDInsight 传感器数据]是完整方案教程，说明如何使用事件集线器、 占领和 HBase 摄取 Hadoop 群集中的传感器数据。
- [开发流数据处理应用程序与 SCP.NET 和 C# 在风暴和 HDInsight]是如何编写使用 C# 的冲击管道的教程。

<!-- Images. -->
[1]: ./media/event-hubs-java-storm-getstarted/create-event-hub1.png
[2]: ./media/event-hubs-java-storm-getstarted/create-event-hub2.png
[3]: ./media/event-hubs-java-storm-getstarted/create-event-hub3.png
[4]: ./media/event-hubs-java-storm-getstarted/create-event-hub4.png
[5]: ./media/event-hubs-java-storm-getstarted/create-event-hub5.png
[6]: ./media/event-hubs-getstarted/create-event-hub6.png
[6 c]: ./media/event-hubs-java-storm-getstarted/create-event-hub6c.png

[22]: ./media/event-hubs-java-storm-getstarted/receive-storm2.png

<!-- Links -->
[Azure 的管理门户]: https://manage.windowsazure.com/
[事件处理器主机]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[事件集线器概述]: http://msdn.microsoft.com/library/azure/dn836025.aspx

[Apache 的暴风雨]: https://storm.incubator.apache.org
[HDInsight 风暴概述]: http://azure.microsoft.com/documentation/articles/hdinsight-storm-overview/
[分析与风暴和 HDInsight 传感器数据]: http://azure.microsoft.com/documentation/articles/hdinsight-storm-sensor-data-analysis/
[开发流数据处理应用程序与 SCP.NET 和 C# 在风暴和 HDInsight]: http://azure.microsoft.com/documentation/articles/hdinsight-hadoop-storm-scpdotnet-csharp-develop-streaming-data-processing-application/
 
测试

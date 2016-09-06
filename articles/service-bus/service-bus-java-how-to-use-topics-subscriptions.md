---
ms.openlocfilehash: e1afea259bf6160f3b108fb6d68918a9b5ffce32
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="如何使用服务总线主题 (Java) |Microsoft Azure"
    description="了解如何使用 Azure 服务总线主题和订阅。 Java 应用程序写入代码示例。"
    services="service-bus"
    documentationCenter="java"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="06/19/2015"
    ms.author="sethm"/>

# 如何使用服务总线主题/订阅

本指南介绍了如何使用服务总线主题和订阅。 这些示例用 Java 编写的并使用[Java 的 Azure SDK][]。 所包含的方案包括**创建主题和订阅**、**创建订阅筛选器**、**发送邮件的主题**、**接收来自订阅邮件**并**删除主题和订阅**。

[AZURE.INCLUDE [service-bus-java-how-to-create-topic](../../includes/service-bus-java-how-to-create-topic.md)]

## 配置应用程序使用服务总线
请确保已安装[Java 的 Azure SDK][]之前生成此示例。 如果您正在使用 Eclipse，可以安装[Eclipse 的 Azure Toolkit][]包含 Java Azure SDK。 然后可以向项目添加**Microsoft Azure Java 库**︰
![](media/service-bus-java-how-to-use-topics-subscriptions/eclipselibs.png)

将下面的导入语句添加到 Java 文件的顶部︰

    // Include the following imports to use service bus APIs
    import com.microsoft.windowsazure.services.servicebus.*;
    import com.microsoft.windowsazure.services.servicebus.models.*;
    import com.microsoft.windowsazure.core.*;
    import javax.xml.datatype.*;

将 Java 的 Azure 库添加到生成路径并将其纳入您的项目部署程序集。

## 如何创建主题

通过**ServiceBusContract**类可以进行服务总线主题的管理操作。 **ServiceBusContract**对象构造与封装的 SAS 标记有权管理它，适当配置， **ServiceBusContract**类与 Azure 的通信的唯一点。

**ServiceBusService**类提供的方法来创建、 枚举和删除的主题。 下面的示例演示如何使用**ServiceBusService**对象来创建一个名为"TestTopic"，一个名为"HowToSample"的命名空间的主题︰

    Configuration config =
        ServiceBusConfiguration.configureWithSASAuthentication(
          "HowToSample",
          "RootManageSharedAccessKey",
          "SAS_key_value",
          ".servicebus.windows.net"
          );

    ServiceBusContract service = ServiceBusService.create(config);
    TopicInfo topicInfo = new TopicInfo("TestTopic");
    try  
    {
        CreateTopicResult result = service.createTopic(topicInfo);
    }
    catch (ServiceException e) {
        System.out.print("ServiceException encountered: ");
        System.out.println(e.getMessage());
        System.exit(-1);
    }

在**TopicInfo**上启用要进行调整的主题的属性的方法 (例如︰ 设置默认"--生存时间"值将应用于发送到该主题的消息)。 下面的示例演示如何创建一个名为"TestTopic"的最大大小为 5 GB 的主题︰

    long maxSizeInMegabytes = 5120;  
    TopicInfo topicInfo = new TopicInfo("TestTopic");  
    topicInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
    CreateTopicResult result = service.createTopic(topicInfo);

请注意，您可以使用**listTopics**方法对**ServiceBusContract**对象检查服务命名空间中是否已存在具有指定名称的主题。

## 如何创建订阅

此外与**ServiceBusService**类创建主题订阅。 被命名为订阅，并且可以具有可选的筛选，用于限制的消息传递给订阅的虚拟队列集。

### 使用默认值 (MatchAll) 筛选器创建订阅

**MatchAll**过滤器是如果没有筛选器指定创建新订阅时使用的默认筛选器。 当使用**MatchAll**筛选器时，预订的虚拟队列中放置发布到该主题的所有邮件。 下面的示例创建一个名为"AllMessages"的订阅，并使用默认**MatchAll**筛选器。

    SubscriptionInfo subInfo = new SubscriptionInfo("AllMessages");
    CreateSubscriptionResult result =
        service.createSubscription("TestTopic", subInfo);

### 使用筛选器创建订阅

此外可以设置筛选器，允许您将消息发送到一个主题范围应显示特定主题订阅中。

最灵活的一种支持订阅筛选器是**SqlFilter**，它实现的 SQL92 子集。 SQL 筛选器操作的属性发布到该主题的消息。 对于有关可以与 SQL 筛选中使用表达式的详细信息，请查看 SqlFilter.SqlExpression 语法。

下面的示例创建名为"HighMessages"与仅选择具有一个自定义的**MessageNumber**属性大于 3 消息**SqlFilter**订阅︰

    // Create a "HighMessages" filtered subscription  
    SubscriptionInfo subInfo = new SubscriptionInfo("HighMessages");
    CreateSubscriptionResult result =
        service.createSubscription("TestTopic", subInfo);
    RuleInfo ruleInfo = new RuleInfo("myRuleGT3");
    ruleInfo = ruleInfo.withSqlExpressionFilter("MessageNumber > 3");
    CreateRuleResult ruleResult =
        service.createRule("TestTopic", "HighMessages", ruleInfo);
    // Delete the default rule, otherwise the new rule won't be invoked.
    service.deleteRule("TestTopic", "HighMessages", "$Default");

同样，下面的示例创建仅选择已到 MessageNumber 属性小于或等于 3 的消息 SqlFilter 名为"LowMessages"的订阅︰

    // Create a "LowMessages" filtered subscription
    SubscriptionInfo subInfo = new SubscriptionInfo("LowMessages");
    CreateSubscriptionResult result =
        service.createSubscription("TestTopic", subInfo);
    RuleInfo ruleInfo = new RuleInfo("myRuleLE3");
    ruleInfo = ruleInfo.withSqlExpressionFilter("MessageNumber <= 3");
    CreateRuleResult ruleResult =
        service.createRule("TestTopic", "LowMessages", ruleInfo);
    // Delete the default rule, otherwise the new rule won't be invoked.
    service.deleteRule("TestTopic", "LowMessages", "$Default");


当一条消息，现在发送到"TestTopic"，它总是会发送到订阅了"AllMessages"主题订阅，并有选择地传送到接收器的接收器订阅的"HighMessages"和"LowMessages"（根据邮件内容） 的主题订阅。

## 如何将消息发送到的主题

若要将消息发送到服务总线主题，您的应用程序将获得一个**ServiceBusContract**对象。 下面的代码演示如何将我们在我们的"HowToSample"服务命名空间中创建上面的"TestTopic"主题消息发送︰

    BrokeredMessage message = new BrokeredMessage("MyMessage");
    service.sendTopicMessage("TestTopic", message);

消息发送到服务总线主题是**BrokeredMessage**类的实例。 **BrokeredMessage**对象有一套标准的方法 （如**setLabel**和**TimeToLive**），用来保存自定义应用程序特定的属性的字典和任意应用程序数据的主体。 应用程序可以通过将任何可序列化的对象传递到构造函数的**BrokeredMessage**，设置邮件正文中的，然后将使用适当的**DataContractSerializer**序列化对象。 或者，可以提供**java.io.InputStream** 。

下面的示例演示如何将五个测试消息发送到我们获得上述代码段中的"TestTopic" **MessageSender** 。
请注意每个消息的**MessageNumber**属性值如何在循环的迭代而异 （这将确定哪些订阅接收它）︰

    for (int i=0; i<5; i++)  {
        // Create message, passing a string message for the body
        BrokeredMessage message = new BrokeredMessage("Test message " + i);
        // Set some additional custom app-specific property
        message.setProperty("MessageNumber", i);
        // Send message to the topic
        service.sendTopicMessage("TestTopic", message);
    }

服务总线主题支持的最大邮件大小为 256 MB （头，其中包括标准和自定义应用程序属性，可以有最大为 64 MB）。 保留在主题中的消息的数量没有限制，但上持有的主题的消息的总大小没有一个盖帽。 此主题大小定义在创建时，具有 5 GB 的上限。

## 如何从订阅接收消息

从订阅接收邮件的主要方法是使用**ServiceBusContract**对象。 收到的邮件可以工作在两个不同的模式︰ **ReceiveAndDelete**和**PeekLock**。

当使用**ReceiveAndDelete**模式，收到是一拍摄操作的即当服务总线接收消息读取的请求，则它将邮件标记为正在使用并将其返回给应用程序。 **ReceiveAndDelete**模式是最简单的模型，最适合的方案，在其中一个应用程序能够容忍不处理出现故障的消息。 要理解这一点，考虑一个方案，其中使用者发出接收请求，然后处理它之前崩溃。 因为服务总线将被标记为正在使用的消息，然后当该应用程序将重新启动，并开始使用消息再次强调，它将丢失了消耗在崩溃之前的消息。

在**PeekLock**模式下，出现两个阶段运算，这样就有可能支持的应用程序不能容忍丢失邮件。 服务总线接收请求时，它查找下一条消息中，要消耗就锁定以防止其他使用者，接收它，，然后将它返回给应用程序。 应用程序完成处理消息 （或可靠地存储以备将来处理后），它会通过接收消息调用**删除**完成接收过程的第二个阶段。 当服务总线看到**删除**调用时，将标记该邮件所占用并删除该主题。

下面的示例演示消息可以接收和处理使用**PeekLock**模式 （不是默认模式） 的方式。 下面的示例执行一个循环，和处理在"HighMessages"订阅的消息，然后退出时没有更多消息 （或者，它可以设置要等待新的消息）。

    try
    {
        ReceiveMessageOptions opts = ReceiveMessageOptions.DEFAULT;
        opts.setReceiveMode(ReceiveMode.PEEK_LOCK);

        while(true)  {
            ReceiveSubscriptionMessageResult  resultSubMsg =
                service.receiveSubscriptionMessage("TestTopic", "HighMessages", opts);
            BrokeredMessage message = resultSubMsg.getValue();
            if (message != null && message.getMessageId() != null)
            {
                System.out.println("MessageID: " + message.getMessageId());
                // Display the topic message.
                System.out.print("From topic: ");
                byte[] b = new byte[200];
                String s = null;
                int numRead = message.getBody().read(b);
                while (-1 != numRead)
                {
                    s = new String(b);
                    s = s.trim();
                    System.out.print(s);
                    numRead = message.getBody().read(b);
                }
                System.out.println();
                System.out.println("Custom Property: " +
                    message.getProperty("MessageNumber"));
                // Delete message.
                System.out.println("Deleting this message.");
                service.deleteMessage(message);
            }  
            else  
            {
                System.out.println("Finishing up - no more messages.");
                break;
                // Added to handle no more messages.
                // Could instead wait for more messages to be added.
            }
        }
    }
    catch (ServiceException e) {
        System.out.print("ServiceException encountered: ");
        System.out.println(e.getMessage());
        System.exit(-1);
    }
    catch (Exception e) {
        System.out.print("Generic exception encountered: ");
        System.out.println(e.getMessage());
        System.exit(-1);
    }

## 如何处理应用程序崩溃和无法读取消息

服务总线提供的功能来帮助您从您的应用程序或困难处理消息中的错误正常恢复。 如果接收方应用程序不能处理该消息，由于某种原因，然后它可以接收消息 （而不是**deleteMessage**方法） 调用**unlockMessage**方法。 这将导致服务总线来解除锁定在主题中的邮件并使其可再次，同一个使用应用程序或其他使用的应用程序接收。

还有与主题内, 锁定超时，如果应用程序不能处理该消息之前锁定超时过期 （例如，如果应用程序崩溃时），则服务总线将自动解锁消息并使其可再次收到。

在该应用程序崩溃之后处理该消息，但**deleteMessage**请求发出之前，然后消息将被重新传递给应用程序重新启动时。 这通常称为**至少处理一次**，即将至少一次处理每个消息，但在某些情况下可能重新发送同样的消息。 如果方案不能容忍重复处理，应用程序开发人员应该向其应用程序来处理传递重复的邮件添加额外的逻辑。 这通常可以实现使用**getMessageId**方法将保持不变在传递尝试的邮件。

## 如何删除主题和订阅

要删除的主题和订阅的主要方法是使用**ServiceBusContract**对象。 删除一个主题时，将同时删除注册该主题的所有订阅。 也可以单独删除订阅。

    // Delete subscriptions
    service.deleteSubscription("TestTopic", "AllMessages");
    service.deleteSubscription("TestTopic", "HighMessages");
    service.deleteSubscription("TestTopic", "LowMessages");

    // Delete a topic
    service.deleteTopic("TestTopic");

## 下一步行动

现在，学服务总线队列的基础知识，请参阅 MSDN 主题[服务总线队列、 主题和订阅][]的详细信息。

  [对于 Java 的 azure SDK]: http://azure.microsoft.com/develop/java/
  [日蚀式的 azure Toolkit]: https://msdn.microsoft.com/en-us/library/azure/hh694271.aspx
  [什么是服务总线主题和订阅？]: #what-are-service-bus-topics
  [创建服务 Namespace]: #create-a-service-namespace
  [Namespace 获得的默认管理凭据]: #obtain-default-credentials
  [配置应用程序使用服务总线]: #bkmk_ConfigYourApp
  [如何︰ 创建主题]: #bkmk_HowToCreateTopic
  [如何︰ 创建订阅]: #bkmk_HowToCreateSubscrip
  [如何︰ 将消息发送到一个主题]: #bkmk_HowToSendMsgs
  [如何︰ 从订阅接收消息]: #bkmk_HowToReceiveMsgs
  [如何︰ 处理应用程序崩溃和无法读取消息]: #bkmk_HowToHandleAppCrash
  [如何︰ 删除主题和订阅]: #bkmk_HowToDeleteTopics
  [下一步行动]: #bkmk_NextSteps
  [服务总线主题图]: ../../../DevCenter/Java/Media/SvcBusTopics_01_FlowDiagram.jpg
  [Azure 的管理门户]: http://manage.windowsazure.com/
  [服务总线节点屏幕抓图]: ../../../DevCenter/dotNet/Media/sb-queues-03.png
  [创建新 Namespace ]: ../../../DevCenter/dotNet/Media/sb-queues-04.png
  [Namespace 列表屏幕抓图]: ../../../DevCenter/dotNet/Media/sb-queues-05.png
  [属性窗格中的屏幕快照]: ../../../DevCenter/dotNet/Media/sb-queues-06.png
  [默认密钥屏幕抓图]: ../../../DevCenter/dotNet/Media/sb-queues-07.png
  [服务总线队列、 主题和订阅]: http://msdn.microsoft.com/library/hh367516.aspx
 

---
ms.openlocfilehash: 304b18fc5aeffbab1dd1fa8d15a9e4dd0d38f03e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="如何使用服务总线队列 (Java) |Microsoft Azure"
    description="了解如何使用在 Azure 服务总线队列。 用 Java 编写的代码样本。"
    services="service-bus"
    documentationCenter="java"
    authors="sethmanheim"
    manager="timlt"
    />

<tags
    ms.service="service-bus"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="06/19/2015"
    ms.author="sethm"/>

# 如何使用服务总线队列

本指南介绍了如何使用服务总线队列。 这些示例用 Java 编写的并使用[Java 的 Azure SDK][]。 所包含的方案包括**创建队列**、**发送和接收邮件**和**删除队列**。

[AZURE.INCLUDE [service-bus-java-how-to-create-queue](../../includes/service-bus-java-how-to-create-queue.md)]

## 配置应用程序使用服务总线
请确保已安装[Java 的 Azure SDK][]之前生成此示例。 如果您正在使用 Eclipse，可以安装[Eclipse 的 Azure Toolkit][]包含 Java Azure SDK。 然后可以向项目添加**Microsoft Azure Java 库**︰
![](media/service-bus-java-how-to-use-queues/eclipselibs.png)

将下面的导入语句添加到 Java 文件的顶部︰

    // Include the following imports to use Service Bus APIs
    import com.microsoft.windowsazure.services.servicebus.*;
    import com.microsoft.windowsazure.services.servicebus.models.*;
    import com.microsoft.windowsazure.core.*;
    import javax.xml.datatype.*;

## 如何创建队列

通过**ServiceBusContract**类可以进行服务总线队列的管理操作。 **ServiceBusContract**对象构造与封装的 SAS 标记有权管理它，适当配置， **ServiceBusContract**类与 Azure 的通信的唯一点。

**ServiceBusService**类提供了创建、 枚举和删除队列的方法。 下面的示例演示如何使用**ServiceBusService**对象来创建队列名为"TestQueue"，一个名为"HowToSample"的命名空间︰

        Configuration config =
            ServiceBusConfiguration.configureWithSASAuthentication(
                    "HowToSample",
                    "RootManageSharedAccessKey",
                    "SAS_key_value",
                    ".servicebus.windows.net"
                    );

    ServiceBusContract service = ServiceBusService.create(config);
    QueueInfo queueInfo = new QueueInfo("TestQueue");
    try
    {
        CreateQueueResult result = service.createQueue(queueInfo);
    }
    catch (ServiceException e)
    {
        System.out.print("ServiceException encountered: ");
        System.out.println(e.getMessage());
        System.exit(-1);
    }

在 QueueInfo 上允许队列的属性以进行优化的方法 (例如︰ 设置默认"--生存时间"值将应用于发送到队列的消息)。 下面的示例演示如何创建名为"TestQueue"的最大大小为 5 GB 的队列︰

    long maxSizeInMegabytes = 5120;
    QueueInfo queueInfo = new QueueInfo("TestQueue");
    queueInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
    CreateQueueResult result = service.createQueue(queueInfo);

请注意，您可以使用**listQueues**方法对**ServiceBusContract**对象检查服务命名空间中是否已存在具有指定名称的队列。

## 如何将消息发送到队列

若要将消息发送到服务总线队列，您的应用程序将获得一个**ServiceBusContract**对象。 下面的代码演示如何将我们在我们的"HowToSample"服务命名空间中创建上面的"TestQueue"队列消息发送︰

    try
    {
        BrokeredMessage message = new BrokeredMessage("MyMessage");
        service.sendQueueMessage("TestQueue", message);
    }
    catch (ServiceException e)
    {
        System.out.print("ServiceException encountered: ");
        System.out.println(e.getMessage());
        System.exit(-1);
    }

邮件发送到的 （和从接收到的） 服务总线队列是**BrokeredMessage**类的实例。 **BrokeredMessage**对象有一套标准的方法 （如**getLabel**、 **getTimeToLive**、 **setLabel**和**setTimeToLive**），用来保存自定义应用程序特定的属性的字典和任意应用程序数据的主体。 应用程序可以通过将任何可序列化的对象传递到构造函数的**BrokeredMessage**，设置邮件正文中的，然后将使用适当的序列化程序将对象序列化。 另外， **java。IO。InputStream**可以提供。

下面的示例演示如何将五个测试消息发送到我们获得上述代码段中的"TestQueue" **MessageSender** :

    for (int i=0; i<5; i++)
    {
         // Create message, passing a string message for the body.
         BrokeredMessage message = new BrokeredMessage("Test message " + i);
         // Set an additional app-specific property.
         message.setProperty("MyProperty", i);
         // Send message to the queue
         service.sendQueueMessage("TestQueue", message);
    }

服务总线队列支持的最大邮件大小为 256 KB （头，其中包括标准和自定义应用程序属性，可以有的最大大小为 64 KB）。 保留在队列中的消息的数量没有限制，但持有队列消息的总大小没有一个盖帽。 此队列大小被定义在创建时，具有 5 GB 的上限。

## 如何从队列接收消息

从队列接收消息的主要方法是使用**ServiceBusContract**对象。 收到的邮件可以工作在两个不同的模式︰ **ReceiveAndDelete**和**PeekLock**。

当使用**ReceiveAndDelete**模式，收到是一拍摄操作的即当服务总线队列中接收消息读取的请求，则它将邮件标记为正在使用并将其返回给应用程序。 **ReceiveAndDelete**模式 （默认模式） 是最简单的模型，最适合的方案，在其中一个应用程序能够容忍不处理出现故障的消息。 要理解这一点，考虑一个方案，其中使用者发出接收请求，然后处理它之前崩溃。
因为服务总线将被标记为正在使用的消息，然后当该应用程序将重新启动，并开始使用消息再次强调，它将丢失了消耗在崩溃之前的消息。

在**PeekLock**模式下，出现两个阶段运算，这样就有可能支持的应用程序不能容忍丢失邮件。 服务总线接收请求时，它查找下一条消息中，要消耗就锁定以防止其他使用者，接收它，，然后将它返回给应用程序。 应用程序完成处理消息 （或可靠地存储以备将来处理后），它会通过接收消息调用**删除**完成接收过程的第二个阶段。 当服务总线看到**删除**调用时，将标记为所消耗的消息并从队列中移除。

下面的示例演示消息可以接收和处理使用**PeekLock**模式 （不是默认模式） 的方式。 下面的示例 does 无限循环，我们"TestQueue"在到达时处理消息︰

        try
    {
        ReceiveMessageOptions opts = ReceiveMessageOptions.DEFAULT;
        opts.setReceiveMode(ReceiveMode.PEEK_LOCK);

        while(true)  {
             ReceiveQueueMessageResult resultQM =
                    service.receiveQueueMessage("TestQueue", opts);
            BrokeredMessage message = resultQM.getValue();
            if (message != null && message.getMessageId() != null)
            {
                System.out.println("MessageID: " + message.getMessageId());
                // Display the queue message.
                System.out.print("From queue: ");
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
                    message.getProperty("MyProperty"));
                // Remove message from queue.
                System.out.println("Deleting this message.");
                //service.deleteMessage(message);
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

服务总线提供的功能来帮助您从您的应用程序或困难处理消息中的错误正常恢复。 如果接收方应用程序不能处理该消息，由于某种原因，然后它可以接收消息 （而不是**deleteMessage**方法） 调用**unlockMessage**方法。 这将导致服务总线来解锁队列中的邮件并使其可再次，同一个使用应用程序或其他使用的应用程序接收。

此外没有与消息在队列中，锁定超时，并且如果应用程序不能处理该消息之前锁定超时过期 （例如，如果应用程序崩溃时），则服务总线将自动解锁消息并使其可再次收到。

在该应用程序崩溃之后处理该消息，但**deleteMessage**请求发出之前，然后消息将被重新传递给应用程序重新启动时。 这通常称为**至少处理一次**，即将至少一次处理每个消息，但在某些情况下可能重新发送同样的消息。 如果方案不能容忍重复处理，应用程序开发人员应该向其应用程序来处理传递重复的邮件添加额外的逻辑。 这通常可以实现使用**getMessageId**方法将保持不变在传递尝试的邮件。

## 下一步行动

现在，学服务总线队列的基础知识，请参阅 MSDN 主题[队列、 主题和订阅][]的详细信息。

  [对于 Java 的 azure SDK]: http://azure.microsoft.com/develop/java/
  [日蚀式的 azure Toolkit]: https://msdn.microsoft.com/en-us/library/azure/hh694271.aspx
  [服务总线队列有哪些？]: #what-are-service-bus-queues
  [创建服务 Namespace]: #create-a-service-namespace
  [Namespace 获得的默认管理凭据]: #obtain-default-credentials
  [配置应用程序使用服务总线]: #bkmk_ConfigApp
  [如何︰ 创建安全令牌提供程序]: #bkmk_HowToCreateQueue
  [如何︰ 向队列发送消息]: #bkmk_HowToSendMsgs
  [如何︰ 从队列接收消息]: #bkmk_HowToReceiveMsgs
  [如何︰ 处理应用程序崩溃和无法读取消息]: #bkmk_HowToHandleAppCrashes
  [下一步行动]: #bkmk_NextSteps
  [Azure 的管理门户]: http://manage.windowsazure.com/
  [队列、 主题和订阅]: http://msdn.microsoft.com/library/windowsazure/hh367516.aspx
 

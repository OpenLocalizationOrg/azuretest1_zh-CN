---
ms.openlocfilehash: 1930e72c09c0c8924128b008cd41644562d11ea3
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="如何使用服务总线主题 (.NET) |Microsoft Azure"
    description="了解如何使用 Azure 服务总线主题和订阅。 为.NET 应用程序编写代码示例。"
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="07/02/2015"
    ms.author="sethm"/>

# 如何使用 Azure 服务总线主题和订阅

本文介绍如何使用服务总线主题和订阅。 示例在 C# 编写和使用.NET Api。 所包含的方案包括创建主题和创建订阅筛选器、 将邮件发送到某个主题、 接收来自订阅，邮件和删除的主题和订阅的订阅。 有关主题和订阅的详细信息，请参阅[后续步骤](#Next-steps)部分。

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## 配置应用程序使用服务总线

创建使用服务总线的应用程序时，必须添加到服务总线的程序集的引用，并包含相应的命名空间。

## 获取服务总线 NuGet 程序包

服务总线 NuGet 程序包是最简单的方法来获取服务总线 API 和服务总线的相关的所有配置应用程序。 NuGet Visual Studio 扩展很容易安装和更新库和工具 Visual Studio 和 Visual Studio 速成版中。 服务总线 NuGet 程序包是最简单的方法来获取服务总线 API 和服务总线的相关的所有配置应用程序。

要在您的应用程序安装 NuGet 程序包，请执行以下操作︰

1.  在解决方案资源管理器，右键单击**引用**，然后单击**管理 NuGet 程序包**。
2.  搜索"服务总线"并选择**Microsoft Azure 服务总线**项。 单击**安装**以完成安装，然后关闭下面的对话框。

    ![][7]

现在您可以为服务总线编写代码。

## 如何设置服务总线连接字符串

服务总线使用连接字符串将存储终结点和凭据。 可以放在配置文件中，而不是它进行硬编码的连接字符串︰

- 使用 Azure 云服务时，建议您将存储连接字符串使用 Azure 服务配置系统 （.csdef 和.cscfg 文件）。
- 当使用 Azure 网站或 Azure 的虚拟机，建议您将存储连接字符串使用.NET 配置系统 （例如，Web.config 文件）。

在这两种情况下，您可以检索连接字符串使用`CloudConfigurationManager.GetSetting`方法，如本文后面所示。

### 在使用云服务时配置您的连接字符串

服务配置机制是唯一的 Azure 云服务项目，并且使您能够动态地从 Azure 门户更改配置设置，而无需重新部署您的应用程序。 例如，添加`Setting`为服务定义的标签 (***.csdef**) 文件中，接下来的示例中所示。

    <ServiceDefinition name="Azure1">
    ...
        <WebRole name="MyRole" vmsize="Small">
            <ConfigurationSettings>
                <Setting name="Microsoft.ServiceBus.ConnectionString" />
            </ConfigurationSettings>
        </WebRole>
    ...
    </ServiceDefinition>

然后服务 (.cscfg) 在配置文件中指定了值。

    <ServiceConfiguration serviceName="Azure1">
    ...
        <Role name="MyRole">
            <ConfigurationSettings>
                <Setting name="Microsoft.ServiceBus.ConnectionString"
                         value="Endpoint=sb://yourServiceNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey" />
            </ConfigurationSettings>
        </Role>
    ...
    </ServiceConfiguration>

使用共享访问签名 (SAS) 键名称和键值检索从 Azure 门户上一节中所述。

### 当使用 Azure 网站或 Azure 的虚拟机配置您的连接字符串

在网站或虚拟机使用，建议使用.NET 配置系统 (例如，Web.config)。 您将存储连接字符串使用`<appSettings>`元素。

    <configuration>
        <appSettings>
            <add key="Microsoft.ServiceBus.ConnectionString"
                 value="Endpoint=sb://yourServiceNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey" />
        </appSettings>
    </configuration>

上一节中所述使用 SAS 名称和您从 Azure 的门户网站，检索到的密钥值。

## 如何创建主题

您可以执行管理操作的服务总线主题并使用订阅[`NamespaceManager`](https://msdn.microsoft.com/library/microsoft.servicebus.namespacemanager.aspx)类。 此类提供用于创建、 枚举和删除的主题。

下面的示例构造`NamespaceManager`对象使用 Azure`CloudConfigurationManager`类的服务总线服务命名空间和适当的 SA 的基址所组成的连接字符串凭据有权管理它。 此连接字符串是以下形式。

    Endpoint=sb://<yourServiceNamespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey

使用以下示例中，指定在上一节中的配置设置。

    // Create the topic if it does not exist already.
    string connectionString =
        CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

    var namespaceManager =
        NamespaceManager.CreateFromConnectionString(connectionString);

    if (!namespaceManager.TopicExists("TestTopic"))
    {
        namespaceManager.CreateTopic("TestTopic");
    }

有[`CreateTopic`](https://msdn.microsoft.com/library/microsoft.servicebus.namespacemanager.createtopic.aspx)方法，您要调整属性的主题，例如，若要设置默认"--生存时间"值将应用于发送到该主题的消息。 通过应用这些设置[`TopicDescription`](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.topicdescription.aspx)类。 下面的示例演示如何创建名为 TestTopic 的最大大小为 5 GB 和默认消息-生存时间为 1 分钟的主题。

    // Configure Topic Settings.
    TopicDescription td = new TopicDescription("TestTopic");
    td.MaxSizeInMegabytes = 5120;
    td.DefaultMessageTimeToLive = new TimeSpan(0, 1, 0);

    // Create a new Topic with custom settings.
    string connectionString =
        CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

    var namespaceManager =
        NamespaceManager.CreateFromConnectionString(connectionString);

    if (!namespaceManager.TopicExists("TestTopic"))
    {
        namespaceManager.CreateTopic(td);
    }

> [AZURE.NOTE] 您可以使用[`TopicExists`](https://msdn.microsoft.com/library/microsoft.servicebus.namespacemanager.topicexists.aspx)方法在[`NamespaceManager`](https://msdn.microsoft.com/library/microsoft.servicebus.namespacemanager.aspx)对象来检查服务命名空间中是否已存在具有指定名称的主题。

## 如何创建订阅

您还可以创建使用主题订阅[`NamespaceManager`](https://msdn.microsoft.com/library/microsoft.servicebus.namespacemanager.aspx)类。 被命名为订阅，并且可以具有可选的筛选，用于限制的消息传递给订阅的虚拟队列集。

### 使用默认值 (MatchAll) 筛选器创建订阅

**MatchAll**过滤器是如果没有筛选器指定创建新订阅时使用的默认筛选器。 当您使用**MatchAll**筛选时，发布到该主题的所有邮件都放预订的虚拟队列中。 下面的示例创建一个名为"AllMessages"的订阅，并使用默认**MatchAll**筛选器。

    string connectionString =
        CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

    var namespaceManager =
        NamespaceManager.CreateFromConnectionString(connectionString);

    if (!namespaceManager.SubscriptionExists("TestTopic", "AllMessages"))
    {
        namespaceManager.CreateSubscription("TestTopic", "AllMessages");
    }

### 使用筛选器创建订阅

您还可以设置筛选器，使您可以指定哪些消息发送到一个主题应该出现在特定主题订阅。

最灵活的一种支持订阅筛选器是[SqlFilter]类中，实现 SQL92 的子集。 SQL 筛选器操作的属性发布到该主题的消息。 有关可与 SQL 筛选中使用表达式的详细信息，请参阅[SqlFilter.SqlExpression][]语法。

下面的示例创建一个[SqlFilter]对象，该对象仅选择具有一个自定义的**MessageNumber**属性大于 3 的消息，名为**HighMessages**的订阅。

     // Create a "HighMessages" filtered subscription.
     SqlFilter highMessagesFilter =
        new SqlFilter("MessageNumber > 3");

     namespaceManager.CreateSubscription("TestTopic",
        "HighMessages",
        highMessagesFilter);

同样，下面的示例创建名为**LowMessages**与[SqlFilter] ，只选择具有到**MessageNumber**属性小于或等于 3 的消息的订阅。

     // Create a "LowMessages" filtered subscription.
     SqlFilter lowMessagesFilter =
        new SqlFilter("MessageNumber <= 3");

     namespaceManager.CreateSubscription("TestTopic",
        "LowMessages",
        lowMessagesFilter);

现在一条消息发送到`TestTopic`，它总是发送到接收器的**AllMessages**主题订阅，订阅和有选择地传送到接收方 （取决于消息内容） 的**HighMessages**和**LowMessages**主题订阅订阅。

## 如何将消息发送到的主题

若要将消息发送到服务总线主题，您的应用程序创建[`TopicClient`](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx)对象使用的连接字符串。

下面的代码演示如何创建[`TopicClient`](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.topicclient.aspx) **TestTopic**主题前面使用创建对象[`CreateFromConnectionString`](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.topicclient.createfromconnectionstring.aspx)API 调用。

    string connectionString =
        CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

    TopicClient Client =
        TopicClient.CreateFromConnectionString(connectionString, "TestTopic");

    Client.Send(new BrokeredMessage());


消息发送到服务总线主题是的实例[`BrokeredMessage`](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx)类。 **BrokeredMessage**对象具有标准属性的一组 (如[`Label`](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx)， [`TimeToLive`](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx))、 字典，用来保存自定义应用程序特定的属性，和任意应用程序数据的主体。 应用程序可以通过将任何可序列化的对象传递给的构造函数来设置邮件正文[`BrokeredMessage`](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx)对象，并且相应的**DataContractSerializer**则用于序列化对象。 或者，可以提供**System.IO.Stream** 。

下面的示例演示如何发送五个测试邮件到**TestTopic** [`TopicClient`](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx)在前面的代码示例获取对象。 请注意，[`MessageNumber`](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.properties.aspx)的每个消息的属性值取决于循环的迭代 （确定哪些订阅接收它）。

     for (int i=0; i<5; i++)
     {
       // Create message, passing a string message for the body.
       BrokeredMessage message =
        new BrokeredMessage("Test message " + i);

       // Set additional custom app-specific property.
       message.Properties["MessageNumber"] = i;

       // Send message to the topic.
       Client.Send(message);
     }

服务总线主题支持[的最大邮件大小为 256 KB](service-bus-quotas.md) （头，其中包括标准和自定义应用程序属性，可以有的最大大小为 64 KB）。 保留在主题中的消息的数量没有限制，但上持有的主题的消息的总大小没有一个盖帽。 此主题大小定义在创建时，具有 5 GB 的上限。 如果启用分区，则上限就是更高。 有关详细信息，请参阅[分区消息处理实体](https://msdn.microsoft.com/library/azure/dn520246.aspx)。

## 如何从订阅接收消息

从订阅接收消息的推荐的方式是使用[`SubscriptionClient`](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.aspx)对象。 **SubscriptionClient**对象可以工作在两个不同的模式︰[`ReceiveAndDelete`和`PeekLock`](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx)。

当使用**ReceiveAndDelete**模式，收到是一拍摄操作的即当服务总线在订阅接收读取的请求消息，则它将邮件标记为正在使用并将其返回给应用程序。 **ReceiveAndDelete**模式是最简单的模型，最适合的方案，在其中一个应用程序能够容忍不处理出现故障的消息。 要理解这一点，考虑一个方案，其中使用者发出接收请求，然后处理它之前崩溃。 因为服务总线将已标记邮件，随着消耗，当应用程序重新启动并开始再次使用消息时，它将错过被消耗在崩溃之前的消息。

在**PeekLock**模式下 （这是默认的模式），接收进程变为两阶段操作，这样就有可能支持的应用程序不能容忍丢失邮件。 服务总线接收请求时，它查找下一条消息中，要消耗就锁定以防止其他使用者，接收它，，然后将它返回给应用程序。 通过调用应用程序完成处理消息 （或可靠地存储以备将来处理后），完成第二阶段接收过程的[`Complete`](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx)上接收到的消息。 当服务总线可以看到`Complete`调用，它将邮件标记为正在消耗并从预订中移除。

下面的示例演示消息可以接收和处理使用默认的**PeekLock**模式的方式。 若要指定不同的[`ReceiveMode`](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx)值，您可以使用另一重载为[`CreateFromConnectionString`](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.createfromconnectionstring.aspx)。 此示例使用[`OnMessage`](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.onmessage.aspx)到达**HighMessages**订阅中时处理消息的回调。

    string connectionString =
        CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

    SubscriptionClient Client =
        SubscriptionClient.CreateFromConnectionString
                (connectionString, "TestTopic", "HighMessages");

    // Configure the callback options.
    OnMessageOptions options = new OnMessageOptions();
    options.AutoComplete = false;
    options.AutoRenewTimeout = TimeSpan.FromMinutes(1);

    Client.OnMessage((message) =>
    {
        try
        {
            // Process message from subscription.
            Console.WriteLine("\n**High Messages**");
            Console.WriteLine("Body: " + message.GetBody<string>());
            Console.WriteLine("MessageID: " + message.MessageId);
            Console.WriteLine("Message Number: " +
                message.Properties["MessageNumber"]);

            // Remove message from subscription.
            message.Complete();
        }
        catch (Exception)
        {
            // Indicates a problem, unlock message in subscription.
            message.Abandon();
        }
    }, options);

本示例配置[`OnMessage`](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.onmessage.aspx)回调使用[`OnMessageOptions`](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.aspx)对象。 [`AutoComplete`](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.autocomplete.aspx)设置为**false**以启用手动控制何时调用[`Complete`](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx)上接收到的消息。 [`AutoRenewTimeout`](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.autorenewtimeout.aspx)设置为 1 分钟，这会导致客户端等待一分钟的时间的一条消息之前调用超时，客户端发出新的调用来检查的消息。 此属性值可以减少客户端无法检索邮件的收费电话的次数。

## 如何处理应用程序崩溃和无法读取消息

服务总线提供的功能来帮助您从您的应用程序或困难处理消息中的错误正常恢复。 如果接收应用程序不能处理该消息，由于某种原因，那么它可以调用[`Abandon`](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.abandon.aspx)所接收消息的方法 (而不是[`Complete`](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx)方法)。 这将导致服务总线来解锁内订阅消息并使其可再次，同一个使用应用程序或其他使用的应用程序接收。

还有与内订阅，锁定超时，如果应用程序不能处理该消息之前锁定超时时间已到 （例如，如果应用程序崩溃时），则服务总线自动解锁消息并使其可供再次收到。

在该应用程序崩溃之前处理该消息之后[`Complete`](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx)发出请求，在它重新启动时，将向应用程序重新传送的消息。 这通常称为**至少一次处理**;即至少一次处理每个消息，但在某些情况下可能重新发送同样的消息。 如果方案不能容忍重复处理，应用程序开发人员应该向其应用程序来处理传递重复的邮件添加额外的逻辑。 这通常实现使用[`MessageId`](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx)属性将保持不变在传递尝试的邮件。

## 如何删除主题和订阅

下面的示例演示如何从**HowToSample**服务命名空间中删除**TestTopic**主题。

     // Delete Topic.
     namespaceManager.DeleteTopic("TestTopic");

删除某个主题也会删除注册该主题的所有订阅。 也可以单独删除订阅。 下面的代码演示如何删除名为**HighMessages** ，从**TestTopic**主题的订阅。

      namespaceManager.DeleteSubscription("TestTopic", "HighMessages");

## 下一步行动

现在，学服务总线主题和订阅的基本知识，按照这些链接以了解详细信息。

-   请参阅 MSDN 参考︰[队列、 主题和订阅][]。
-   [SqlFilter][]的 API 参考。
-   建立的工作应用程序发送和接收消息的服务总线队列与︰[服务总线通过代理消息.NET 教程][]。
-   服务总线示例︰ 从[Azure 示例][]下载，或请参阅[MSDN][]上的概述。

  [Azure 门户]: http://manage.windowsazure.com

  [7]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/getting-started-multi-tier-13.png

  [队列、 主题和订阅]: http://msdn.microsoft.com/library/hh367516.aspx
  [SqlFilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx
  [SqlFilter.SqlExpression]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
  [通过代理进行服务总线消息.NET 教程]: http://msdn.microsoft.com/library/azure/hh367512.aspx
  [Azure 的示例]: https://code.msdn.microsoft.com/windowsazure/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=2
  [MSDN]: https://msdn.microsoft.com/library/azure/dn194201.aspx

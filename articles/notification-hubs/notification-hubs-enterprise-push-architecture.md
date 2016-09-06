---
ms.openlocfilehash: 3c283be83cef547e7d89c6e987c9c83400dbd21e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="通知集线器的企业推式体系结构"
    description="在企业环境中使用 Azure 通知集线器指南"
    services="notification-hubs"
    documentationCenter=""
    authors="wesmc7777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/18/2015" 
    ms.author="wesmc"/>

# 企业推式体系结构指南

企业现在正在逐渐移向创建任何一个移动应用程序最终用户 （外部） 或 （内部） 的员工。 它们具有现有后端系统中的位置是大型机还是某些 LoB 应用程序，它必须集成到移动应用程序体系结构。 本指南将讨论如何最好地执行这一集成常见场景建议可能的解决方案。

频繁的需求是后端系统中发生的感兴趣事件时用户可以通过其移动应用程序发送推式通知。 例如 在她的 iPhone 有银行的银行业务应用程序的银行客户希望借由上面一定从她的帐户或内联网方案预算批准应用程序对其 Windows Phone 从财务部门的员工希望得到通知，当他收到审批请求时得到通知。

银行帐户或批准过程很可能在一些后端系统，这样就必须进行向用户推送完成。 可能有多个这种后端系统的所有必须生成相同类型的逻辑来实现推送，当事件触发通知。 这里的复杂性就在于集成多个后端单个推送系统最终用户可能具有不同的通知订阅，并可能甚至有多个移动应用程序，例如在 intranet 移动应用程序的情况下一个移动应用程序可能需要接收来自多个这样的后端系统的通知。 不了解或不需要知道的推式语义/技术，因此常见的解决方案传统上已被引入的轮询后端系统的感兴趣的任何事件，并负责发送推送到客户端的组件的后端系统。
这里我们将讨论使用 Azure 服务总线的主题中的订阅模型，这将降低复杂性，同时使可伸缩的解决方案更好的解决方案。

下面是该解决方案的一般体系结构 （与多个移动应用程序的通用，但同样适用，只有一个移动应用程序时）

## 体系结构

![][1]

在此体系结构示意图中的一项关键是 Azure 服务总线提供编程模型 （详细介绍它在[服务总线 Pub/Sub 编程]） 的主题/订阅。 接收方，在这种情况下，为移动后端 （通常[Azure 手机信息服务]，这将启动推到移动应用程序） 不直接从后端系统接收邮件，但相反，我们必须使移动的后端以从一个或多个后端系统接收消息的[Azure 服务总线]提供一个中间的抽象层。 服务总线主题需要创建的每个后端系统如帐户，人力资源、 财务基本上是感兴趣，这将启动以推式通知的形式发送的邮件的"主题"。 后端系统将发送消息到这些主题。 移动后端可以通过创建服务总线的订阅订阅一个或多个这样的主题。 这会有移动的后端从相应的后端系统接收的通知。 移动后端继续侦听其订阅消息，一旦消息到达时，它将变回并将其发送到它的通知中心的通知为。 通知集线器然后最终将邮件传递到移动应用程序。 若要汇总的关键组件，我们有︰

1. 后端系统 （LoB/旧系统）
    - 创建服务总线主题
    - 发送消息
2. 移动后端
    - 创建服务预订
    - （从后端系统） 接收消息
    - 将通知发送到客户端 （通过 Azure 通知中心）
3. 移动应用程序
    - 接收并显示通知

###有以下好处︰

1. 接收方 （移动应用程序/服务通过通知中心） 和发件人 （后端系统） 之间的分离使其他后端系统集成与最小的变更。
2. 这也是多个移动的应用程序能够从一个或多个后端系统接收事件的方案。  

## 示例︰

###先决条件
请完成下面的教程，以熟悉的概念，以及常见的创建和配置步骤︰

1. [服务总线 Pub/Sub 编程]的详细工作与服务总线主题/预订，这说明如何如何创建命名空间可以包含主题/订阅，如何发送和接收来自它们的消息。
2. [通知集线器-Windows 通用教程]-这可以说明如何设置 Windows 应用商店应用程序和通知集线器用于注册和再收到通知。

###示例代码

[通知中心示例]提供了完整的示例代码。 它分为三个组件︰

1. **EnterprisePushBackendSystem**

    一。 此项目时使用*WindowsAzure.ServiceBus* Nuget 程序包，取决于[服务总线 Pub/Sub 编程]。

    b。 这是一个简单 C# 控制台应用程序来模拟一个 LoB 系统启动的消息发送到移动应用程序的。

        static void Main(string[] args)
        {
            string connectionString =
                CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

            // Create the topic where we will send notifications
            CreateTopic(connectionString);

            // Send message
            SendMessage(connectionString);
        }

    c。 `CreateTopic` 用于创建服务总线主题，我们将发送消息。

        public static void CreateTopic(string connectionString)
        {
            // Create the topic if it does not exist already

            var namespaceManager =
                NamespaceManager.CreateFromConnectionString(connectionString);

            if (!namespaceManager.TopicExists(sampleTopic))
            {
                namespaceManager.CreateTopic(sampleTopic);
            }
        }

    d。 `SendMessage` 用于将消息发送到此服务总线主题。 这里我们只正在发送一的组随机消息定期来此示例的主题。 通常会有事件发生时，将发送消息的后端系统。

        public static void SendMessage(string connectionString)
        {
            TopicClient client =
                TopicClient.CreateFromConnectionString(connectionString, sampleTopic);

            // Sends random messages every 10 seconds to the topic
            string[] messages =
            {
                "Employee Id '{0}' has joined.",
                "Employee Id '{0}' has left.",
                "Employee Id '{0}' has switched to a different team."
            };

            while (true)
            {
                Random rnd = new Random();
                string employeeId = rnd.Next(10000, 99999).ToString();
                string notification = String.Format(messages[rnd.Next(0,messages.Length)], employeeId);

                // Send Notification
                BrokeredMessage message = new BrokeredMessage(notification);
                client.Send(message);

                Console.WriteLine("{0} Message sent - '{1}'", DateTime.Now, notification);

                System.Threading.Thread.Sleep(new TimeSpan(0, 0, 10));
            }
        }

2. **ReceiveAndSendNotification**

    一。 此项目使用的*WindowsAzure.ServiceBus*和*Microsoft.Web.WebJobs.Publish* Nuget 程序包和基于[服务总线 Pub/Sub 编程]。

    b。 这是另一个 C# 控制台应用程序我们将作为运行的[Azure WebJob]因为它具有连续运行侦听来自 LoB/后端系统的消息。 这将是您移动的后端的一部分。

        static void Main(string[] args)
        {
            string connectionString =
                     CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

            // Create the subscription which will receive messages
            CreateSubscription(connectionString);

            // Receive message
            ReceiveMessageAndSendNotification(connectionString);
        }

    c。 `CreateSubscription` 用于创建服务总线预订主题后, 端系统将发送消息。 此组件将根据业务方案中，创建一个或多个订阅 （例如某些可能会收到邮件从 HR 系统中，有些是从金融系统等等） 的相应主题

        static void CreateSubscription(string connectionString)
        {
            // Create the subscription if it does not exist already
            var namespaceManager =
                NamespaceManager.CreateFromConnectionString(connectionString);

            if (!namespaceManager.SubscriptionExists(sampleTopic, sampleSubscription))
            {
                namespaceManager.CreateSubscription(sampleTopic, sampleSubscription);
            }
        }

    d。 ReceiveMessageAndSendNotification 用于从使用其订阅的主题阅读邮件和如果成功读取然后手工创建 （在 Windows 本机 toast 通知示例方案） 的通知发送到使用 Azure 通知集线器的移动应用程序。

        static void ReceiveMessageAndSendNotification(string connectionString)
        {
            // Initialize the Notification Hub
            string hubConnectionString = CloudConfigurationManager.GetSetting
                    ("Microsoft.NotificationHub.ConnectionString");
            hub = NotificationHubClient.CreateClientFromConnectionString
                    (hubConnectionString, "enterprisepushservicehub");

            SubscriptionClient Client =
                SubscriptionClient.CreateFromConnectionString
                        (connectionString, sampleTopic, sampleSubscription);

            Client.Receive();

            // Continuously process messages received from the subscription
            while (true)
            {
                BrokeredMessage message = Client.Receive();
                var toastMessage = @"<toast><visual><binding template=""ToastText01""><text id=""1"">{messagepayload}</text></binding></visual></toast>";

                if (message != null)
                {
                    try
                    {
                        Console.WriteLine(message.MessageId);
                        Console.WriteLine(message.SequenceNumber);
                        string messageBody = message.GetBody<string>();
                        Console.WriteLine("Body: " + messageBody + "\n");

                        toastMessage = toastMessage.Replace("{messagepayload}", messageBody);
                        SendNotificationAsync(toastMessage);

                        // Remove message from subscription
                        message.Complete();
                    }
                    catch (Exception)
                    {
                        // Indicate a problem, unlock message in subscription
                        message.Abandon();
                    }
                }
            }
        }
        static async void SendNotificationAsync(string message)
        {
            await hub.SendWindowsNativeNotificationAsync(message);
        }

    电子。 作为**WebJob**中发布此，右击在 Visual Studio 解决方案并选择**发布以 WebJob**

    ![][2]

    f。 选择您的发布配置文件，如果它不存在已它将承载此 WebJob，网站则**发布**之后创建新的 Azure 网站。

    ![][3]

    g。 配置作业为"连续运行"，这样当您登录到 Azure 管理门户中应类似如下︰

    ![][4]


3. **EnterprisePushMobileApp**

    一。 这是一个 Windows 应用商店应用程序将接收来自作为您移动的后端的一部分运行 WebJob toast 通知并将其显示出来。 这根据[通知集线器-Windows 通用教程]。  

    b。 确保您的应用程序已启用接收 toast 通知。

    c。 确保以下通知集线器注册代码正在调用该应用程序在启动 (后替换的*HubName*和*DefaultListenSharedAccessSignature*:

        private async void InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            var hub = new NotificationHub("[HubName]", "[DefaultListenSharedAccessSignature]");
            var result = await hub.RegisterNativeAsync(channel.Uri);

            // Displays the registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }

### 运行示例︰

1. 确保您 WebJob 成功运行，并计划为"连续运行"。
2. 运行**EnterprisePushMobileApp**将启动 Windows 应用商店应用程序。
3. 运行**EnterprisePushBackendSystem**控制台应用程序将模拟 LoB 后端，并将开始发送消息，您应该会看到 toast 通知出现如下所示︰

    ![][5]

4. 服务总线主题所监视的服务总线订阅 Web 作业中最初发送邮件了。 一旦收到一条消息，通知已创建并发送到移动应用程序。 您可以查看通过 WebJob 日志以确认时转到 Azure 管理门户 Web 作业的日志链接进行处理︰

    ![][6]

<!-- Images -->
[1]: ./media/notification-hubs-enterprise-push-architecture/architecture.png
[2]: ./media/notification-hubs-enterprise-push-architecture/WebJobsContextMenu.png
[3]: ./media/notification-hubs-enterprise-push-architecture/PublishAsWebJob.png
[4]: ./media/notification-hubs-enterprise-push-architecture/WebJob.png
[5]: ./media/notification-hubs-enterprise-push-architecture/Notifications.png
[6]: ./media/notification-hubs-enterprise-push-architecture/WebJobsLog.png

<!-- Links -->
[通知中心示例]: https://github.com/Azure/azure-notificationhubs-samples
[Azure 的移动服务]: http://azure.microsoft.com/documentation/services/mobile-services/
[Azure 服务总线]: http://azure.microsoft.com/documentation/articles/fundamentals-service-bus-hybrid-solutions/
[服务总线 Pub/Sub 编程]: http://azure.microsoft.com/documentation/articles/service-bus-dotnet-how-to-use-topics-subscriptions/
[Azure WebJob]: http://azure.microsoft.com/documentation/articles/web-sites-create-web-jobs/
[通知集线器-Windows 通用教程]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/

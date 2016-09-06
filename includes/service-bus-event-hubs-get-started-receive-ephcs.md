---
ms.openlocfilehash: d2c9f8d6445baab20d944e13515e7e9960e05e70
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
## 接收邮件 EventProcessorHost

[EventProcessorHost]是一个.NET 类，简化了接收事件从事件集线器通过持续的管理检查点和并行接收来自这些事件集线器。 使用[EventProcessorHost]，可以拆分事件跨多个接收器，即使承载在不同的节点中。 此示例演示如何为单个接收器使用[EventProcessorHost] 。 [成比例出事件处理]示例演示如何使用具有多个接收器的[EventProcessorHost] 。

为了使用[EventProcessorHost]，您必须[Azure 存储帐户]︰

1. 登录到[Azure 的门户网站]，然后单击**新建**，在屏幕的底部。

2. **数据服务**，**存储**，然后单击**快速创建**，请单击，然后键入您的存储帐户的名称。 选择您需要的区域，然后单击**创建存储帐户**。

    ![][11]

3. 单击新创建的存储帐户，然后单击**管理访问键**︰

    ![][12]

    复制访问密钥使用本教程的后面。

4. 在 Visual Studio 中，创建一个新的视觉 C# 桌面应用程序项目使用**控制台应用程序**项目模板。 **接收方**为项目命名。

    ![][14]

5. 在解决方案资源管理器中，右击解决方案，然后单击**管理 NuGet 程序包**。

    出现**管理 NuGet 程序包**对话框。

6. 搜索`Microsoft Azure Service Bus Event Hub - EventProcessorHost`，单击**安装**，并接受使用条款。

    ![][13]

    此下载，安装，并将添加到[Azure 服务总线事件中枢-EventProcessorHost NuGet 程序包](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost)，与其依赖项的引用。

7. 用鼠标右键单击**接收器**项目，单击**添加**，然后单击**类**。 命名为**SimpleEventProcessor**，该新类，然后单击**确定**以创建类。

8. 在 SimpleEventProcessor.cs 文件的顶部添加以下语句︰

        using Microsoft.ServiceBus.Messaging;
        using System.Diagnostics;
        using System.Threading.Tasks;

    然后，用替代类主体的以下代码︰

        class SimpleEventProcessor : IEventProcessor
        {
            Stopwatch checkpointStopWatch;

            async Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
            {
                Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
                if (reason == CloseReason.Shutdown)
                {
                    await context.CheckpointAsync();
                }
            }

            Task IEventProcessor.OpenAsync(PartitionContext context)
            {
                Console.WriteLine("SimpleEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);
                this.checkpointStopWatch = new Stopwatch();
                this.checkpointStopWatch.Start();
                return Task.FromResult<object>(null);
            }

            async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
            {
                foreach (EventData eventData in messages)
                {
                    string data = Encoding.UTF8.GetString(eventData.GetBytes());

                    Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
                        context.Lease.PartitionId, data));
                }

                //Call checkpoint every 5 minutes, so that worker can resume processing from the 5 minutes back if it restarts.
                if (this.checkpointStopWatch.Elapsed > TimeSpan.FromMinutes(5))
                {
                    await context.CheckpointAsync();
                    this.checkpointStopWatch.Restart();
                }
            }
        }

    将通过处理事件从事件中心收到的**EventProcessorHost**调用此类。 请注意，`SimpleEventProcessor`类使用秒表来定期**EventProcessorHost**上下文上调用检查点方法。 这样可以确保，如果接收端重新启动时，它将丢失的处理工作不能超过五分钟。

9. 在**程序**类中，添加以下`using`顶部的语句︰

        using Microsoft.ServiceBus.Messaging;
        using Microsoft.Threading;
        using System.Threading.Tasks;

    然后，修改**程序**类的**Main**方法，如下所示，替代事件中心名称和连接字符串和存储帐户和在前面的部分中复制的注册表项︰

        static void Main(string[] args)
        {
          string eventHubConnectionString = "{event hub connection string}";
          string eventHubName = "{event hub name}";
          string storageAccountName = "{storage account name}";
          string storageAccountKey = "{storage account key}";
          string storageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}",
              storageAccountName, storageAccountKey);

          string eventProcessorHostName = Guid.NewGuid().ToString();
          EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, eventHubName, EventHubConsumerGroup.DefaultGroupName, eventHubConnectionString, storageConnectionString);
          Console.WriteLine("Registering EventProcessor...");
          eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>().Wait();

          Console.WriteLine("Receiving. Press enter key to stop worker.");
          Console.ReadLine();
          eventProcessorHost.UnregisterEventProcessorAsync().Wait();
        }

> [AZURE.NOTE] 本教程使用了[EventProcessorHost]的一个实例。 为了提高吞吐量，建议您运行多个实例的[EventProcessorHost]，[成比例出事件处理]示例中所示。 在这些情况下，各种实例自动协调彼此为了负载平衡接收的事件。 如果您希望多个接收器连接到每个进程*所有*事件，则必须使用**ConsumerGroup**概念。 在不同的计算机接收事件，可能有助于为基于计算机 （或角色） 的[EventProcessorHost]实例中部署它们指定名称。 有关这些主题的详细信息，请参阅[事件集线器概述]和[事件集线器编程指南]主题。

<!-- Links -->
[事件集线器概述]: event-hubs-overview.md
[扩容事件处理]: https://code.msdn.microsoft.com/windowsazure/Service-Bus-Event-Hub-45f43fc3
[Azure 存储帐户]: storage-create-storage-account.md
[EventProcessorHost]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost(v=azure.95).aspx
[Azure 门户]: http://manage.windowsazure.com

<!-- Images -->

[11]: ./media/service-bus-event-hubs-getstarted/create-eph-csharp2.png
[12]: ./media/service-bus-event-hubs-getstarted/create-eph-csharp3.png
[13]: ./media/service-bus-event-hubs-getstarted/create-eph-csharp1.png
[14]: ./media/service-bus-event-hubs-getstarted/create-sender-csharp1.png

[事件集线器编程指南]: event-hubs-programming-guide.md
[异步等待在控制台应用程序]: http://blogs.msdn.com/b/pfxteam/archive/2012/01/20/10259049.aspx
[AsyncPump.cs]: http://blogs.msdn.com/cfs-file.ashx/__key/communityserver-components-postattachments/00-10-25-90-49/AsyncPump_2E00_cs

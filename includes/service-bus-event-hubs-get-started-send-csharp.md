---
ms.openlocfilehash: 973ab5441742aa7063ee6fe6491377d8317e826f
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
## 将消息发送到事件集线器

在本节中，您将编写 Windows 控制台应用程序将事件发送到事件中心。

1. 在 Visual Studio 中，创建一个新的视觉 C# 桌面应用程序项目使用**控制台应用程序**项目模板。 **发件人**为项目命名。

    ![][7]

2. 在解决方案资源管理器中，右击解决方案，，然后单击**解决方案...管理 NuGet 程序包**。 

    这将显示管理 NuGet 程序包对话框。

3. 搜索`Microsoft Azure Service Bus`，单击**安装**，并接受使用条款。 

    ![][8]

    此下载、 安装，并将引用添加到<a href="https://www.nuget.org/packages/WindowsAzure.ServiceBus/">Azure 服务总线库 NuGet 程序包</a>。

4. 添加以下`using` **Program.cs**文件顶部的语句︰

        using System.Threading;
        using Microsoft.ServiceBus.Messaging;

5. 替换占位符值与您在上一节中创建事件中心的名称和连接字符串**发送**权限的**程序**类中加入以下字段︰

        static string eventHubName = "{event hub name}";
        static string connectionString = "{send connection string}";

6. 将下面的方法添加到**程序**类︰

        static void SendingRandomMessages()
        {
            var eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, eventHubName);
            while (true)
            {
                try
                {
                    var message = Guid.NewGuid().ToString();
                    Console.WriteLine("{0} > Sending message: {1}", DateTime.Now, message);
                    eventHubClient.Send(new EventData(Encoding.UTF8.GetBytes(message)));
                }
                catch (Exception exception)
                {
                    Console.ForegroundColor = ConsoleColor.Red;
                    Console.WriteLine("{0} > Exception: {1}", DateTime.Now, exception.Message);
                    Console.ResetColor();
                }

                Thread.Sleep(200);
            }
        }

    此方法不断地将事件发送到 200 毫秒延迟事件网络集线器。

7. 最后，将以下行添加到**Main**方法︰

        Console.WriteLine("Press Ctrl-C to stop the sender process");
        Console.WriteLine("Press Enter to start now");
        Console.ReadLine();
        SendingRandomMessages();


<!-- Images -->
[7]: ./media/service-bus-event-hubs-getstarted/create-sender-csharp1.png
[8]: ./media/service-bus-event-hubs-getstarted/create-sender-csharp2.png
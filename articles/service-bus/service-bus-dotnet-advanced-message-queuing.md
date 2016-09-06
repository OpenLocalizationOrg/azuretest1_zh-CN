---
ms.openlocfilehash: c4bc9826a3700c35a40bba1142219853e2cc667a
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何使用.NET 服务总线 API AMQP 1.0 |Microsoft Azure" 
    description="了解如何使用 Azure.NET 服务总线 API 使用高级消息队列 Protodol (AMQP) 1.0。" 
    services="service-bus" 
    documentationCenter=".net" 
    authors="sethmanheim" 
    manager="timlt" 
    editor="mattshel"/>

<tags 
    ms.service="service-bus" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="07/02/2015" 
    ms.author="sethm"/>

# 如何使用服务总线.NET API AMQP 1.0

高级消息队列协议 (AMQP) 1.0 是高效、 可靠、 不是线级消息传递协议，可用来构建可靠、 跨平台的消息应用程序。

支持用于在服务总线 AMQP 1.0 意味着您可以使用队列和发布/订阅具有邮件功能从一系列使用高效的二进制协议的平台。 此外，您可以生成应用程序使用的语言、 框架和操作系统组合构建的组件组成。

本操作指南介绍如何使用.NET 应用程序使用服务总线.NET API 中的中转服务总线消息传递功能 （队列和发布/订阅的主题）。 没有解释如何执行同一使用标准 Java 消息服务 (JMS) API 的助理操作指南。 您可以一起使用这两本手册以了解有关交叉平台消息传送使用 AMQP 1.0。

## 开始使用服务总线

本指南假定您已经服务总线命名空间包含队列名为"queue1"。 如果不这样做，，您可以创建的命名空间和队列使用[Azure 管理门户](http://manage.windowsazure.com)。 有关如何创建服务总线命名空间和队列的详细信息，请参阅标题为帮助指南"[如何使用服务总线队列。](https://azure.microsoft.com/develop/net/how-to-guides/service-bus-queues/)"

## 下载 SDK 服务总线

AMQP 1.0 支持是在服务总线 SDK 版本 2.1 或更高版本中可用。 在[http://nuget.org/packages/WindowsAzure.ServiceBus/](http://nuget.org/packages/WindowsAzure.ServiceBus/)，您可以从 NuGet 下载最新的 SDK。

## 代码的.NET 应用程序

默认情况下，服务总线.NET 客户端库与服务总线服务使用的是专用的基于 SOAP 协议进行通信。 而不是默认使用 AMQP 1.0 协议需要显式配置在下一节中所述的服务总线连接字符串上。 除这种更改，应用程序代码始终基本上保持不变时使用 AMQP 1.0。

在当前版本中有几个使用 AMQP 时不受支持的 API 功能。 在部分后面列出这些不支持的功能"不支持的功能和限制"。 使用 AMQP 时，某些高级的配置设置也具有不同的含义。 没有这些设置使用在此简短的操作指南，但[服务总线 AMQP 1.0 开发人员指南 》](http://msdn.microsoft.com/library/jj841071.aspx)中提供了更多详细信息。

### 通过 App.config 配置

建议应用程序可以使用 App.config 配置文件来存储设置的做法。 对于服务总线的应用程序，您可以使用 App.config 存储服务总线**连接字符串**。 此示例应用程序还使用 App.config 存储服务总线消息使用的实体的名称。

一个 App.config 文件示例如下所示︰

    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
        <appSettings>
            <add key="Microsoft.ServiceBus.ConnectionString"
                 value="Endpoint=sb://[namespace].servicebus.windows.net;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp" />
            <add key="EntityName" value="queue1" />
        </appSettings>
    </configuration>

### 配置服务总线连接字符串

**Microsoft.ServiceBus.ConnectionString**设置的值是用于配置连接到服务总线的服务总线连接字符串。 将为以下格式︰

    Endpoint=sb://[namespace].servicebus.windows.net;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp

其中 [命名空间] 和 [SA 密钥] 从 Azure 管理门户网站获得。 有关详细信息，请参阅 [如何使用服务总线队列] []。

当使用 AMQP，附加的连接字符串";传输类别 = Amqp"，据以使其连接到服务总线的客户端库使用 AMQP 1.0。

### 配置实体名称

此示例应用程序使用`EntityName`设置的**appSettings**部分的 App.config 文件来配置应用程序与其交换消息的队列的名称。

### 一个简单的.NET 应用程序使用服务总线队列

下面的示例发送和接收消息的服务总线队列中。

    // SimpleSenderReceiver.cs
    
    using System;
    using System.Configuration;
    using System.Threading;
    using Microsoft.ServiceBus.Messaging;
    
    namespace SimpleSenderReceiver
    {
        class SimpleSenderReceiver
        {
            private static string connectionString;
            private static string entityName;
            private static Boolean runReceiver = true;
            private MessagingFactory factory;
            private MessageSender sender;
            private MessageReceiver receiver;
            private MessageListener messageListener;
            private Thread listenerThread;
    
            static void Main(string[] args)
            {
                try
                {
                    if ((args.Length > 0) && args[0].ToLower().Equals("sendonly"))
                        runReceiver = false;
                    
                    string ConnectionStringKey = "Microsoft.ServiceBus.ConnectionString";
                    string entityNameKey = "EntityName";
                    entityName = ConfigurationManager.AppSettings[entityNameKey];
                    connectionString = ConfigurationManager.AppSettings[ConnectionStringKey];
                    SimpleSenderReceiver simpleSenderReceiver = new SimpleSenderReceiver();
    
                    Console.WriteLine("Press [enter] to send a message. " +
                        "Type 'exit' + [enter] to quit.");
    
                    while (true)
                    {
                        string s = Console.ReadLine();
                        if (s.ToLower().Equals("exit"))
                        {
                            simpleSenderReceiver.Close();
                            System.Environment.Exit(0);
                        }
                        else
                            simpleSenderReceiver.SendMessage();
                    }
                }
                catch (Exception e)
                {
                    Console.WriteLine("Caught exception: " + e.Message);
                }
            }
    
            public SimpleSenderReceiver()
            {
                factory = MessagingFactory.CreateFromConnectionString(connectionString);
                sender = factory.CreateMessageSender(entityName);
    
                if (runReceiver)
                {
                    receiver = factory.CreateMessageReceiver(entityName);
                    messageListener = new MessageListener(receiver);
                    listenerThread = new Thread(messageListener.Listen);
                    listenerThread.Start();
                }
            }
    
            public void Close()
            {
                messageListener.RequestStop();
                listenerThread.Join();
                factory.Close();
            }
    
            private void SendMessage()
            {
                BrokeredMessage message = 
                    new BrokeredMessage("Test AMQP message from .NET");
                sender.Send(message);
                Console.WriteLine("Sent message with MessageID = " 
                    + message.MessageId);
            }
    
        }
    
        public class MessageListener
        {
            private MessageReceiver messageReceiver;
            public MessageListener(MessageReceiver receiver)
            {
                messageReceiver = receiver;
            }
    
            public void Listen()
            {
                while (!_shouldStop)
                {
                    try
                    {
                        BrokeredMessage message = 
                            messageReceiver.Receive(new TimeSpan(0, 0, 10));
                        if (message != null)
                        {
                            Console.WriteLine("Received message with MessageID = " + 
                                message.MessageId);
                            message.Complete();
                        }
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("Caught exception: " + e.Message);
                        break;
                    }
                }
            }
    
            public void RequestStop()
            {
                _shouldStop = true;
            }
    
            private volatile bool _shouldStop;
        }
    }

### 运行应用程序

运行该应用程序生成的窗体的输出︰

    > SimpleSenderReceiver.exe
    Press [enter] to send a message. Type 'exit' + [enter] to quit.
    
    Sent message with MessageID = fb7f5d3733704e4ba4bd55f759d9d7cf
    Received message with MessageID = fb7f5d3733704e4ba4bd55f759d9d7cf
    
    Sent message with MessageID = d00e2e088f06416da7956b58310f7a06
    Received message with MessageID = d00e2e088f06416da7956b58310f7a06
    
    Received message with MessageID = f27f79ec124548c196fd0db8544bca49
    Sent message with MessageID = f27f79ec124548c196fd0db8544bca49
    exit

## 交叉平台消息传送 JMS 和.NET 之间

本指南介绍了如何将消息发送到服务总线使用.NET，以及如何接收这些消息使用.NET。 但是，AMQP 1.0 的主要优点之一是它使应用程序可以从组件中不同的语言，用交换可靠地和完全可靠的消息。

使用上面所述的示例.NET 应用程序和来自配套指南，[如何使用 Java 消息服务 (JMS) 与服务总线和 AMQP 1.0 的 API](service-bus-java-how-to-use-jms-api-amqp.md)，类似的 Java 应用程序就可以在.NET 和 Java 之间交换消息。 

有关交叉平台消息传送使用服务总线和 AMQP 1.0 的详细信息，请参阅[服务总线 AMQP 1.0 开发人员指南 》](http://msdn.microsoft.com/library/azure/jj841071.aspx)。

### 为.NET 的 JMS

演示.NET 消息到 JMS:

* 启动.NET 示例应用程序不使用任何命令行参数。
* 用"sendonly"命令行参数启动的 Java 示例应用程序。 在此模式下，应用程序不会收到消息从队列，它才会发送。
* 按**enter 键**几次在 Java 应用程序控制台中，将导致要发送的消息。
* 由.NET 应用程序接收这些消息。

### 从 JMS 应用程序的输出

    > java SimpleSenderReceiver sendonly
    Press [enter] to send a message. Type 'exit' + [enter] to quit.
    Sent message with JMSMessageID = ID:4364096528752411591
    Sent message with JMSMessageID = ID:459252991689389983
    Sent message with JMSMessageID = ID:1565011046230456854
    exit

### .NET 应用程序的输出

    > SimpleSenderReceiver.exe  
    Press [enter] to send a message. Type 'exit' + [enter] to quit.
    Received message with MessageID = 4364096528752411591
    Received message with MessageID = 459252991689389983
    Received message with MessageID = 1565011046230456854
    exit

## 对 JMS 的.NET

演示.NET 到 JMS 消息传递︰

* 使用"sendonly"的命令行参数启动.NET 示例应用程序。 在此模式下，应用程序不会收到消息从队列，它才会发送。
* 启动 Java 示例应用程序不使用任何命令行参数。
* 按**enter 键**几次在.NET 应用程序控制台中，将导致要发送的消息。
* 通过 Java 应用程序来接收这些消息。

#### .NET 应用程序的输出

    > SimpleSenderReceiver.exe sendonly
    Press [enter] to send a message. Type 'exit' + [enter] to quit.
    Sent message with MessageID = d64e681a310a48a1ae0ce7b017bf1cf3  
    Sent message with MessageID = 98a39664995b4f74b32e2a0ecccc46bb
    Sent message with MessageID = acbca67f03c346de9b7893026f97ddeb
    exit


#### 从 JMS 应用程序的输出

    > java SimpleSenderReceiver 
    Press [enter] to send a message. Type 'exit' + [enter] to quit.
    Received message with JMSMessageID = ID:d64e681a310a48a1ae0ce7b017bf1cf3
    Received message with JMSMessageID = ID:98a39664995b4f74b32e2a0ecccc46bb
    Received message with JMSMessageID = ID:acbca67f03c346de9b7893026f97ddeb
    exit

## 不支持的功能和限制

以下.NET 服务总线 API 的功能当前不支持使用 AMQP 时︰

* 事务
* 通过传输目标发送
* 接收的消息的序列号
* 邮件和会话浏览
* 会话状态
* 基于批次的 Api
* 向外扩展的接收
* 运行时操作的订阅规则
* 会话锁续订
* 在行为中的一些细微差别

有关详细信息，请参阅[服务总线 AMQP 1.0 开发人员指南 》](http://msdn.microsoft.com/library/azure/jj841071.aspx)。 本主题包含不受支持的 Api 的详细的列表。

## 摘要

本帮助指南演示了如何从.NET 访问 （队列和发布/订阅的主题） 的中转服务总线消息传递功能使用 AMQP 1.0 和服务总线.NET API。

您还可以从其他语言，包括 Java、 C，Python 和 PHP 使用服务总线 AMQP 1.0。 可靠地使用在服务总线 AMQP 1.0 的完全保真的在使用这些语言构建的组件可以交换消息。 有关详细信息，请参阅[服务总线 AMQP 1.0 开发人员指南 》](http://msdn.microsoft.com/library/azure/jj841071.aspx)。

## 下一步行动

* [AMQP 1.0 在 Azure 服务总线的支持](service-bus-amqp-overview.md)
* [如何使用 Java 消息服务 (JMS) API 服务总线和 AMQP 1.0](service-bus-java-how-to-use-jms-api-amqp.md)
* [服务总线 AMQP 1.0 开发人员指南 》](http://msdn.microsoft.com/library/azure/jj841071.aspx)
* [如何使用服务总线队列](service-bus-dotnet-how-to-use-queues.md)
 

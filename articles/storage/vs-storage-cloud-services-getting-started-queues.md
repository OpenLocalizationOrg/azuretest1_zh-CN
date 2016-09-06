---
ms.openlocfilehash: c054ad638ae09a3165e59e252be6c953438db765
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="开始使用 Azure 队列存储和 Visual Studio 连接服务 （云服务项目）" 
    description="如何开始使用 Azure 队列存储在 Visual Studio 中一个云服务项目" 
    services="storage" 
    documentationCenter="" 
    authors="patshea123" 
    manager="douge" 
    editor="tglee"/>

<tags 
    ms.service="storage" 
    ms.workload="web" 
    ms.tgt_pltfrm="vs-getting-started" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/04/2015" 
    ms.author="patshea123"/>

# 开始使用 Azure 队列存储和 Visual Studio 连接的服务 （云服务项目）

> [AZURE.SELECTOR]
> - [入门教程](vs-storage-cloud-services-getting-started-queues.md)
> - [发生了什么事](vs-storage-cloud-services-what-happened.md)

> [AZURE.SELECTOR]
> - [Blob](vs-storage-cloud-services-getting-started-blobs.md)
> - [队列](vs-storage-cloud-services-getting-started-queues.md)
> - [表](vs-storage-cloud-services-getting-started-tables.md)

##概述

本文介绍如何开始使用 Azure 队列存储在 Visual Studio 中创建或通过使用 Visual Studio**中添加连接服务**对话框引用 Azure 存储帐户在一个云服务项目中。 

我们将向您展示如何在代码中创建的队列。 我们将还显示如何执行基本的队列操作，例如添加、 修改、 读取和删除队列的消息。 示例在 C# 代码中编写和使用[Azure 存储.NET 客户端库](https://msdn.microsoft.com/library/azure/dn261237.aspx)。 

**添加连接服务**操作安装适当的 NuGet 程序包，以访问您的项目在 Azure 存储并将存储帐户连接字符串添加到项目配置文件。

 - 操作代码中的队列的详细信息，请参阅[如何使用队列存储从.NET](storage-dotnet-how-to-use-queues.md) 。
 - Azure 存储有关的一般信息，请参阅[存储文档](https://azure.microsoft.com/documentation/services/storage/)。
 - Azure 的云服务的常规信息，请参阅[云服务文档](http://azure.microsoft.com/documentation/services/cloud-services/)。
 - 有关编程 ASP.NET 应用程序的详细信息，请参见[ASP.NET](http://www.asp.net) 。


Azure 队列存储是一种服务来存储大量的邮件，可以从任何位置访问世界上通过身份验证的调用使用 HTTP 或 HTTPS。 单个队列消息可达 64 KB 的大小，并且队列可以包含数以百万计的存储帐户的总容量限制的邮件。


##在代码中访问队列

若要访问队列在 Visual Studio 的云服务项目中，您需要包括以下各项的任何 C# 源代码文件访问 Azure 队列存储。

1. 请确保 C# 文件顶部的命名空间声明包含这些**using**语句。

        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;

2. 得到一个**CloudStorageAccount**对象，该对象表示您存储的帐户信息。 下面的代码用于获取您的存储连接字符串和从 Azure 服务配置的存储帐户信息。

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

3. 得到一个**CloudQueueClient**对象引用的队列对象在您的存储帐户。  

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

4. 得到一个**CloudQueue**来引用特定的队列。

        // Get a reference to a queue named "messageQueue"
        CloudQueue messageQueue = queueClient.GetQueueReference("messageQueue");


**注意︰**在下面的示例使用所有上面的代码，该代码的前面。

##在代码中创建队列

若要在代码中创建队列，只需添加对**CreateIfNotExists**的调用。

    // Get a reference to a CloudQueue object with the variable name 'messageQueue' 
    // as described in the "Access queues in code" section.
    
    // Create the CloudQueue if it does not exist
    messageQueue.CreateIfNotExists();

##向队列中添加消息

要插入到现有队列的消息，请创建一个新的**CloudQueueMessage**对象，然后调用**AddMessage**方法。

可以从 （以 utf-8 格式） 的字符串或字节数组创建一个**CloudQueueMessage**对象。

下面是一个示例将插入 Hello，World 的消息。

    // Get a reference to a CloudQueue object with the variable name 'messageQueue' as described in 
    // the "Access queues in code" section.

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    messageQueue.AddMessage(message);

##读取队列中的消息

您可以查看队列前面的消息但不从队列移除它通过调用**PeekMessage**方法。

    // Get a reference to a CloudQueue object with the variable name 'messageQueue' 
    // as described in the "Access queues in code" section.
    
    // Peek at the next message
    CloudQueueMessage peekedMessage = messageQueue.PeekMessage();

##读取和删除队列中的消息

可以删除您的代码 （队） 在两个步骤中的队列的消息。

1. 调用**GetMessage**获取队列中的下一条消息。 从**GetMessage**返回一条消息变得看不到从该队列中读取消息的任何其他代码。 默认情况下，此消息将 30 秒保持不可见。
2.  要从队列中移除该消息，请致电**DeleteMessage**。

如果您的代码无法处理的消息，由于硬件或软件故障，您的代码的另一个实例可以得到同样的消息，然后再试，可以确保删除邮件这两步过程。 在处理完邮件后，下面的代码将调用**DeleteMessage**右。

    // Get a reference to a CloudQueue object with the variable name 'messageQueue' 
    // as described in the "Access queues in code" section.
    
    // Get the next message in the queue.
    CloudQueueMessage retrievedMessage = messageQueue.GetMessage();

    // Process the message in less than 30 seconds

    // Then delete the message.
    await messageQueue.DeleteMessage(retrievedMessage);


## 利用其他处理和删除队列消息选项

有两种方法，您可以自定义消息从队列中的检索。

 - 您可以获取一批消息 （最多 32 个)。
 - 您可以设置隐蔽性更长或更短的超时值，允许您的代码更多或更少的时间来充分处理每条消息。 下面的代码示例使用**GetMessages**方法在一个调用中获取 20 个邮件。 然后它将处理每条消息使用**foreach**循环。 此外将隐蔽性超时设置为 5 分钟，每一封邮件。 请注意，所有邮件在同一时间，在 5 分钟启动后 5 分钟有传递由于调用**GetMessages**，任何未被删除的消息再次将变为可见。

下面是一个示例︰

    // Get a reference to a CloudQueue object with the variable name 'messageQueue' 
    // as described in the "Access queues in code" section.
    
    foreach (CloudQueueMessage message in messageQueue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.
    
        // Then delete the message after processing
        messageQueue.DeleteMessage(message);
    
    }

## 获取队列长度

您可以队列中获取消息数的估计值。 **FetchAttributes**方法询问队列服务检索队列属性，包括邮件计数。 **ApproximateMethodCount**属性返回的**FetchAttributes**方法，而无需调用队列服务检索到的最后一个值。

    // Get a reference to a CloudQueue object with the variable name 'messageQueue' 
    // as described in the "Access queues in code" section.
    
    // Fetch the queue attributes.
    messageQueue.FetchAttributes();

    // Retrieve the cached approximate message count.
    int? cachedMessageCount = messageQueue.ApproximateMessageCount;

    // Display number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## 对常见的 Azure 队列 Api 使用异步等待模式

此示例演示如何对常见 Azure 队列 Api 使用异步等待模式。 本示例调用每个给定方法的异步版本，这可以看到每个方法的**异步**后修复。 当使用异步方法异步-等待模式挂起本地执行，直到调用完成。 此行为允许当前线程用来进行其他工作，这有助于避免性能瓶颈，并提高了应用程序的总体响应速度。 在.NET 中使用异步等待模式的更多详细信息请参阅 [异步和等待 （C# 和 Visual Basic）] (https://msdn.microsoft.com/library/hh191443.aspx)

    // Get a reference to a CloudQueue object with the variable name 'messageQueue' 
    // as described in the "Access queues in code" section.
    
    // Create a message to put in the queue
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Add the message asynchronously
    await messageQueue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue the message
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Delete the message asynchronously
    await messageQueue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");

## 删除队列

要删除队列中包含的所有邮件，请在队列对象上调用**都 Delete**方法。

    // Get a reference to a CloudQueue object with the variable name 'messageQueue' 
    // as described in the "Access queues in code" section.
    
    // Delete the queue.
    messageQueue.Delete();

##下一步行动

[AZURE.INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]
            

---
ms.openlocfilehash: 76554843715d502e2fb4cfc32f42643057f61808
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="开始使用 Azure 队列存储和 Visual Studio 连接服务 |Microsoft Azure"
    description="如何开始使用 ASP.NET 项目在 Visual Studio 中的 Azure 队列存储"
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

# 开始使用 Azure 队列存储和 Visual Studio 连接服务

> [AZURE.SELECTOR]
> - [入门教程](vs-storage-aspnet-getting-started-queues.md)
> - [发生了什么事](vs-storage-aspnet-what-happened.md)

> [AZURE.SELECTOR]
> - [Blob](vs-storage-aspnet-getting-started-blobs.md)
> - [队列](vs-storage-aspnet-getting-started-queues.md)
> - [表](vs-storage-aspnet-getting-started-tables.md)

## 概述

本文介绍了如何开始使用 Azure 队列存储在 Visual Studio 中，您创建或通过使用 Visual Studio**中添加连接服务**对话框引用 ASP.NET 项目的 Azure 存储帐户后。

我们将向您展示如何创建和访问您的存储帐户在 Azure 队列。 我们将还显示如何执行基本的队列操作，例如添加、 修改、 读取和删除队列的消息。 示例在 C# 代码中编写和使用[Azure 存储.NET 客户端库](https://msdn.microsoft.com/library/azure/dn261237.aspx)。 有关 ASP.NET 的更多信息，请参见[ASP.NET](http://www.asp.net)。

Azure 队列存储是一种服务来存储大量的邮件，可以从任何位置访问世界上通过身份验证的调用使用 HTTP 或 HTTPS。 单个队列消息可达 64 KB 的大小，并且队列可以包含数以百万计的存储帐户的总容量限制的邮件。

## 在代码中访问队列

若要访问 ASP.NET 项目中的队列，您需要包括以下各项的任何 C# 源代码文件访问 Azure 队列存储。

1. 请确保 C# 文件顶部的命名空间声明包含这些`using`语句。

        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;

2. 获得`CloudStorageAccount`对象，它表示您存储的帐户信息。 下面的代码用于获取您的存储连接字符串和从 Azure 服务配置的存储帐户信息。

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

3. 获得`CloudQueueClient`对象引用 queueobjects 在您的存储帐户。  

        // Create the queueclient.
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

4. 获得`CloudQueue`来引用特定的队列的对象。

        // Get a reference to a queue named "messageQueue"
        CloudQueue messageQueue = queueClient.GetQueueReference("messageQueue");


**请注意**在下面的示例使用所有上面的代码，该代码的前面。

## 在代码中创建队列

若要在代码中创建 Azure 的队列，只需添加对的调用`CreateIfNotExists`为上面的代码。

    // Create the CloudQuecClient  if it does not exist
    messageQueue.CreateIfNotExists();

## 向队列中添加消息

要插入到现有队列的消息，请创建一个新`CloudQueueMessage`对象，然后调用`AddMessage`方法。

A `CloudQueueMessage` （以 utf-8 格式） 的字符串或字节数组中，可以创建对象。

下面是一个示例将插入 Hello，World 的消息。

    // Get a reference to the CloudQueue object named 'messageQueue' as described in "Access a queue in code"

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    messageQueue.AddMessage(message);

## 读取队列中的消息

您可以查看队列前面的消息但不从队列移除它通过调用 PeekMessage() 方法。

    // Get a reference to the CloudQueue object named 'messageQueue' as described in "Access a queue in code"

    // Peek at the next message
    CloudQueueMessage peekedMessage = messageQueue.PeekMessage();

## 读取和删除队列中的消息

可以删除您的代码 （队） 在两个步骤中的队列的消息。
1. 调用 getmessage （） 来获取队列中的下一条消息。 从 getmessage （） 返回的消息成为看不到从该队列中读取消息的任何其他代码。 默认情况下，此消息将 30 秒保持不可见。
2.  要从队列中移除消息，调用`DeleteMessage`。

如果您的代码无法处理的消息，由于硬件或软件故障，您的代码的另一个实例可以得到同样的消息，然后再试，可以确保删除邮件这两步过程。 下面的代码调用`DeleteMessage`在处理完邮件后右。

    // Get a reference to the CloudQueue object named 'messageQueue' as described in "Access a queue in code"

    // Get the next message in the queue.
    CloudQueueMessage retrievedMessage = messageQueue.GetMessage();

    // Process the message in less than 30 seconds

    // Then delete the message.
    await messageQueue.DeleteMessage(retrievedMessage);


## 使用其他选项取消队列的消息

有两种方法，您可以自定义消息从队列中的检索。
首先，您可以获取一批消息 （最多 32 个)。 第二，您可以设置隐蔽性更长或更短的超时值，允许您的代码更多或更少的时间来充分处理每条消息。 下面的代码示例使用`GetMessages`方法在一个调用中获取 20 个邮件。 然后它处理每个消息都使用`foreach`循环。 此外将隐蔽性超时设置为 5 分钟，每一封邮件。 请注意，在同一时间，所有邮件从开始的 5 分钟后 5 分钟有传递以来对`GetMessages`，任何未被删除的消息再次将变为可见。

    // Get a reference to the CloudQueue object named 'messageQueue' as described in "Access a queue in code"

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    foreach (CloudQueueMessage message in queue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.
        queue.DeleteMessage(message);
    }

## 获取队列长度

您可以队列中获取消息数的估计值。 `FetchAttributes`方法询问队列服务检索队列属性，包括邮件计数。 `ApproximateMethodCount`属性返回的最后一个值，通过`FetchAttributes`方法，而无需调用队列服务。

    // Get a reference to the CloudQueue object named 'messageQueue' as described in "Access a queue in code"

    // Fetch the queue attributes.
    messageQueue.FetchAttributes();

    // Retrieve the cached approximate message count.
    int? cachedMessageCount = messageQueue.ApproximateMessageCount;

    // Display number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## 对常用的 queueAPIs 使用异步等待模式

此示例演示如何对常见的 queueAPIs 使用异步等待模式。 本示例调用每个给定方法的异步版本，这可以看到每个方法的异步后修复。 当使用异步方法异步-等待模式挂起本地执行，直到调用完成。 此行为允许当前线程用来进行其他工作，这有助于避免性能瓶颈，并提高了应用程序的总体响应速度。 在.NET 中使用异步等待模式的更多详细信息请参阅 [异步和等待 （C# 和 Visual Basic）] (https://msdn.microsoft.com/library/hh191443.aspx)

    // Get a reference to the CloudQueue object named 'messageQueue' as described in "Access a queue in code"

    // Create a message to put in the queue
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Async enqueue the message
    await messageQueue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue the message
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Async delete the message
    await messageQueue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");

## 删除队列

若要删除队列中包含的所有消息，调用`Delete`队列对象上的方法。

    // Get a reference to the CloudQueue object named 'messageQueue' as described in "Access a queue in code"

    // Delete the queue.
    messageQueue.Delete();



## 下一步行动

[AZURE.INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]

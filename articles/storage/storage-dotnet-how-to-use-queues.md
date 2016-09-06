---
ms.openlocfilehash: ee55be3d714f015ec1eb7d903e8d3c78dd85908b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="如何使用队列存储从.NET |Microsoft Azure"
    description="了解如何使用 Microsoft Azure 队列存储，以创建和删除队列并插入、 窥视、 获取，和删除队列的邮件。"
    services="storage"
    documentationCenter=".net"
    authors="tamram"
    manager="adinah"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article" 
    ms.date="08/04/2015"
    ms.author="tamram"/>

# 如何使用队列存储从.NET

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]

## 概述

本指南将向您介绍如何执行使用 Azure 队列存储服务的常见方案。 用 c 语言编写的示例\#和 Azure 存储客户端用于.NET 代码。 所包含的方案包括**插入**、**查看**、**获取**、 和**删除**队列的消息，以及**创建和删除队列**。

[AZURE.INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-configure-connection-string-include](../../includes/storage-configure-connection-string-include.md)]

## 以编程方式访问队列存储

[AZURE.INCLUDE [storage-dotnet-obtain-assembly](../../includes/storage-dotnet-obtain-assembly.md)]

### Namespace 声明
将下面的代码命名空间声明添加到顶部的任何 C\#想以编程方式访问 Azure 存储文件︰

    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Queue;

请确保您引用`Microsoft.WindowsAzure.Storage.dll`程序集。

[AZURE.INCLUDE [storage-dotnet-retrieve-conn-string](../../includes/storage-dotnet-retrieve-conn-string.md)]

## 创建队列

**CloudQueueClient**对象允许您获取队列的引用对象。
下面的代码创建一个**CloudQueueClient**对象。 本指南中的所有代码都使用 Azure 应用程序服务配置中存储的存储连接字符串。 还有其他方法来创建一个**CloudStorageAccount**对象。 请参阅[CloudStorageAccount][]文档的详细信息。

    // Retrieve storage account from connection string
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

使用**queueClient**对象来获取对要使用的队列的引用。 如果它不存在，您可以创建队列。

    // Retrieve a reference to a queue
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Create the queue if it doesn't already exist
    queue.CreateIfNotExists();

## 插入到队列的消息

要插入到现有队列的消息，首先创建新的**CloudQueueMessage**。 接下来，调用**AddMessage**方法。 可以从 （以 utf-8 格式） 的字符串或**字节**数组创建的**CloudQueueMessage** 。 下面是创建一个队列 （如果存在） 的代码并将其插入 Hello，World 的消息︰

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Create the queue if it doesn't already exist.
    queue.CreateIfNotExists();

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    queue.AddMessage(message);

## 查看下一条消息

您可以查看队列前面的消息但不从队列移除它通过调用**PeekMessage**方法。

    // Retrieve storage account from connection string
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Peek at the next message
    CloudQueueMessage peekedMessage = queue.PeekMessage();

    // Display message.
    Console.WriteLine(peekedMessage.AsString);

## 更改队列的消息的内容

您可以更改邮件的就地在队列中的内容。 如果该消息表示的工作任务，可以使用此功能来更新工作任务的状态。 下面的代码使用新内容，更新队列消息并设置可见性超时值来扩展另一个 60 秒。 这样可以节省与消息关联的工作状态并为客户端提供了另一分钟，以继续对该消息进行处理。 您可以使用此技术来跟踪多个步骤的工作流队列消息，而无需从头开始如果由于硬件或软件故障而失败的处理步骤。 通常情况下，可以保持，重试计数，如果多于*n*次重试消息，则将删除它。 这样可以防止触发应用程序错误每次处理一条消息。

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Get the message from the queue and update the message contents.
    CloudQueueMessage message = queue.GetMessage();
    message.SetMessageContent("Updated contents.") ;
    queue.UpdateMessage(message,
        TimeSpan.FromSeconds(0.0),  // Make it visible immediately.
        MessageUpdateFields.Content | MessageUpdateFields.Visibility);

## 队的下一个邮件

您的代码消除排队两个步骤中的队列中的消息。 **GetMessage**调用时，您将获得下一条消息在队列中。 从**GetMessage**返回一条消息变得看不到从该队列中读取消息的任何其他代码。 默认情况下，此消息将 30 秒保持不可见。 要从队列中移除该消息，则必须调用**DeleteMessage**。 如果您的代码无法处理的消息，由于硬件或软件故障，您的代码的另一个实例可以得到同样的消息，然后再试，可以确保删除邮件这两步过程。 在处理完邮件后，您的代码将调用**DeleteMessage**右。

    // Retrieve storage account from connection string
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Get the next message
    CloudQueueMessage retrievedMessage = queue.GetMessage();

    //Process the message in less than 30 seconds, and then delete the message
    queue.DeleteMessage(retrievedMessage);

## 与公共队列存储 Api 使用异步等待模式

此示例演示如何使用公共队列存储 Api 使用异步等待模式。 本示例调用给定方法的异步版本，如所示的每个方法的*异步*后缀。 使用异步方法时，异步的等待模式挂起本地执行，直到调用完成。 此行为允许当前线程用来进行其他工作，这有助于避免性能瓶颈，并提高了应用程序的总体响应速度。 在.NET 中使用异步等待模式的更多详细信息请参阅[异步和等待 （C# 和 Visual Basic）](https://msdn.microsoft.com/library/hh191443.aspx)

    // Create the queue if it doesn't already exist
    if(await queue.CreateIfNotExistsAsync())
    {
        Console.WriteLine("Queue '{0}' Created", queue.Name);
    }
    else
    {
        Console.WriteLine("Queue '{0}' Exists", queue.Name);
    }

    // Create a message to put in the queue
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Async enqueue the message
    await queue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue the message
    CloudQueueMessage retrievedMessage = await queue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Async delete the message
    await queue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");

## 利用取消队列邮件的其他选项

有两种方法，您可以自定义消息从队列中的检索。
首先，您可以获取一批消息 （最多 32 个)。 第二，您可以设置隐蔽性更长或更短的超时值，允许您的代码更多或更少的时间来充分处理每条消息。 下面的代码示例使用**GetMessages**方法在一个调用中获取 20 个邮件。 然后它将处理每条消息使用**foreach**循环。 此外将隐蔽性超时设置为 5 分钟，每一封邮件。 请注意，所有邮件在同一时间，在 5 分钟启动后 5 分钟有传递由于调用**GetMessages**，任何未被删除的消息再次将变为可见。

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

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

您可以队列中获取消息数的估计值。 **FetchAttributes**方法询问队列服务检索队列属性，包括邮件计数。 **ApproximateMethodCount**属性返回的**FetchAttributes**方法，而无需调用队列服务检索到的最后一个值。

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Fetch the queue attributes.
    queue.FetchAttributes();

    // Retrieve the cached approximate message count.
    int? cachedMessageCount = queue.ApproximateMessageCount;

    // Display number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## 删除队列

要删除队列中包含的所有邮件，请在队列对象上调用**都 Delete**方法。

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Delete the queue.
    queue.Delete();

## 下一步行动

现在，您已经学习了队列存储的基本知识，按照这些链接以了解更复杂的存储任务。

- 查看队列服务参考文档中有关 Api 的完整详细信息︰
    - [对于.NET 引用存储客户端库](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
    - [REST API 参考](http://msdn.microsoft.com/library/azure/dd179355)
- 了解可以使用 Azure 存储在[存储和访问 Azure 中的数据](http://msdn.microsoft.com/library/azure/gg433040.aspx)执行的更高级任务
- 了解如何简化为使用 Azure 存储通过使用[Azure WebJobs SDK](../websites-dotnet-webjobs-sdk/)编写代码。
- 查看更多的功能指南以了解如何将数据存储在 Azure 中的其他选项。
    - 使用[表格存储](storage-dotnet-how-to-use-tables.md)来存储结构化的数据。 
    - 使用[Blob 存储](storage-dotnet-how-to-use-blobs.md)来存储非结构化的数据。
    - 使用[SQL 数据库](sql-database-dotnet-how-to-use.md)来存储关系数据。

  [下载并安装的.NET 的 Azure SDK]: /develop/net/
  [.NET 客户端库参考]: http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
  [在 Visual Studio 中创建一个 Azure 项目]: http://msdn.microsoft.com/library/azure/ee405487.aspx
  [CloudStorageAccount]: http://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudstorageaccount_methods.aspx
  [存储和访问 Azure 中的数据]: http://msdn.microsoft.com/library/azure/gg433040.aspx
  [Azure 存储团队博客]: http://blogs.msdn.com/b/windowsazurestorage/
  [配置连接字符串]: http://msdn.microsoft.com/library/azure/ee758697.aspx
  [OData]: http://nuget.org/packages/Microsoft.Data.OData/5.0.2
  [Edm]: http://nuget.org/packages/Microsoft.Data.Edm/5.0.2
  [空间]: http://nuget.org/packages/System.Spatial/5.0.2
 

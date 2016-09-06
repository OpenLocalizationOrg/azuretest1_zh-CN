---
ms.openlocfilehash: ad8fd47e1729e57bce27b7ee940bd59be741aa2b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何使用队列存储从 Java |Microsoft Azure" 
    description="了解如何使用 Azure 队列服务创建和删除队列，并插入、 获取，和删除消息。 用 Java 编写的示例。" 
    services="storage" 
    documentationCenter="java" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/31/2015" 
    ms.author="robmcm"/>

# 如何使用队列存储从 Java

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]

## 概述

本指南将向您介绍如何执行使用 Azure 队列存储服务的常见方案。 这些示例用 Java 编写的并使用[Azure 存储 Java SDK][]。 所包含的方案包括**插入**、**查看**、**获取**、 和**删除**队列的消息，以及**创建**和**删除**队列。 有关队列的详细信息，请参阅[后续步骤](#NextSteps)部分。

注︰ 对于开发人员来说在 Android 设备上使用 Azure 存储提供了 SDK。 有关详细信息，请参阅[为 Android 的 Azure 存储 SDK][]。 

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## 创建一个 Java 应用程序

在本指南中，您将使用存储功能，可以在本地或在 web 角色或在 Azure 中的辅助角色中运行的代码中运行 Java 应用程序中。

若要执行此操作，您将需要安装 Java 开发工具箱 (JDK) 并在 Azure 订阅创建 Azure 存储帐户。 一旦您这样做了，您需要验证您开发的系统满足最低要求和相关性在 GitHub [Azure 存储 Java SDK][]存储库中列出了这些。 如果您的系统符合这些要求，您可以按照说明下载和安装 Java Azure 存储库，该存储库从您的系统上。 完成这些任务之后，您将能够创建一个 Java 应用程序，它使用本文中的示例。

## 配置应用程序来访问队列存储

将下面的导入语句添加到想要使用 Azure 存储 Api 来访问队列的 Java 文件的顶部︰

    // Include the following imports to use queue APIs.
    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.queue.*;

## 安装程序将 Azure 存储连接字符串

Azure 存储客户端使用存储的连接字符串将存储终结点和凭据以访问数据管理服务。 在客户端应用程序运行时，您必须提供存储连接字符串以下面的格式，用于存储帐户的*帐户名*和*AccountKey*值管理门户中列出您的存储帐户和主访问键的名称。 本示例显示了如何声明一个静态字段来保存连接字符串︰

    // Define the connection-string with your values.
    public static final String storageConnectionString = 
        "DefaultEndpointsProtocol=http;" + 
        "AccountName=your_storage_account;" + 
        "AccountKey=your_storage_account_key";

在 Microsoft Azure 角色中运行的应用程序，此字符串将存储在服务配置文件中， *ServiceConfiguration.cscfg*，并可以通过调用**RoleEnvironment.getConfigurationSettings**方法。 这里是从名为*StorageConnectionString*的服务配置文件中的**设置**元素获取连接字符串的示例︰

    // Retrieve storage account from connection-string.
    String storageConnectionString = 
        RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");

下面的示例假定您已经使用这两种方法之一来获取存储连接字符串。

## 如何︰ 创建队列

**CloudQueueClient**对象允许您获取队列的引用对象。 下面的代码创建一个**CloudQueueClient**对象。 (注︰ 还有其他的方式来创建**CloudStorageAccount**对象; 有关详细信息，请参阅**CloudStorageAccount** [Azure 存储客户端 SDK 参考]中。)

使用**CloudQueueClient**对象来获取对要使用的队列的引用。 如果它不存在，您可以创建队列。

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount = 
           CloudStorageAccount.parse(storageConnectionString);

       // Create the queue client.
       CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

       // Retrieve a reference to a queue.
       CloudQueue queue = queueClient.getQueueReference("myqueue");

       // Create the queue if it doesn't already exist.
       queue.createIfNotExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## 如何︰ 添加到队列的消息

要插入到现有队列的消息，首先创建新的**CloudQueueMessage**。 接下来，调用**addMessage**方法。 可以从 （以 utf-8 格式） 的字符串或字节数组创建的**CloudQueueMessage** 。 下面是创建一个队列 （如果存在） 的代码并将其插入"Hello，World"的消息。

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount = 
           CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Create the queue if it doesn't already exist.
        queue.createIfNotExists();

        // Create a message and add it to the queue.
        CloudQueueMessage message = new CloudQueueMessage("Hello, World");
        queue.addMessage(message);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## 如何︰ 查看下一条消息

您可以查看队列前面的消息但不从队列移除它通过调用**peekMessage**。

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount = 
           CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");
            
        // Peek at the next message.
        CloudQueueMessage peekedMessage = queue.peekMessage();
            
        // Output the message value.
        if (peekedMessage != null)
        {
          System.out.println(peekedMessage.getMessageContentAsString());
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## 如何︰ 更改排队的邮件的内容

您可以更改邮件的就地在队列中的内容。 如果该消息表示的工作任务，可以使用此功能来更新工作任务的状态。 下面的代码使用新内容，更新队列消息并设置可见性超时值来扩展另一个 60 秒。 这样可以节省与消息关联的工作状态并为客户端提供了另一分钟，以继续对该消息进行处理。 您可以使用此技术来跟踪多个步骤的工作流队列消息，而无需从头开始如果由于硬件或软件故障而失败的处理步骤。 通常情况下，可以保持，重试计数，如果多于*n*次重试消息，则将删除它。 这样可以防止触发应用程序错误每次处理一条消息。

通过消息队列执行下列代码示例搜索查找匹配"Hello，World"的内容，然后修改内容的消息并退出的第一个消息。 

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount = 
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // The maximum number of messages that can be retrieved is 32. 
        final int MAX_NUMBER_OF_MESSAGES_TO_PEEK = 32;

        // Loop through the messages in the queue.
        for (CloudQueueMessage message : queue.retrieveMessages(MAX_NUMBER_OF_MESSAGES_TO_PEEK,1,null,null))
        {
            // Check for a specific string.
            if (message.getMessageContentAsString().equals("Hello, World"))
            {
                // Modify the content of the first matching message.
                message.setMessageContent("Updated contents.");
                // Set it to be visible in 30 seconds.
                EnumSet<MessageUpdateFields> updateFields = 
                    EnumSet.of(MessageUpdateFields.CONTENT,
                    MessageUpdateFields.VISIBILITY);
                // Update the message.
                queue.updateMessage(message, 30, updateFields, null, null);
                break;
            }
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

或者，下面的代码示例更新只是第一个显示一条消息在队列中。

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount = 
           CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Retrieve the first visible message in the queue.
        CloudQueueMessage message = queue.retrieveMessage();
            
        if (message != null)
        {
            // Modify the message content.
            message.setMessageContent("Updated contents.");
            // Set it to be visible in 60 seconds.
            EnumSet<MessageUpdateFields> updateFields = 
                EnumSet.of(MessageUpdateFields.CONTENT,
                MessageUpdateFields.VISIBILITY);
            // Update the message.
            queue.updateMessage(message, 60, updateFields, null, null);
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## 如何︰ 获取队列长度

您可以队列中获取消息数的估计值。 **DownloadAttributes**方法询问几个当前值，包括队列中的消息数计数的队列服务。 由于消息可以添加或删除队列服务响应您的请求后，计数才近似。 **GetApproximateMessageCount**方法返回通过调用**downloadAttributes**，而无需调用队列服务中检索最后一个值。

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount = 
           CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

       // Download the approximate message count from the server.
        queue.downloadAttributes();

        // Retrieve the newly cached approximate message count.
        long cachedMessageCount = queue.getApproximateMessageCount();
            
        // Display the queue length.
        System.out.println(String.format("Queue length: %d", cachedMessageCount));
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## 如何︰ 下一条消息的队列中去除

您的代码中取消排队的消息从队列中两个步骤。 在调用**retrieveMessage**时，您将获得下一条消息在队列中。 从**retrieveMessage**返回的消息成为看不到从该队列中读取消息的任何其他代码。 默认情况下，此消息将 30 秒保持不可见。 要从队列中移除该消息，则必须调用**deleteMessage**。 如果您的代码无法处理的消息，由于硬件或软件故障，您的代码的另一个实例可以得到同样的消息，然后再试，可以确保删除邮件这两步过程。 在处理完邮件后，您的代码将调用**deleteMessage**右。

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount = 
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Retrieve the first visible message in the queue.
        CloudQueueMessage retrievedMessage = queue.retrieveMessage();
            
        if (retrievedMessage != null)
        {
            // Process the message in less than 30 seconds, and then delete the message.
            queue.deleteMessage(retrievedMessage);
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }


## 出列邮件的其他选项

有两种方法，您可以自定义消息从队列中的检索。 首先，您可以获取一批消息 （最多 32 个)。 第二，您可以设置隐蔽性更长或更短的超时值，允许您的代码更多或更少的时间来充分处理每条消息。

下面的代码示例使用**retrieveMessages**方法在一个调用中获取 20 个邮件。 然后它将处理每条消息使用**for**循环。 它还将隐蔽性超时设置为 5 分钟 （300 秒） 为每个消息。 五分钟开始的所有消息的同时，请注意因此当五分钟已通过以来对**retrieveMessages**的调用时，任何未被删除的消息再次将变为可见。

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount = 
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Retrieve 20 messages from the queue with a visibility timeout of 300 seconds.
        for (CloudQueueMessage message : queue.retrieveMessages(20, 300, null, null)) {
            // Do processing for all messages in less than 5 minutes, 
            // deleting each message after processing.
            queue.deleteMessage(message);
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## 如何︰ 列出队列

若要获取当前队列的列表，请调用**CloudQueueClient.listQueues()**方法，将返回的**CloudQueue**对象的集合。 

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient =
            storageAccount.createCloudQueueClient();

        // Loop through the collection of queues.
        for (CloudQueue queue : queueClient.listQueues())
        {
            // Output each queue name.
            System.out.println(queue.getName());
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## 如何︰ 删除队列

要删除队列中包含的所有消息，请在**CloudQueue**对象上调用**deleteIfExists**方法。

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount = 
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Delete the queue if it exists.
        queue.deleteIfExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## 下一步行动

现在，您已经学习了队列存储的基本知识，按照这些链接以了解更复杂的存储任务。

- [Azure 存储 Java SDK][]
- [Azure 存储客户端 SDK 参考][]
- [REST API，azure 存储][]
- [Azure 存储团队博客][]

[对于 Java 的 azure SDK]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure 存储 Java SDK]: https://github.com/azure/azure-storage-java
[Azure 存储为 Android SDK]: https://github.com/azure/azure-storage-android
[Azure 存储客户端 SDK 参考]: http://dl.windowsazure.com/storage/javadoc/
[REST API，azure 存储]: http://msdn.microsoft.com/library/azure/gg433040.aspx
[Azure 存储团队博客]: http://blogs.msdn.com/b/windowsazurestorage/

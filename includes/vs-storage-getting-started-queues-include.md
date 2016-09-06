---
ms.openlocfilehash: 0fbbe936364d1a62fe18c62769c0bfc2cb5285e5
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
#####创建队列
**CloudQueueClient**对象允许您获取队列的引用对象。 下面的代码创建一个**CloudQueueClient**对象。 本主题中的所有代码都使用 Azure 应用程序服务配置中存储的存储连接字符串。 还有其他方法来创建一个**CloudStorageAccount**对象。 [CloudStorageAccount](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudstorageaccount_methods.aspx "CloudStorageAccount")文档，了解详细信息，请参阅。

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

使用**queueClient**对象来获取对要使用的队列的引用。 该代码尝试引用的队列名为"myqueue"。 如果它找不到具有该名称的队列，则会创建一个。

    // Get a reference to a queue named “myqueue”.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // If the queue isn’t already there, then create it.
    queue.CreateIfNotExists();

**注意︰**以下各节中使用此代码块代码的前面。

#####插入到队列的消息
要插入到现有队列的消息，首先创建一个新的**CloudQueueMessage**对象。 接下来，调用 AddMessage() 方法。 可以从 （以 utf-8 格式） 的字符串或字节数组创建一个**CloudQueueMessage**对象。 下面是创建一个队列 （如果存在） 的代码并将其插入 Hello，World 的消息。

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    queue.AddMessage(message);

#####查看下一条消息
您可以查看队列前面的消息但不从队列移除它通过调用 PeekMessage() 方法。

    // Peek at the next message in the queue.
    CloudQueueMessage peekedMessage = queue.PeekMessage();

    // Display the message.
    Console.WriteLine(peekedMessage.AsString);

#####删除下一封邮件
可以删除您的代码 （队） 在两个步骤中的队列的消息。 


1. 调用 getmessage （） 来获取队列中的下一条消息。 从 getmessage （） 返回的消息成为看不到从该队列中读取消息的任何其他代码。 默认情况下，此消息将 30 秒保持不可见。 
2.  要从队列中移除该消息，请致电 DeleteMessage()。 

如果您的代码无法处理的消息，由于硬件或软件故障，您的代码的另一个实例可以得到同样的消息，然后再试，可以确保删除邮件这两步过程。 下面的代码会调用 DeleteMessage() 正确处理该消息之后。

    // Get the next message in the queue.
    CloudQueueMessage retrievedMessage = queue.GetMessage();

    // Process the message in less than 30 seconds, and then delete the message.
    queue.DeleteMessage(retrievedMessage);

[了解更多关于 Azure 存储](http://azure.microsoft.com/documentation/services/storage/)请参阅[浏览服务器资源管理器中的存储资源](http://msdn.microsoft.com/library/azure/ff683677.aspx)。
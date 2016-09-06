---
ms.openlocfilehash: 0951a35748d6f743517f8e2b6730bd7540880dca
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何使用队列存储 （c + +） |Microsoft Azure" 
    description="了解如何在 Azure 中使用队列存储服务。 示例是用 c + + 编写的。" 
    services="storage" 
    documentationCenter=".net" 
    authors="tamram" 
    manager="adinah" 
    editor=""/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/19/2015" 
    ms.author="tamram"/>

# 如何使用 c + + 中的队列存储  

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]

## 概述
本指南将向您介绍如何执行使用 Azure 队列存储服务的常见方案。 这些示例用 c + + 编写和使用[c + + 的 Azure 存储客户端库](https://github.com/Azure/azure-storage-cpp/blob/v1.0.0/README.md)。  所包含的方案包括**插入**、**查看**、**获取**、 和**删除**队列的消息，以及**创建和删除队列**。  

>[AZURE.NOTE] 本指南面向 Azure 存储客户端库，c + + 版本 1.0.0 和上面。 建议使用的版本是存储客户端库 1.0.0 版，可通过[NuGet](http://www.nuget.org/packages/wastorage)或[GitHub](https://github.com/)。 

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]
[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## 创建一个 c + + 应用程序  
在本指南中，您将使用存储功能，可以在 c + + 应用程序中运行。  

为此，将需要安装 c + + 的 Azure 存储客户端库和在 Azure 订阅创建 Azure 存储帐户。  

为 c + + 安装 Azure 存储客户端库，您可以使用以下方法︰

-   **Linux:**按照在[Azure 存储客户端库的 c + + 自述文件](https://github.com/Azure/azure-storage-cpp/blob/master/README.md)页中的说明进行操作。  
-   **窗口︰**在 Visual Studio 中，单击**工具 > NuGet 程序包管理器 > 程序包管理器控制台**。 [NuGet 程序包管理器控制台](http://docs.nuget.org/docs/start-here/using-the-package-manager-console)中键入以下命令，然后按**enter 键**。  

        Install-Package wastorage 
 
## 配置应用程序来访问队列存储
添加以下语句，您希望使用 Azure 存储 Api 来访问队列的 c + + 文件的顶部︰  

    #include "was/storage_account.h"
    #include "was/queue.h"

## 安装程序将 Azure 存储连接字符串

Azure 存储客户端使用存储的连接字符串将存储终结点和凭据以访问数据管理服务。 在客户端应用程序运行时，您必须提供存储连接字符串以下面的格式，用于存储帐户的*帐户名*和*AccountKey*值管理门户中列出您的存储帐户和存储访问键的名称。 有关存储帐户和访问密钥的信息，请参阅[关于 Azure 存储帐户](storage-create-storage-account.md)。 本示例显示了如何声明一个静态字段来保存连接字符串︰  

    // Define the connection-string with your values.
    const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));

若要在您的本地 Windows 计算机中测试您的应用程序，您可以使用[Azure SDK](http://azure.microsoft.com/downloads/)安装 Microsoft Azure[存储仿真器](https://msdn.microsoft.com/library/azure/hh403989.aspx)。 存储仿真器是一个实用程序，模拟在 Azure 中本地开发计算机上可用的 Blob、 队列和表服务。 下面的示例演示如何声明一个静态字段来保存到本地存储仿真程序的连接字符串︰  

    // Define the connection-string with Azure Storage Emulator.
    const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  

要启动 Azure 存储仿真程序，请选择**开始**按钮或按**Windows**键。 开始键入**Azure 存储仿真程序**，并从应用程序的列表中选择**Microsoft Azure 存储仿真器**。  

下面的示例假定您已经使用这两种方法之一来获取存储连接字符串。  

## 检索连接字符串
您可以使用**cloud_storage_account**类来表示您的存储帐户信息。 若要检索您的存储帐户信息从存储连接字符串，可以使用**parse**方法。 

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

## 如何︰ 创建队列
**Cloud_queue_client**对象允许您获取队列的引用对象。 下面的代码创建一个**cloud_queue_client**对象。  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create a queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

使用**cloud_queue_client**对象来获取对要使用的队列的引用。 如果它不存在，您可以创建队列。

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Create the queue if it doesn't already exist.
    queue.create_if_not_exists();  

## 如何︰ 将消息插入到队列
要插入到现有队列的消息，首先创建新的**cloud_queue_message**。 接下来，调用**add_message**方法。 可以从一个字符串或**字节**数组创建的**cloud_queue_message** 。 下面是创建一个队列 （如果存在） 的代码并将其插入 Hello，World 的消息︰

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Create the queue if it doesn't already exist.
    queue.create_if_not_exists();

    // Create a message and add it to the queue.
    azure::storage::cloud_queue_message message1(U("Hello, World"));
    queue.add_message(message1);  

## 如何︰ 查看下一条消息
您可以查看队列前面的消息但不从队列移除它通过调用**peek_message**方法。  

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Peek at the next message.
    azure::storage::cloud_queue_message peeked_message = queue.peek_message();

    // Output the message content.
    std::wcout << U("Peeked message content: ") << peeked_message.content_as_string() << std::endl;

## 如何︰ 更改排队的邮件的内容
您可以更改邮件的就地在队列中的内容。 如果该消息表示的工作任务，可以使用此功能来更新工作任务的状态。 下面的代码使用新内容，更新队列消息并设置可见性超时值来扩展另一个 60 秒。 这样可以节省与消息关联的工作状态并为客户端提供了另一分钟，以继续对该消息进行处理。 您可以使用此技术来跟踪多个步骤的工作流队列消息，而无需从头开始如果由于硬件或软件故障而失败的处理步骤。 通常情况下，可以保持，重试计数，如果消息重试超过 n 次，则会删除它。 这样可以防止触发应用程序错误每次处理一条消息。  

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_conection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));
        
    // Get the message from the queue and update the message contents.
    // The visibility timeout "0" means make it visible immediately.
    // The visibility timeout "60" means the client can get another minute to continue 
    // working on the message.
    azure::storage::cloud_queue_message changed_message = queue.get_message();

    changed_message.set_content(U("Changed message"));
    queue.update_message(changed_message, std::chrono::seconds(60), true);

    // Output the message content.
    std::wcout << U("Changed message content: ") << changed_message.content_as_string() << std::endl;  

## 如何︰ 队下一封邮件
您的代码消除排队两个步骤中的队列中的消息。 在调用**get_message**时，您将获得下一条消息在队列中。 从**get_message**返回的消息成为看不到从该队列中读取消息的任何其他代码。 要从队列中移除该消息，则必须调用**delete_message**。 如果您的代码无法处理的消息，由于硬件或软件故障，您的代码的另一个实例可以得到同样的消息，然后再试，可以确保删除邮件这两步过程。 在处理完邮件后，您的代码将调用**delete_message**右。

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Get the next message.
    azure::storage::cloud_queue_message dequeued_message = queue.get_message();
    std::wcout << U("Dequeued message: ") << dequeued_message.content_as_string() << std::endl;

    // Delete the message.
    queue.delete_message(dequeued_message); 

## 方法︰ 利用取消队列邮件的其他选项
有两种方法，您可以自定义消息从队列中的检索。 首先，您可以获取一批消息 （最多 32 个)。 第二，您可以设置隐蔽性更长或更短的超时值，允许您的代码更多或更少的时间来充分处理每条消息。 下面的代码示例使用**get_messages**方法在一个调用中获取 20 个邮件。 然后它将处理每条消息使用**for**循环。 此外将隐蔽性超时设置为 5 分钟，每一封邮件。 请注意，所有邮件在同一时间，在 5 分钟启动后 5 分钟有传递由于调用**get_messages**，任何未被删除的消息再次将变为可见。  

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Dequeue some queue messages (maximum 32 at a time) and set their visibility timeout to 
    // 5 minutes (300 seconds).
    azure::storage::queue_request_options options;
    azure::storage::operation_context context;

    // Retrieve 20 messages from the queue with a visibility timeout of 300 seconds.
    std::vector<azure::storage::cloud_queue_message> messages = queue.get_messages(20, std::chrono::seconds(300), options, context);
        
    for (auto it = messages.cbegin(); it != messages.cend(); ++it)
    {
        // Display the contents of the message.
        std::wcout << U("Get: ") << it->content_as_string() << std::endl;
    }

## 如何︰ 获取队列长度
您可以队列中获取消息数的估计值。 **Download_attributes**方法询问队列服务检索队列属性，包括邮件计数。 **Approximate_message_count**方法获取队列中的消息的大致数目。  

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Fetch the queue attributes.
    queue.download_attributes();

    // Retrieve the cached approximate message count.
    int cachedMessageCount = queue.approximate_message_count();

    // Display number of messages.
    std::wcout << U("Number of messages in queue: ") << cachedMessageCount << std::endl;  

## 如何︰ 删除队列
要删除队列中包含的所有消息，请在队列对象上调用**delete_queue_if_exists**方法。

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // If the queue exists and delete it.
    queue.delete_queue_if_exists();  

## 下一步行动
现在，您已经学习了队列存储的基本知识，按照这些链接以了解更多关于 Azure 存储。  

-   [如何使用 c + + 中的 Blob 存储](storage-c-plus-plus-how-to-use-blobs.md)
-   [如何使用 c + + 中的表存储](storage-c-plus-plus-how-to-use-tables.md)
-   [在 c + + 的列表 Azure 存储资源](storage-c-plus-plus-enumeration.md)
-   [对于 c + + 引用存储客户端库](http://azure.github.io/azure-storage-cpp)
-   [Azure 存储文档](http://azure.microsoft.com/documentation/services/storage/)

 

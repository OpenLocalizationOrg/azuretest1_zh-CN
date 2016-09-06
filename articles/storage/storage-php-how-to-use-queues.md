---
ms.openlocfilehash: d160373c78f44a3d21716545433f4f63c5dd5ef1
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="如何使用队列存储 PHP 从 |Microsoft Azure"
    description="了解如何使用 Azure 队列存储服务来创建和删除队列，并插入、 获取，和删除消息。 示例是用 PHP 编写的。"
    documentationCenter="php"
    services="storage"
    authors="tfitzmac"
    manager="adinah"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="07/29/2015"
    ms.author="tomfitz"/>

# 如何使用 PHP 从队列存储

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]

## 概述

本指南将向您介绍如何通过使用 Azure 队列存储服务来执行常见的方案。 这些示例是类通过为 PHP 编写从 Windows SDK。 已覆盖的方案包括插入、 查看、 获取，并删除队列消息，以及创建和删除队列。

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## 创建一个 PHP 应用程序

创建 PHP 应用程序访问 Azure 队列存储唯一的要求是引用的 Azure SDK 中的类从 php 代码中。 任何开发工具可用于创建您的应用程序，其中包括记事本。

在本指南中，您将使用队列存储功能，可以在本地，PHP 应用程序中或在 Azure web 角色、 工作人员角色或网站中运行的代码中调用。

## 获取 Azure 的客户端库

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## 配置应用程序来访问队列存储

若要使用 Azure 队列存储 Api，您需要︰

1. 通过使用[require_once][require_once]语句的自动磁带加载机文件的引用。
2. 参考您可以使用任何类。

下面的示例演示如何引用**ServicesBuilder**类包含自动磁带加载机文件。

> [AZURE.NOTE]
> 本示例 （和本文中的其他示例） 假定您已安装了 Azure 作曲家通过 PHP 客户端库。 如果您已安装的手动或梨树打包库，您将需要引用`WindowsAzure.php`自动磁带加载机文件。

    require_once 'vendor\autoload.php';
    use WindowsAzure\Common\ServicesBuilder;


在下面示例中，`require_once`语句将显示始终，但将引用所需的示例执行的类。

## 设置 Azure 存储连接

要实例化的 Azure 队列存储的客户端，必须首先具有有效的连接字符串。 对于队列服务连接字符串的格式如下所示。

用于访问实时服务︰

    DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]

用于访问仿真器存储︰

    UseDevelopmentStorage=true


若要创建任何 Azure 服务客户端，您需要使用**ServicesBuilder**类。 您可以使用下列方法之一︰

* 连接字符串将直接传递给它。
* 使用**CloudConfigurationManager (CCM)**检查多个外部源的连接字符串︰
    * 默认情况下，它有一个外部源的支持 — — 环境变量。
    * 通过扩展的**ConnectionStringSource**类，您可以添加新的源。

对于此处列出的示例中，将直接传递连接字符串。

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;

    $queueRestProxy = ServicesBuilder::getInstance()->createQueueService($connectionString);


## 创建队列

**QueueRestProxy**对象，可以通过使用**createQueue**方法来创建队列。 在创建队列时，可以设置选项在队列中，但这样做很不必要。 （下面的示例演示如何设置队列上的元数据）。

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use WindowsAzure\Common\ServiceException;
    use WindowsAzure\Queue\Models\CreateQueueOptions;

    // Create queue REST proxy.
    $queueRestProxy = ServicesBuilder::getInstance()->createQueueService($connectionString);

    // OPTIONAL: Set queue metadata.
    $createQueueOptions = new CreateQueueOptions();
    $createQueueOptions->addMetaData("key1", "value1");
    $createQueueOptions->addMetaData("key2", "value2");

    try {
        // Create queue.
        $queueRestProxy->createQueue("myqueue", $createQueueOptions);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179446.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

> [AZURE.NOTE] 您不应依赖元数据密钥区分大小写。 所有密钥将都读取从服务中的小写字母。


## 向队列中添加消息

若要向队列中添加一条消息，使用**QueueRestProxy-> createMessage**。 方法采用队列名称、 消息文本和消息选项 （可选）。

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use WindowsAzure\Common\ServiceException;
    use WindowsAzure\Queue\Models\CreateMessageOptions;

    // Create queue REST proxy.
    $queueRestProxy = ServicesBuilder::getInstance()->createQueueService($connectionString);

    try {
        // Create message.
        $builder = new ServicesBuilder();
        $queueRestProxy->createMessage("myqueue", "Hello World!");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179446.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## 查看下一条消息

您可以查看邮件 （或邮件） 在前面的队列而无需通过调用**QueueRestProxy-> peekMessages**队列中移除它。 默认情况下， **peekMessage**方法将返回一条消息，但您可以通过使用**PeekMessagesOptions-> setNumberOfMessages**方法更改该值。

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use WindowsAzure\Common\ServiceException;
    use WindowsAzure\Queue\Models\PeekMessagesOptions;

    // Create queue REST proxy.
    $queueRestProxy = ServicesBuilder::getInstance()->createQueueService($connectionString);

    // OPTIONAL: Set peek message options.
    $message_options = new PeekMessagesOptions();
    $message_options->setNumberOfMessages(1); // Default value is 1.

    try {
        $peekMessagesResult = $queueRestProxy->peekMessages("myqueue", $message_options);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179446.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

    $messages = $peekMessagesResult->getQueueMessages();

    // View messages.
    $messageCount = count($messages);
    if($messageCount <= 0){
        echo "There are no messages.<br />";
    }
    else{
        foreach($messages as $message)  {
            echo "Peeked message:<br />";
            echo "Message Id: ".$message->getMessageId()."<br />";
            echo "Date: ".date_format($message->getInsertionDate(), 'Y-m-d')."<br />";
            echo "Message text: ".$message->getMessageText()."<br /><br />";
        }
    }

## 队的下一个邮件

您的代码从两个步骤中的队列中移除消息。 首先，调用**QueueRestProxy-> listMessages**，从而看不到任何其他代码都从队列中读取消息。 默认情况下，此消息将 30 秒保持不可见。 （如果在此时间段内未删除该消息，它将再次变为可见的队列上。）要从队列中移除该消息，则必须调用**QueueRestProxy-> deleteMessage**。 删除邮件这两步过程可确保当您的代码无法处理由于硬件或软件故障的消息，您的代码的另一个实例可以得到同样的消息，然后再试。 在处理完邮件后，您的代码将调用**deleteMessage**右。

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use WindowsAzure\Common\ServiceException;

    // Create queue REST proxy.
    $queueRestProxy = ServicesBuilder::getInstance()->createQueueService($connectionString);

    // Get message.
    $listMessagesResult = $queueRestProxy->listMessages("myqueue");
    $messages = $listMessagesResult->getQueueMessages();
    $message = $messages[0];

    /* ---------------------
        Process message.
       --------------------- */

    // Get message ID and pop receipt.
    $messageId = $message->getMessageId();
    $popReceipt = $message->getPopReceipt();

    try {
        // Delete message.
        $queueRestProxy->deleteMessage("myqueue", $messageId, $popReceipt);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179446.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## 更改队列的消息的内容

您可以通过调用**QueueRestProxy-> updateMessage**更改消息就地在队列中的内容。 如果该消息表示的工作任务，可以使用此功能来更新工作任务的状态。 下面的代码以新的内容，更新队列消息并设置可见性超时值来扩展另一个 60 秒。 这保存的与消息相关联的工作状态并为客户端提供另一分钟，以继续对该消息进行处理。 您可以使用此技术来跟踪多个步骤的工作流队列消息，而无需从头开始如果由于硬件或软件故障而失败的处理步骤。 通常情况下，可以保持，重试计数，如果多于*n*次重试消息，则将删除它。 这样可以防止触发应用程序错误每次处理一条消息。

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use WindowsAzure\Common\ServiceException;

    // Create queue REST proxy.
    $queueRestProxy = ServicesBuilder::getInstance()->createQueueService($connectionString);

    // Get message.
    $listMessagesResult = $queueRestProxy->listMessages("myqueue");
    $messages = $listMessagesResult->getQueueMessages();
    $message = $messages[0];

    // Define new message properties.
    $new_message_text = "New message text.";
    $new_visibility_timeout = 5; // Measured in seconds.

    // Get message ID and pop receipt.
    $messageId = $message->getMessageId();
    $popReceipt = $message->getPopReceipt();

    try {
        // Update message.
        $queueRestProxy->updateMessage("myqueue",
                                    $messageId,
                                    $popReceipt,
                                    $new_message_text,
                                    $new_visibility_timeout);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179446.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## 用于取消排队的邮件的其他选项

有两种方法，您可以自定义消息从队列中的检索。 首先，您可以获取一批消息 （最多 32 个)。 第二，您可以设置更长或更短的可见性超时值，允许您的代码更多或更少的时间来充分处理每条消息。 下面的代码示例使用**getMessages**方法在一个调用中获取 16 的消息。 然后它使用一个**for**循环处理每个消息。 此外将隐蔽性超时设置为 5 分钟，每一封邮件。

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use WindowsAzure\Common\ServiceException;
    use WindowsAzure\Queue\Models\ListMessagesOptions;

    // Create queue REST proxy.
    $queueRestProxy = ServicesBuilder::getInstance()->createQueueService($connectionString);

    // Set list message options.
    $message_options = new ListMessagesOptions();
    $message_options->setVisibilityTimeoutInSeconds(300);
    $message_options->setNumberOfMessages(16);

    // Get messages.
    try{
        $listMessagesResult = $queueRestProxy->listMessages("myqueue",
                                                         $message_options);
        $messages = $listMessagesResult->getQueueMessages();

        foreach($messages as $message){

            /* ---------------------
                Process message.
            --------------------- */

            // Get message Id and pop receipt.
            $messageId = $message->getMessageId();
            $popReceipt = $message->getPopReceipt();

            // Delete message.
            $queueRestProxy->deleteMessage("myqueue", $messageId, $popReceipt);
        }
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179446.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## 获取队列长度

您可以队列中获取消息数的估计值。 **QueueRestProxy-> getQueueMetadata**方法询问队列服务返回有关该队列的元数据。 在返回的对象上调用**getApproximateMessageCount**方法提供了在队列中的消息数的计数。 由于消息可以添加或删除队列服务响应您的请求后，计数才近似。

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use WindowsAzure\Common\ServiceException;

    // Create queue REST proxy.
    $queueRestProxy = ServicesBuilder::getInstance()->createQueueService($connectionString);

    try {
        // Get queue metadata.
        $queue_metadata = $queueRestProxy->getQueueMetadata("myqueue");
        $approx_msg_count = $queue_metadata->getApproximateMessageCount();
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179446.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

    echo $approx_msg_count;

## 删除队列

要删除队列中的所有消息，请致电**QueueRestProxy-> deleteQueue**方法。

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use WindowsAzure\Common\ServiceException;

    // Create queue REST proxy.
    $queueRestProxy = ServicesBuilder::getInstance()->createQueueService($connectionString);

    try {
        // Delete queue.
        $queueRestProxy->deleteQueue("myqueue");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179446.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }


## 下一步行动

现在，您已经学习了 Azure 队列存储的基本知识，按照这些链接以了解更复杂的存储任务︰

- 请参阅 MSDN 参考[Azure 存储](http://msdn.microsoft.com/library/azure/gg433040.aspx)。
- 访问[Azure 存储团队博客](http://blogs.msdn.com/b/windowsazurestorage/)。

[下载]: http://go.microsoft.com/fwlink/?LinkID=252473
[require_once]: http://www.php.net/manual/en/function.require-once.php
[Azure 的管理门户]: http://manage.windowsazure.com/
[存储和访问 Azure 中的数据]: http://msdn.microsoft.com/library/azure/gg433040.aspx

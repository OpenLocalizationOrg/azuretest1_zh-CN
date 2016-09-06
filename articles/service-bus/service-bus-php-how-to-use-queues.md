---
ms.openlocfilehash: a1b76a2a05a09ac7887ee0aa4a12c29e50913c6c
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何使用服务总线队列 (PHP) |Microsoft Azure" 
    description="了解如何使用在 Azure 服务总线队列。 用 PHP 编写的代码样本。" 
    services="service-bus" 
    documentationCenter="php" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="PHP" 
    ms.topic="article" 
    ms.date="07/06/2015" 
    ms.author="sethm"/>

# 如何使用服务总线队列

本指南介绍了如何使用服务总线队列。 示例用 PHP 编写的并使用[PHP 的 Azure SDK](../php-download-sdk.md)。 所包含的方案包括**创建队列**、**发送和接收邮件**和**删除队列**。

[AZURE.INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

## 创建一个 PHP 应用程序

创建 PHP 应用程序访问 Azure Blob 服务唯一的要求是在[PHP 的 Azure SDK](../php-download-sdk.md)从您的代码中的类的引用。 任何开发工具可用于创建您的应用程序或记事本。

> [AZURE.NOTE] 您的 PHP 安装还必须[OpenSSL 扩展](http://php.net/openssl)安装并启用。

在本指南中，您将使用服务功能，可以从本地，PHP 应用程序中或在 Azure web 角色、 工作人员角色或网站中运行的代码中调用。

## 获取 Azure 的客户端库

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## 配置应用程序使用服务总线

要使用 Azure 服务总线队列 Api，请执行以下操作︰

1. 使用[require_once][require_once]语句的自动磁带加载机文件的引用。
2. 参考您可以使用任何类。

下面的示例演示如何引用**ServicesBuilder**类包含自动磁带加载机文件。

> [AZURE.NOTE] 本示例 （和本文中的其他示例） 假定安装了 Azure 作曲家通过 PHP 客户端库。 如果您已安装的手动或梨树打包库，您必须引用**WindowsAzure.php**自动磁带加载机文件。

    require_once 'vendor\autoload.php';
    use WindowsAzure\Common\ServicesBuilder;

在下面示例中，`require_once`将始终显示语句，但仅执行该示例所需的类引用。

## 设置 Azure 服务总线连接

来实例化服务总线客户端，必须首先以这种格式具有有效的连接字符串︰

    Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]

**终结点**位置的格式通常是`https://[yourNamespace].servicebus.windows.net`。

若要创建任何 Azure 服务客户端必须使用**ServicesBuilder**类。 您可以：

* 连接字符串将直接传递给它。
* 使用**CloudConfigurationManager (CCM)**检查多个外部源的连接字符串︰
    * 默认情况下它有一个外部源的环境变量对的支持
    * 通过扩展**ConnectionStringSource**类可以添加新的源

对于此处列出的示例中，将直接传递连接字符串。

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;

    $connectionString = "Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]";

    $serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);


## 如何︰ 创建队列

您还可以通过**ServiceBusRestProxy**类的服务总线队列的管理操作。 通过使用适当的连接字符串封装的令牌的权限来管理它的**ServicesBuilder::createServiceBusService**工厂方法构造一个**ServiceBusRestProxy**对象。

下面的示例演示如何实例化**ServiceBusRestProxy**并调用**ServiceBusRestProxy-> createQueue**来创建队列名为`myqueue`在`MySBNamespace`服务命名空间︰

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use WindowsAzure\Common\ServiceException;
    use WindowsAzure\ServiceBus\Models\QueueInfo;

    // Create Service Bus REST proxy.
        $serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
    
    try {
        $queueInfo = new QueueInfo("myqueue");
        
        // Create queue.
        $serviceBusRestProxy->createQueue($queueInfo);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here: 
        // http://msdn.microsoft.com/library/windowsazure/dd179357
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

> [AZURE.NOTE] 您可以使用`listQueues`方法在`ServiceBusRestProxy`对象来检查服务命名空间中是否已存在具有指定名称的队列。

## 如何︰ 向队列发送消息

若要将消息发送到服务总线队列，应用程序调用的**ServiceBusRestProxy-> sendQueueMessage**方法。 下面的代码演示如何将消息发送到`myqueue`在以前创建的队列`MySBNamespace`服务命名空间。

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use WindowsAzure\Common\ServiceException;
    use WindowsAzure\ServiceBus\Models\BrokeredMessage;

    // Create Service Bus REST proxy.
    $serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
        
    try {
        // Create message.
        $message = new BrokeredMessage();
        $message->setBody("my message");
    
        // Send message.
        $serviceBusRestProxy->sendQueueMessage("myqueue", $message);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here: 
        // http://msdn.microsoft.com/library/windowsazure/hh780775
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

邮件发送到的 （和从接收到的） 服务总线队列是**BrokeredMessage**类的实例。 **BrokeredMessage**对象都有一套标准的方法 （如**getLabel**、 **getTimeToLive**、 **setLabel**和**setTimeToLive**） 和用于保存自定义应用程序特定的属性和任意应用程序数据的正文的属性。

服务总线队列支持的最大邮件大小为 256 KB （头，其中包括标准和自定义应用程序属性，可以有的最大大小为 64 KB）。 保留在队列中的消息的数量没有限制，但持有队列消息的总大小没有一个盖帽。 此队列大小的上限为 5 GB。

## 如何从队列接收消息

从队列接收消息的最好方法是使用**ServiceBusRestProxy-> receiveQueueMessage**方法。 在两个不同的模式可以接收消息︰ **ReceiveAndDelete** （默认值） 和**PeekLock**。

当使用**ReceiveAndDelete**模式，收到是一拍摄操作的即当服务总线队列中接收消息读取的请求，则它将邮件标记为正在使用并将其返回给应用程序。 **ReceiveAndDelete**模式是最简单的模型，最适合的方案，在其中一个应用程序能够容忍不处理出现故障的消息。 要理解这一点，考虑一个方案，其中使用者发出接收请求，然后处理它之前崩溃。 因为服务总线将被标记为正在使用的消息，然后当该应用程序将重新启动，并开始使用消息再次强调，它将丢失了消耗在崩溃之前的消息。

在**PeekLock**模式中，接收一条消息将成为一个两个阶段的操作，使得支持的应用程序不能容忍丢失邮件可以。 服务总线接收请求时，它查找下一条消息中，要消耗就锁定以防止其他使用者接收它，，然后将它返回给应用程序。 应用程序完成处理消息 （或可靠地存储以备将来处理后），它会通过将收到的邮件传递给**ServiceBusRestProxy-> deleteMessage**完成接收过程的第二个阶段。 当服务总线看到调用**deleteMessage**时，将标记为所消耗的消息并从队列中移除它。

下面的示例显示一条消息可以接收和处理使用**PeekLock**模式 （不是默认模式） 的方式。

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use WindowsAzure\Common\ServiceException;
    use WindowsAzure\ServiceBus\Models\ReceiveMessageOptions;

    // Create Service Bus REST proxy.
    $serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
        
    try {
        // Set the receive mode to PeekLock (default is ReceiveAndDelete).
        $options = new ReceiveMessageOptions();
        $options->setPeekLock();
        
        // Receive message.
        $message = $serviceBusRestProxy->receiveQueueMessage("myqueue", $options);
        echo "Body: ".$message->getBody()."<br />";
        echo "MessageID: ".$message->getMessageId()."<br />";
        
        /*---------------------------
            Process message here.
        ----------------------------*/
        
        // Delete message. Not necessary if peek lock is not set.
        echo "Message deleted.<br />";
        $serviceBusRestProxy->deleteMessage($message);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/windowsazure/hh780735
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## 如何︰ 处理应用程序崩溃和无法读取消息

服务总线提供的功能来帮助您从您的应用程序或困难处理消息中的错误正常恢复。 如果接收方应用程序不能处理该消息，由于某种原因，然后它可以接收消息 （而不是**deleteMessage**方法） 调用**unlockMessage**方法。 这将导致服务总线来解锁队列中的邮件并使其可再次，同一个使用应用程序或其他使用的应用程序接收。

此外没有与消息在队列中，锁定超时，并且如果应用程序不能处理该消息之前锁定超时过期 （例如，如果应用程序崩溃时），则服务总线将自动解锁消息并使其可再次收到。

在该应用程序崩溃之后处理该消息，但**deleteMessage**请求发出之前，然后消息将被重新传递给应用程序重新启动时。 这通常称为**至少一次处理**;即至少一次处理每个消息，但在某些情况下可能重新发送同样的消息。 如果情况不能容忍重复处理，然后将其他逻辑添加到应用程序可以处理重复的邮件传递建议。 这通常可以实现使用**getMessageId**方法将保持不变在传递尝试的邮件。

## 下一步行动

现在，学服务总线队列的基础知识，请参阅 MSDN 主题[队列、 主题和订阅][]的详细信息。

[服务总线队列图]: ../../../DevCenter/Java/Media/SvcBusQueues_01_FlowDiagram.jpg
[Azure 的管理门户]: http://manage.windowsazure.com/
[服务总线节点屏幕抓图]: ../../../DevCenter/Java/Media/SvcBusQueues_02_SvcBusNode.jpg
[创建新 Namespace 屏幕抓图]: ../../../DevCenter/Java/Media/SvcBusQueues_03_CreateNewSvcNamespace.jpg
[可用的命名空间的屏幕抓图]: ../../../DevCenter/Java/Media/SvcBusQueues_04_SvcBusNode_AvailNamespaces.jpg
[Namespace 列表屏幕抓图]: ../../../DevCenter/Java/Media/SvcBusQueues_05_NamespaceList.jpg
[属性窗格中的屏幕快照]: ../../../DevCenter/Java/Media/SvcBusQueues_06_PropertiesPane.jpg
[默认密钥屏幕抓图]: ../../../DevCenter/Java/Media/SvcBusQueues_07_DefaultKey.jpg
[队列、 主题和订阅]: http://msdn.microsoft.com/library/azure/hh367516.aspx
[require_once]: http://php.net/require_once

 

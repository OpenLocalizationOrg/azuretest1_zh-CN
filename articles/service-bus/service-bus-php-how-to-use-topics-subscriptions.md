---
ms.openlocfilehash: 29d1a396456b5c24c956e6817c54193abad5c09c
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何使用服务总线主题 (PHP) |Microsoft Azure" 
    description="学习如何使用 PHP 在 Azure 服务总线主题。" 
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


# 如何使用服务总线主题和订阅

本指南介绍了如何使用服务总线主题和订阅。 示例用 PHP 编写的并使用[PHP 的 Azure SDK](../php-download-sdk.md)。 所包含的方案包括**创建主题和订阅**、**创建订阅筛选器**、**发送邮件的主题**、**接收来自订阅邮件**并**删除主题和订阅**。

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## 创建一个 PHP 应用程序

创建 PHP 应用程序访问 Azure Blob 服务唯一的要求是在[PHP 的 Azure SDK](../php-download-sdk.md)从您的代码中的类的引用。 任何开发工具可用于创建您的应用程序或记事本。

> [AZURE.NOTE] 您的 PHP 安装还必须[OpenSSL 扩展](http://php.net/openssl)安装并启用。

在本指南中，您将使用服务功能，可以在本地，PHP 应用程序中或在 Azure web 角色、 工作人员角色或网站中运行的代码中调用。

## 获取 Azure 的客户端库

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## 配置应用程序使用服务总线

若要使用服务总线 Api:

1. 引用使用[require_once]自动磁带加载机文件[要求-一旦]语句。
2. 参考您可以使用任何类。

下面的示例演示如何引用**ServiceBusService**类包含自动磁带加载机文件。

> [AZURE.NOTE] 本示例 （和本文中的其他示例） 假定安装了 Azure 作曲家通过 PHP 客户端库。 如果您已安装的手动或梨树打包库，您必须引用**WindowsAzure.php**自动磁带加载机文件。

    require_once 'vendor\autoload.php';
    use WindowsAzure\Common\ServicesBuilder;

在下面示例中，`require_once`将始终显示语句，但仅执行该示例所需的类引用。

## 设置服务总线连接

要实例化的 Azure 服务总线的客户端必须首先以这种格式具有有效的连接字符串︰

    Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]

**终结点**位置的格式通常是`https://[yourNamespace].servicebus.windows.net`。

若要创建任何 Azure 服务客户端必须使用**ServicesBuilder**类。 您可以：

* 连接字符串将直接传递给它。
* 使用**CloudConfigurationManager (CCM)**检查多个外部源的连接字符串︰
    * 默认情况下附带一个外部来源的环境变量中的支持。
    * 通过扩展的**ConnectionStringSource**类，您可以添加新的源。

对于此处列出的示例中，将直接传递连接字符串。

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    
    $connectionString = "Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]";

    $serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

## 如何︰ 创建主题

通过**ServiceBusRestProxy**类可以进行服务总线主题的管理操作。 通过使用适当的连接字符串封装的令牌的权限来管理它的**ServicesBuilder::createServiceBusService**工厂方法构造一个**ServiceBusRestProxy**对象。

下面的示例演示如何实例化**ServiceBusRestProxy**并调用**ServiceBusRestProxy-> createTopic**来创建名为的主题`mytopic`在`MySBNamespace`服务命名空间︰

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use WindowsAzure\Common\ServiceException;
    use WindowsAzure\ServiceBus\Models\TopicInfo;
    
    // Create Service Bus REST proxy.
    $serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
    
    try {       
        // Create topic.
        $topicInfo = new TopicInfo("mytopic");
        $serviceBusRestProxy->createTopic($topicInfo);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here: 
        // http://msdn.microsoft.com/library/windowsazure/dd179357
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

> [AZURE.NOTE] 您可以使用`listTopics`方法在`ServiceBusRestProxy`对象来检查服务命名空间中是否已存在具有指定名称的主题。

## 如何︰ 创建订阅

此外与**ServiceBusRestProxy-> createSubscription**方法创建主题订阅。 被命名为订阅，并且可以具有可选的筛选，用于限制的消息传递给订阅的虚拟队列集。

### 使用默认值 (MatchAll) 筛选器创建订阅

**MatchAll**过滤器是如果没有筛选器指定创建新订阅时使用的默认筛选器。 当使用**MatchAll**筛选器时，预订的虚拟队列中放置发布到该主题的所有邮件。 下面的示例创建一个名为 mysubscription 的订阅，并使用默认**MatchAll**筛选器。

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use WindowsAzure\Common\ServiceException;
    use WindowsAzure\ServiceBus\Models\SubscriptionInfo;

    // Create Service Bus REST proxy.
    $serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
    
    try {
        // Create subscription.
        $subscriptionInfo = new SubscriptionInfo("mysubscription");
        $serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here: 
        // http://msdn.microsoft.com/library/windowsazure/dd179357
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

### 使用筛选器创建订阅

您还可以设置筛选器，使您可以指定哪些消息发送到一个主题应显示特定主题订阅中。 最灵活的一种支持订阅筛选器是**SqlFilter**，它实现的 SQL92 子集。 SQL 筛选器操作的属性发布到该主题的消息。 有关 SqlFilters 的详细信息，请参阅[SqlFilter.SqlExpression 属性][sqlfilter]。

> [AZURE.NOTE] 每个订阅上的规则将其结果消息添加到订阅独立，处理进来的消息。 此外，每个新的预订已将所有消息的主题都添加到订阅的筛选器的默认**规则**对象。 若要接收您的筛选器匹配的邮件，您必须删除默认规则。 您可以通过删除默认规则`ServiceBusRestProxy->deleteRule`方法。

下面的示例创建名为**HighMessages** **SqlFilter** ，只选择具有一个自定义的**MessageNumber**属性大于 3 的消息与订阅 (请参阅[如何︰ 将消息发送到一个主题](#SendMessage)将自定义属性添加到消息有关的信息):

    $subscriptionInfo = new SubscriptionInfo("HighMessages");
    $serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

    $serviceBusRestProxy->deleteRule("mytopic", "HighMessages", '$Default');

    $ruleInfo = new RuleInfo("HighMessagesRule");
    $ruleInfo->withSqlFilter("MessageNumber > 3");
    $ruleResult = $serviceBusRestProxy->createRule("mytopic", "HighMessages", $ruleInfo);

注意上面的代码要求使用其他的命名空间︰ `WindowsAzure\ServiceBus\Models\SubscriptionInfo`。

同样，下面的示例创建名为**LowMessages**与**SqlFilter** ，只选择具有到**MessageNumber**属性小于或等于 3 的消息的订阅︰

    $subscriptionInfo = new SubscriptionInfo("LowMessages");
    $serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

    $serviceBusRestProxy->deleteRule("mytopic", "LowMessages", '$Default');

    $ruleInfo = new RuleInfo("LowMessagesRule");
    $ruleInfo->withSqlFilter("MessageNumber <= 3");
    $ruleResult = $serviceBusRestProxy->createRule("mytopic", "LowMessages", $ruleInfo);

现在，一条消息发送到`mytopic`主题，它总是发送到接收者订阅`mysubscription`订阅，并有选择地向接收者同意传送`HighMessages`和`LowMessages`订阅 （根据邮件内容）。

## 如何︰ 将消息发送到一个主题

若要将消息发送到服务总线主题，您的应用程序调用的**ServiceBusRestProxy-> sendTopicMessage**方法。 下面的代码演示如何将消息发送到`mytopic`在以前创建的主题`MySBNamespace`服务命名空间。

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
        $serviceBusRestProxy->sendTopicMessage("mytopic", $message);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here: 
        // http://msdn.microsoft.com/library/windowsazure/hh780775
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

消息发送到服务总线主题是**BrokeredMessage**类的实例。 **BrokeredMessage**对象具有一组标准属性和方法 （如**getLabel**、 **getTimeToLive**、 **setLabel**和**setTimeToLive**），可用于保存应用程序专用的自定义属性的属性。 下面的示例演示如何以 5 测试将消息发送到`mytopic`以前创建的主题。 使用**setProperty**方法以添加自定义属性 (`MessageNumber`) 向每条消息。 请注意，`MessageNumber`属性值会有所不同的每封邮件 (您可以使用此值来确定哪些订阅接收，如中所示[如何︰ 创建订阅](#CreateSubscription)节):

    for($i = 0; $i < 5; $i++){
        // Create message.
        $message = new BrokeredMessage();
        $message->setBody("my message ".$i);
            
        // Set custom property.
        $message->setProperty("MessageNumber", $i);
            
        // Send message.
        $serviceBusRestProxy->sendTopicMessage("mytopic", $message);
    }

服务总线队列支持的最大邮件大小为 256 KB （头，其中包括标准和自定义应用程序属性，可以有的最大大小为 64 KB）。 保留在队列中的消息的数量没有限制，但持有队列消息的总大小没有一个盖帽。 此队列大小的上限为 5 GB。

## 如何︰ 从订阅接收消息

从订阅接收消息的最好方法是使用**ServiceBusRestProxy-> receiveSubscriptionMessage**方法。 收到的邮件可以工作在两个不同的模式︰ **ReceiveAndDelete** （默认值） 和**PeekLock**。

当使用**ReceiveAndDelete**模式，收到是一拍摄操作的即当服务总线在订阅接收读取的请求消息，则它将邮件标记为正在使用并将其返回给应用程序。 **ReceiveAndDelete**模式是最简单的模型，最适合的方案，在其中一个应用程序能够容忍不处理出现故障的消息。 要理解这一点，考虑一个方案，其中使用者发出接收请求，然后处理它之前崩溃。 因为服务总线将被标记为正在使用的消息，然后当该应用程序将重新启动，并开始使用消息再次强调，它将丢失了消耗在崩溃之前的消息。

在**PeekLock**模式中，接收一条消息将成为一个两个阶段的操作，使得支持的应用程序不能容忍丢失邮件可以。 服务总线接收请求时，它查找下一条消息中，要消耗就锁定以防止其他使用者，接收它，，然后将它返回给应用程序。 应用程序完成处理消息 （或可靠地存储以备将来处理后），它会通过将收到的邮件传递给**ServiceBusRestProxy-> deleteMessage**完成接收过程的第二个阶段。 当服务总线看到调用**deleteMessage**时，将标记为所消耗的消息并从队列中移除它。

下面的示例显示一条消息可以接收和处理使用**PeekLock**模式 （不是默认模式） 的方式。 

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use WindowsAzure\Common\ServiceException;
    use WindowsAzure\ServiceBus\Models\ReceiveMessageOptions;

    // Create Service Bus REST proxy.
    $serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
        
    try {
        // Set receive mode to PeekLock (default is ReceiveAndDelete)
        $options = new ReceiveMessageOptions();
        $options->setPeekLock();
    
        // Get message.
        $message = $serviceBusRestProxy->receiveSubscriptionMessage("mytopic", 
                                                                    "mysubscription", 
                                                                    $options);
        echo "Body: ".$message->getBody()."<br />";
        echo "MessageID: ".$message->getMessageId()."<br />";
        
        /*---------------------------
            Process message here.
        ----------------------------*/
        
        // Delete message. Not necessary if peek lock is not set.
        echo "Deleting message...<br />";
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

在该应用程序崩溃之后处理该消息，但**deleteMessage**请求发出之前，然后消息将被重新传递给应用程序重新启动时。 这通常称为**至少一次处理**;即至少一次处理每个消息，但在某些情况下可能重新发送同样的消息。 如果该方案不能容忍重复处理，应用程序开发人员应该应用程序可以处理重复的邮件传递到添加额外的逻辑。 这通常可以实现使用**getMessageId**方法将保持不变在传递尝试的邮件。

## 如何删除主题和订阅

若要删除某个主题或订阅， **ServiceBusRestProxy-> deleteTopic**或**ServiceBusRestProxy-> deleteSubscripton**方法中，分别使用。 请注意，删除一个主题也会删除注册该主题的所有订阅。

下面的示例演示如何删除一个主题 (`mytopic`) 和它的已注册的订阅。

    require_once 'vendor\autoload.php';

    use WindowsAzure\ServiceBus\ServiceBusService;
    use WindowsAzure\ServiceBus\ServiceBusSettings;
    use WindowsAzure\Common\ServiceException;

    // Create Service Bus REST proxy.
    $serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
    
    try {       
        // Delete topic.
        $serviceBusRestProxy->deleteTopic("mytopic");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here: 
        // http://msdn.microsoft.com/library/windowsazure/dd179357
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

通过使用**deleteSubscription**方法，您可以单独删除订阅︰

    $serviceBusRestProxy->deleteSubscription("mytopic", "mysubscription");

## 下一步行动

现在，学服务总线队列的基础知识，请参阅 MSDN 主题[队列、 主题和订阅][]的详细信息。

[什么是服务总线主题和订阅？]: #bkmk_WhatAreSvcBusTopics
[创建服务 Namespace]: #bkmk_CreateSvcNamespace
[Namespace 获得的默认管理凭据]: #bkmk_ObtainDefaultMngmntCredentials
[配置应用程序使用服务总线]: #bkmk_ConfigYourApp
[如何︰ 创建主题]: #bkmk_HowToCreateTopic
[如何︰ 创建订阅]: #bkmk_HowToCreateSubscrip
[如何︰ 将消息发送到一个主题]: #bkmk_HowToSendMsgs
[如何︰ 从订阅接收消息]: #bkmk_HowToReceiveMsgs
[如何︰ 处理应用程序崩溃和无法读取消息]: #bkmk_HowToHandleAppCrash
[如何︰ 删除主题和订阅]: #bkmk_HowToDeleteTopics
[下一步行动]: #bkmk_NextSteps
[服务总线主题图]: ../../../DevCenter/Java/Media/SvcBusTopics_01_FlowDiagram.jpg
[Azure 的管理门户]: http://manage.windowsazure.com/
[服务总线节点屏幕抓图]: ../../../DevCenter/dotNet/Media/sb-queues-03.png
[创建新 Namespace 屏幕抓图]: ../../../DevCenter/dotNet/Media/sb-queues-04.png
[Namespace 列表屏幕抓图]: ../../../DevCenter/dotNet/Media/sb-queues-05.png
[属性窗格中的屏幕快照]: ../../../DevCenter/dotNet/Media/sb-queues-06.png
[默认密钥屏幕抓图]: ../../../DevCenter/dotNet/Media/sb-queues-07.png
[队列、 主题和订阅]: http://msdn.microsoft.com/library/azure/hh367516.aspx
[可用的命名空间的屏幕抓图]: ../../../DevCenter/Java/Media/SvcBusQueues_04_SvcBusNode_AvailNamespaces.jpg
[sqlfilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
[要求的一次]: http://php.net/require_once
 

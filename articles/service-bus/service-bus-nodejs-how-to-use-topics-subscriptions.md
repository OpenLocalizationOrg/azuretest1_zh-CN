---
ms.openlocfilehash: 73061cc618741d43a26ab9642c607c721297922f
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何使用服务总线主题 (Node.js) |Microsoft Azure" 
    description="了解如何从 Node.js app 在 Azure 中使用服务总线主题和订阅。" 
    services="service-bus" 
    documentationCenter="nodejs" 
    authors="MikeWasson" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="07/02/2015" 
    ms.author="mwasson"/>


# 如何使用服务总线主题和订阅

本指南介绍了如何使用服务总线主题和 Node.js 应用程序中的订阅。 所包含的方案包括为主题、**接收来自订阅邮件**并**删除主题和订阅****创建主题和创建订阅筛选器，发送消息的订阅**。 主题和订阅的详细信息，请参阅[后续步骤](#next-steps)部分。

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## 创建 Node.js 应用程序

创建一个空白的 Node.js 应用。 创建 Node.js 应用程序的说明，请参阅[创建和部署 Node.js 应用到 Azure 网站]， [Node.js 云服务][Node.js 云服务]（使用 Windows PowerShell） 或与 WebMatrix 的网站。

## 配置应用程序使用服务总线

若要使用服务总线，下载 Node.js azure 程序包。 该软件包包括一组与服务总线 REST 服务通信的库。

### 使用节点程序包管理器 (NPM) 来获取包

1.  使用命令行界面**PowerShell** (Windows)，如**终端**(Mac，) 或**指责**(Unix)，定位到您在其中创建的示例应用程序的文件夹。

2.  键入**npm 安装 azure**在命令窗口中，应该产生下面的输出︰

         azure@0.7.5 node_modules\azure
        ├── dateformat@1.0.2-1.2.3
        ├── xmlbuilder@0.4.2
        ├── node-uuid@1.2.0
        ├── mime@1.2.9
        ├── underscore@1.4.4
        ├── validator@1.1.1
        ├── tunnel@0.0.2
        ├── wns@0.5.3
        ├── xml2js@0.2.7 (sax@0.5.2)
        └── request@2.21.0 (json-stringify-safe@4.0.0, forever-agent@0.5.0, aws-sign@0.3.0, tunnel-agent@0.3.0, oauth-sign@0.3.0, qs@0.6.5, cookie-jar@0.3.0, node-uuid@1.4.0, http-signature@0.9.11, form-data@0.0.8, hawk@0.13.1)

3.  您可以手动运行**ls**命令以验证**节点\_模块**文件夹的创建时间。 在该文件夹中查找**azure**软件包，其中包含您需要访问服务总线主题的库。

### 导入模块

使用记事本或其他文本编辑器中，添加以下应用程序的**server.js**文件的顶部︰

    var azure = require('azure');

### 设置服务总线连接

Azure 模块将读取环境变量 AZURE\_SERVICEBUS\_命名空间和 AZURE\_SERVICEBUS\_访问\_关键到您的 Azure 服务总线连接所需的信息。 如果没有设置这些环境变量，必须在调用**createServiceBusService**时指定的帐户信息。

Azure 云服务配置文件中设置环境变量的示例，请参见[Node.js 云存储服务]。

在 Azure 网站管理门户设置环境变量的示例，请参见[Node.js Web 应用程序与存储]

## 如何创建主题

**ServiceBusService**对象使您可以使用主题。 下面的代码创建一个**ServiceBusService**对象。 语句以导入 azure 模块后**server.js**文件的顶部附近添加它︰

    var serviceBusService = azure.createServiceBusService();

通过在**ServiceBusService**对象上调用**createTopicIfNotExists** ，（如果有的话，），则将返回指定的主题或将创建一个具有指定名称的新主题。 下面的代码使用**createTopicIfNotExists**来创建或连接到名为 MyTopic 的主题︰

    serviceBusService.createTopicIfNotExists('MyTopic',function(error){
        if(!error){
            // Topic was created or exists
            console.log('topic created or exists.');
        }
    });

**createServiceBusService**还支持其他的选项，使您可以重写默认主题设置，例如消息生存时间或最大主题的大小。 下面的示例演示演示将主题最大大小设置为 5 GB 1 分钟生存时间︰

    var topicOptions = {
            MaxSizeInMegabytes: '5120',
            DefaultMessageTimeToLive: 'PT1M'
        };

    serviceBusService.createTopicIfNotExists('MyTopic', topicOptions, function(error){
        if(!error){
            // topic was created or exists
        }
    });

### 筛选器

可选的过滤操作可以应用到使用**ServiceBusService**执行的操作。 筛选操作可以包括记录、 自动重试等。筛选器是实现具有签名的方法的对象︰

        function handle (requestOptions, next)

执行操作后其预处理对请求选项，该方法需要调用"下一步"将传递一个回调具有下列签名︰

        function (returnObject, finalCallback, next)

在此回调中，并且处理 returnObject （到服务器的请求响应），则回调需要调用下一步如果存在尝试继续处理其他筛选器，或者只需调用 finalCallback 否则攒服务调用。

实现重试逻辑的两个筛选器还包括 Azure SDK Node.js、 **ExponentialRetryPolicyFilter**和**LinearRetryPolicyFilter**。 下面创建一个**ServiceBusService**对象，该对象使用**ExponentialRetryPolicyFilter**:

    var retryOperations = new azure.ExponentialRetryPolicyFilter();
    var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);

## 如何创建订阅

此外带有**ServiceBusService**对象创建主题订阅。 被命名为订阅，并且可以具有可选的筛选，用于限制的邮件传递到预订的虚拟队列集。

> [AZURE.NOTE] 订阅是持久的和将继续存在直到或者它们或它们是相关联的主题，都将被删除。 如果您的应用程序中包含逻辑来创建的订阅，应首先检查是否订阅已存在通过使用**getSubscription**方法。

### 使用默认值 (MatchAll) 筛选器创建订阅

**MatchAll**过滤器是如果没有筛选器指定创建新订阅时使用的默认筛选器。 当使用**MatchAll**筛选器时，预订的虚拟队列中放置发布到该主题的所有邮件。 下面的示例创建一个名为 AllMessages 的订阅，并使用默认**MatchAll**筛选器。

    serviceBusService.createSubscription('MyTopic','AllMessages',function(error){
        if(!error){
            // subscription created
        }
    });

### 使用筛选器创建订阅

您还可以创建允许您将消息发送到一个主题范围应显示在特定主题订阅的筛选器。

最灵活的一种支持订阅筛选器是**SqlFilter**，它实现的 SQL92 子集。 SQL 筛选器操作的属性发布到该主题的消息。 对于有关可以与 SQL 筛选中使用表达式的详细信息，请查看[SqlFilter.SqlExpression][SqlFilter.SqlExpression]语法。

通过使用**ServiceBusService**对象的**createRule**方法，可以为订阅添加筛选器。 此方法允许您将新筛选器添加到现有订阅。

> [AZURE.NOTE]

> 由于默认筛选器自动应用于所有新的订阅，您必须首先删除默认筛选器或**MatchAll**将重写您可能指定的任何其他筛选器。 您可以通过使用**ServiceBusService**对象的**deleteRule**方法删除默认规则。

下面的示例创建名为 HighMessages 与仅选择具有自定义**messagenumber**属性大于 3 的消息**SqlFilter**订阅︰

    serviceBusService.createSubscription('MyTopic', 'HighMessages', function (error){
        if(!error){
            // subscription created
            rule.create();
        }
    });
    var rule={
        deleteDefault: function(){
            serviceBusService.deleteRule('MyTopic',
                'HighMessages', 
                azure.Constants.ServiceBusConstants.DEFAULT_RULE_NAME, 
                rule.handleError);
        },
        create: function(){
            var ruleOptions = {
                sqlExpressionFilter: 'messagenumber > 3'
            };
            rule.deleteDefault();
            serviceBusService.createRule('MyTopic', 
                'HighMessages', 
                'HighMessageFilter', 
                ruleOptions, 
                rule.handleError);
        },
        handleError: function(error){
            if(error){
                console.log(error)
            }
        }
    }

同样，下面的示例创建仅选择已到**messagenumber**属性小于或等于 3 的消息**SqlFilter**名为 LowMessages 的订阅︰

    serviceBusService.createSubscription('MyTopic', 'LowMessages', function (error){
        if(!error){
            // subscription created
            rule.create();
        }
    });
    var rule={
        deleteDefault: function(){
            serviceBusService.deleteRule('MyTopic',
                'LowMessages', 
                azure.Constants.ServiceBusConstants.DEFAULT_RULE_NAME, 
                rule.handleError);
        },
        create: function(){
            var ruleOptions = {
                sqlExpressionFilter: 'messagenumber <= 3'
            };
            rule.deleteDefault();
            serviceBusService.createRule('MyTopic', 
                'LowMessages', 
                'LowMessageFilter', 
                ruleOptions, 
                rule.handleError);
        },
        handleError: function(error){
            if(error){
                console.log(error)
            }
        }
    }

当一条消息，现在发送到 MyTopic，它总是将发送到 AllMessages 主题订阅，订阅和有选择地传送到接收器的接收器订阅的 HighMessages 和 LowMessages （根据邮件内容） 的主题订阅。

## 如何将消息发送到的主题

若要将消息发送到服务总线主题，您的应用程序必须使用**ServiceBusService**对象的**sendTopicMessage**方法。
消息发送到服务总线主题是**BrokeredMessage**对象。
**BrokeredMessage**对象具有标准属性 （如**标签**和**TimeToLive**），一套用于保存自定义应用程序特定属性的字典和正文中的字符串数据。 应用程序可以通过将一个字符串值传递给**sendTopicMessage**设置邮件正文和任何所需的标准属性将使用默认值填充。

下面的示例演示如何发送五个测试为 MyTopic 的消息。 请注意，每个消息的**messagenumber**属性值各不相同的循环迭代 （这将确定哪些订阅接收它）︰

    var message = {
        body: '',
        customProperties: {
            messagenumber: 0
        }
    }

    for (i = 0;i < 5;i++) {
        message.customProperties.messagenumber=i;
        message.body='This is Message #'+i;
        serviceBusService.sendTopicMessage(topic, message, function(error) {
          if (error) {
            console.log(error);
          }
        });
    }

服务总线主题支持的最大邮件大小为 256 MB （头，其中包括标准和自定义应用程序属性，可以有最大为 64 MB）。 保留在主题中的消息的数量没有限制，但上持有的主题的消息的总大小没有一个盖帽。 此主题大小定义在创建时，具有 5 GB 的上限。

## 如何从订阅接收消息

从订阅**ServiceBusService**对象上使用**receiveSubscriptionMessage**方法将接收消息。 默认情况下，邮件被删除订阅中读取;但是，您可以读取 (peek) 和锁定而不删除它从订阅**isPeekLock**可选参数设置为**true**的消息。

阅读和删除邮件接收操作的默认行为是最简单的模型，和最适合的方案，在其中一个应用程序能够容忍不处理出现故障的消息。 要理解这一点，考虑一个方案，其中使用者发出接收请求，然后处理它之前崩溃。 因为服务总线将被标记为正在使用的消息，然后当该应用程序将重新启动，并开始使用消息再次强调，它将丢失了消耗在崩溃之前的消息。

如果将**isPeekLock**参数设置为**true**，接收将成为两个阶段操作，这样就有可能支持的应用程序不能容忍丢失邮件。 服务总线接收请求时，它查找下一条消息中，要消耗就锁定以防止其他使用者，接收它，，然后将它返回给应用程序。
应用程序完成处理消息 （或可靠地存储以备将来处理后），它会完成接收过程的第二个阶段通过调用**deleteMessage**方法，并提供一个参数作为要删除的消息。 **DeleteMessage**方法将标记邮件所占用，并从订阅中移除它。

下面的示例演示消息可以接收和处理使用**receiveSubscriptionMessage**的方式。 该示例首先接收和邮件删除 LowMessages 的订阅，然后使用**isPeekLock**设置为 true 的 'HighMessages' 订阅中接收消息。 然后，它将删除使用**deleteMessage**的消息︰

    serviceBusService.receiveSubscriptionMessage('MyTopic', 'LowMessages', function(error, receivedMessage){
        if(!error){
            // Message received and deleted
            console.log(receivedMessage);
        }
    });
    serviceBusService.receiveSubscriptionMessage('MyTopic', 'HighMessages', { isPeekLock: true }, function(error, lockedMessage){
        if(!error){
            // Message received and locked
            console.log(lockedMessage);
            serviceBusService.deleteMessage(lockedMessage, function (deleteError){
                if(!deleteError){
                    // Message deleted
                    console.log('message has been deleted.');
                }
            }
        }
    });

## 如何处理应用程序崩溃和无法读取消息

服务总线提供的功能来帮助您从您的应用程序或困难处理消息中的错误正常恢复。 如果接收方应用程序不能处理该消息，由于某种原因，然后它可以调用**unlockMessage**方法在**ServiceBusService**对象上。 这将导致服务总线来解锁内订阅消息并使其可再次，同一个使用应用程序或其他使用的应用程序接收。

此外还有与内订阅，锁定超时，如果应用程序不能处理该消息之前锁定超时过期 （例如，如果应用程序崩溃时），则服务总线将自动解锁消息并使其可再次收到。

应用程序崩溃之后处理该消息，但之前调用**deleteMessage**方法，然后消息将被重新传递给应用程序重新启动时。 这通常称为**至少处理一次**，即将至少一次处理每个消息，但在某些情况下可能重新发送同样的消息。 如果方案不能容忍重复处理，应用程序开发人员应该向其应用程序来处理传递重复的邮件添加额外的逻辑。 这通常是使用将保持不变在传递尝试的邮件的**邮件 Id**属性实现的。

## 如何删除主题和订阅

主题和订阅是持久的必须通过 Azure 管理门户或通过编程方式显式删除。
下面的示例演示如何删除名为 MyTopic 的主题︰

    serviceBusService.deleteTopic('MyTopic', function (error) {
        if (error) {
            console.log(error);
        }
    });

删除一个主题时，将同时删除注册该主题的所有订阅。 也可以单独删除订阅。 下面的代码演示如何删除名 HighMessages 为 MyTopic 主题的订阅︰

    serviceBusService.deleteSubscription('MyTopic', 'HighMessages', function (error) {
        if(error) {
            console.log(error);
        }
    });

## 下一步行动

既然已经了解服务总线主题的基本知识，按照这些链接以了解详细信息。

-   请参阅 MSDN 参考︰[队列、 主题和订阅][]。
-   [SqlFilter][]的 API 参考。
-   在 GitHub 上访问[节点的 Azure SDK]资料库。

  [节点的 azure SDK]: https://github.com/WindowsAzure/azure-sdk-for-node
  [Azure 的管理门户]: http://manage.windowsazure.com
  [SqlFilter.SqlExpression]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
  [队列、 主题和订阅]: http://msdn.microsoft.com/library/azure/hh367516.aspx
  [SqlFilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx
  [Node.js 云服务]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
  [创建和部署的 Node.js 应用到 Azure 网站]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
  [使用存储的 Node.js 云服务]: /develop/nodejs/tutorials/web-app-with-storage/
  [Node.js Web 应用程序与存储]: /develop/nodejs/tutorials/web-site-with-storage/
 

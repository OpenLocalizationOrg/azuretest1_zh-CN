---
ms.openlocfilehash: 157b65cb198b265ed3d55e6e4c2c7ac2f5a4c6a0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何使用服务总线队列 (Node.js) |Microsoft Azure" 
    description="了解如何从 Node.js 应用程序使用 Azure 服务总线队列。" 
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
    ms.date="07/06/2015" 
    ms.author="mwasson"/>

# 如何使用服务总线队列

本指南介绍了如何使用服务总线队列。 这些示例用 JavaScript 编写的并使用 Node.js Azure 模块。 所包含的方案包括**创建队列、 发送和接收邮件**，并**删除队列**。 有关队列的详细信息，请参阅[后续步骤]部分。

[AZURE.INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

## 创建 Node.js 应用程序

创建一个空白的 Node.js 应用。 创建 Node.js 应用程序的说明，请参阅[创建和部署 Node.js 应用到 Azure 网站]，或 （使用 Windows PowerShell） [Node.js 云服务][Node.js 云服务]。

## 配置应用程序使用服务总线

若要使用 Azure 服务总线，请下载并使用 Node.js azure 的包。 这包括一组与服务总线 REST 服务通信的库。

### 使用节点程序包管理器 (NPM) 来获取包

1.  使用**Windows PowerShell 的 Node.js**命令窗口定位到**c:\\节点\\sbqueues\\WebRole1**在其中创建的示例应用程序的文件夹。

2.  **Npm 安装 azure**命令窗口中键入，这应导致类似于下面的输出︰

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

3.  您可以手动运行**ls**命令以验证**节点\_模块**文件夹的创建时间。 在该文件夹中查找**azure**软件包，其中包含您需要访问服务总线队列的库。

### 导入模块

使用记事本或其他文本编辑器中，添加以下应用程序的**server.js**文件的顶部︰

    var azure = require('azure');

### 设置 Azure 服务总线连接

Azure 模块读取环境变量 AZURE\_SERVICEBUS\_命名空间和 AZURE\_SERVICEBUS\_访问\_密钥，从而能够连接到服务总线所需的信息。 如果没有设置这些环境变量，必须在调用**createServiceBusService**时指定的帐户信息。

Azure 云服务配置文件中设置环境变量的示例，请参见[Node.js 云存储服务]。

在 Azure 网站管理门户设置环境变量的示例，请参见[Node.js Web 应用程序与存储]

## 如何创建队列

**ServiceBusService**对象使您能够使用队列。 下面的代码创建一个**ServiceBusService**对象。 语句以导入 Azure 模块后**server.js**文件的顶部附近添加它︰

    var serviceBusService = azure.createServiceBusService();

通过在**ServiceBusService**对象上调用**createQueueIfNotExists** ，（如果有的话，），则将返回指定的队列或将创建具有指定名称的新队列。 下面的代码使用**createQueueIfNotExists**来创建或连接到名为 myqueue 的队列︰

    serviceBusService.createQueueIfNotExists('myqueue', function(error){
        if(!error){
            // Queue exists
        }
    });

**createServiceBusService**还支持其他的选项，使您可以重写默认队列设置，例如实时或最大队列大小的邮件时间。 下面的示例将最大队列大小设置为 5 GB 和时间存活 1 分钟的值︰

    var queueOptions = {
          MaxSizeInMegabytes: '5120',
          DefaultMessageTimeToLive: 'PT1M'
        };

    serviceBusService.createQueueIfNotExists('myqueue', queueOptions, function(error){
        if(!error){
            // Queue exists
        }
    });

### 筛选器

可选的过滤操作可以应用到使用**ServiceBusService**执行的操作。 筛选操作可以包括记录、 自动重试等。筛选器是实现具有签名的方法的对象︰

        function handle (requestOptions, next)

方法必须调用请求选项及其预处理之后， `next`，传递一个回调具有下列签名︰

        function (returnObject, finalCallback, next)

在此回调中，并且处理**returnObject** （到服务器的请求响应），或者必须调用回调`next`如果存在继续处理其他筛选器，或只需调用`finalCallback`，其结束服务调用。

实现重试逻辑的两个筛选器还包括 Azure SDK Node.js、 **ExponentialRetryPolicyFilter**和**LinearRetryPolicyFilter**。 下面创建一个**ServiceBusService**对象，该对象使用**ExponentialRetryPolicyFilter**:

    var retryOperations = new azure.ExponentialRetryPolicyFilter();
    var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);

## 如何将消息发送到队列

若要将消息发送到服务总线队列，您的应用程序**ServiceBusService**对象上调用**sendQueueMessage**方法。 邮件发送到的 （和从接收到的） 服务总线队列是**BrokeredMessage**对象和有一套标准属性 （如**标签**和**TimeToLive**），用于保存自定义应用程序特定属性的字典和任意应用程序数据的主体。 应用程序可以通过将字符串作为邮件传递设置邮件的正文。 用默认值填充任何所需的标准属性。

下面的示例演示如何将测试消息发送到队列名为`myqueue`使用**sendQueueMessage**:

    var message = {
        body: 'Test message',
        customProperties: {
            testproperty: 'TestValue'
        }};
    serviceBusService.sendQueueMessage('myqueue', message, function(error){
        if(!error){
            // message sent
        }
    });

服务总线队列支持的最大邮件大小为 256 KB （头，其中包括标准和自定义应用程序属性，可以有的最大大小为 64 KB）。 保留在队列中的消息的数量没有限制，但持有队列消息的总大小没有一个盖帽。 此队列大小被定义在创建时，具有 5 GB 的上限。

## 如何从队列接收消息

从**ServiceBusService**对象上使用**receiveQueueMessage**方法队列接收消息。 默认情况下，邮件被删除从队列读取;但是，您可以读取 (peek) 和锁定消息但不从队列删除它通过**isPeekLock**可选参数设置为**true**。

阅读和删除邮件接收操作的默认行为是最简单的模型，和最适合的方案，在其中一个应用程序能够容忍不处理出现故障的消息。 要理解这一点，考虑一个方案，其中使用者发出接收请求，然后处理它之前崩溃。 因为服务总线将被标记为正在使用的消息，然后当该应用程序将重新启动，并开始使用消息再次强调，它将丢失了消耗在崩溃之前的消息。

如果将**isPeekLock**参数设置为**true**，接收将成为两个阶段操作，这样就有可能支持的应用程序不能容忍丢失邮件。 服务总线接收请求时，它查找下一条消息中，要消耗就锁定以防止其他使用者，接收它，，然后将它返回给应用程序。 应用程序完成处理消息 （或可靠地存储以备将来处理后），它会完成接收过程的第二个阶段通过调用**deleteMessage**方法，并提供一个参数作为要删除的消息。 **DeleteMessage**方法将标记所占用的消息并从队列中移除它。

下面的示例演示如何接收和处理消息使用**receiveQueueMessage**。 该示例首先接收和删除消息，收到一条消息，使用**isPeekLock**将设置为**true**，然后删除该邮件使用**deleteMessage**:

    serviceBusService.receiveQueueMessage('myqueue', function(error, receivedMessage){
        if(!error){
            // Message received and deleted
        }
    });
    serviceBusService.receiveQueueMessage('myqueue', { isPeekLock: true }, function(error, lockedMessage){
        if(!error){
            // Message received and locked
            serviceBusService.deleteMessage(lockedMessage, function (deleteError){
                if(!deleteError){
                    // Message deleted
                }
            });
        }
    });

## 如何处理应用程序崩溃和无法读取消息

服务总线提供的功能来帮助您从您的应用程序或困难处理消息中的错误正常恢复。 如果接收方应用程序不能处理该消息，由于某种原因，然后它可以调用**unlockMessage**方法在**ServiceBusService**对象上。 这将导致服务总线来解锁队列中的邮件并使其可再次，同一个使用应用程序或其他使用的应用程序接收。

此外没有与消息在队列中，锁定超时，并且如果应用程序不能处理该消息之前锁定超时过期 （例如，如果应用程序崩溃时），则服务总线将自动解锁消息并使其可再次收到。

应用程序崩溃之后处理该消息，但之前调用**deleteMessage**方法，然后消息将被重新传递给应用程序重新启动时。 这通常称为**至少处理一次**，即将至少一次处理每个消息，但在某些情况下可能重新发送同样的消息。 如果方案不能容忍重复处理，应用程序开发人员应该向其应用程序来处理传递重复的邮件添加额外的逻辑。 这通常是使用将保持不变在传递尝试的邮件的**邮件 Id**属性实现的。

## 下一步行动

现在，学服务总线队列的基本知识，按照这些链接以了解详细信息。

-   请参阅 MSDN 参考︰[队列、 主题和订阅。][]
-   在 GitHub 上访问[节点的 Azure SDK]资料库。

  [节点的 azure SDK]: https://github.com/Azure/azure-sdk-for-node
  [下一步行动]: #next-steps
  [服务总线队列有哪些？]: #what-are-service-bus-queues
  [创建服务 Namespace]: #create-a-service-namespace
  [Namespace 获得的默认管理凭据]: #obtain-default-credentials
  [创建 Node.js 应用程序]: #create-app
  [配置应用程序使用服务总线]: #configure-app
  [如何︰ 创建队列]: #create-queue
  [如何︰ 向队列发送消息]: #send-messages
  [如何︰ 从队列接收消息]: #receive-messages
  [如何︰ 处理应用程序崩溃和无法读取消息]: #handle-crashes
  [队列的概念]: ../../dotNet/Media/sb-queues-08.png
  [Azure 的管理门户]: http://manage.windowsazure.com
  
  [Node.js 云服务]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
  [队列、 主题和订阅。]: http://msdn.microsoft.com/library/azure/hh367516.aspx
  [创建和部署的 Node.js 应用到 Azure 网站]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
  [使用存储的 Node.js 云服务]: ../cloud-services/storage-nodejs-use-table-storage-cloud-service-app.md
  [Node.js Web 应用程序与存储]: ../storage/storage-nodejs-how-to-use-table-storage.md
 

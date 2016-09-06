---
ms.openlocfilehash: 7326965eac192c8c3ca23247c08c557aa23f2b66
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何使用队列存储从 Node.js |Microsoft Azure" 
    description="了解如何使用 Azure 队列服务创建和删除队列，并插入、 获取，和删除消息。 用 Node.js 编写的示例。" 
    services="storage" 
    documentationCenter="nodejs" 
    authors="MikeWasson" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="09/01/2015" 
    ms.author="mwasson"/>


# 如何使用队列存储从 Node.js

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]

## 概述

本指南介绍了如何执行常见方案使用 Microsoft Azure 队列服务。 这些示例是使用 Node.js API 来编写的。 所包含的方案包括**插入**、**查看**、**获取**、 和**删除**队列的消息，以及**创建和删除队列**。

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## 创建 Node.js 应用程序

创建一个空白的 Node.js 应用。 创建 Node.js 应用程序的说明，请参阅[创建和部署 Node.js 应用到 Azure 网站]， [Node.js 云服务][Node.js 云服务]（使用 Windows PowerShell） 或[与 WebMatrix 的网站]。

## 配置应用程序访问存储

若要使用 Azure 存储，需要 Node.js，其中包括一组与存储其他服务进行通信的便利库 Azure 存储 SDK。

### 使用节点程序包管理器 (NPM) 来获取包

1.  使用命令行界面**PowerShell** (Windows)，如**终端**(Mac，) 或**指责**(Unix)，定位到您在其中创建的示例应用程序的文件夹。

2.  在命令窗口中键入**npm 安装 azure 存储**。 命令的输出将类似于下面的示例。

        azure-storage@0.5.0 node_modules\azure-storage
        +-- extend@1.2.1
        +-- xmlbuilder@0.4.3
        +-- mime@1.2.11
        +-- node-uuid@1.4.3
        +-- validator@3.22.2
        +-- underscore@1.4.4
        +-- readable-stream@1.0.33 (string_decoder@0.10.31, isarray@0.0.1, inherits@2.0.1, core-util-is@1.0.1)
        +-- xml2js@0.2.7 (sax@0.5.2)
        +-- request@2.57.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, oauth-sign@0.8.0, tunnel-agent@0.4.1, isstream@0.1.2, json-stringify-safe@5.0.1, bl@0.9.4, combined-stream@1.0.5, qs@3.1.0, mime-types@2.0.14, form-data@0.2.0, http-signature@0.11.0, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)

3.  您可以手动运行**ls**命令以验证**节点\_模块**文件夹的创建时间。 在该文件夹中，您将找到**azure 存储**软件包，其中包含需要访问存储库。

### 导入包

使用记事本或其他文本编辑器，您打算使用存储应用程序的**server.js**文件的顶部添加以下︰

    var azure = require('azure-storage');

## 设置 Azure 存储连接

Azure 模块将读取环境变量 AZURE\_存储\_帐户和 AZURE\_存储\_访问\_键或 AZURE\_存储\_连接\_字符串连接到 Azure 存储帐户所需的信息。 如果没有设置这些环境变量，必须在调用**createQueueService**时指定的帐户信息。

在 Azure 网站管理门户设置环境变量的示例，请参见[Node.js Web 应用程序与存储]

## 如何︰ 创建队列

下面的代码创建一个**队列服务**对象，它使您能够使用队列。

    var queueSvc = azure.createQueueService();

使用**createQueueIfNotExists**方法，返回指定的队列，如果已经存在，或者如果不存在具有指定的名称创建的新队列。

    queueSvc.createQueueIfNotExists('myqueue', function(error, result, response){
      if(!error){
        // Queue created or exists
      }
    });

如果创建该队列，则`result`也是如此。 如果该队列存在，`result`为假。

### 筛选器

可选的过滤操作可以应用于使用**队列服务**执行的操作。 筛选操作可以包括记录、 自动重试等。筛选器是实现具有签名的方法的对象︰

        function handle (requestOptions, next)

执行操作后其预处理对请求选项，该方法需要调用"下一步"将传递一个回调具有下列签名︰

        function (returnObject, finalCallback, next)

在此回调中，并且处理 returnObject （到服务器的请求响应），则回调需要调用下一步如果存在尝试继续处理其他筛选器，或者只需调用 finalCallback 否则攒服务调用。

实现重试逻辑的两个筛选器还包括 Azure SDK Node.js、 **ExponentialRetryPolicyFilter**和**LinearRetryPolicyFilter**。 下面创建一个**队列服务**对象，使用**ExponentialRetryPolicyFilter**对象︰

    var retryOperations = new azure.ExponentialRetryPolicyFilter();
    var queueSvc = azure.createQueueService().withFilter(retryOperations);

## 如何︰ 将消息插入到队列

要插入到队列的消息，使用**createMessage**方法来创建新邮件并将其添加到队列。

    queueSvc.createMessage('myqueue', "Hello world!", function(error, result, response){
      if(!error){
        // Message inserted
      }
    });

## 如何︰ 查看下一条消息

您可以查看队列前面的消息但不从队列移除它通过调用**peekMessages**方法。 默认情况下， **peekMessages**查看单个消息。

    queueSvc.peekMessages('myqueue', function(error, result, response){
      if(!error){
        // Message text is in messages[0].messagetext
      }
    });

`result`包含的消息。

> [AZURE.NOTE] 队列中的没有消息将返回错误时，请使用**peekMessages** ，但是任何邮件都将不返回。

## 如何︰ 取消排队的下一封邮件

处理一条消息是一个两阶段的过程︰

1. 该消息的队列中去除。

2. 删除该邮件。

若要将邮件出列，使用**getMessages**。 这使得消息队列中不可见，因此没有其他客户机可以处理它们。 一旦您的应用程序已处理消息，调用**deleteMessage**以将其从队列中删除。 下面的示例获取一条消息，然后将其删除︰

    queueSvc.getMessages('myqueue', function(error, result, response){
      if(!error){
        // Message text is in messages[0].messagetext
        var message = result[0];
        queueSvc.deleteMessage('myqueue', message.messageid, message.popreceipt, function(error, response){
          if(!error){
            //message deleted
          }
        });
      }
    });

> [AZURE.NOTE] 默认情况下，将隐藏消息，仅 30 秒，其后是对其他客户端可见。 您可以通过指定不同的值`options.visibilityTimeout`与**getMessages**。

> [AZURE.NOTE]
> 队列中的没有消息将返回错误时，请使用**getMessages** ，但是任何邮件都将不返回。

## 如何︰ 更改排队的邮件的内容

您可以更改消息的就地队列使用**updateMessage**中的内容。 下面的示例更新消息的文本︰

    queueSvc.getMessages('myqueue', function(error, result, response){
      if(!error){
        // Got the message
        var message = result[0];
        queueSvc.updateMessage('myqueue', message.messageid, message.popreceipt, 10, {messageText: 'new text'}, function(error, result, response){
          if(!error){
            // Message updated successfully
          }
        });
      }
    });

## 如何︰ 附加选项出列邮件

有两种方法，您可以自定义队列中的邮件检索︰

* `options.numOfMessages` -检索一批消息 （最多 32 个）。
* `options.visibilityTimeout` -设置更长或更短的隐蔽性超时。

下面的示例使用**getMessages**方法在一个调用中获取 15 的消息。 然后它处理每个消息都使用 for 循环。 它还将隐蔽性超时设置为由此方法返回的所有消息五分钟的时间。

    queueSvc.getMessages('myqueue', {numOfMessages: 15, visibilityTimeout: 5 * 60}, function(error, result, response){
      if(!error){
        // Messages retreived
        for(var index in result){
          // text is available in result[index].messageText
          var message = result[index];
          queueSvc.deleteMessage(queueName, message.messageid, message.popreceipt, function(error, response){
            if(!error){
              // Message deleted
            }
          });
        }
      }
    });

## 如何︰ 获取队列长度

**GetQueueMetadata**返回队列，包括队列中正在等待的邮件数有关的元数据。

    queueSvc.getQueueMetadata('myqueue', function(error, result, response){
      if(!error){
        // Queue length is available in result.approximatemessagecount
      }
    });

## 如何︰ 列表队列

若要检索队列列表，请使用**listQueuesSegmented**。 若要检索筛选一个特定的前缀列表，请使用**listQueuesSegmentedWithPrefix**。

    queueSvc.listQueuesSegmented(null, function(error, result, response){
      if(!error){
        // result.entries contains the list of queues
      }
    });

如果不能返回所有队列，`result.continuationToken`可以用作为第一个参数的**listQueuesSegmented**或**listQueuesSegmentedWithPrefix**第二个参数来检索多个结果。

## 如何︰ 删除队列

要删除队列中包含的所有消息，请在队列对象上调用**deleteQueue**方法。

    queueSvc.deleteQueue(queueName, function(error, response){
        if(!error){
            // Queue has been deleted
        }
    });

若要清除队列中的所有消息，而不删除它，请使用**clearMessages**。

## 如何︰ 使用共享的访问签名

共享访问权限签名 (SAS) 是一种安全的方法，而无需提供您的存储帐户名或密钥提供细粒度访问队列。 SA 通常用于提供有限的访问队列，例如允许移动应用程序提交的邮件。

信任的应用程序，如基于云的服务生成 SAS 使用**队列服务**， **generateSharedAccessSignature** ，并提供给不受信任或部分信任应用程序。 例如，移动应用程序。 SAS 是使用一个策略，它描述了 SA 在此期间是有效的开始和结束日期，以及 SAS 持有人所授予的访问级别生成的。

下面的示例生成新的共享的访问策略，将允许 SAS 持有者将消息添加到队列中，并且到期后创建的时间 100 分钟。

    var startDate = new Date();
    var expiryDate = new Date(startDate);
    expiryDate.setMinutes(startDate.getMinutes() + 100);
    startDate.setMinutes(startDate.getMinutes() - 100);
    
    var sharedAccessPolicy = {
      AccessPolicy: {
        Permissions: azure.QueueUtilities.SharedAccessPermissions.ADD,
        Start: startDate,
        Expiry: expiryDate
      }
    };

    var queueSAS = queueSvc.generateSharedAccessSignature('myqueue', sharedAccessPolicy);
    var host = queueSvc.host;

注意根据需要的 SAS 持有者尝试访问队列时，，必须也提供主机信息。

然后，客户端应用程序与**QueueServiceWithSAS**使用了 SA 对队列执行操作。 下面的示例连接到队列并创建一条消息。

    var sharedQueueService = azure.createQueueServiceWithSas(host, queueSAS);
    sharedQueueService.createMessage('myqueue', 'Hello world from SAS!', function(error, result, response){
      if(!error){
        //message added
      }
    });

因为 SA 生成添加访问权限，如果尝试进行读取、 更新或删除的邮件，将返回错误。

### 访问控制列表

您可以使用访问控制列表 (ACL) 为 SAS 设置访问策略。 这很有用，如果您想要允许多个客户端访问队列，但为每个客户端提供不同的访问策略。

ACL 实现数组访问策略的使用与每个策略相关的 ID。 下面的示例定义两个策略;其中一个为"用户 1"和"用户 2":

    var sharedAccessPolicy = [
      {
        AccessPolicy: {
          Permissions: azure.QueueUtilities.SharedAccessPermissions.PROCESS,
          Start: startDate,
          Expiry: expiryDate
        },
        Id: 'user1'
      },
      {
        AccessPolicy: {
          Permissions: azure.QueueUtilities.SharedAccessPermissions.ADD,
          Start: startDate,
          Expiry: expiryDate
        },
        Id: 'user2'
      }
    ];

下面的示例获取当前 ACL **myqueue**，然后将使用**setQueueAcl**的新策略。 这种方法允许︰

    queueSvc.getQueueAcl('myqueue', function(error, result, response) {
      if(!error){
        //push the new policy into signedIdentifiers
        result.signedIdentifiers.push(sharedAccessPolicy);
        queueSvc.setQueueAcl('myqueue', result, function(error, result, response){
          if(!error){
            // ACL set
          }
        });
      }
    });

ACL 设置后，可以创建基于策略 ID SA。 下面的示例创建新的"用户 2"SA:

    queueSAS = queueSvc.generateSharedAccessSignature('myqueue', { Id: 'user2' });

## 下一步行动

现在，您已经学习了队列存储的基本知识，按照这些链接以了解更复杂的存储任务。

-   请参阅 MSDN 参考︰[在 Azure 存储和访问数据][]。
-   访问[团队博客，Azure 存储][]。
-   在 GitHub 访问[Azure 存储 SDK 节点][]存储库。

  [Azure 存储节点的 SDK]: https://github.com/Azure/azure-storage-node
  [使用 REST API，]: http://msdn.microsoft.com/library/azure/hh264518.aspx
  [Azure 的管理门户]: http://manage.windowsazure.com
  [创建和部署的 Node.js 应用到 Azure 网站]: ../web-sites-nodejs-develop-deploy-mac.md
  [使用存储的 Node.js 云服务]: ../storage-nodejs-use-table-storage-cloud-service-app.md
  [Node.js Web 应用程序与存储]: ../storage-nodejs-use-table-storage-web-site.md

  
  [Queue1]: ./media/storage-nodejs-how-to-use-queues/queue1.png
  [新加]: ./media/storage-nodejs-how-to-use-queues/plus-new.png
  [快速创建存储]: ./media/storage-nodejs-how-to-use-queues/quick-storage.png
  
  
  
  [Node.js 云服务]: ../cloud-services-nodejs-develop-deploy-app.md
  [存储和访问 Azure 中的数据]: http://msdn.microsoft.com/library/azure/gg433040.aspx
  [Azure 存储团队博客]: http://blogs.msdn.com/b/windowsazurestorage/
 [WebMatrix 网站]: ../web-sites-nodejs-use-webmatrix.md
 
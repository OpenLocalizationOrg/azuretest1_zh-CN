---
ms.openlocfilehash: c9bab7e2234c4fcf98809ae335020c4477c93cc1
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="如何使用 Node.js 通知集线器"
    description="了解如何使用通知集线器 Node.js 应用程序发送推式通知。"
    services="notification-hubs"
    documentationCenter="nodejs"
    authors="wesmc7777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="06/16/2015"
    ms.author="wesmc"/>

# 如何使用从 Node.js 通知集线器

> [AZURE.SELECTOR]
- [Java](notification-hubs-java-backend-how-to.md)
- [PHP](notification-hubs-php-backend-how-to.md)
- [Python](notification-hubs-python-backend-how-to)

##概述

本指南将向您介绍如何使用通知集线器从 Node.js 应用程序。 所包含的方案包括**发送通知到 Android、 iOS、 Windows Phone 和 Windows 应用商店应用程序**。 在通知集线器上的详细信息，请参阅[后续步骤](#next)部分。

##什么是通知集线器？

Azure 通知集线器提供易于使用、 多平台、 可扩展的基础结构向移动设备发送推式通知。 有关详细信息，请参阅[Azure 通知集线器](http://msdn.microsoft.com/library/windowsazure/jj927170.aspx)。

##创建 Node.js 应用程序

创建一个空白的 Node.js 应用。 创建 Node.js 应用程序的说明，请参阅[创建和部署 Node.js 应用到 Azure 网站][nodejswebsite]， [Node.js 云服务][Node.js 云服务]（使用 Windows PowerShell） 或[具有 WebMatrix 网站]。

##配置应用程序使用通知中心

若要使用 Azure 通知中心，您需要下载并使用 Node.js azure 的包。 这包括一组与其他服务通信的便利库。

### 使用节点程序包管理器 (NPM) 来获取包

1.  使用命令行界面**PowerShell** (Windows)，如**终端**(Mac，) 或**指责**(Unix)，定位到您在其中创建的示例应用程序的文件夹。

2.  键入**npm 安装 azure**在命令窗口中，应该产生下面的输出︰

        azure@0.7.0 node_modules\azure
        |-- dateformat@1.0.2-1.2.3
        |-- xmlbuilder@0.4.2
        |-- node-uuid@1.2.0
        |-- mime@1.2.9
        |-- underscore@1.4.4
        |-- validator@0.4.28
        |-- tunnel@0.0.2
        |-- wns@0.5.3
        |-- xml2js@0.2.6 (sax@0.4.2)
        |-- request@2.16.6 (forever-agent@0.2.0, aws-sign@0.2.0, tunnel-agent@0.2.0, oauth-sign@0.2.0, json-stringify-safe@3.0.0, cookie-jar@0.2.0, node-uuid@1.4.0, qs@0.5.5, hawk@0.10.2, form-data@0.0.7)

3.  您可以手动运行**ls**或**dir**命令，以验证**节点\_模块**文件夹的创建时间。 在该文件夹中查找**azure**软件包，其中包含您需要访问通知中心库。

### 导入模块

使用文本编辑器，添加以下应用程序的**server.js**文件的顶部︰

    var azure = require('azure');

### 设置 Azure 通知集线器连接

**NotificationHubService**对象使您可以使用通知集线器。 下面的代码创建名为**hubname**的 nofication 中心的**NotificationHubService**对象。 语句以导入 azure 模块后**server.js**文件的顶部附近添加它︰

    var notificationHubService = azure.createNotificationHubService('hubname','connectionstring');

连接**连接字符串**值可以从 Azure 管理门户，请执行以下步骤︰

1. 从 Azure 管理门户，选择**服务总线**，然后选择包含通知中心的命名空间。

2. 选择**通知集线器**，然后选择您想要使用的中心。

3. 从**快速**部分中，选择**视图连接字符串，**并将复制的连接字符串值。

> [AZURE.NOTE] 您也可以使用**Get AzureSbNamespace** cmdlet Azure PowerShell 或**azure sb 命名空间显示**命令使用 Azure 命令行界面 (Azure CLI) 所提供的连接字符串。

</div>

##如何发送通知

**NotificationHubService**对象公开将通知发送给特定的设备和应用程序的以下对象实例︰

* **Android**的使用**GcmService**对象，可在**notificationHubService.gcm**
* **iOS** -使用**ApnsService**对象，可以访问**notificationHubService.apns**
* **Windows Phone** -使用**MpnsService**对象，可在**notificationHubService.mpns**
* **Windows 应用商店应用程序**-使用**WnsService**对象，可在**notificationHubService.wns**

### 如何发送 Android 应用程序通知

**GcmService**对象提供了可用于将通知发送到 Android 应用程序**发送**的方法。 **Send**方法接受以下参数︰

* 标记 — 标记标识符。 如果未不提供任何标记，通知将发送到 al 客户端
* 有效负载的消息的 JSON 或字符串负载
* 回调的回调函数

有效负载格式的详细信息，请参见[实现 GCM 服务器](http://developer.android.com/google/gcm/server.html#payload)的有效载荷部分。

下面的代码使用公开的**NotificationHubService** **GcmService**实例将消息发送到所有客户端。

    var payload = {
      data: {
        msg: 'Hello!'
      }
    };
    notificationHubService.gcm.send(null, payload, function(error){
      if(!error){
        //notification sent
      }
    });

### 如何发送 iOS 应用程序通知

**ApnsService**对象提供了可用于将通知发送到 iOS 应用程序**发送**的方法。 **Send**方法接受以下参数︰

* 标记 — 标记标识符。 如果未不提供任何标记，通知将发送到所有客户端
* 有效负载的消息的 JSON 或字符串负载
* 回调的回调函数

有效负载格式的详细信息，请参阅[本地和编程指南推送通知](http://developer.apple.com/library/ios/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ApplePushService/ApplePushService.html)的通知有效载荷部分。

下面的代码使用公开的**NotificationHubService** **ApnsService**实例将警报消息发送到所有客户端︰

    var payload={
        alert: 'Hello!'
      };
    notificationHubService.apns.send(null, payload, function(error){
      if(!error){
        // notification sent
      }
    });

### 如何发送 Windows Phone 通知

**MpnsService**对象提供了可用于将通知发送到 Windows Phone 应用程序**发送**的方法。 **Send**方法接受以下参数︰

* 标记 — 标记标识符。 如果未不提供任何标记，通知将发送到所有客户端
* 有效负载的消息的 XML 有效负载
* TargetName-toast 通知大功告成。 token 拼贴通知。
* 通知类的通知的优先级。 请参阅 HTTP 标头元素部分[推式通知从服务器](http://msdn.microsoft.com/library/hh221551.aspx)为有效的值。
* 选项-可选的请求标头
* 回调的回调函数

有关有效 TargetName 列表，通知类和标头选项，请参阅[推来自服务器的通知](http://msdn.microsoft.com/library/hh221551.aspx)。

下面的代码使用公开的**NotificationHubService** **MpnsService**实例发送祝酒警报︰

    var payload = '<?xml version="1.0" encoding="utf-8"?><wp:Notification xmlns:wp="WPNotification"><wp:Toast><wp:Text1>string</wp:Text1><wp:Text2>string</wp:Text2></wp:Toast></wp:Notification>';
    notificationHubService.mpns.send(null, payload, 'toast', 22, function(error){
      if(!error){
        //notification sent
      }
    });

### 如何发送 Windows 应用商店应用程序通知

**WnsService**对象提供了可用于将通知发送到 Windows 应用商店应用程序**发送**的方法。  **Send**方法接受以下参数︰

* 标记 — 标记标识符。 如果未不提供任何标记，通知将发送到所有客户端
* 有效载荷的 XML 消息有效负载
* 类型-通知类型
* 选项-可选的请求标头
* 回调的回调函数

有效类型和请求标头的列表，请参阅[推送通知服务请求和响应头](http://msdn.microsoft.com/library/windows/apps/hh465435.aspx)。

下面的代码使用公开的**NotificationHubService** **WnsService**实例发送祝酒警报︰

    var payload = '<toast><visual><binding template="ToastText01"><text id="1">Hello!</text></binding></visual></toast>';
    notificationHubService.wns.send(null, payload , 'wns/toast', function(error){
      if(!error){
        // notification sent
      }
    });

## 下一步行动

现在，您已经了解使用通知中心的基本知识，按照这些链接以了解详细信息。

-   请参阅 MSDN 参考: [Azure 通知集线器] []。
-   在 GitHub 上访问[节点的 Azure SDK]资料库。

  [节点的 azure SDK]: https://github.com/WindowsAzure/azure-sdk-for-node
  [下一步行动]: #nextsteps
  [什么是服务总线主题和订阅？]: #what-are-service-bus-topics
  [创建服务 Namespace]: #create-a-service-namespace
  [Namespace 获得的默认管理凭据]: #obtain-default-credentials
  [创建 Node.js 应用程序]: #Create_a_Nodejs_Application
  [配置应用程序使用服务总线]: #Configure_Your_Application_to_Use_Service_Bus
  [如何︰ 创建主题]: #How_to_Create_a_Topic
  [如何︰ 创建订阅]: #How_to_Create_Subscriptions
  [如何︰ 将消息发送到一个主题]: #How_to_Send_Messages_to_a_Topic
  [如何︰ 从订阅接收消息]: #How_to_Receive_Messages_from_a_Subscription
  [如何︰ 处理应用程序崩溃和无法读取消息]: #How_to_Handle_Application_Crashes_and_Unreadable_Messages
  [如何︰ 删除主题和订阅]: #How_to_Delete_Topics_and_Subscriptions
  [1]: #Next_Steps
  [主题概念]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-topics-01.png
  [Azure 的管理门户]: http://manage.windowsazure.com
  [image]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-03.png
  [2]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-04.png
  [3]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-05.png
  [4]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-06.png
  [5]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-07.png
  [SqlFilter.SqlExpression]: http://msdn.microsoft.com/library/windowsazure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
  [Azure 服务总线通知集线器]: http://msdn.microsoft.com/library/windowsazure/jj927170.aspx
  [SqlFilter]: http://msdn.microsoft.com/library/windowsazure/microsoft.servicebus.messaging.sqlfilter.aspx
  [WebMatrix 网站]: /develop/nodejs/tutorials/web-site-with-webmatrix/
  [Node.js 云服务]: ../cloud-services-nodejs-develop-deploy-app.md
[以前的管理门户]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/previous-portal.png
  [nodejswebsite]: /develop/nodejs/tutorials/create-a-website-(mac)/
  [使用存储的 Node.js 云服务]: /develop/nodejs/tutorials/web-app-with-storage/
  [Node.js Web 应用程序与存储]: /develop/nodejs/tutorials/web-site-with-storage/

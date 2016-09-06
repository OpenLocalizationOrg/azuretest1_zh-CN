---
ms.openlocfilehash: 392040dc5e4311a2a539468048a4814de9d63d52
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---


在后端应用程序中，您现在需要切换至发送模板而不是本机负载的通知。 这将简化后端代码，因为您不需要发送多个不同平台的载荷。

发送模板通知您只需提供一组属性时，在我们的例子中我们将发送一的组属性包含本地化的版本的当前新闻，例如︰

    {
        "News_English": "World News in English!",
        "News_French": "World News in French!",
        "News_Mandarin": "World News in Mandarin!"
    }


本节说明如何在两种不同方式发送通知︰

- 使用控制台应用程序
- 使用移动服务脚本

包含的代码广播 Windows 应用商店和 iOS 设备，因为后端可以向任何受支持的设备进行广播。



## 若要发送通知使用 C# 控制台应用程序 ##

我们将发送单个模板通知，只需修改*SendNotificationAsync*方法。

    var hub = NotificationHubClient.CreateClientFromConnectionString("<connection string>", "<hub name>");
    var notification = new Dictionary<string, string>() {
                            {"News_English", "World News in English!"},
                            {"News_French", "World News in French!"},
                            {"News_Mandarin", "World News in Mandarin!"}};
    await hub.SendTemplateNotificationAsync(notification, "World");

请注意，此简单的调用将提供正确的本地化的新闻为**所有**您的设备，而不考虑平台，如通知中心能够构建和提供正确的本机负载订阅特定标记的所有设备。

### 移动服务

在您的移动服务计划，覆盖的脚本︰

    var azure = require('azure');
    var notificationHubService = azure.createNotificationHubService('<hub name>', '<connection string with full access>');
    var notification = {
            "News_English": "World News in English!",
            "News_French": "World News in French!",
            "News_Mandarin", "World News in Mandarin!"
    }
    notificationHubService.send('World', notification, function(error) {
        if (!error) {
            console.warn("Notification successful");
        }
    });
    
请注意如何在这种情况下没有必要将发送不同的区域设置和平台的多个通知。

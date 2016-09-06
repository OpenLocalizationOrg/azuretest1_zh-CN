---
ms.openlocfilehash: 09c5ab4a7f7d526c859372b057acaed72d31ae89
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="将跨平台通知发送给用户的通知集线器 (ASP.NET)" description="了解如何使用通知集线器模板发送一个请求，针对所有平台的独立于平台的通知中。"
    services="notification-hubs"
    documentationCenter=""
    authors="wesmc7777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="08/18/2015" 
    ms.author="wesmc"/>

# 将跨平台通知发送给用户提供通知集线器


在上一个教程[通知用户使用通知集线器]中，您学习了如何注册的特定身份验证的用户的所有设备到推式通知。 在此教程中，多个请求需要发送通知到每个受支持的客户端平台。 通知集线器支持模板，使您可以指定一个特定的设备要接收通知的方式。 这简化了发送跨平台通知。 本主题演示如何利用模板发送一个请求，针对所有平台的独立于平台的通知中。 有关模板的详细信息，请参阅[Azure 通知集线器概述][模板]。

> [AZURE.NOTE] 通知集线器允许设备使用相同的标记注册多个模板。 在这种情况下，针对该标记传入消息将导致发送到设备，另一个用于每个模板的多个通知。 这使您可以在多个视觉通知，例如同时作为徽章和 toast 通知 Windows 应用商店应用程序中显示相同的消息。

完成以下步骤来发送跨平台通知使用的模板︰

1. 在 Visual Studio 中的解决方案资源管理器中，展开**控制器**文件夹，然后打开 RegisterController.cs 文件。

2. 在创建新的注册替换的**Post**方法中找到的代码块`switch`内容与下面的代码︰

        switch (deviceUpdate.Platform)
        {
            case "mpns":
                var toastTemplate = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                    "<wp:Notification xmlns:wp=\"WPNotification\">" +
                       "<wp:Toast>" +
                            "<wp:Text1>$(message)</wp:Text1>" +
                       "</wp:Toast> " +
                    "</wp:Notification>";
                registration = new MpnsTemplateRegistrationDescription(deviceUpdate.Handle, toastTemplate);
                break;
            case "wns":
                toastTemplate = @"<toast><visual><binding template=""ToastText01""><text id=""1"">$(message)</text></binding></visual></toast>";
                registration = new WindowsTemplateRegistrationDescription(deviceUpdate.Handle, toastTemplate);
                break;
            case "apns":
                var alertTemplate = "{\"aps\":{\"alert\":\"$(message)\"}}";
                registration = new AppleTemplateRegistrationDescription(deviceUpdate.Handle, alertTemplate);
                break;
            case "gcm":
                var messageTemplate = "{\"data\":{\"msg\":\"$(message)\"}}";
                registration = new GcmTemplateRegistrationDescription(deviceUpdate.Handle, messageTemplate);
                break;
            default:
                throw new HttpResponseException(HttpStatusCode.BadRequest);
        }

    此代码调用特定于平台的方法来创建模板而不是本机的注册登记。 因为模板登记从本机登记，无需修改现有登记。

3. 在**通知**控制器中，用下面的代码替换的**sendNotification**方法︰

        public async Task<HttpResponseMessage> Post()
        {
            var user = HttpContext.Current.User.Identity.Name;
            var userTag = "username:" + user;

            var notification = new Dictionary<string, string> { { "message", "Hello, " + user } };
            await Notifications.Instance.Hub.SendTemplateNotificationAsync(notification, userTag);

            return Request.CreateResponse(HttpStatusCode.OK);
        }

    此代码发送通知到所有平台，在相同的时间，而无需指定本机负载。 集线器能够构建和提供正确的有效负载为提供的_标记_值，在已注册的模板中指定的每个设备的通知。

4. 重新发布您的 WebApi 后端项目。

5. 再次运行客户端应用程序并验证注册成功。

6. （可选）将客户端应用程序部署到第二台设备，然后运行该应用程序。

    请注意，每个设备上显示一个通知。

## 下一步行动

现在，您已完成本教程，了解更多有关通知集线器和在这些主题中的模板︰

+ **[使用通知集线器发送的新闻]** <br/>演示如何使用模板的另一种情况

+  **[Azure 通知集线器概述][模板]**<br/>概述主题已详细信息模板。

+  **[通知中心如何为 Windows 应用商店]**<br/>包括模板表达式语言参考。



<!-- Anchors. -->

<!-- Images. -->




<!-- URLs. -->
[向 ASP.NET 用户推送]: /manage/services/notification-hubs/notify-users-aspnet
[将推送给用户移动服务]: /manage/services/notification-hubs/notify-users/
[Windows 8 的 visual Studio 2012 速成版]: http://go.microsoft.com/fwlink/?LinkId=257546

[管理门户]: https://manage.windowsazure.com/
[使用通知集线器发送的新闻]: notification-hubs-windows-store-dotnet-send-breaking-news.md
[Azure 通知集线器]: http://go.microsoft.com/fwlink/p/?LinkId=314257
[通知用户使用通知集线器]: notification-hubs-aspnet-backend-windows-dotnet-notify-users.md
[模板]: http://go.microsoft.com/fwlink/p/?LinkId=317339
[通知中心如何为 Windows 应用商店]: http://msdn.microsoft.com/library/windowsazure/jj927172.aspx

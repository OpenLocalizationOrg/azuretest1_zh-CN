---
ms.openlocfilehash: a1acf54065f80264db40f81ee03825024f3605d8
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="对 Xamarin iOS 客户端的特定用户发送推式通知" 
    description="了解如何向用户的所有设备发送推式通知" 
    services="app-service\mobile" 
    documentationCenter="windows" 
    authors="ysxu" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="app-service-mobile" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-xamarin-ios" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="06/25/2015"
    ms.author="yuaxu"/>

# 将跨平台通知发送给特定用户

[AZURE.INCLUDE [app-service-mobile-selector-push-users-preview](../../includes/app-service-mobile-selector-push-users-preview.md)]
&nbsp;  
[AZURE.INCLUDE [app-service-mobile-note-mobile-services-preview](../../includes/app-service-mobile-note-mobile-services-preview.md)]

本主题演示如何从您的移动后端将通知发送到特定用户的所有已注册的设备。 它引入了[模板]，以使客户端应用程序指定有效负载格式和在注册变量占位符的自由的概念。 发送后点击量与这些占位符，启用跨平台通知每个平台。

> [AZURE.NOTE] 若要获取跨平台客户端处理推式，您需要完成本教程为您想要启用的每个平台。 您只需执行[移动后端更新](#backend)客户端共享同一个移动的后端。
 
##先决条件 

在开始本教程之前，您必须已经完成这些服务应用程序教程要为每个客户端平台工作︰

+ [开始使用身份验证]<br/>应做事项列表示例应用程序中添加登录要求。

+ [开始使用推式通知]<br/>配置任务列表示例应用程序用于推式通知。

##<a name="client"></a>更新您的客户端注册的模板来处理跨平台推进

1. 从**FinishedLaunching** **AppDelegate.cs**在 APNs 注册段转为中**QSTodoListViewController.cs**的**RefreshAsync**任务定义。 在身份验证完成后，登记应发生这种情况。

        ...
        if (todoService.User == null) {
            await QSTodoService.DefaultService.Authenticate (this);
            if (todoService.User == null) {
                Console.WriteLine ("couldn't login!!");
                return;
            }

            // registers for push for iOS8
            var settings = UIUserNotificationSettings.GetSettingsForTypes (
                UIUserNotificationType.Alert
                | UIUserNotificationType.Badge
                | UIUserNotificationType.Sound, 
                new NSSet ());

            UIApplication.SharedApplication.RegisterUserNotificationSettings (settings); 
            UIApplication.SharedApplication.RegisterForRemoteNotifications ();
        }
        ...

2. 在**AppDelegate.cs**，与下面要使用模板替换中**RegisteredForRemoteNotifications**的**RegisterAsync**调用︰

        // delete await push.RegisterAsync (deviceToken);
        
        var notificationTemplate = "{\"aps\": {\"alert\":\"$(message)\"}}";

        JObject templateBody = new JObject();
        templateBody["body"] = notificationTemplate;

        JObject templates = new JObject();
        templates["testApnsTemplate"] = templateBody;

        // register with templates
        await push.RegisterAsync (deviceToken, templates);

##<a name="backend"></a>更新您的服务后端将通知发送给特定用户

1. 在 Visual Studio，更新`PostTodoItem`方法定义为以下代码︰  

        public async Task<IHttpActionResult> PostTodoItem(TodoItem item)
        {
            TodoItem current = await InsertAsync(item);

            // get notification hubs credentials associated with this mobile app
            string notificationHubName = this.Services.Settings.NotificationHubName;
            string notificationHubConnection = this.Services.Settings.Connections[ServiceSettingsKeys.NotificationHubConnectionString].ConnectionString;

            // connect to notification hub
            NotificationHubClient Hub = NotificationHubClient.CreateClientFromConnectionString(notificationHubConnection, notificationHubName)

            // get the current user id and create given user tag
            ServiceUser authenticatedUser = this.User as ServiceUser;
            string userTag = "_UserId:" + authenticatedUser.Id;

            var notification = new Dictionary<string, string>{{"message", item.Text}};

            try
            {
                await Hub.Push.SendTemplateNotificationAsync(notification, userTag);
            }
            catch (System.Exception ex)
            {
                throw;
            }
            return CreatedAtRoute("Tables", new { id = current.Id }, current);
        }

##<a name="test"></a>测试应用程序

重新发布移动后端项目并运行任何客户端应用程序已设置。 上项插入后端将为所有用户登录的客户端应用程序发送通知。

<!-- URLs. -->
[开始使用身份验证]: app-service-mobile-dotnet-backend-xamarin-ios-get-started-users-preview.md
[开始使用推式通知]: app-service-mobile-dotnet-backend-xamarin-ios-get-started-push-preview.md
[模板]: https://msdn.microsoft.com/en-us/library/dn530748.aspx 
测试t

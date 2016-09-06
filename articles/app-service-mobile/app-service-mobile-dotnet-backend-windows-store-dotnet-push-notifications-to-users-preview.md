---
ms.openlocfilehash: 0336d57f8da52a5455e1209169135f28d8942c47
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="将 x plat 通知发送给特定用户与 Windows 应用商店的客户机"
    description="了解如何在特定用户的所有设备发送推式通知。"
    services="app-service\mobile" 
    documentationCenter="windows" 
    authors="ysxu" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="app-service-mobile" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/14/2015"
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

1. 我们将改为**InitNotificationAsync**在**MainPage.cs**使用用户身份验证。 删除您的**InitNotificationAsync**方法定义和调用在**App.xmal.cs**，在**MainPage.cs**下添加**主页**类中︰

        private async void InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
 
            // building templates for wns
            var toastTemplate = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(message)</text></binding></visual></toast>";
            JObject templateBody = new JObject();
            templateBody["body"] = toastTemplate;
 
            JObject wnsToastHeaders = new JObject();
            wnsToastHeaders["X-WNS-Type"] = "wns/toast";
            templateBody["headers"] = wnsToastHeaders;
 
            JObject templates = new JObject();
            templates["testTemplate"] = templateBody;
 
            await App.MobileService.GetPush().RegisterAsync(channel.Uri, templates);
        }

    您需要转移部分使用**MainPage.cs**语句。

2. 后在**ButtonLogin_Click**中的**AuthenticateAsync**调用中使用此方法权限。

        await AuthenticateAsync();
        InitNotificationAsync();

现在将您的应用程序设置为注册用户设备与用户登录信息。

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

            // get the current user id and create tag to identify user
            ServiceUser authenticatedUser = this.User as ServiceUser;
            string userTag = "_UserId:" + authenticatedUser.Id;

            // build dictionary for template
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
[开始使用身份验证]: app-service-mobile-dotnet-backend-windows-store-dotnet-get-started-users-preview.md
[开始使用推式通知]: app-service-mobile-dotnet-backend-windows-store-dotnet-get-started-push-preview.md
[模板]: https://msdn.microsoft.com/en-us/library/dn530748.aspx
 
测试

---
ms.openlocfilehash: 946a7647ecc2d48010323633f5209c998a834a43
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="将推式通知添加到您的 Windows 运行时 8.1 通用应用程序 |Azure 的移动应用程序" 
    description="了解如何使用 Azure 应用程序服务移动应用程序和 Azure 通知集线器到您的 Windows 应用程序发送推式通知。" 
    services="app-service\mobile,notification-hubs" 
    documentationCenter="windows" 
    authors="ggailey777" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="app-service-mobile" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/14/2015" 
    ms.author="glenga"/>

# 将推式通知添加到您的 Windows 运行时 8.1 通用应用程序

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push-preview](../../includes/app-service-mobile-selector-get-started-push-preview.md)]
&nbsp;  
[AZURE.INCLUDE [app-service-mobile-note-mobile-services-preview](../../includes/app-service-mobile-note-mobile-services-preview.md)]

##概述

本主题演示如何通过使用 Azure 应用程序服务移动应用程序和 Azure 通知集线器到 Windows 运行时 8.1 通用应用程序发送推式通知。 在这种情况下，当添加新项时，您移动应用程序的后端向推式通知注册与 Windows 通知服务 (WNS) 的所有 Windows 应用程序。

本教程基于应用程序服务移动应用程序快速入门。 在开始本教程之前，您必须首先完成快速入门教程[创建一个 Windows 应用程序](../app-service-mobile-dotnet-backend-windows-store-dotnet-get-started-preview.md)。 如果您不使用下载快速入门服务器项目，您必须添加到项目的推式通知扩展包。 有关服务器扩展软件包的详细信息，请参阅[使用.NET 后端服务器用于 Azure 的移动应用程序的 SDK](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)。 

##先决条件

若要完成本教程，您需要以下各项︰

* 活动的[Microsoft 存储帐户](http://go.microsoft.com/fwlink/p/?LinkId=280045)。
* [Visual Studio 社区 2013](https://go.microsoft.com/fwLink/p/?LinkID=391934)
* 完成本[快速入门教程](../app-service-mobile-dotnet-backend-windows-store-dotnet-get-started-preview.md)。  



##<a name="create-gateway"></a>创建通知中心

请按照以下步骤创建新的通知中心来处理推式通知。 如果同一个资源组中已集线器，则不必完成此部分。

1. 请访问[Azure 的门户]。 单击**浏览所有** > **移动应用程序**> 您刚刚创建的后端。 单击**设置** > **移动** > **推**。 

2. 按照工作流来创建通知中心。 您将需要如果不存在在您当前的资源组中创建新的命名空间。 一旦您已配置的所有设置，请单击**创建**。

接下来，将使用此 notificaton 中心为您的应用程序启用推式。

##注册您的推送通知的应用程序

从 Azure 发送推式通知您的 Windows 应用程序之前，您必须提交到 Windows 应用商店应用程序。 然后，您可以配置服务器项目与 WNS 集成。

1. 在 Visual Studio 解决方案资源管理器中，右击 Windows 应用商店应用程序项目，单击**存储** > **相关联的应用程序与存储...**。 

    ![][3]
    
2. 在向导中，单击**下一步**，使用您的 Microsoft 帐户登录、**保留新的应用程序名称**，键入您的应用程序的名称，然后单击**保留**。

3. 成功创建的应用程序注册后，选择新的应用程序名称，请单击**下一步**，，然后单击**关联**。 将所需的 Windows 应用商店注册信息添加到应用程序清单。 

7. 为 Windows 应用商店应用程序使用您以前创建的同一个注册 Windows Phone 存储应用程序项目，请重复步骤 1 到 3。  

7. 导航到[Windows 开发人员中心](https://dev.windows.com/en-us/overview)，登录与您的 Microsoft 帐户，请单击新的应用程序注册，在**我的应用程序**，然后展开**服务** > **推式通知**。 

8. 在**推式通知**页上，单击**Microsoft Azure 移动服务**下的**Live 服务网站**。

9. 在**应用程序设置**选项卡上，记下**客户的机密**和**包 SID**值。 

    ![][6]

    > [AZURE.IMPORTANT] 客户的机密和包 SID 是重要的安全凭据。 不要与任何人共享这些值或将其与您的应用程序一起分发。

##配置移动应用程序发送推送请求

1. 登录到[Azure 门户]中，选择**浏览** > **移动应用程序**> 应用程序 >**推送通知服务**。

2. 在**Windows 通知服务**中，输入的**安全密钥**（客户机密） 和**包 SID**获得从 Live 服务站点，然后单击**保存**。

移动应用程序端现在已配置为与 WNS 合作。

##<a id="update-service"></a>更新服务器发送推式通知

现在，应用程序中启用推式通知，您必须更新您的应用程序后端发送推式通知。 

1. 在 Visual Studio 中，用鼠标右键单击该服务器项目并单击**管理 NuGet 程序包**，搜索`Microsoft.Azure.NotificationHubs`，然后单击**安装**。 这将安装集线器通知客户端库。

3. 在服务器项目中，打开**控制器** > **TodoItemController.cs**，然后添加以下使用语句︰

        using System.Collections.Generic;
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.Mobile.Server.Config;
    

2. 在**PostTodoItem**方法中，对**InsertAsync**的调用之后添加以下代码︰  

        // Get the settings for the server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings = 
            this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();
        
        // Get the Notification Hubs credentials for the Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create a new Notification Hub client.
        NotificationHubClient hub = NotificationHubClient
        .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

        // Define a WNS payload
        var windowsToastPayload = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" 
                                + item.Text + @"</text></binding></visual></toast>";

        try
        {
            // Send the push notification and log the results.
            var result = await hub.SendWindowsNativeNotificationAsync(windowsToastPayload);

            // Write the success result to the logs.
            config.Services.GetTraceWriter().Info(result.State.ToString());
        }
        catch (System.Exception ex)
        {
            // Write the failure result to the logs.
            config.Services.GetTraceWriter()
                .Error(ex.Message, null, "Push.SendAsync Error");
        }

    此代码指示通知的集线器，以插入一个新项目之后，将发送推式通知。


## <a name="publish-the-service"></a>发布到 Azure 的移动后端

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-publish-service-preview](../../includes/app-service-mobile-dotnet-backend-publish-service-preview.md)]

##<a id="update-service"></a>将推式通知添加到您的应用程序

1. 在 Visual Studio 中，右击解决方案，然后单击**管理 NuGet 程序包**。 

    这将显示管理 NuGet 程序包对话框。

2. 搜索托管应用程序服务移动应用程序客户端 SDK 和单击**安装**，选择客户端的所有项目在解决方案中，并接受使用条款。 

    这将下载、 安装、 安装和 Windows 到 Azure 移动推库添加的引用所有客户端项目中。 

3. 打开共享的**App.xaml.cs**的项目文件，然后添加以下`using`语句︰

        using System.Threading.Tasks;  
        using Windows.Networking.PushNotifications;       

4. 在同一文件中，对**应用程序**类中添加下面的**InitNotificationsAsync**方法定义︰
    
        private async Task InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager
                .CreatePushNotificationChannelForApplicationAsync();

            await App.MobileService.GetPush().RegisterAsync(channel.Uri);
        }
    
    此代码从 WNS，检索应用程序 ChannelURI，然后您的应用程序服务移动的应用程序中注册的 ChannelURI。
    
5. 在**App.xaml.cs**的**OnLaunched**事件处理程序的顶部，将**异步**修饰符添加到方法定义并添加以下调用到新的**InitNotificationsAsync**方法，如以下示例所示︰

        protected async override void OnLaunched(LaunchActivatedEventArgs e)
        {
            await InitNotificationsAsync();

            // ...
        }

    这可以保证短期 ChannelURI 已注册，每次启动应用程序。

6. 在解决方案资源管理器中双击 Windows 应用商店应用程序，**通知**中的**Package.appxmanifest** ，将**祝酒能够**设置为**是**。

    从**文件**菜单上，单击**全部保存**。

7. 重复上一步在 Windows Phone 存储应用程序项目中。

您的应用程序现在就可以收到 toast 通知。

##<a id="test"></a>测试您的应用程序中的推式通知

[AZURE.INCLUDE [app-service-mobile-windows-universal-test-push-preview](../../includes/app-service-mobile-windows-universal-test-push-preview.md)]

<!-- Anchors. -->
<!-- URLs. -->
[Azure 门户]: https://portal.azure.com/
<!-- Images. -->
[0]: ./media/app-service-mobile-dotnet-backend-windows-store-dotnet-get-started-push-preview/mobile-services-submit-win8-app.png
[1]: ./media/app-service-mobile-dotnet-backend-windows-store-dotnet-get-started-push-preview/mobile-services-win8-app-name.png
[2]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-create-windows-universal-app.png
[3]: ./media/app-service-mobile-dotnet-backend-windows-store-dotnet-get-started-push-preview/notification-hub-associate-win8-app.png
[4]: ./media/app-service-mobile-dotnet-backend-windows-store-dotnet-get-started-push-preview/mobile-services-select-app-name.png
[5]: ./media/app-service-mobile-dotnet-backend-windows-store-dotnet-get-started-push-preview/mobile-services-win8-edit-app.png
[6]: ./media/app-service-mobile-dotnet-backend-windows-store-dotnet-get-started-push-preview/mobile-services-win8-app-push-auth.png
[7]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-create-from-portal.png
[17]: ./media/app-service-mobile-dotnet-backend-windows-store-dotnet-get-started-push-preview/mobile-services-win8-edit2-app.png

<!-- URLs. -->
[提交应用程序页]: http://go.microsoft.com/fwlink/p/?LinkID=266582 
测试t

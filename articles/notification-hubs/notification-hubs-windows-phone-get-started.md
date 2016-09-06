---
ms.openlocfilehash: 9c7d68c1a885861082da7efd3801e5ba5faf12a1
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="开始使用 Azure 通知集线器 |Microsoft Azure"
    description="在本教程中，您将学习如何使用 Azure 通知集线器到 Windows Phone 8 或 Windows Phone 8.1 Silverlight 应用程序推送通知。"
    services="notification-hubs"
    documentationCenter="windows"
    authors="wesmc7777"
    manager="dwrede"
    editor="dwrede"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="06/16/2015"
    ms.author="wesmc"/>

# 开始使用通知集线器

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##概述

本教程展示如何使用 Azure 通知集线器到 Windows Phone 8 或 Windows Phone 8.1 Silverlight 应用程序发送推式通知。 如果您面向 Windows Phone 8.1 (非 Silverlight)，然后参考[Windows 通用](notification-hubs-windows-store-dotnet-get-started.md)版本。
在本教程中，您将创建空的 Windows Phone 8 应用程序通过使用 Microsoft 推送通知服务 (MPNS) 接收推式通知。 完成后，您将能够使用通知中心广播运行您的应用程序的所有设备的推式通知。

> [AZURE.NOTE] 通知集线器 Windows Phone SDK 不支持 Windows Phone 8.1 Silverlight 应用程序中使用 Windows 推送通知服务 (WNS)。 若要使用 Windows Phone 8.1 Silverlight 应用程序 （而不是 MPNS) 的 WNS，按照 [通知集线器的 Windows Phone Silverlight 教程]，它使用 REST Api。

本教程演示中使用通知集线器的简单广播的方案。

##先决条件

本教程要求如下︰

+ [Visual Studio 2012 速成版的 Windows Phone]或更高版本。

学完本教程是为 Windows Phone 8 应用程序的所有其他通知集线器教程的先决条件。

> [AZURE.NOTE] 若要完成本教程，您必须为活动 Azure 帐户。 如果您没有帐户，您可以在几分钟创建免费的试用帐户。 有关详细信息，请参阅[Azure 免费试用版](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-phone-get-started%2F)。

##创建通知中心

1. 登录到[Azure 门户]，然后单击**+ 新建**在屏幕的底部。

2. 单击**服务应用程序**，单击**服务总线**，**通知集线器**中，请单击，然后单击**快速创建**。

    ![][7]

3. 通知中心为键入一个名称，选择您需要的区域，然后单击**创建新的通知中心**。

    ![][8]

4. 单击您刚创建 (通常是***通知中心名称*-ns**)，命名空间，然后单击顶部的**配置**选项卡。

    ![][9]

5. 单击**通知集线器**选项卡的顶部，然后单击您刚刚创建的通知中心。

    ![][10]

6. 单击底部的**连接信息**。 请注意两个连接字符串。

    ![][12]

7. 单击**配置**选项卡，然后单击**Windows Phone 通知设置**部分中的**启用未经验证的推式通知**复选框。

    ![][15]

现在，您可以注册您的 Windows Phone 8 应用程序和发送通知所需的连接字符串。

> [AZURE.NOTE] 本教程使用 MPNS 在未经身份验证的模式。 MPNS 未验证模式下通知，您可以发送给每个通道上附带的限制。 通知集线器支持[MPNS 身份验证模式](http://msdn.microsoft.com/library/windowsphone/develop/ff941099(v=vs.105).aspx)。 <!--Refer to [Notification Hubs How-To for Windows Phone 8] for more information on how to use MPNS authenticated mode.-->

##将您的应用程序连接到通知中心

1. 在 Visual Studio 中，将创建新的 Windows Phone 8 应用程序。

    ![][13]

    在 Visual Studio 2013年更新 2 或更高版本，而是创建一个 Windows Phone Silverlight 应用程序。

    ![][11]

2. 在 Visual Studio 中，解决方案中，用鼠标右键单击，然后单击**管理 NuGet 程序包**。

    这将显示**管理 NuGet 程序包**对话框。

3. 搜索`WindowsAzure.Messaging.Managed`并单击**安装**，然后接受使用条款。

    ![][20]

    此下载、 安装、 安装和使用 windows 添加对 Azure 消息传递库的引用<a href="http://nuget.org/packages/WindowsAzure.Messaging.Managed/">WindowsAzure.Messaging.Managed NuGet 程序包</a>。

4. 打开 App.xaml.cs 文件，然后添加以下`using`语句︰

        using Microsoft.Phone.Notification;
        using Microsoft.WindowsAzure.Messaging;

5. 在 App.xaml.cs 中的**Application_Launching**方法的顶部添加以下代码︰

        var channel = HttpNotificationChannel.Find("MyPushChannel");
        if (channel == null)
        {
            channel = new HttpNotificationChannel("MyPushChannel");
            channel.Open();
            channel.BindToShellToast();
        }

        channel.ChannelUriUpdated += new EventHandler<NotificationChannelUriEventArgs>(async (o, args) =>
        {
            var hub = new NotificationHub("<hub name>", "<connection string>");
            await hub.RegisterNativeAsync(args.ChannelUri.ToString());
        });

    请确保将您的中枢和称为**DefaultListenSharedAccessSignature**中获得上一节中的连接字符串的名称。
    此代码 MPNS，从中检索应用程序该信道的 URI，然后向您通知中心注册该信道的 URI。 它还保证，URI 在中注册通知中心每次启动应用程序的通道。

    >[AZURE.NOTE]本教程将 toast 通知发送到该设备。 当您发送一个拼贴通知时，必须改为通道上调用**BindToShellTile**方法。 若要支持这两个祝酒和拼贴通知， **BindToShellTile**和**BindToShellToast**。

6. 在解决方案资源管理器，展开**属性**打开 WMAppManifest.xml 文件，单击**功能**选项卡，确保**ID_CAP_PUSH_NOTIFICATION**功能已。

    ![][14]

    这可以确保您的应用程序可以接收推式通知。

7. 按 F5 键以运行该应用程序。

    将显示一条注册消息。

##从您的后端发送通知

您可以从任何后端通过使用通知集线器发送通知<a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">REST 接口</a>。 在本教程中，您可以通过使用.NET 控制台应用程序发送通知。 如何从通知集线器与集成 Azure 移动服务后端发送通知的示例，请参阅"入门使用推式通知中移动服务"([.NET 后端](../mobile-services-javascript-backend-windows-phone-get-started-push.md) | [JavaScript 后端](../mobile-services-javascript-backend-windows-phone-get-started-push.md))。  有关如何通过使用 REST Api 发送通知的示例，请参见"如何使用 Java/PHP 从通知集线器"([Java](notification-hubs-java-backend-how-to.md) | [PHP](notification-hubs-php-backend-how-to.md))。

1. 右击解决方案，选择**添加**和**新项目...**，然后下**C#**，请单击**Windows**和**控制台应用程序**，并单击**确定**。

    ![][6]

    这将添加一个新 C# 控制台应用程序解决方案。 此外可以执行此单独的解决方案中。

4. 用鼠标右键单击，单击**工具**，单击**库程序包管理器**，然后单击**程序包管理器控制台**。

    此时将显示程序包管理器控制台。

6. 在控制台窗口中，将设置为新的控制台应用程序项目，**默认项目**，然后在控制台窗口中，执行以下命令︰

        Install-Package WindowsAzure.ServiceBus

    这将引用添加到带有 Azure 服务总线 SDK <a href="http://nuget.org/packages/WindowsAzure.ServiceBus/">WindowsAzure.ServiceBus NuGet 程序包</a>。

5. 打开 Program.cs 文件，然后添加以下`using`语句︰

        using Microsoft.ServiceBus.Notifications;

6. 在**程序**类中，添加下面的方法︰

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient
                .CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            string toast = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                "<wp:Notification xmlns:wp=\"WPNotification\">" +
                   "<wp:Toast>" +
                        "<wp:Text1>Hello from a .NET App!</wp:Text1>" +
                   "</wp:Toast> " +
                "</wp:Notification>";
            await hub.SendMpnsNativeNotificationAsync(toast);
        }

    请确保"中心名称"占位符替换通知集线器中的门户**通知集线器**选项卡上显示的名称。 此外，将连接字符串占位符替换调用**DefaultFullSharedAccessSignature**获取"配置通知中心。"一节中的连接字符串

    >[AZURE.NOTE]请确保您具有**完全**访问权限，不**听**访问使用的连接字符串。 听访问字符串没有发送通知的权限。

4. 在**Main**方法中添加以下行︰

         SendNotificationAsync();
         Console.ReadLine();

5. 与 Windows Phone 仿真程序运行并关闭您的应用程序，设置为默认启动项目的控制台应用程序项目，然后按 F5 键以运行该应用程序。

    您将收到 toast 通知。 点击祝酒横幅加载该应用程序。

[Toast 目录]和[目录平铺]在 MSDN 上主题中，您可以找到所有可能的有效载荷。

##下一步行动

在此简单示例中，您为所有 Windows Phone 8 设备广播通知。 为了瞄准特定的用户，是指本教程[使用通知集线器到推式通知给用户]。 如果您想要段由有兴趣的集团用户，您可以阅读[使用通知集线器发送的新闻]。 了解更多关于如何使用[通知集线器指南]中的通知集线器。



<!-- Images. -->
[6]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-console-app.png
[7]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-from-portal.png
[8]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-from-portal2.png
[9]: ./media/notification-hubs-windows-phone-get-started/notification-hub-select-from-portal.png
[10]: ./media/notification-hubs-windows-phone-get-started/notification-hub-select-from-portal2.png
[11]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-wp-silverlight-app.png
[12]: ./media/notification-hubs-windows-phone-get-started/notification-hub-connection-strings.png

[13]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-wp-app.png
[14]: ./media/notification-hubs-windows-phone-get-started/mobile-app-enable-push-wp8.png
[15]: ./media/notification-hubs-windows-phone-get-started/notification-hub-pushauth.png
[20]: ./media/notification-hubs-windows-phone-get-started/notification-hub-windows-universal-app-install-package.png
[213]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-console-app.png





<!-- URLs. -->
[Windows Phone 的 visual Studio 2012 速成版]: https://go.microsoft.com/fwLink/p/?LinkID=268374
[Azure 门户]: https://manage.windowsazure.com/
[通知集线器指南]: http://msdn.microsoft.com/library/jj927170.aspx
[MPNS 身份验证模式]: http://msdn.microsoft.com/library/windowsphone/develop/ff941099(v=vs.105).aspx
[使用推式通知给用户通知集线器]: notification-hubs-aspnet-backend-windows-dotnet-notify-users.md
[使用通知集线器发送的新闻]: notification-hubs-windows-phone-send-breaking-news.md
[toast 目录]: http://msdn.microsoft.com/library/windowsphone/develop/jj662938(v=vs.105).aspx
[平铺目录]: http://msdn.microsoft.com/library/windowsphone/develop/hh202948(v=vs.105).aspx
[通知中心的 WP Silverlight 教程]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToSLPhoneApp

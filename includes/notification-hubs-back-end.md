---
ms.openlocfilehash: 7eae08885e4626f407ca925faab2316b7763eac1
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

本节演示如何发送通知从.NET 控制台应用程序以及任何其他类型。
如果您使用的移动服务[推入门](mobile-services-dotnet-backend-windows-store-dotnet-get-started-push.md)教程，请参阅。 如果您想要使用 Java 或 PHP，请参阅[如何使用 Java/PHP 从通知集线器](../articles/notification-hubs/notification-hubs-java-backend-how-to.md)。 您可以从任何后端使用[通知中心 REST 接口](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx)发送通知。

下面的代码将通知发送到 Windows 应用商店、 Windows Phone、 iOS 和 Android 设备。 

如果您创建一个控制台应用程序完成[入门通知集线器][的入门]，请跳过步骤 1-3。

1. 在 Visual Studio 中创建一个新 C# 控制台应用程序︰ 

    ![][13]

2. 在 Visual Studio 主菜单中，单击**工具**、**库程序包管理器**和**程序包管理器控制台**，然后在控制台窗口中键入以下并按**enter 键**︰

        Install-Package WindowsAzure.ServiceBus
    
    这将引用添加到 Azure 服务总线 SDK 通过<a href="http://nuget.org/packages/WindowsAzure.ServiceBus/">WindowsAzure.ServiceBus NuGet 程序包</a>。 

3. 打开 Program.cs 文件，然后添加以下`using`语句︰

        using Microsoft.ServiceBus.Notifications;

4. 在`Program`类，添加下面的方法，或如果已经存在，则替换它︰

        private static async void SendNotificationAsync()
        {
            // Define the notification hub.
            NotificationHubClient hub = 
                NotificationHubClient.CreateClientFromConnectionString(
                    "<connection string with full access>", "<hub name>");
        
            // Create an array of breaking news categories.
            var categories = new string[] { "World", "Politics", "Business", 
                "Technology", "Science", "Sports"};
        
            foreach (var category in categories)
            {
                try
                {
                    // Define a Windows Store toast.
                    var wnsToast = "<toast><visual><binding template=\"ToastText01\">" 
                        + "<text id=\"1\">Breaking " + category + " News!" 
                        + "</text></binding></visual></toast>";         
                    await hub.SendWindowsNativeNotificationAsync(wnsToast, category);

                    // Define a Windows Phone toast.
                    var mpnsToast =
                        "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                        "<wp:Notification xmlns:wp=\"WPNotification\">" +
                            "<wp:Toast>" +
                                "<wp:Text1>Breaking " + category + " News!</wp:Text1>" +
                            "</wp:Toast> " +
                        "</wp:Notification>";         
                    await hub.SendMpnsNativeNotificationAsync(mpnsToast, category);

                    // Define an iOS alert.
                    var alert = "{\"aps\":{\"alert\":\"Breaking " + category + " News!\"}}";
                    await hub.SendAppleNativeNotificationAsync(alert, category);

                    // Define an Android notification.
                    var notification = "{\"data\":{\"msg\":\"Breaking " + category + " News!\"}}";
                    await hub.SendGcmNativeNotificationAsync(notification, category);
                }
                catch (ArgumentException)
                {
                    // An exception is raised when the notification hub hasn't been 
                    // registered for the iOS, Windows Store, or Windows Phone platform. 
                }
            }
         }

    此代码将字符串数组中的六个标记的每个的通知发送到 Windows 应用商店、 Windows Phone 和 iOS 设备。 使用的标记可确保设备接收通知只为已注册的类别。
    
    > [AZURE.NOTE] 该后端代码支持 Windows 应用商店、 Windows Phone、 iOS 和 Android 的客户端。 发送时通知中心尚未尚未配置为特定的客户端平台，方法返回错误响应。 

6. 在上面的代码中，替换`<hub name>`，`<connection string with full access>`与您通知中心名称和之前获得的*DefaultFullSharedAccessSignature*的连接字符串的占位符。

7. **Main**方法中添加以下行︰

         SendNotificationAsync();
         Console.ReadLine();

<!-- Anchors -->
[从控制台应用程序]: #console
[从移动服务]: #mobile-services
[运行应用程序并生成通知]: #test-app

<!-- Images. -->
[13]: ./media/notification-hubs-back-end/notification-hub-create-console-app.png

[15]: ./media/notification-hubs-back-end/notification-hub-scheduler1.png
[16]: ./media/notification-hubs-back-end/notification-hub-scheduler2.png

<!-- URLs. -->
[入门-]: ../articles/notification-hubs/notification-hubs-windows-store-dotnet-get-started.md
[使用通知集线器来向用户发送通知]: ../articles/tutorial-notify-users-mobileservices.md
[开始使用移动服务]: /develop/mobile/tutorials/get-started/#create-new-service
[Azure 的管理门户]: https://manage.windowsazure.com/
[wns 对象]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[通知集线器指南]: http://msdn.microsoft.com/library/jj927170.aspx
[为 Windows 应用商店的通知集线器操作方法]: http://msdn.microsoft.com/library/jj927172.aspx
[通知集线器 REST 接口]: http://msdn.microsoft.com/library/windowsazure/dn223264.aspx

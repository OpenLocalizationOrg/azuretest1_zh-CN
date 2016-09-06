---
ms.openlocfilehash: b5910473b174e3189d4dc118dc46b3bff2012d31
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="将推式通知添加到您的移动服务应用程序 （Windows 应用商店） |Microsoft Azure" 
    description="了解如何使用 Azure 移动服务和通知集线器到您的 Windows 应用商店应用程序发送推式通知。" 
    services="mobile-services,notification-hubs" 
    documentationCenter="windows" 
    authors="ggailey777" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="06/11/2015" 
    ms.author="glenga"/>


# 向您的移动服务应用程序添加推式通知

[AZURE.INCLUDE [mobile-services-selector-get-started-push](../../includes/mobile-services-selector-get-started-push.md)]

##概述

本主题演示如何使用 Azure 移动服务为 Windows Phone Silverlight 应用程序发送推式通知。 在本教程中启用推式通知使用 Azure 通知集线器到快速启动项目。 完成后，您的移动服务将发送通知集线器插入记录每次均使用推式通知。 创建通知中心是自由与您的移动服务，可以来管理独立于移动服务，并可由其他应用程序和服务。

本教程基于任务列表示例应用程序。 在开始本教程之前，您必须首先完成的主题[添加到现有应用程序的移动服务]，以连接到移动服务的项目。 当尚未连接的移动服务时，添加推式通知向导可以创建此连接，则为。 

>[AZURE.NOTE]发送推式通知到 Windows Phone 8.1 存储应用程序，请按照本教程的[Windows 应用商店应用程序](../mobile-services-javascript-backend-windows-store-dotnet-get-started-push.md)版本。

##<a id="update-app"></a> 更新应用程序注册通知

您的应用程序可以接收推式通知之前，您必须注册通知通道。

1. 在 Visual Studio，打开 App.xaml.cs 文件，然后添加以下`using`语句︰

        using Microsoft.Phone.Notification;

3. App.xaml.cs 中添加以下项︰
    
        public static HttpNotificationChannel CurrentChannel { get; private set; }

        private void AcquirePushChannel()
        {
            CurrentChannel = HttpNotificationChannel.Find("MyPushChannel");

            if (CurrentChannel == null)
            {
                CurrentChannel = new HttpNotificationChannel("MyPushChannel");
                CurrentChannel.Open();
                CurrentChannel.BindToShellToast();
            }

            CurrentChannel.ChannelUriUpdated +=
                new EventHandler<NotificationChannelUriEventArgs>(async (o, args) =>
                {

                    // Register for notifications using the new channel
                    await MobileService.GetPush()
                        .RegisterNativeAsync(CurrentChannel.ChannelUri.ToString());
                });
        }

    此代码检索应用程序从 Microsoft 推送通知服务 (MPNS) 使用的 Windows Phone ChannelURI 8.x"Silverlight"，，，然后注册该 ChannelURI 的推式通知。

    >[AZURE.NOTE]在这本教程中，移动服务 toast 通知将发送到设备。 当您发送一个拼贴通知时，必须改为通道上调用**BindToShellTile**方法。

4. 在 App.xaml.cs 的**Application_Launching**事件处理程序的顶部，添加以下新的**AcquirePushChannel**方法调用︰

        AcquirePushChannel();

    这将确保注册请求每次加载页。 在您的应用程序，您可能只想要定期以确保注册为当前此登记。 

5. 按**F5**键以运行该应用程序。 显示弹出式对话框中的注册密钥。
  
6.  在解决方案资源管理器中，展开**属性**打开 WMAppManifest.xml 文件，单击**功能**选项卡，确保已**ID___CAP___PUSH_NOTIFICATION**功能。

    ![启用 VS 中的通知](./media/mobile-services-javascript-backend-windows-phone-get-started-push/mobile-app-enable-push-wp8.png)

    这将确保您的应用程序可以引发 toast 通知。 

##<a id="update-scripts"></a> 更新服务器脚本发送推式通知

最后，您必须更新注册到要发送通知的 TodoItem 表上执行插入操作的脚本。

1. 单击**TodoItem**，单击**脚本**，然后选择**插入**。 

2. 插入函数替换为下面的代码，然后单击**保存**:

        function insert(item, user, request) {
        // Define a payload for the Windows Phone toast notification.
        var payload = '<?xml version="1.0" encoding="utf-8"?>' +
            '<wp:Notification xmlns:wp="WPNotification"><wp:Toast>' +
            '<wp:Text1>New Item</wp:Text1><wp:Text2>' + item.text + 
            '</wp:Text2></wp:Toast></wp:Notification>';
        
        request.execute({
            success: function() {
                // If the insert succeeds, send a notification.
                push.mpns.send(null, payload, 'toast', 22, {
                    success: function(pushResponse) {
                        console.log("Sent push:", pushResponse);
                        request.respond();
                        },              
                        error: function (pushResponse) {
                            console.log("Error Sending push:", pushResponse);
                            request.respond(500, { error: pushResponse });
                            }
                        });
                    }
                });      
        }

    此插入脚本插入成功后，对所有 Windows Phone 应用程序登记发送推式通知 （带有插入的项的文本）。

3. 单击**推送**选项卡，检查**启用未经验证的推式通知**，然后单击**保存**。

    这使移动的服务，以连接到 MPNS 中未验证模式下发送推式通知。

    >[AZURE.NOTE]本教程使用 MPNS 在未经身份验证的模式。 在此模式中，MPNS 限制可以将发送到设备信道的通知数。 取消此限制，您必须生成并上载通过单击**上载**并选择该证书的证书。 生成证书的详细信息，请参阅[设置发送推式通知 Windows Phone 为已经过身份验证的 web 服务]。

##<a id="test"></a> 测试您的应用程序中的推式通知

1. 在 Visual Studio 中，请按 F5 键以运行该应用程序。

    >[AZURE.NOTE] 在 Windows Phone 仿真程序上测试时，您可能会遇到 401 未经授权的 RegistrationAuthorizationException。 这过程中可能出现`RegisterNativeAsync()`的方式调用 Windows Phone 仿真器与主机 PC 同步的时钟。 它可能会导致将被拒绝的安全令牌。 要解决这个问题只需手动在仿真程序中测试之前设置时钟。

5. 在应用程序中，文本框中输入文本"你好推送"，单击**保存**，然后立即单击开始按钮或后退按钮来离开应用程序。

    ![将文本输入到应用程序](./media/mobile-services-javascript-backend-windows-phone-get-started-push/mobile-quickstart-push3-wp8.png)

    这将插入请求发送到存储所添加的项的移动服务。 请注意该设备接收 toast 通知说**你好推**。

    ![Toast 通知接收](./media/mobile-services-javascript-backend-windows-phone-get-started-push/mobile-quickstart-push5-wp8.png)

    >[AZURE.NOTE]仍在该应用程序时，不会收到通知。 若要接收 toast 通知，应用程序处于活动状态时，您必须处理[ShellToastNotificationReceived](http://msdn.microsoft.com/library/windowsphone/develop/microsoft.phone.notification.httpnotificationchannel.shelltoastnotificationreceived.aspx)事件。

## <a name="next-steps"> </a>下一步行动

本教程说明了启用 Windows 应用商店应用程序可以使用移动服务和通知集线器发送推式通知的基础知识。 接下来，考虑下面的教程之一完成︰

+ [对经过身份验证的用户发送推式通知](mobile-services-javascript-backend-windows-phone-push-notifications-app-users.md)
    <br/>了解如何使用标记来向只有已通过身份验证的用户从您的移动服务发送推式通知。

+ [将广播的通知发送到订阅服务器](../notification-hubs-windows-phone-send-breaking-news.md)
    <br/>了解用户可以注册和接收推式通知类别他们感兴趣的方式。

+ [将与平台无关的通知发送到订阅服务器](../notification-hubs-aspnet-cross-platform-notify-users.md)
    <br/>了解如何使用模板发送推式通知从您的移动服务，无需在后端的手工创建特定于平台的载荷。


以下主题中了解有关移动服务和通知集线器的更多信息︰

* [Azure 通知集线器的诊断指南](../notification-hubs-diagnosing.md)
    <br/>了解如何解决您推式通知的问题。

* [开始使用身份验证]
  <br/>了解如何使用不同的帐户类型使用移动服务对您的应用程序的用户进行身份验证。

* [什么是通知集线器？]
  <br/>了解更多有关通知集线器原理将通知传递到您的应用程序跨所有主要客户平台。

* [移动服务.NET 帮助概念参考]
  <br/>了解更多关于如何使用.NET 的移动服务。

* [移动服务服务器脚本引用]
  <br/>了解有关如何在您的移动服务中实现业务逻辑。

<!-- Anchors. -->

<!-- Images. -->


<!-- URLs. -->
[提交应用程序页]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[我的应用程序]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[对于 Windows live SDK]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[添加到现有应用程序的移动服务]: mobile-services-windows-phone-get-started-data.md
[开始使用身份验证]: mobile-services-windows-phone-get-started-users.md

[设置身份验证的 web 服务发送 Windows Phone 的推式通知]: http://msdn.microsoft.com/library/windowsphone/develop/ff941099(v=vs.105).aspx

[移动服务服务器脚本引用]: http://go.microsoft.com/fwlink/?LinkId=262293
[移动服务.NET 帮助概念参考]: mobile-services-windows-dotnet-how-to-use-client-library.md

[什么是通知集线器？]: ../notification-hubs-overview.md

 
测试

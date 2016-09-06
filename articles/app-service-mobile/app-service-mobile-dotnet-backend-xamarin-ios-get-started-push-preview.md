---
ms.openlocfilehash: 713aa9fc2e1c82dbdff2cba7ced53b93f348b6db
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="将推式通知添加到您的 Xamarin.iOS 应用程序与 Azure 应用程序服务" 
    description="了解如何使用 Azure 应用程序服务于 Xamarin.iOS 应用程序发送推式通知" 
    services="app-service\mobile" 
    documentationCenter="xamarin" 
    authors="wesmc7777"
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="app-service-mobile" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-xamarin-ios" 
    ms.devlang="dotnet" 
    ms.topic="article"
    ms.date="08/22/2015" 
    ms.author="yuaxu"/>

# 向您的 Xamarin.iOS 应用程序添加推式通知

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push-preview](../../includes/app-service-mobile-selector-get-started-push-preview.md)]
&nbsp;  
[AZURE.INCLUDE [app-service-mobile-note-mobile-services-preview](../../includes/app-service-mobile-note-mobile-services-preview.md)]

##概述

在本教程中，您添加推式通知到[Xamarin.iOS 快速启动]项目以便在每次插入一条记录时，发送推式通知。 本教程基于[Xamarin.iOS 快速启动]教程，您必须首先完成。 如果您不使用下载快速入门服务器项目，您必须添加到项目的推式通知扩展包。 有关服务器扩展软件包的详细信息，请参阅[使用.NET 后端服务器用于 Azure 的移动应用程序的 SDK](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)。 

[IOS 模拟器不支持推式通知](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html)，因此您必须使用物理的 iOS 设备。 您还需要注册一个[苹果开发商计划的成员资格](https://developer.apple.com/programs/ios/)。

##先决条件

若要完成本教程，您需要以下各项︰

* 活动的 Azure 帐户。  
如果您还没有帐户，注册 Azure 的试用版，获得最多 10 个免费的移动应用程序。 您可以继续使用它们即使在试用期结束后。 [Azure 的免费试用版](http://azure.microsoft.com/pricing/free-trial/)，请参阅。

* Mac [Xamarin Studio]和[Xcode] v4.4 后或更高版本安装它。  
请注意，通过使用 Xamarin Studio 在 Mac 上运行您的 Xamarin.iOS 应用程序更容易。 您可以运行 Xamarin.iOS 应用程序通过使用 Visual Studio 在 Windows 计算机上，如果您需要，但它有点更加复杂，因为您必须连接到网络的 mac。 如果您感兴趣进行该操作，请参阅[在 Windows 上安装 Xamarin.iOS]。

* 一种物理的 iOS 设备。

* 完成本[快速入门教程](../app-service-mobile-dotnet-backend-xamarin-ios-get-started-preview.md)。

## <a id="register"></a>推式通知有关注册应用程序

[AZURE.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

## 配置 Azure 发送推式通知

[AZURE.INCLUDE [app-service-mobile-apns-configure-push-preview](../../includes/app-service-mobile-apns-configure-push-preview.md)]

##<a id="update-server"></a>更新发送推式通知的服务器项目

[AZURE.INCLUDE [app-service-mobile-apns-configure-push-preview](../../includes/app-service-mobile-dotnet-backend-configure-push-apns.md)]

## <a name="publish-the-service"></a>部署到 Azure 服务器项目

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-publish-service-preview](../../includes/app-service-mobile-dotnet-backend-publish-service-preview.md)]

## <a name="configure-app"></a>对 Xamarin.iOS 项目进行配置

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-publish-service-preview](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]

## <a name="add-push"></a>将推式通知添加到您的应用程序

1. 在**QSTodoService**，以便**AppDelegate**可以获取移动客户端添加以下属性︰
        
            public MobileServiceClient GetClient {
            get
            {
                return client;
            }
            private set
            {
                client = value;
            }
        }

1. 添加以下`using`到**AppDelegate.cs**文件的开头的语句。

        using Microsoft.WindowsAzure.MobileServices;

2. 在**AppDelegate**，重写**FinishedLaunching**事件︰ 

        public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
        {
            // registers for push for iOS8
            var settings = UIUserNotificationSettings.GetSettingsForTypes(
                UIUserNotificationType.Alert 
                | UIUserNotificationType.Badge 
                | UIUserNotificationType.Sound, 
                new NSSet());

            UIApplication.SharedApplication.RegisterUserNotificationSettings(settings); 
            UIApplication.SharedApplication.RegisterForRemoteNotifications();

            return true;
        }

3. 在同一文件中，重写**RegisteredForRemoteNotifications**事件︰

        public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
        {
            MobileServiceClient client = QSTodoService.DefaultService.GetClient;

            // Register for push with your mobile app
            var push = client.GetPush();
            push.RegisterAsync(deviceToken);
        }

4. 然后，重写**DidReceivedRemoteNotification**事件︰

        public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
        {
            NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;

            string alert = string.Empty;
            if (aps.ContainsKey(new NSString("alert")))
                alert = (aps [new NSString("alert")] as NSString).ToString();

            //show alert
            if (!string.IsNullOrEmpty(alert))
            {
                UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                avAlert.Show();
            }
        }

现在，您的应用程序将更新以支持推送通知。

## <a name="test"></a>测试您的应用程序中的推式通知

1. 按**运行**按钮以生成项目并在 iOS 设备，启动应用程序，然后单击**确定**以接受推式通知。
    
    > [AZURE.NOTE] 从您的应用程序，您必须显式地接受推式通知。 此请求仅发生第一次应用程序运行。

2. 在应用程序中，键入任务，然后单击加号 (**+**) 图标。

3. 请验证通知收到后，然后单击**确定**以关闭通知。

4. 重复步骤 2，并立即关闭应用程序，然后验证显示通知。

您已成功完成本教程。

<!-- Images. -->

[24]: ./media/mobile-services-ios-get-started-push/mobile-services-quickstart-push2-ios.png
[Xamarin.iOS 快速入门]: app-service-mobile-dotnet-backend-xamarin-ios-get-started-preview.md

[5]: ./media/app-service-mobile-dotnet-backend-xamarin-ios-get-started-push-preview/mobile-services-ios-push-step5.png
[6]: ./media/app-service-mobile-dotnet-backend-xamarin-ios-get-started-push-preview/mobile-services-ios-push-step6.png
[7]: ./media/app-service-mobile-dotnet-backend-xamarin-ios-get-started-push-preview/mobile-services-ios-push-step7.png

[9]: ./media/app-service-mobile-dotnet-backend-xamarin-ios-get-started-push-preview/mobile-services-ios-push-step9.png
[10]: ./media/app-service-mobile-dotnet-backend-xamarin-ios-get-started-push-preview/mobile-services-ios-push-step10.png

[17]: ./media/partner-xamarin-mobile-services-ios-get-started-push/mobile-services-ios-push-step17.png
[18]: ./media/partner-xamarin-mobile-services-ios-get-started-push/mobile-services-selection.png
[19]: ./media/partner-xamarin-mobile-services-ios-get-started-push/mobile-push-tab-ios.png
[20]: ./media/partner-xamarin-mobile-services-ios-get-started-push/mobile-push-tab-ios-upload.png
[21]: ./media/partner-xamarin-mobile-services-ios-get-started-push/mobile-portal-data-tables.png
[22]: ./media/partner-xamarin-mobile-services-ios-get-started-push/mobile-insert-script-push2.png
[23]: ./media/partner-xamarin-mobile-services-ios-get-started-push/mobile-quickstart-push1-ios.png
[25]: ./media/partner-xamarin-mobile-services-ios-get-started-push/mobile-quickstart-push3-ios.png
[26]: ./media/partner-xamarin-mobile-services-ios-get-started-push/mobile-quickstart-push4-ios.png
[28]: ./media/partner-xamarin-mobile-services-ios-get-started-push/mobile-services-ios-push-step18.png

[101]: ./media/partner-xamarin-mobile-services-ios-get-started-push/mobile-services-ios-push-01.png
[102]: ./media/app-service-mobile-dotnet-backend-xamarin-ios-get-started-push-preview/mobile-services-ios-push-02.png
[103]: ./media/app-service-mobile-dotnet-backend-xamarin-ios-get-started-push-preview/mobile-services-ios-push-03.png
[104]: ./media/app-service-mobile-dotnet-backend-xamarin-ios-get-started-push-preview/mobile-services-ios-push-04.png
[105]: ./media/app-service-mobile-dotnet-backend-xamarin-ios-get-started-push-preview/mobile-services-ios-push-05.png
[106]: ./media/app-service-mobile-dotnet-backend-xamarin-ios-get-started-push-preview/mobile-services-ios-push-06.png
[107]: ./media/app-service-mobile-dotnet-backend-xamarin-ios-get-started-push-preview/mobile-services-ios-push-07.png
[108]: ./media/app-service-mobile-dotnet-backend-xamarin-ios-get-started-push-preview/mobile-services-ios-push-08.png

[110]: ./media/app-service-mobile-dotnet-backend-xamarin-ios-get-started-push-preview/mobile-services-ios-push-10.png
[111]: ./media/app-service-mobile-dotnet-backend-xamarin-ios-get-started-push-preview/mobile-services-ios-push-11.png
[112]: ./media/app-service-mobile-dotnet-backend-xamarin-ios-get-started-push-preview/mobile-services-ios-push-12.png
[113]: ./media/app-service-mobile-dotnet-backend-xamarin-ios-get-started-push-preview/mobile-services-ios-push-13.png
[114]: ./media/app-service-mobile-dotnet-backend-xamarin-ios-get-started-push-preview/mobile-services-ios-push-14.png
[115]: ./media/app-service-mobile-dotnet-backend-xamarin-ios-get-started-push-preview/mobile-services-ios-push-15.png
[116]: ./media/app-service-mobile-dotnet-backend-xamarin-ios-get-started-push-preview/mobile-services-ios-push-16.png
[117]: ./media/app-service-mobile-dotnet-backend-xamarin-ios-get-started-push-preview/mobile-services-ios-push-17.png

[120]:./media/app-service-mobile-dotnet-backend-xamarin-ios-get-started-push-preview/mobile-services-ios-push-20.png
[121]:./media/app-service-mobile-dotnet-backend-xamarin-ios-get-started-push-preview/mobile-services-ios-push-21.png
[122]:./media/app-service-mobile-dotnet-backend-xamarin-ios-get-started-push-preview/mobile-services-ios-push-22.png
[123]:./media/app-service-mobile-dotnet-backend-xamarin-ios-get-started-push-preview/mobile-services-ios-push-23.png
[124]:./media/app-service-mobile-dotnet-backend-xamarin-ios-get-started-push-preview/mobile-services-ios-push-24.png

[Xamarin Studio]: http://xamarin.com/platform
[安装 Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Xcode]: https://go.microsoft.com/fwLink/?LinkID=266532
[iOS 资源调配门户]: http://go.microsoft.com/fwlink/p/?LinkId=272456
[移动服务 iOS SDK]: https://go.microsoft.com/fwLink/p/?LinkID=266533
[苹果推送通知服务]: http://go.microsoft.com/fwlink/p/?LinkId=272584
[开始使用移动服务]: /en-us/develop/mobile/tutorials/get-started-xamarin-ios
[有关数据入门]: /en-us/develop/mobile/tutorials/get-started-with-data-xamarin-ios
[开始使用身份验证]: /en-us/develop/mobile/tutorials/get-started-with-users-xamarin-ios
[开始使用推式通知]: /en-us/develop/mobile/tutorials/get-started-with-push-xamarin-ios
[推式通知给应用程序用户]: /en-us/develop/mobile/tutorials/push-notifications-to-users-ios
[授权用户使用的脚本]: /en-us/develop/mobile/tutorials/authorize-users-in-scripts-xamarin-ios
[Xamarin 设备置备]: http://developer.xamarin.com/guides/ios/getting_started/installation/device_provisioning/
[在 Windows 上安装 Xamarin.iOS]: http://developer.xamarin.com/guides/ios/getting_started/installation/windows/


[Azure 的管理门户]: https://manage.windowsazure.com/
[apns 对象]: http://go.microsoft.com/fwlink/p/?LinkId=272333
[Azure 的移动服务组件]: http://components.xamarin.com/view/azure-mobile-services/
[已完成的示例项目]: http://go.microsoft.com/fwlink/p/?LinkId=331303

 
测试

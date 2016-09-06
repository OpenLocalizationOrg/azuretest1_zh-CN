---
ms.openlocfilehash: e921d5f3a559ed203ad824ab69b77e1c8ce490b5
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="将推送通知添加到 iOS Azure 移动应用程序的应用程序"
    description="了解如何使用 Azure 移动应用程序可于 iOS 应用程序发送推式通知。"
    services="app-service\mobile"
    documentationCenter="ios"
    manager="dwrede"
    editor=""
    authors="krisragh"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="08/22/2015"
    ms.author="krisragh"/>


# 添加到您的 iOS 应用程序推送通知

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push-preview](../../includes/app-service-mobile-selector-get-started-push-preview.md)]
&nbsp;  
[AZURE.INCLUDE [app-service-mobile-note-mobile-services-preview](../../includes/app-service-mobile-note-mobile-services-preview.md)]

在本教程中，您添加推式通知到[iOS 快速启动]项目以便在每次插入一条记录时，发送推式通知。 本教程基于[iOS 快速启动]教程，您必须首先完成。 如果您不使用下载快速入门服务器项目，您必须添加到项目的推式通知扩展包。 有关服务器扩展软件包的详细信息，请参阅[使用.NET 后端服务器用于 Azure 的移动应用程序的 SDK](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)。 

[IOS 模拟器不支持推式通知](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html)，因此，对于本教程，您需要物理的 iOS 设备和一个[苹果开发商计划的成员资格](https://developer.apple.com/programs/ios/)。 

## <a id="register"></a>推式通知有关注册应用程序

[AZURE.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

## 配置 Azure 发送推式通知

[AZURE.INCLUDE [app-service-mobile-apns-configure-push-preview](../../includes/app-service-mobile-apns-configure-push-preview.md)]

##<a id="update-server"></a>更新服务器项目发送推式通知

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-configure-push-apns](../../includes/app-service-mobile-dotnet-backend-configure-push-apns.md)]

## <a name="publish-the-service"></a>部署到 Azure 服务器项目

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-publish-service-preview](../../includes/app-service-mobile-dotnet-backend-publish-service-preview.md)]

## <a id="add-push"></a>向应用程序添加推式通知

[AZURE.INCLUDE [Add Push Notifications to App](../../includes/app-service-add-push-notifications-to-app.md)]

## <a id="test"></a>在应用程序中测试推式通知

[AZURE.INCLUDE [Test Push Notifications in App](../../includes/test-push-notifications-in-app.md)]

<!-- Anchors.  -->
[生成 iOS 的证书签名请求]: #certificates
[注册您的应用程序并启用推式通知]: #register
[创建应用程序设置的配置文件]: #profile
[向应用程序添加推式通知]: #add-push
[配置您移动的后端，发送推送请求]: #configure
[更新服务器发送推式通知]: #update-server
[发布到 Azure 的移动后端]: #publish-mobile-service
[测试您的应用程序]: #test-the-service

<!-- Images. -->
[5]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-step5.png
[6]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-step6.png
[7]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-step7.png

[9]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-step9.png
[10]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-step10.png
[17]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-step17.png
[18]: ./media/mobile-services-ios-get-started-push/mobile-services-selection.png
[19]: ./media/mobile-services-ios-get-started-push/mobile-push-tab-ios.png
[20]: ./media/mobile-services-ios-get-started-push/mobile-push-tab-ios-upload.png
[21]: ./media/mobile-services-ios-get-started-push/mobile-portal-data-tables.png
[22]: ./media/mobile-services-ios-get-started-push/mobile-insert-script-push2.png
[23]: ./media/mobile-services-ios-get-started-push/mobile-quickstart-push1-ios.png
[24]: ./media/mobile-services-ios-get-started-push/mobile-quickstart-push2-ios.png
[25]: ./media/mobile-services-ios-get-started-push/mobile-quickstart-push3-ios.png
[26]: ./media/mobile-services-ios-get-started-push/mobile-quickstart-push4-ios.png
[28]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-step18.png

[101]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-01.png
[102]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-02.png
[103]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-03.png
[104]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-04.png
[105]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-05.png
[106]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-06.png
[107]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-07.png
[108]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-08.png

[110]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-10.png
[111]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-11.png
[112]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-12.png
[113]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-13.png
[114]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-14.png
[115]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-15.png
[116]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-16.png
[117]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-17.png

<!-- URLs. -->
[iOS 快速入门]: app-service-mobile-dotnet-backend-ios-get-started-preview.md
[安装 Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS 资源调配门户]: http://go.microsoft.com/fwlink/p/?LinkId=272456
[Azure 的移动应用程序 iOS SDK]: https://go.microsoft.com/fwLink/?LinkID=529823
[Azure 通知集线器 Nuget]: https://www.nuget.org/packages/WindowsAzure.ServiceBus/
[苹果推送通知服务]: http://go.microsoft.com/fwlink/p/?LinkId=272584
[开始使用移动服务]: ../mobile-services-dotnet-backend-ios-get-started.md
[开始使用移动应用程序]: app-service-mobile-dotnet-backend-ios-get-started-preview.md
[Azure 的管理门户]: https://manage.windowsazure.com/
[apns 对象]: http://go.microsoft.com/fwlink/p/?LinkId=272333
 

测试

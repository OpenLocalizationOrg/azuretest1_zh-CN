---
ms.openlocfilehash: 57d5bfacafa172d5801a883d9824964c1c070755
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
    ms.tgt_pltfrm="windows"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="06/16/2015"
    ms.author="glenga"/>


# 向您的移动服务应用程序添加推式通知

[AZURE.INCLUDE [mobile-services-selector-get-started-push-legacy](../../includes/mobile-services-selector-get-started-push-legacy.md)]

本主题演示如何使用 Azure 移动服务为 Windows 应用商店应用程序发送推式通知。 在本教程中启用推式通知使用 Azure 通知集线器到快速启动项目。 完成后，您的移动服务将发送通知集线器插入记录每次均使用推式通知。 创建通知中心是自由与您的移动服务，可以来管理独立于移动服务，并可由其他应用程序和服务。

>[AZURE.NOTE]本主题演示如何手动配置为 Windows 应用商店应用程序使用 Windows 通知服务 (WNS) 推式通知。 您可以使用 Visual Studio 2013年工具自动配置相同的推式通知，Windows 应用程序项目中。 有关详细信息，请参阅本教程的最[通用的 Windows 应用程序版本](mobile-services-javascript-backend-windows-store-javascript-get-started-push.md)。

本教程将引导您完成以下基本步骤启用推式通知︰

1. [与 WNS 注册您的应用程序并配置移动服务](#register)
2. [更新应用程序注册通知](#update-app)
3. [更新服务器脚本发送推式通知](#update-scripts)
3. [插入数据接收推式通知](#test)

本教程基于移动服务快速入门。 在开始本教程之前，您必须首先完成[移动服务入门]或[入门数据]以连接到移动服务的项目。 移动服务尚未连接，当添加推式通知向导将创建此连接，为您。

##<a id="register"></a> 与 WNS 注册您的应用程序并配置移动服务

[AZURE.INCLUDE [mobile-services-notification-hubs-register-windows-store-app](../../includes/mobile-services-notification-hubs-register-windows-store-app.md)]

现在将您的移动服务和应用程序配置为使用 WNS 和通知集线器。 接下来，您将更新您的 Windows 应用商店应用程序注册的通知。

##<a id="update-app"></a> 更新应用程序注册通知

您的应用程序可以接收推式通知之前，您必须注册通知通道。

1. 打开文件 default.js 和**MobileServiceClient**创建的代码后插入以下代码︰

        // Request a push notification channel.
        Windows.Networking.PushNotifications
            .PushNotificationChannelManager
            .createPushNotificationChannelForApplicationAsync()
            .then(function (channel) {
                // Register for notifications using the new channel
                client.push.registerNative(channel.uri);
            });

    这段代码从 WNS，检索应用程序 ChannelURI，，然后注册该 ChannelURI 的推式通知。

2. 按**F5**键以运行该应用程序。 显示弹出式对话框中的注册密钥。

6. 打开 Package.appxmanifest 文件，请确保在**应用程序用户界面**选项卡中，**烘烤能够**被设置为**是**。

    ![][2]

    这将确保您的应用程序可以引发 toast 通知。

##<a id="update-scripts"></a> 更新服务器脚本发送推式通知

[AZURE.INCLUDE [mobile-services-javascript-update-script-notification-hubs](../../includes/mobile-services-javascript-update-script-notification-hubs.md)]

##<a id="test"></a> 测试您的应用程序中的推式通知

[AZURE.INCLUDE [mobile-services-windows-store-test-push](../../includes/mobile-services-windows-store-test-push.md)]

## <a name="next-steps"> </a>下一步行动

本教程说明了启用 Windows 应用商店应用程序可以使用移动服务和通知集线器发送推式通知的基础知识。 接下来，考虑完成下一步的教程中，[为发送推式通知经过身份验证的用户]，它显示了如何使用标签来从手机服务向只有已通过身份验证的用户发送推式通知。

<!---+ [Send push notifications to authenticated users]
    <br/>Learn how to use tags to send push notifications from a Mobile Service to only an authenticated user.

+ [Send broadcast notifications to subscribers]
    <br/>Learn how users can register and receive push notifications for categories they're interested in.

+ [Send template-based notifications to subscribers]
    <br/>Learn how to use templates to send push notifications from a Mobile Service, without having to craft platform-specific payloads in your back-end.
-->

以下主题中了解有关移动服务和通知集线器的更多信息︰

* [有关数据入门]
  <br/>了解有关存储和查询数据使用移动服务。

* [开始使用身份验证]
  <br/>了解如何使用不同的帐户类型使用移动服务对您的应用程序的用户进行身份验证。

* [什么是通知集线器？]
  <br/>了解更多有关通知集线器原理将通知传递到您的应用程序跨所有主要客户平台。

* [调试应用程序通知集线器](http://go.microsoft.com/fwlink/p/?linkid=386630)
  </br>获得故障诊断指南和通知集线器的解决方案进行调试。

* [移动服务.NET 帮助概念参考]
  <br/>了解更多关于如何使用.NET 的移动服务。

* [移动服务服务器脚本引用]
  <br/>了解有关如何在您的移动服务中实现业务逻辑。

<!-- Anchors. -->

<!-- Images. -->


[2]: ./media/mobile-services-javascript-backend-windows-store-javascript-get-started-push/mobile-app-enable-toast-win8.png


<!-- URLs. -->
[提交应用程序页]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[我的应用程序]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[对于 Windows live SDK]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[开始使用移动服务]: ../mobile-services-windows-store-get-started.md
[有关数据入门]: mobile-services-windows-store-javascript-get-started-data.md
[开始使用身份验证]: mobile-services-windows-store-javascript-get-started-users.md

[移动服务服务器脚本引用]: http://go.microsoft.com/fwlink/?LinkId=262293
[移动服务.NET 帮助概念参考]: mobile-services-windows-dotnet-how-to-use-client-library.md


[对经过身份验证的用户发送推式通知]: mobile-services-javascript-backend-windows-store-javascript-push-notifications-app-users.md

[什么是通知集线器？]: ../notification-hubs-overview.md
[将广播的通知发送到订阅服务器]: ../notification-hubs-windows-store-javascript-send-breaking-news.md
[将基于模板的通知发送到订阅服务器]: ../notification-hubs-windows-store-javascript-send-localized-breaking-news.md

测试

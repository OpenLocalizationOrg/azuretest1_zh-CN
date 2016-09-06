---
ms.openlocfilehash: e81d47351944a0d3f2db951b67ad1b921e1f2806
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

<properties
    pageTitle="开始使用推式通知 (Android JavaScript) |Microsoft Azure"
    description="了解如何使用 Azure 移动服务于 Android 的 JavaScript 应用程序发送推式通知。"
    services="mobile-services, notification-hubs"
    documentationCenter="android"
    authors="RickSaling"
    writer="ricksal"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="06/16/2015" 
    ms.author="ricksal"/>


# 向您的移动服务 Android 应用程序添加推式通知

[AZURE.INCLUDE [mobile-services-selector-get-started-push](../../includes/mobile-services-selector-get-started-push.md)]

## 摘要

本主题演示如何使用 Azure 移动服务于 Android 应用程序使用 Google 云消息 ("GCM") 发送推式通知。 您将推式通知的快速启动项目的是本教程中的先决条件。 通过使用 Azure 通知中心包括在您的移动服务启用推式通知。 完成后，您的移动服务将发送推式通知每当插入记录。

## 先决条件

[AZURE.INCLUDE [mobile-services-android-prerequisites](../../includes/mobile-services-android-prerequisites.md)]

##<a id="register"></a>启用消息的 Google 云

[AZURE.INCLUDE [mobile-services-enable-Google-cloud-messaging](../../includes/mobile-services-enable-google-cloud-messaging.md)]

##<a id="configure"></a>配置移动服务发送推送请求

[AZURE.INCLUDE [mobile-services-android-configure-push](../../includes/mobile-services-android-configure-push.md)]

##<a id="add-push"></a>将推式通知添加到您的应用程序



下一步是安装 Google 播放服务。 Google 云消息有某些最低 API 级别要求开发和测试，该清单中的**minSdkVersion**属性必须符合。

如果将测试与较旧的设备，然后请参阅[设置了 Google 播放服务 SDK]来确定如何低可以设置此值，并进行适当设置。

###向项目中添加 Google 播放服务

[AZURE.INCLUDE [Add Play Services](../../includes/mobile-services-add-google-play-services.md)]

###添加代码

[AZURE.INCLUDE [mobile-services-android-getting-started-with-push](../../includes/mobile-services-android-getting-started-with-push.md)]


##<a id="update-scripts"></a>更新管理门户中的注册的插入脚本

[AZURE.INCLUDE [mobile-services-javascript-backend-android-push-insert-script](../../includes/mobile-services-javascript-backend-android-push-insert-script.md)]


##<a id="test"></a>测试您的应用程序中的推式通知

通过直接附加 Android 使用 USB 电缆，电话或通过在仿真程序中使用的虚拟设备，可以测试该应用程序。

###设置测试的 Android 模拟器

在仿真程序中运行此应用程序时，请确保您使用 Android 虚拟设备 (AVD) 支持 Google 的 Api。

1. 从工具栏的右端，选择 Android 的虚拟设备管理器，选择您的设备，单击右侧的编辑图标。

    ![](./media/mobile-services-javascript-backend-android-get-started-push/mobile-services-android-virtual-device-manager.png)

2. 设备说明行上选择**更改**，选择**Google 的 Api**，然后单击确定。

    ![](./media/mobile-services-javascript-backend-android-get-started-push/mobile-services-android-virtual-device-manager-edit.png)

    此目标使用 Google 的 Api AVD。

###运行测试

1. 从**运行**菜单项，请单击**运行应用程序**以启动该应用程序。

2. 在应用程序中，键入有意义的文本，例如，_新的移动服务任务_，然后单击**添加**按钮。

    ![](./media/mobile-services-javascript-backend-android-get-started-push/mobile-quickstart-push1-android.png)

3. 从屏幕打开设备的通知的抽屉中，可看到通告的顶部向下刷。


您已成功完成本教程。

## 疑难解答

### 验证 Android SDK 版本

[AZURE.INCLUDE [Verify SDK](../../includes/mobile-services-verify-android-sdk-version.md)]


## 旧版代码

如果您想要查看本教程的 Eclipse 版本，请转到[开始使用推式通知 (Eclipse)]。


<!--
To see a completed version of the source code in an Eclipse project, go <a href="https://github.com/Azure/mobile-services-samples/tree/master/GettingStartedWithData/Android">here</a>.
-->


## <a name="next-steps"> </a>下一步行动

<!---This tutorial demonstrated the basics of enabling an Android app to use Mobile Services and Notification Hubs to send push notifications. Next, consider completing the next tutorial, [Send push notifications to authenticated users], which shows how to use tags to send push notifications from a Mobile Service to only an authenticated user.

+ [Send push notifications to authenticated users]
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

* [如何对移动服务使用 Android 的客户端库]
  <br/>了解更多关于如何使用 Android 的移动服务。

* [移动服务服务器脚本引用]
  <br/>了解有关如何在您的移动服务中实现业务逻辑。


<!-- Anchors. -->
[注册您的推送通知的应用程序并配置移动服务]: #register
[更新生成的推式通知代码]: #update-scripts
[插入数据接收通知]: #test
[下一步行动]:#next-steps

<!-- Images. -->
[13]: ./media/mobile-services-windows-store-javascript-get-started-push/mobile-quickstart-push1.png
[14]: ./media/mobile-services-windows-store-javascript-get-started-push/mobile-quickstart-push2.png


<!-- URLs. -->
[开始使用推式通知 (Eclipse)]: mobile-services-javascript-backend-android-get-started-push-ec.md
[提交应用程序页]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[我的应用程序]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[开始使用移动服务]: mobile-services-android-get-started.md
[有关数据入门]: mobile-services-android-get-started-data.md
[开始使用身份验证]: mobile-services-android-get-started-users.md
[开始使用推式通知]: /develop/mobile/tutorials/get-started-with-push-js
[推式通知给应用程序用户]: /develop/mobile/tutorials/push-notifications-to-users-js
[授权用户使用的脚本]: /develop/mobile/tutorials/authorize-users-in-scripts-js
[JavaScript 和 HTML]: /develop/mobile/tutorials/get-started-with-push-js
[安装了 Google 播放服务 SDK]: http://go.microsoft.com/fwlink/?LinkId=389801
[Azure 的管理门户]: https://manage.windowsazure.com/
[如何对移动服务使用 Android 的客户端库]: mobile-services-android-how-to-use-client-library.md

[gcm 对象]: http://go.microsoft.com/fwlink/p/?LinkId=282645

[移动服务服务器脚本引用]: http://go.microsoft.com/fwlink/?LinkId=262293

[对经过身份验证的用户发送推式通知]: mobile-services-javascript-backend-android-push-notifications-app-users.md

[什么是通知集线器？]: ../notification-hubs-overview.md
[将广播的通知发送到订阅服务器]: ../notification-hubs-android-send-breaking-news.md
[将基于模板的通知发送到订阅服务器]: ../notification-hubs-android-send-localized-breaking-news.md

测试

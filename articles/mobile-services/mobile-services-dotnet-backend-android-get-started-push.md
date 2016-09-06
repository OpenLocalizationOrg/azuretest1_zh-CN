---
ms.openlocfilehash: 7840b57168576c27742ecdc60111a64c115a4824
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="开始使用推 (Android) |Microsoft Azure" 
    description="了解如何使用 Azure 移动服务于 Android 的.Net 应用程序发送推式通知。" 
    services="mobile-services, notification-hubs" 
    documentationCenter="android" 
    authors="RickSaling" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-android" 
    ms.devlang="java" 
    ms.topic="article" 
    ms.date="07/02/2015" 
    ms.author="ricksal"/>

# 向您的移动服务应用程序添加推式通知

[AZURE.INCLUDE [mobile-services-selector-get-started-push](../../includes/mobile-services-selector-get-started-push.md)]

本主题演示如何使用 Azure 移动服务于 Android 应用程序发送推式通知。 在本教程中，您添加到快速入门项目使用 Google 云消息 (GCM) 的推式通知。 完成后，您的移动服务将发送推式通知每当插入记录。 

本教程基于移动服务快速入门。 在开始本教程之前，您必须首先完成[移动服务入门]或[入门数据]以连接到移动服务的项目。 在这种情况下，本教程还要求 Visual Studio 2013年。 

>[AZURE.NOTE] 本教程的 Eclipse 版本，请参阅[开始使用推式通知 (Eclipse)]。
 
##<a id="register"></a>启用消息的 Google 云

[AZURE.INCLUDE [mobile-services-enable-Google-cloud-messaging](../../includes/mobile-services-enable-google-cloud-messaging.md)]

##<a id="configure"></a>配置移动服务发送推送请求

[AZURE.INCLUDE [mobile-services-android-configure-push](../../includes/mobile-services-android-configure-push.md)]

##<a id="update-server"></a>更新移动服务发送推式通知

[AZURE.INCLUDE [mobile-services-dotnet-backend-android-push-update-service](../../includes/mobile-services-dotnet-backend-android-push-update-service.md)]

##<a name="update-app"></a>将推式通知添加到您的应用程序

###验证 Android SDK 版本

[AZURE.INCLUDE [mobile-services-verify-android-sdk-version](../../includes/mobile-services-verify-android-sdk-version.md)]


下一步是安装 Google 播放服务。 Google 云消息有某些最低 API 级别要求开发和测试，该清单中的**minSdkVersion**属性必须符合。 

如果将测试与较旧的设备，然后请参考 [设置了 Google 播放服务 SDK] 来确定如何低可以设置此值，并进行适当设置。

###向项目中添加 Google 播放服务

[AZURE.INCLUDE [Add Play Services](../../includes/mobile-services-add-google-play-services.md)]

###添加代码

[AZURE.INCLUDE [mobile-services-android-getting-started-with-push](../../includes/mobile-services-android-getting-started-with-push.md)]

##<a name="test-app"></a>测试针对手机信息发布服务应用程序

通过直接附加 Android 使用 USB 电缆，电话或通过在仿真程序中使用的虚拟设备，可以测试该应用程序。

###<a id="local-testing"></a> 启用推式通知进行本地测试

[AZURE.INCLUDE [mobile-services-dotnet-backend-configure-local-push](../../includes/mobile-services-dotnet-backend-configure-local-push.md)]

[AZURE.INCLUDE [mobile-services-android-push-notifications-test](../../includes/mobile-services-android-push-notifications-test.md)]

您已成功完成本教程。

## <a name="next-steps"> </a>下一步行动

本教程说明了启用使用移动服务和通知集线器发送推式通知的 Android 应用程序的基础知识。 接下来，考虑完成下一步的教程中，[为发送推式通知经过身份验证的用户]，它显示了如何使用标签来向只有已通过身份验证的用户从您的移动服务发送推式通知。

+ [对经过身份验证的用户发送推式通知]
    <br/>了解如何使用标记来从手机服务向只有已通过身份验证的用户发送推式通知。

+ [将广播的通知发送到订阅服务器]
    <br/>了解用户可以注册和接收推式通知类别他们感兴趣的方式。

+ [将基于模板的通知发送到订阅服务器]
    <br/>了解如何使用模板从手机信息服务，发送推式通知，无需在后端的手工创建特定于平台的载荷。

以下主题中了解有关移动服务和通知集线器的更多信息︰

* [添加到您的应用程序的身份验证][开始使用身份验证]
  <br/>了解如何使用不同的帐户类型使用移动服务对您的应用程序的用户进行身份验证。

* [什么是通知集线器？]
  <br/>了解更多有关通知集线器原理将通知传递到您的应用程序跨所有主要客户平台。

* [调试应用程序通知集线器](http://go.microsoft.com/fwlink/p/?linkid=386630)
  </br>获得故障诊断指南和通知集线器的解决方案进行调试。 

* [如何对移动服务使用 Android 的客户端库]
  <br/>了解更多关于如何使用 Android 的移动服务。  
  
<!-- Anchors. -->

[创建新的移动服务]: #create-service
[下载本地服务]: #download-the-service-locally
[测试移动服务]: #test-the-service
[下载 GetStartedWithData 项目]: #download-app
[更新应用程序来使用用于数据访问的移动服务]: #update-app
[测试 Android 应用程序针对本地承载的服务]: #test-locally-hosted
[发布到 Azure 的移动服务]: #publish-mobile-service
[Android 应用程序承载于 Azure 服务测试]: #test-azure-hosted
[测试针对手机信息发布服务应用程序]: #test-app
[下一步行动]:#next-steps

<!-- Images. -->

<!-- URLs. -->
[开始使用推式通知 (Eclipse)]: mobile-services-dotnet-backend-android-get-started-push-ec.md
[开始使用移动服务]: mobile-services-dotnet-backend-android-get-started.md
[有关数据入门]: mobile-services-dotnet-backend-android-get-started-data.md
[开始使用身份验证]: mobile-services-dotnet-backend-android-get-started-users.md
[管理门户]: https://manage.windowsazure.com/
[移动服务 SDK]: http://go.microsoft.com/fwlink/p/?LinkId=257545

[如何对移动服务使用 Android 的客户端库]: mobile-services-android-how-to-use-client-library.md

[对经过身份验证的用户发送推式通知]: mobile-services-dotnet-backend-android-push-notifications-app-users.md

[什么是通知集线器？]: ../notification-hubs-overview.md
[将广播的通知发送到订阅服务器]: ../notification-hubs-windows-store-dotnet-send-breaking-news.md
[将基于模板的通知发送到订阅服务器]: ../notification-hubs-windows-store-dotnet-send-localized-breaking-news.md
[Azure 的管理门户]: https://manage.windowsazure.com/
 

测试

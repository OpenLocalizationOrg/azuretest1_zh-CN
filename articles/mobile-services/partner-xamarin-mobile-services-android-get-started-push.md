---
ms.openlocfilehash: 25039f7ea1b5f8bc1beea5525c3829907d329b85
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="将推式通知添加到您的 Xamarin Android 应用程序 |Microsoft Azure" 
    description="了解如何配置推式通知 Google 云消息为您 Xamarin.Android 应用程序使用 Azure 移动服务和 Azure 通知集线器。" 
    documentationCenter="xamarin" 
    authors="ggailey777" 
    manager="dwrede" 
    services="mobile-services" 
    editor=""/>

<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-xamarin-android" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="06/19/2015" 
    ms.author="glenga"/>

# 向您的移动服务应用程序添加推式通知

[AZURE.INCLUDE [mobile-services-selector-get-started-push](../../includes/mobile-services-selector-get-started-push.md)]

##概述
本主题演示如何使用 Azure 移动服务向 Xamarin.Android 应用程序发送推式通知。 在本教程中，您将添加使用 Google 云消息 (GCM)[开始使用移动服务]项目服务的推式通知。 完成后，您的移动服务将发送推式通知每当插入记录。

本教程要求如下︰

+ 活动的 Google 帐户。
+ [Google 云消息传递客户端组件]。 在本教程中，您将添加该组件。

您应该已经[Xamarin.Android]和[Azure 移动服务组件]安装从项目中，当您完成[开始使用移动服务]或[添加到现有应用程序的移动服务]。

##<a id="register"></a>启用消息的 Google 云

[AZURE.INCLUDE [mobile-services-enable-Google-cloud-messaging](../../includes/mobile-services-enable-google-cloud-messaging.md)]

##<a id="configure"></a>配置移动服务发送推送请求

[AZURE.INCLUDE [mobile-services-android-configure-push](../../includes/mobile-services-android-configure-push.md)]

##<a id="update-scripts"></a>注册的更新插入脚本发送通知

>[AZURE.TIP] 以下步骤显示了如何更新注册到 Azure 管理门户中的 TodoItem 表的插入操作的脚本。 您还可以访问并编辑此移动服务脚本直接在 Visual Studio，Azure 服务器资源管理器的节点中。 

[AZURE.INCLUDE [mobile-services-javascript-backend-android-push-insert-script](../../includes/mobile-services-javascript-backend-android-push-insert-script.md)]


##<a id="configure-app"></a>配置推式通知的现有项目

[AZURE.INCLUDE [mobile-services-xamarin-android-push-configure-project](../../includes/mobile-services-xamarin-android-push-configure-project.md)]

##<a id="add-push"></a>将推式通知代码添加到您的应用程序

[AZURE.INCLUDE [mobile-services-xamarin-android-push-add-to-app](../../includes/mobile-services-xamarin-android-push-add-to-app.md)]

##<a id="test"></a>测试您的应用程序中的推式通知

通过直接附加 Android 使用 USB 电缆，电话或通过在仿真程序中使用的虚拟设备，可以测试该应用程序。

[AZURE.INCLUDE [mobile-services-android-push-notifications-test](../../includes/mobile-services-android-push-notifications-test.md)]

您已成功完成本教程。

## <a name="next-steps"></a>下一步行动

以下主题中了解有关移动服务和通知集线器的更多信息︰

* [添加到现有应用程序的移动服务]
  <br/>了解有关存储和查询数据使用移动服务。

* [开始使用身份验证](mobile-services-android-get-started-users.md)
  <br/>了解如何使用不同的帐户类型使用移动服务对您的应用程序的用户进行身份验证。

* [什么是通知集线器？](../notification-hubs-overview.md)
  <br/>了解更多有关通知集线器原理将通知传递到您的应用程序跨所有主要客户平台。

* [调试应用程序通知集线器](http://go.microsoft.com/fwlink/p/?linkid=386630)
  </br>获得故障诊断指南和通知集线器的解决方案进行调试。 

* [如何使用移动服务.NET 客户端库](mobile-services-windows-dotnet-how-to-use-client-library.md)
  <br/>了解更多关于如何使用移动服务与 Xamarin C# 的代码。

* [移动服务服务器脚本引用](mobile-services-how-to-use-server-scripts.md)
  <br/>了解有关如何在您的移动服务中实现业务逻辑。

<!-- URLs. -->
[开始使用移动服务]: mobile-services-ios-get-started.md
[添加到现有应用程序的移动服务]: mobile-services-android-get-started-data.md

[Google 云消息传递客户端组件]: http://components.xamarin.com/view/GCMClient/
[Xamarin.Android]: http://xamarin.com/download/
[Azure 的移动服务组件]: http://components.xamarin.com/view/azure-mobile-services/
 
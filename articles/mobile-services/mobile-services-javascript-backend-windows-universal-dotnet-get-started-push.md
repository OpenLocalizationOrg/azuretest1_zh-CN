---
ms.openlocfilehash: f049f7888fe8d8153a832a0d492b620d4e241384
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="将推式通知添加到 Windows 8.1 通用应用程序 |Microsoft Azure" 
    description="了解如何从使用 Azure 通知集线器 JavaScript 后端移动服务于通用 Windows 8.1 应用程序发送推式通知。" 
    services="mobile-services,notification-hubs" 
    documentationCenter="windows" 
    authors="ggailey777" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="07/22/2015" 
    ms.author="glenga"/>


# 向您的移动服务应用程序添加推式通知

[AZURE.INCLUDE [mobile-services-selector-get-started-push](../../includes/mobile-services-selector-get-started-push.md)]

本主题演示如何使用 JavaScript 后端 Azure 移动服务到通用的 Windows 应用程序发送推式通知。 在本教程中，您启用推式通知通用的 Windows 应用程序项目中使用 Azure 通知集线器。 完成后，您的移动服务将发送推式通知从 JavaScript 后端到所有已注册 Windows 应用商店和 Windows Phone 存储应用程序每次任务列表的表中插入记录。 创建通知中心是自由与您的移动服务，可以来管理独立于移动服务，并可由其他应用程序和服务。

>[AZURE.NOTE]本主题演示如何使用 Visual Studio 2013 更新 3 中刀具到通用的 Windows 应用程序从移动服务中添加对推式通知的支持。 可以使用相同的步骤添加到 Windows 应用商店或 Windows Phone 商店 8.1 app 推式通知移动服务。 若要添加 Windows Phone 8 或 Windows Phone Silverlight 8.1 app 推式通知，请参阅此版本[开始使用推式通知中移动服务](mobile-services-javascript-backend-windows-phone-get-started-push.md)。

本教程将引导您完成以下基本步骤启用推式通知︰

1. [注册您的推送通知的应用程序](#register)
2. [更新服务发送推式通知](#update-service)
3. [测试您的应用程序中的推式通知](#test)

若要完成本教程，您需要以下各项︰

* 活动的[Microsoft 存储帐户](http://go.microsoft.com/fwlink/p/?LinkId=280045)。
* [Visual Studio 2013年速成版的 Windows](http://go.microsoft.com/fwlink/?LinkId=257546)更新 3 或更高版本

##<a id="register"></a>注册您的推送通知的应用程序

[AZURE.INCLUDE [mobile-services-create-new-push-vs2013](../../includes/mobile-services-create-new-push-vs2013.md)]

&nbsp;&nbsp;6.浏览到`\Services\MobileServices\your_service_name`项目文件夹，打开生成的 push.register.cs 文件，并检查注册通知集线器设备的通道 URL 的**UploadChannel**方法。

&nbsp;&nbsp;7.打开的共享的文件 App.xaml.cs，并注意到**OnLaunched**事件处理程序中添加了对新的**UploadChannel**方法的调用。 这样可确保每次启动应用程序时，尝试注册的设备。

&nbsp;&nbsp;8.重复以上步骤，将推式通知添加到 Windows Phone 存储应用程序项目，然后在共享的 App.xaml.cs 文件，删除多余调用**UploadChannel** ，剩余`#if...#endif`条件包装。 现在，这两个项目可以共享单个调用**UploadChannel**。 

&nbsp;&nbsp;请注意，还可以通过统一简化生成的代码`#if...#endif`包装成单个解包定义这两个版本的应用程序所使用的[MobileServiceClient]定义。

现在，应用程序中启用推式通知，您必须更新移动服务发送推式通知。 

##<a id="update-service"></a>更新服务发送推式通知

以下步骤更新注册到 TodoItem 表插入脚本。 在服务器中的任何脚本或后端服务的其他任何地方，您可以实现类似的代码。 

[AZURE.INCLUDE [mobile-services-javascript-update-script-notification-hubs](../../includes/mobile-services-javascript-update-script-notification-hubs.md)]


##<a id="test"></a> 测试您的应用程序中的推式通知

[AZURE.INCLUDE [mobile-services-javascript-backend-windows-universal-test-push](../../includes/mobile-services-javascript-backend-windows-universal-test-push.md)]

## <a name="next-steps"> </a>下一步行动

本教程说明了启用 Windows 应用商店应用程序可以使用移动服务和通知集线器发送推式通知的基础知识。 接下来，考虑下面的教程之一完成︰

+ [对经过身份验证的用户发送推式通知](mobile-services-javascript-backend-windows-store-dotnet-push-notifications-app-users.md)
    <br/>了解如何使用标记来向只有已通过身份验证的用户从您的移动服务发送推式通知。

+ [将广播的通知发送到订阅服务器](../notification-hubs-windows-store-dotnet-send-breaking-news.md)
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

* [如何使用.NET 客户端 Azure 移动服务]
  <br/>了解更多关于如何使用移动服务从 C# Windows 应用程序。

<!-- Anchors. -->

<!-- Images. -->

<!-- URLs. -->
[提交应用程序页]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[我的应用程序]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[对于 Windows live SDK]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[开始使用移动服务]: mobile-services-dotnet-backend-windows-store-dotnet-get-started.md
[有关数据入门]: mobile-services-javascript-backend-windows-universal-dotnet-get-started-data.md
[开始使用身份验证]: mobile-services-javascript-backend-windows-universal-dotnet-get-started-users.md

[对经过身份验证的用户发送推式通知]: mobile-services-javascript-backend-windows-store-dotnet-push-notifications-app-users.md

[什么是通知集线器？]: ../notification-hubs-overview.md

[如何使用.NET 客户端 Azure 移动服务]: mobile-services-windows-dotnet-how-to-use-client-library.md
[MobileServiceClient]: http://go.microsoft.com/fwlink/p/?LinkId=302030
 
测试

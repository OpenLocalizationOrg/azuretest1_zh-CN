---
ms.openlocfilehash: 833dd54ef48d82954b8496eebbd2c945aea35af9
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="将推式通知添加到 Windows 8.1 通用应用程序 |Microsoft Azure" 
    description="了解如何从使用 Azure 通知集线器.NET 后端移动服务于通用 Windows 8.1 应用程序发送推式通知。" 
    services="mobile-services,notification-hubs" 
    documentationCenter="windows" 
    authors="ggailey777" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-store" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="07/21/2015" 
    ms.author="glenga"/>

# 向您的移动服务应用程序添加推式通知

[AZURE.INCLUDE [mobile-services-selector-get-started-push](../../includes/mobile-services-selector-get-started-push.md)]

##概述
本主题演示如何使用.NET 后端 Azure 移动服务到通用的 Windows 应用程序发送推式通知。 在本教程中，您启用推式通知通用的 Windows 应用程序项目中使用 Azure 通知集线器。 完成后，您的移动服务将发送推式通知从.NET 后端到所有已注册 Windows 应用商店和 Windows Phone 存储应用程序每次任务列表的表中插入记录。 创建通知中心是自由与您的移动服务，可以来管理独立于移动服务，并可由其他应用程序和服务。

>[AZURE.NOTE]本主题演示如何使用 Visual Studio 专业版 2013 更新 3 中刀具到通用的 Windows 应用程序从移动服务中添加对推式通知的支持。 可以使用相同的步骤添加到 Windows 应用商店或 Windows Phone 商店 8.1 app 推式通知移动服务。 若要添加 Windows Phone 8 或 Windows Phone Silverlight 8.1 app 推式通知，请参阅此版本[开始使用推式通知中移动服务](mobile-services-dotnet-backend-windows-phone-get-started-push.md)。

若要完成本教程，您需要以下各项︰

* 活动的[Microsoft 存储帐户](http://go.microsoft.com/fwlink/p/?LinkId=280045)。
* <a href="https://go.microsoft.com/fwLink/p/?LinkID=391934" target="_blank">Visual Studio 社区 2013年</a>。 

##<a id="register"></a>注册您的推送通知的应用程序

[AZURE.INCLUDE [mobile-services-create-new-push-vs2013](../../includes/mobile-services-create-new-push-vs2013.md)]

&nbsp;&nbsp;6.浏览到`\Services\MobileServices\your_service_name`项目文件夹，打开生成的 push.register.cs 文件，并检查注册通知集线器设备的通道 URL 的**UploadChannel**方法。
 
&nbsp;&nbsp;7.打开的共享的文件 App.xaml.cs，并注意到**OnLaunched**事件处理程序中添加了对新的**UploadChannel**方法的调用。 这样可确保每次启动应用程序时，尝试注册的设备。

&nbsp;&nbsp;8.重复以上步骤，将推式通知添加到 Windows Phone 存储应用程序项目，然后在共享的 App.xaml.cs 文件，删除多余调用**UploadChannel** ，剩余`#if...#endif`条件包装。 现在，这两个项目可以共享单个调用**UploadChannel**。

> [AZURE.NOTE] 此外可以通过统一简化生成的代码`#if...#endif`包装成单个解包定义这两个版本的应用程序所使用的[MobileServiceClient](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.aspx)定义。

现在，应用程序中启用推式通知，您必须更新移动服务发送推式通知。 

##<a id="update-service"></a>更新服务发送推式通知

以下步骤更新现有的 TodoItemController 类，以插入一个新项时，所有已注册的设备发送推式通知。 在任何自定义的[ApiController](https://msdn.microsoft.com/library/system.web.http.apicontroller.aspx)、 [TableController](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobile.service.tables.tablecontroller.aspx)，或任何后端服务中的其他位置，可以实现类似的代码。 

[AZURE.INCLUDE [mobile-services-dotnet-backend-update-server-push](../../includes/mobile-services-dotnet-backend-update-server-push.md)]

##<a id="local-testing"></a> 启用推式通知进行本地测试

[AZURE.INCLUDE [mobile-services-dotnet-backend-configure-local-push-vs2013](../../includes/mobile-services-dotnet-backend-configure-local-push-vs2013.md)]

本节中的其余步骤是可选的。 它们允许您测试您的应用程序对您的本地计算机上运行的移动服务。 如果您计划测试使用移动服务运行在 Azure 的推式通知，您可以只需跳到最后一节。 这是因为添加推式通知向导配置您的应用程序连接到您在 Azure 上运行的服务。 

>[AZURE.NOTE]不应使用生产移动服务进行测试和开发工作。 总是将您的移动服务项目发布到测试单独的临时服务。

&nbsp;&nbsp;5.打开共享的 App.xaml.cs 的项目文件，并找到任何创建访问移动服务在 Azure 上运行的[MobileServiceClient]类的一个新实例的代码行。

&nbsp;&nbsp;6.注释出此代码和添加创建具有相同名称的新[MobileServiceClient]代码，但使用的本地主机 URL，在构造函数中，与以下内容类似︰

    // This MobileServiceClient has been configured to communicate with your local
    // test project for debugging purposes.
    public static MobileServiceClient todolistClient = new MobileServiceClient(
        "http://localhost:4584"
    );

&nbsp;&nbsp;使用此[MobileServiceClient]，应用程序将连接到本地服务，而不是在 Azure 中承载的版本。 当您想要切换回来，并对移动服务承载 Azure 中运行的应用程序时，更改回原始的[MobileServiceClient]定义。

##<a id="test"></a> 测试您的应用程序中的推式通知

[AZURE.INCLUDE [mobile-services-dotnet-backend-windows-universal-test-push](../../includes/mobile-services-dotnet-backend-windows-universal-test-push.md)]

## <a name="next-steps"> </a>下一步行动

本教程说明了启用 Windows 应用商店应用程序可以使用移动服务和通知集线器发送推式通知的基础知识。 接下来，考虑完成下一步的教程中，[为发送推式通知经过身份验证的用户]，它显示了如何使用标签来从手机服务向只有已通过身份验证的用户发送推式通知。

以下主题中了解有关移动服务和通知集线器的更多信息︰

* [添加到现有应用程序的移动服务][有关数据入门]
  <br/>了解有关存储和查询数据使用移动服务。

* [添加到您的应用程序的身份验证][开始使用身份验证]
  <br/>了解如何使用不同的帐户类型使用移动服务对您的应用程序的用户进行身份验证。

* [什么是通知集线器？]
  <br/>了解更多有关通知集线器原理将通知传递到您的应用程序跨所有主要客户平台。

* [调试应用程序通知集线器](http://go.microsoft.com/fwlink/p/?linkid=386630)
  </br>获得故障诊断指南和通知集线器的解决方案进行调试。 

* [如何使用.NET 客户端 Azure 移动服务]
  <br/>了解更多关于如何使用移动服务从 C# Windows 应用程序。

<!-- Anchors. -->

<!-- Images. -->

<!-- URLs. -->
[提交应用程序页]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[我的应用程序]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[对于 Windows live SDK]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[开始使用移动服务]: mobile-services-dotnet-backend-windows-store-dotnet-get-started.md
[有关数据入门]: mobile-services-dotnet-backend-windows-universal-dotnet-get-started-data.md
[开始使用身份验证]: mobile-services-dotnet-backend-windows-universal-dotnet-get-started-users.md

[对经过身份验证的用户发送推式通知]: mobile-services-dotnet-backend-windows-store-dotnet-push-notifications-app-users.md

[什么是通知集线器？]: ../notification-hubs-overview.md

[如何使用.NET 客户端 Azure 移动服务]: mobile-services-windows-dotnet-how-to-use-client-library.md
[MobileServiceClient]: http://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.aspx
测试t

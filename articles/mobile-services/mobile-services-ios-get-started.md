---
ms.openlocfilehash: 361c0e960bced2fe93e8a71b7c87c8f243a4e5c0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="开始使用 Azure 的 iOS 应用程序的移动服务"
    description="按照本教程中若要开始使用 Azure 的 iOS 开发的移动服务。"
    services="mobile-services"
    documentationCenter="ios"
    authors="krisragh"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="hero-article"
    ms.date="07/28/2015"
    ms.author="krisragh"/>

# <a name="getting-started"> </a>开始使用移动服务

[AZURE.INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]

本教程展示了如何将基于云的后端服务添加到 iOS 应用程序使用 Azure 移动服务。

在本教程中，您将创建新的移动服务和一个简单_的待办事项列表_的应用程序将应用程序数据存储在新的移动服务。 手机信息服务，您将创建用于服务器端业务逻辑使用 JavaScript。 .NET 中的服务器端业务逻辑创建移动服务，请参见[.NET 后端版本]的主题。

> [AZURE.NOTE] 若要完成本教程，您需要一个 Azure 帐户。 如果您没有帐户，您可以注册 Azure 试验并获得[可用的移动服务，您可以继续使用您试用结束后甚至](http://azure.microsoft.com/pricing/details/mobile-services/)。 有关详细信息，请参阅[Azure 免费试用版](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AE564AB28&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started-ios%2F%20 target="_blank")。

## <a name="create-new-service"> </a>创建新的移动服务

[AZURE.INCLUDE [mobile-services-create-new-service](../../includes/mobile-services-create-new-service.md)]

## 创建新的 iOS 应用程序

您可以按照易于快速开始创建新的应用程序连接到您的移动服务管理门户中︰

1. 在管理门户中，单击**移动服务**，然后单击您刚刚创建的移动服务。

2. 在快速启动选项卡中，单击**选择一种平台**下的**iOS** ，展开**新的 iOS 应用程序的创建**。 这将显示创建 iOS 应用程序连接到您的移动服务的步骤。

3. 单击**创建 TodoItem 表**创建一个表来存储应用程序数据。

4. 在**下载并运行您的应用程序**，请单击**下载**。 这会将下载的示例_的待办事项列表_应用程序连接到您的移动服务，以及移动服务 iOS SDK 的项目。 压缩的项目文件保存到本地计算机，并记下的保存位置。

## 运行新的 iOS 应用程序

[AZURE.INCLUDE [mobile-services-ios-run-app](../../includes/mobile-services-ios-run-app.md)]

<ol start="4">
<li><p>回在管理门户中，单击**数据**选项卡，然后单击**TodoItem**表。 这允许您浏览该应用程序插入表的数据。<p></li></ol></p>

## <a name="next-steps"> </a>下一步行动
了解如何在移动服务中执行其他重要的任务︰

* [添加到现有应用程序的移动服务]
    <br/>了解有关存储和查询数据使用移动服务。

* [开始使用脱机数据同步]
    <br/>了解如何使用脱机数据同步，让您的应用程序响应迅速可靠。

* [添加到现有应用程序的身份验证]
    <br/>了解如何标识提供程序与应用程序的用户进行身份验证。

* [向现有应用程序添加推式通知]
    <br/>了解如何将非常基本的推式通知发送到您的应用程序。


<!-- Anchors. -->
[移动服务入门]:#getting-started
[创建新的移动服务]:#create-new-service
[定义移动服务实例]:#define-mobile-service-instance
[下一步行动]:#next-steps

<!-- Images. -->
[6]: ./media/mobile-services-ios-get-started/mobile-portal-quickstart-ios.png
[7]: ./media/mobile-services-ios-get-started/mobile-quickstart-steps-ios.png
[8]: ./media/mobile-services-ios-get-started/mobile-xcode-project.png

[10]: ./media/mobile-services-ios-get-started/mobile-quickstart-startup-ios.png
[11]: ./media/mobile-services-ios-get-started/mobile-data-tab.png
[12]: ./media/mobile-services-ios-get-started/mobile-data-browse.png


<!-- URLs. -->
[添加到现有应用程序的移动服务]: mobile-services-dotnet-backend-ios-get-started-data.md
[开始使用脱机数据同步]: mobile-services-ios-get-started-offline-data.md
[添加到现有应用程序的身份验证]: mobile-services-dotnet-backend-ios-get-started-users.md
[向现有应用程序添加推式通知]: mobile-services-dotnet-backend-ios-get-started-push.md


[移动服务 iOS SDK]: https://go.microsoft.com/fwLink/p/?LinkID=266533
[管理门户]: https://manage.windowsazure.com/
[XCode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[.NET 后端版本]: mobile-services-dotnet-backend-ios-get-started.md

测试

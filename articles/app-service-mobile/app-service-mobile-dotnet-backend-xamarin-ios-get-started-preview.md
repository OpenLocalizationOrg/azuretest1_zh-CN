---
ms.openlocfilehash: b93f0e9651212e56b1b53e070ccdb4f22012a446
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="Xamarin.iOS 应用程序的开始使用 Azure 应用程序服务移动应用程序 |Microsoft Azure"
    description="按照本教程中使用 Xamarin.iOS 开发的移动应用程序入门。"
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
    ms.topic="hero-article"
    ms.date="08/12/2015"
    ms.author="normesta"/>


#创建一个 Xamarin.iOS 应用程序

[AZURE.INCLUDE [app-service-mobile-selector-get-started-preview](../../includes/app-service-mobile-selector-get-started-preview.md)]
&nbsp;  
[AZURE.INCLUDE [app-service-mobile-note-mobile-services-preview](../../includes/app-service-mobile-note-mobile-services-preview.md)]

##概述

本教程展示如何使用 Azure 的移动应用程序的后端添加到 Xamarin.iOS 的移动应用程序的基于云的后端服务。  您将创建新的移动应用程序后端和一个简单的_Todo 列表_Xamarin.iOS 在 Azure 存储应用程序数据的应用程序。

学完本教程是在 Azure 应用程序服务使用移动应用程序功能有关的所有其他 Xamarin.iOS 教程的先决条件。

##先决条件

若要完成本教程，您需要以下各项︰

* 活动的 Azure 帐户。 如果您没有帐户，可以注册 Azure 的试验，并获得最多 10 个自由移动应用程序，您可以继续使用您试用结束后甚至。 有关详细信息，请参阅[Azure 免费试用版](http://azure.microsoft.com/pricing/free-trial/)。

* [Visual Studio 社区 2013] 或更高版本。  如果安装了 Visual Studio 社区 2013年，请单独安装[Xamarin] 。  在安装 Visual Studio 2015年时，您可以安装 Xamarin 工具。

* Mac [Xcode] 7.0 版或更高版本，并安装[Xamarin Studio] 。

     >[AZURE.NOTE]如果您计划使用 Visual Studio 在基于 Windows 的计算机上生成您的应用程序，您仍需要访问网络 Mac 呢。

>[AZURE.NOTE] 如果您想要开始使用 Azure 应用程序服务注册 Azure 帐户之前，请转到[尝试应用程序服务](http://go.microsoft.com/fwlink/?LinkId=523751&appServiceName=mobile)。 那里，您立即可以应用程序服务中创建短期的初学者移动应用程序 — 需要，没有信用卡，没有承诺。

## 创建新 Azure 的移动应用程序的后端

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service-preview](../../includes/app-service-mobile-dotnet-backend-create-new-service-preview.md)]

## 下载服务器项目

1. 在您的 PC 上访问[Azure 的门户]。 单击**浏览所有** > **移动应用程序**，然后单击您刚刚创建的移动应用程序后端。

2. 在移动应用程序刀片式服务器，单击**设置**，然后在**移动应用程序**，单击**快速启动** > **Xamarin.iOS**。

3. 在**下载并运行服务器项目**，单击**下载**。 压缩的项目将文件解压缩到您的电脑，并在 Visual Studio 中打开该解决方案。

## 测试本地端项目

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-test-local-service-preview](../../includes/app-service-mobile-dotnet-backend-test-local-service-preview.md)]

## 发布到 Azure 服务器项目

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-publish-service-preview](../../includes/app-service-mobile-dotnet-backend-publish-service-preview.md)]

## 下载并运行 Xamarin.iOS 的应用程序

1. 在您 Mac 浏览器窗口中打开[Azure 的门户]。

>[AZURE.NOTE] 更容易地运行您的 Xamarin.iOS 应用程序上的 mac。 您可以运行 Xamarin.iOS 应用程序通过使用 Visual Studio 在基于 Windows 的计算机上，如果您需要，但它有点更加复杂，因为您必须连接到网络的 mac。 如果您感兴趣进行该操作，请参阅[在 Windows 上安装 Xamarin.iOS]。

2. 在**下载并运行 Xamarin.iOS 项目**，请单击**下载**按钮。

    此下载包含的客户端应用程序连接到您的移动应用程序的项目。 压缩的项目文件保存到本地计算机，并记下其保存的位置。

3. 提取已下载，该项目，然后将其打开 Xamarin Studio （或 Visual Studio） 中。

    ![][9]

    ![][8]

4. 按 F5 键以生成项目，然后启动该应用程序在 iPhone 模拟器。

5. 在应用程序中，键入有意义的文本，如_学习 Xamarin_，，然后单击**+**按钮。

    ![][10]

    这将 POST 请求发送到 Azure 中承载新的移动应用程序后端。 从请求的数据插入到 TodoItem 表中。 通过移动应用程序的后端，返回项存储在表中，数据显示在列表中。

>[AZURE.NOTE]您可以查看访问您的移动应用程序后端查询并将数据插入 QSTodoService.cs C# 文件中的代码。

##下一步行动

* [添加到您的应用程序的身份验证 ](app-service-mobile-dotnet-backend-xamarin-ios-get-started-users-preview.md)
  <br/>了解如何使用标识提供程序的应用程序的用户进行身份验证。

* [将推式通知添加到您的应用程序](app-service-mobile-dotnet-backend-xamarin-ios-get-started-push-preview.md)
  <br/>了解如何将非常基本的推式通知发送到您的应用程序。

<!-- Anchors. -->
[要开始使用移动应用程序 backends]:#getting-started
[创建新的移动应用程序后端]:#create-new-service
[下一步行动]:#next-steps



<!-- Images. -->
[6]: ./media/app-service-mobile-dotnet-backend-xamarin-ios-get-started-preview/xamarin-ios-quickstart.png
[8]: ./media/app-service-mobile-dotnet-backend-xamarin-ios-get-started-preview/mobile-xamarin-project-ios-vs.png
[9]: ./media/app-service-mobile-dotnet-backend-xamarin-ios-get-started-preview/mobile-xamarin-project-ios-xs.png
[10]: ./media/app-service-mobile-dotnet-backend-xamarin-ios-get-started-preview/mobile-quickstart-startup-ios.png

<!-- URLs. -->
[开始使用脱机数据同步]: app-service-mobile-xamarin-ios-get-started-offline-data-preview.md
[开始使用身份验证]: ../app-service-mobile-dotnet-backend-xamarin-ios-get-started-preview-users.md
[开始使用推式通知]: ../app-service-mobile-dotnet-backend-xamarin-ios-get-started-preview-push.md
[Visual Studio 专业版 2013]: https://go.microsoft.com/fwLink/p/?LinkID=257546
[移动应用程序 SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[Azure 门户]: https://portal.azure.com/
[JavaScript 后端版本]: ../partner-xamarin-mobile-services-ios-get-started.md
[开始使用应用程序服务使用 Visual Studio 2012 中的数据]: ../app-service-mobile-windows-store-dotnet-get-started-data-vs2012-preview.md
[解决移动应用程序的.NET 后端]: ../app-service-mobile-dotnet-backend-how-to-troubleshoot-preview.md


[Xamarin Studio]: http://xamarin.com/download
[Xamarin]: http://xamarin.com/download
[Xcode]: https://go.microsoft.com/fwLink/?LinkID=266532
[对于 Windows Xamarin]: https://go.microsoft.com/fwLink/?LinkID=330242&clcid=0x409
[在 Windows 上安装 Xamarin.iOS]: http://developer.xamarin.com/guides/ios/getting_started/installation/windows/

测试

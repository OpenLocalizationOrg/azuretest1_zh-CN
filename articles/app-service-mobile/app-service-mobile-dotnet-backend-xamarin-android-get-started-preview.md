---
ms.openlocfilehash: 472147f7849dc7dbe69c022e1410d01d4f0655ab
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="开始使用 Azure 的 Xamarin.Android 应用程序的移动应用程序"
    description="请按照 Xamarin Android 开发使用 Azure 移动应用程序开始在本教程"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="wesmc7777"
    manager="dwrede"
    editor="" />

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="08/12/2015"
    ms.author="normesta" />

#创建一个 Xamarin.Android 应用程序

[AZURE.INCLUDE [app-service-mobile-selector-get-started-preview](../../includes/app-service-mobile-selector-get-started-preview.md)]
&nbsp;  
[AZURE.INCLUDE [app-service-mobile-note-mobile-services-preview](../../includes/app-service-mobile-note-mobile-services-preview.md)]
 
##概述

本教程展示如何使用 Azure 的移动应用程序的后端 Xamarin.Android 应用程序添加一个基于云的后端服务。  您将创建新的移动应用程序后端和一个简单的_Todo 列表_在 Azure 存储应用程序数据的 Xamarin.Andorid 应用程序。

下面是从已完成的应用程序的一个屏幕快照︰

![][0]

学完本教程是为 Xamarin.Android 应用程序的所有其他移动应用程序教程的先决条件。
 
##先决条件

若要完成本教程，您需要以下各项︰

* 活动的 Azure 帐户。 如果您没有帐户，可以注册 Azure 试用并获得最多 10 自由移动应用程序，您可以继续使用您试用结束后甚至。 有关详细信息，请参阅[Azure 免费试用版](http://azure.microsoft.com/pricing/free-trial/)。
 
* [Visual Studio 社区 2013年]或更高版本。  如果安装了 Visual Studio 社区 2013年，请单独安装[Xamarin] 。  在安装 Visual Studio 2015年时，您可以安装 Xamarin 工具。
 
>[AZURE.NOTE] 如果您想要怎样的 Azure 帐户之前开始使用 Azure 应用程序服务，请转到[尝试应用程序服务](http://go.microsoft.com/fwlink/?LinkId=523751&appServiceName=mobile)，立即可以在此创建短期的初学者移动应用程序在应用程序服务。 没有信用卡，所需;没有承诺。


## 创建新 Azure 的移动应用程序的后端

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service-preview](../../includes/app-service-mobile-dotnet-backend-create-new-service-preview.md)]

## 下载服务器项目

1. 在您的 PC 上访问[Azure 门户]。 单击**浏览所有** > **移动应用程序**，然后单击您刚刚创建的移动应用程序后端。

2. 在移动应用程序刀片式服务器，单击**设置**并单击**移动应用程序**下的**快速入门** > **Xamarin.Android**。
 
3. 在**下载并运行服务器项目**，单击**下载**。 压缩的项目将文件解压缩到您的电脑，并在 Visual Studio 中打开该解决方案。
 
## 测试本地端项目

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-test-local-service-preview](../../includes/app-service-mobile-dotnet-backend-test-local-service-preview.md)]

## 发布到 Azure 服务器项目

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-publish-service-preview](../../includes/app-service-mobile-dotnet-backend-publish-service-preview.md)]

## 下载并运行 Xamarin.Android 的应用程序

1. 在**下载并运行 Xamarin.Android 项目**，请单击**下载**按钮。

    此下载包含的客户端应用程序连接到您的移动应用程序的项目。 压缩的项目文件保存到本地计算机，并记下其保存的位置。

    ![][8]

    ![][9]

2. 按**F5**键以生成项目，然后启动应用程序。 

3. 在应用程序中，键入有意义的文本，例如_完成本教程_，然后单击**添加**按钮。

    ![][10]

    这将 POST 请求发送到 Azure 中承载新的移动应用程序后端。 从请求的数据插入到 TodoItem 表中。 在表中存储的项目由移动应用程序的后端，和出现在列表中的数据。

    > [AZURE.NOTE]
    > 您可以查看您的移动应用程序后端来查询、 插入 ToDoActivity.cs C# 文件中找到的数据访问的代码。

##下一步行动

* [添加到您的应用程序的身份验证 ](app-service-mobile-dotnet-backend-xamarin-android-get-started-users-preview.md)
  <br/>了解如何标识提供程序与应用程序的用户进行身份验证。


<!-- Images. -->
[0]: ./media/app-service-mobile-dotnet-backend-xamarin-android-get-started-preview/mobile-quickstart-completed-android.png
[6]: ./media/app-service-mobile-dotnet-backend-xamarin-android-get-started-preview/mobile-portal-quickstart-xamarin.png
[8]: ./media/app-service-mobile-dotnet-backend-xamarin-android-get-started-preview/mobile-xamarin-project-android-vs.png
[9]: ./media/app-service-mobile-dotnet-backend-xamarin-android-get-started-preview/mobile-xamarin-project-android-xs.png
[10]: ./media/app-service-mobile-dotnet-backend-xamarin-android-get-started-preview/mobile-quickstart-startup-android.png

<!-- URLs. -->
[Azure 门户]: https://azure.portal.com/
[Xamarin]: http://xamarin.com/download
[Xcode]: https://go.microsoft.com/fwLink/?LinkID=266532&clcid=0x409
[对于 Windows Xamarin]: https://go.microsoft.com/fwLink/?LinkID=330242&clcid=0x409
 
[Visual Studio 社区 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203

测试

---
ms.openlocfilehash: 05cfac3b755388442190b93ec086b24dc08e0efa
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="在 Azure 应用程序服务移动应用程序上创建 iOS 应用程序 |Microsoft Azure"
    description="按照本教程中使用 Azure 的移动应用程序 backends 为目标 C 或 Swift 的 iOS 开发入门"
    services="app-service\mobile"
    documentationCenter="ios"
    authors="krisragh"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="hero-article"
    ms.date="08/11/2015"
    ms.author="krisragh"/>

#创建 iOS 应用程序

[AZURE.INCLUDE [app-service-mobile-selector-get-started-preview](../../includes/app-service-mobile-selector-get-started-preview.md)]
&nbsp;  
[AZURE.INCLUDE [app-service-mobile-note-mobile-services-preview](../../includes/app-service-mobile-note-mobile-services-preview.md)]

##概述

本教程展示如何使用 Azure 的移动应用程序的后端添加到 iOS 移动应用程序的基于云的后端服务。  您将创建新的移动应用程序后端和一个简单的_Todo 列表_iOS 应用程序在 Azure 存储应用程序数据。

学完本教程是在 Azure 应用程序服务使用移动应用程序功能有关的所有其他 iOS 教程的先决条件。

##先决条件

若要完成本教程，您需要以下各项︰

* 活动的 Azure 帐户。 如果您没有帐户，可以注册 Azure 的试验，并获得最多 10 个自由移动应用程序，您可以继续使用您试用结束后甚至。 有关详细信息，请参阅[Azure 免费试用版](http://azure.microsoft.com/pricing/free-trial/)。

* [Visual Studio 社区 2013年]或更高版本。

* 使用 Xcode 7.0 版或更高版本的 Mac。

>[AZURE.NOTE] 如果您想要开始使用 Azure 应用程序服务注册 Azure 帐户之前，请转到[尝试应用程序服务](http://go.microsoft.com/fwlink/?LinkId=523751&appServiceName=mobile)。 那里，您立即可以应用程序服务中创建短期的初学者移动应用程序 — 需要，没有信用卡，没有承诺。

## 创建新 Azure 的移动应用程序的后端

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service-preview](../../includes/app-service-mobile-dotnet-backend-create-new-service-preview.md)]

## 下载服务器项目

1. 在您的 PC 上访问[Azure 的门户]。 单击**浏览所有** > **移动应用程序**，然后单击您刚刚创建的移动应用程序后端。

2. 在移动应用程序刀片式服务器，单击**设置**，然后在**移动应用程序**，单击**快速启动** > **Io (目标 C)**。 如果您愿意 Swift，单击**快速启动** > **Io (Swift)**相反。

3. 在**下载并运行服务器项目**，单击**下载**。 压缩的项目将文件解压缩到您的电脑，并在 Visual Studio 中打开该解决方案。

## 发布到 Azure 服务器项目

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-publish-service-preview](../../includes/app-service-mobile-dotnet-backend-publish-service-preview.md)]

## 下载并运行 iOS 应用程序

[AZURE.INCLUDE [app-service-mobile-ios-run-app-preview](../../includes/app-service-mobile-ios-run-app-preview.md)]


<!-- Images. -->
[0]: ./media/mobile-services-dotnet-backend-ios-get-started/mobile-quickstart-completed-ios.png
[1]: ./media/mobile-services-dotnet-backend-ios-get-started/mobile-quickstart-steps-vs.png

[6]: ./media/app-service-mobile-dotnet-backend-ios-get-started-preview/ios-quickstart.png

[7]: ./media/mobile-services-dotnet-backend-ios-get-started/mobile-quickstart-steps-ios.png
[8]: ./media/mobile-services-dotnet-backend-ios-get-started/mobile-xcode-project.png

[10]: ./media/mobile-services-dotnet-backend-ios-get-started/mobile-quickstart-startup-ios.png
[11]: ./media/mobile-services-dotnet-backend-ios-get-started/mobile-data-tab.png
[12]: ./media/mobile-services-dotnet-backend-ios-get-started/mobile-data-browse.png

[Azure 门户]: https://portal.azure.com/
[Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532

[Visual Studio 社区 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203

测试

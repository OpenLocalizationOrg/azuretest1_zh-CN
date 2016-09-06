---
ms.openlocfilehash: 7ed98dbedd34caec396e67608cb771931c9970eb
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

<properties 
    pageTitle="开始使用 Azure 的 Android 应用程序的移动服务" 
    description="按照本教程中若要开始使用 Azure 开发 Android 的移动服务。" 
    services="mobile-services" 
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
    ms.date="08/18/2015" 
    ms.author="ricksal"/>


# <a name="getting-started"> </a>开始使用移动服务

[AZURE.INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]

本教程展示如何使用 Azure 移动服务的 Android 应用程序添加一个基于云的后端服务。 在本教程中，您将创建新的移动服务和一个简单_的待办事项列表_的应用程序将应用程序数据存储在新的移动服务。 您将创建移动服务使用受支持的.NET 语言使用 Visual Studio 的服务器端业务逻辑和管理移动服务。 若要创建可以在 JavaScript 编写服务器端业务逻辑的移动服务，请参阅此主题的[JavaScript 后端版本](mobile-services-android-get-started.md)。

下面是从已完成的应用程序的一个屏幕快照︰

![][0]

学完本教程需要[Android 开发人员工具][Android Studio]，其中包括 Android Studio 集成的开发环境，以及最新的 Android 平台。 Android 的 4.2 或更高版本是必需的。  

下载快速入门项目包含的 Android 手机服务 SDK。 

> [AZURE.IMPORTANT] 若要完成本教程，您需要一个 Azure 帐户。 如果您没有帐户，可以注册 Azure 的试验，并获得最多 10 个免费的移动服务，您可以继续使用您试用结束后甚至。 有关详细信息，请参阅[Azure 免费试用版](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AE564AB28)。

<!-- -->

> [AZURE.NOTE] 如果您想要查看本教程的 Eclipse 版本，请转到︰[获得启动 (Eclipse)]。

## <a name="create-new-service"> </a>创建新的移动服务

[AZURE.INCLUDE [mobile-services-dotnet-backend-create-new-service](../../includes/mobile-services-dotnet-backend-create-new-service.md)]

## 下载到本地计算机上的移动服务

现在，您已经创建了移动服务，下载您的个性化的移动服务项目，您可以在您的本地计算机或虚拟机上运行。

1. 单击刚创建的然后在快速启动选项卡上，单击**选择平台**下**Android** ，展开**新的 Android 应用程序创建**的移动服务。

    ![][1]  

2. 如果您还没有这样做，下载并安装[Visual Studio 专业版 2013年](https://go.microsoft.com/fwLink/p/?LinkID=391934)或更高版本。

3. 在步骤 2 中，在**下载和发布到云服务**中单击**下载**。

    这会将下载的 Visual Studio 项目的实现移动服务。 压缩的项目文件保存到本地计算机，并记下的保存位置。

## 测试移动服务

[AZURE.INCLUDE [mobile-services-dotnet-backend-test-local-service](../../includes/mobile-services-dotnet-backend-test-local-service.md)]

## 发布您的移动服务

[AZURE.INCLUDE [mobile-services-dotnet-backend-publish-service](../../includes/mobile-services-dotnet-backend-publish-service.md)]

## 创建新的 Android 应用程序

在本节中，您将创建新的 Android 应用程序连接到您的移动服务。

1. 在[管理门户网站]中，单击**移动服务**，然后单击移动您刚刚创建的服务。

2. 在快速启动选项卡，单击**选择平台**下的**Android**并展开**创建新的 Android 应用程序**。 
 
    ![][2]  

3. 如果还没有这么做，下载并在您的本地计算机或虚拟机上安装[Android 开发人员工具][Android SDK] 。

4. 在**下载并运行您的应用程序**，请单击**下载**。 

    这会将下载的示例_的待办事项列表_应用程序连接到您的移动服务的项目。 压缩的项目文件保存到本地计算机，并记下其保存的位置。

## 运行 Android 的应用程序

[AZURE.INCLUDE [mobile-services-run-your-app](../../includes/mobile-services-android-get-started.md)]

## <a name="next-steps"> </a>下一步行动
现在，您已完成快速入门，学会如何在移动服务中执行其他重要的任务︰ 

* [开始使用身份验证]
  <br/>了解如何标识提供程序与应用程序的用户进行身份验证。

* [开始使用推式通知] 
  <br/>了解如何将非常基本的推式通知发送到您的应用程序。

* [解决移动服务.NET 后端]
  <br/> 了解如何诊断和解决与移动服务.NET 后端可能出现的问题。 

<!-- Anchors. -->
[移动服务入门]:#getting-started
[创建新的移动服务]:#create-new-service
[定义移动服务实例]:#define-mobile-service-instance
[下一步行动]:#next-steps

<!-- Images. -->
[0]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-quickstart-completed-android.png
[1]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-quickstart-steps-vs-AS.png
[2]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-quickstart-steps-android-AS.png


[6]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-portal-quickstart-android.png
[7]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-quickstart-steps-android.png
[8]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-eclipse-quickstart.png

[10]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-quickstart-startup-android.png
[11]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-data-tab.png
[12]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-data-browse.png

[14]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-services-import-android-workspace.png
[15]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-services-import-android-project.png

<!-- URLs. -->
[获取启动 (Eclipse)]: mobile-services-dotnet-backend-android-get-started-ec.md
[有关数据入门]: mobile-services-dotnet-backend-android-get-started-data.md
[开始使用身份验证]: mobile-services-dotnet-backend-android-get-started-users.md
[开始使用推式通知]: mobile-services-dotnet-backend-android-get-started-push.md
[Android SDK]: https://go.microsoft.com/fwLink/p/?LinkID=280125
[Android 的 Studio]: https://developer.android.com/sdk/index.html
[移动服务 Android SDK]: https://go.microsoft.com/fwLink/p/?LinkID=266533
[解决移动服务.NET 后端]: mobile-services-dotnet-backend-how-to-troubleshoot.md

[管理门户]: https://manage.windowsazure.com/
 

测试

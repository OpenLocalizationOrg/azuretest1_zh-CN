---
ms.openlocfilehash: d117aec8c469e3bb650b2ed2407598d2bb4f414c
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="开始使用移动服务用于 Xamarin 的 iOS 应用程序 |Microsoft Azure"
    description="按照本教程中若要开始使用 Azure Xamarin iOS 开发的移动服务"
    services="mobile-services"
    documentationCenter="xamarin"
    authors="lindydonna"
    manager="dwrede"
    editor="mollybos"/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/18/2015"
    ms.author="donnam"/>

# <a name="getting-started"> </a>开始使用移动服务

[AZURE.INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]

本教程展示如何使用 Azure 移动服务 Xamarin iOS 应用程序添加一个基于云的后端服务。 在本教程中，您将创建新的移动服务和一个简单_的待办事项列表_的应用程序将应用程序数据存储在新的移动服务。 您将创建移动服务使用受支持的.NET 语言使用 Visual Studio 的服务器端业务逻辑和管理移动服务。 若要创建可以在 JavaScript 编写服务器端业务逻辑的移动服务，请参阅此主题的[JavaScript 后端版本]。

>[AZURE.NOTE]本主题演示如何通过使用 Azure 管理门户创建一个新的移动服务项目。 通过使用 Visual Studio 2013年更新 2，还可以向现有的 Visual Studio 解决方案中添加新的移动服务项目。 有关详细信息，请参阅[快速入门︰ 添加移动服务 （.NET 后端）](http://msdn.microsoft.com/library/windows/apps/dn629482.aspx)

下面是从已完成的应用程序的一个屏幕快照︰

![][0]


学完本教程是为 Xamarin 的 iOS 应用程序的所有其他移动服务教程的先决条件。

>[AZURE.NOTE]若要完成本教程，您需要一个 Azure 帐户。 如果您没有帐户，可以注册 Azure 的试验，并获得最多 10 个免费的移动服务，您可以继续使用您试用结束后甚至。 有关详细信息，请参阅<a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fwww.windowsazure.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-services-dotnet-backend-xamarin-ios-get-started" target="_blank">Azure 免费试用版</a>。<br />本教程需要<a href="https://go.microsoft.com/fwLink/p/?LinkID=257546" target="_blank">Visual Studio 专业版 2013年</a>。 免费试用版是可用的。

## 创建新的移动服务

[AZURE.INCLUDE [mobile-services-dotnet-backend-create-new-service](../../includes/mobile-services-dotnet-backend-create-new-service.md)]

## 创建一个新的 Xamarin iOS 应用程序

一旦您已经创建了您的移动服务，则可以按照在管理门户中创建新的应用程序或修改现有应用程序连接到您的移动服务方便快速入门。

在本节中将下载新的 Xamarin iOS 应用程序和服务项目为您的移动服务。

1. 在管理门户中，单击**移动服务**，然后单击您刚刚创建的移动服务。

2. 在快速启动选项卡，单击**选择平台**下的**Xamarin**和展开**创建一个新的 Xamarin 应用程序**。

    ![][6]

    这将显示三个简单的步骤来创建一个 Xamarin iOS 应用程序连接到您的移动服务。

    ![][7]

3. 如果您还没有这样做，下载并安装<a href="https://go.microsoft.com/fwLink/p/?LinkID=257546" target="_blank">Visual Studio 专业版 2013年</a>在您的本地计算机或虚拟机上。  

4. 下载并安装[Xcode] v4.4 后或更高版本和[Xamarin Studio]。 您还可以使用 Visual Studio 为 Xamarin。

5. 在**下载并发布到云服务**，选择**iOS** ，然后单击**下载**。

    此下载解决方案包含项目对移动服务和示例_的待办事项列表_的应用程序连接到您的移动服务。 压缩的项目文件保存到本地计算机，并记下其保存的位置。

6. 下载您的发布配置文件，下载的文件保存到本地计算机，请记下其保存的位置。

## 测试移动服务

[AZURE.INCLUDE [mobile-services-dotnet-backend-test-local-service](../../includes/mobile-services-dotnet-backend-test-local-service.md)]

## 发布您的移动服务

[AZURE.INCLUDE [mobile-services-dotnet-backend-publish-service](../../includes/mobile-services-dotnet-backend-publish-service.md)]

## 运行 Xamarin 的 iOS 应用程序

本教程的最后阶段是生成并运行您的新应用程序。

1. 定位到移动服务解决方案，在 Visual Studio 或 Xamarin Studio 中的客户端项目。

    ![][8]

    ![][9]

2. 按**运行**按钮以生成客户端项目，然后 iPhone 仿真程序中启动该应用程序。

3. 在应用程序中，键入有意义的文本，例如，_完成本教程_，然后单击加号 (**+**) 图标。

    ![][10]

    将 POST 请求发送到 Azure 中承载新的移动服务。 从请求的数据插入到 TodoItem 表中。 在表中存储的项目由手机信息服务，以及数据显示在列表中。

>[AZURE.NOTE]您可以查看访问您的移动服务查询并将数据插入 QSTodoService.cs C# 文件中的代码。


## 下一步行动
现在，您已完成快速入门，学会如何在移动服务中执行其他重要的任务︰

* [开始使用脱机数据同步]
  <br/>了解如何快速入门使用脱机数据同步以使应用程序响应迅速、 可靠。

* [开始使用身份验证]
  <br/>了解如何标识提供程序与应用程序的用户进行身份验证。

* [开始使用推式通知]
  <br/>了解如何将非常基本的推式通知发送到您的应用程序。

* [解决移动服务.NET 后端]
  <br/> 了解如何诊断和解决与移动服务.NET 后端可能出现的问题。

<!-- Anchors. -->
[移动服务入门]:#getting-started
[创建新的移动服务]:#create-new-service
[下一步行动]:#next-steps



<!-- Images. -->
[0]: ./media/mobile-services-dotnet-backend-xamarin-ios-get-started/mobile-quickstart-completed-ios.png
[6]: ./media/mobile-services-dotnet-backend-xamarin-ios-get-started/mobile-portal-quickstart-xamarin-ios.png
[7]: ./media/mobile-services-dotnet-backend-xamarin-ios-get-started/mobile-quickstart-steps-xamarin-ios.png
[8]: ./media/mobile-services-dotnet-backend-xamarin-ios-get-started/mobile-xamarin-project-ios-vs.png
[9]: ./media/mobile-services-dotnet-backend-xamarin-ios-get-started/mobile-xamarin-project-ios-xs.png
[10]: ./media/mobile-services-dotnet-backend-xamarin-ios-get-started/mobile-quickstart-startup-ios.png

<!-- URLs. -->
[开始使用脱机数据同步]: mobile-services-xamarin-ios-get-started-offline-data.md
[开始使用身份验证]: mobile-services-dotnet-backend-xamarin-ios-get-started-users.md
[开始使用推式通知]: mobile-services-dotnet-backend-xamarin-ios-get-started-push.md
[Visual Studio 专业版 2013]: https://go.microsoft.com/fwLink/p/?LinkID=257546
[移动服务 SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[JavaScript 和 HTML]: mobile-services-win8-javascript/
[管理门户]: https://manage.windowsazure.com/
[JavaScript 后端版本]: mobile-services-ios-get-started.md
[开始使用 Visual Studio 2012 的移动服务中的数据]: ../mobile-services-windows-store-dotnet-get-started-data-vs2012.md
[解决移动服务.NET 后端]: mobile-services-dotnet-backend-how-to-troubleshoot.md


[Xamarin Studio]: http://xamarin.com/download
[Xcode]: https://go.microsoft.com/fwLink/?LinkID=266532&clcid=0x409
[对于 Windows Xamarin]: https://go.microsoft.com/fwLink/?LinkID=330242&clcid=0x409

测试

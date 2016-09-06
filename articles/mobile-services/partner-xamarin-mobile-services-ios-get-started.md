---
ms.openlocfilehash: 8353646d5f0ee4d7d512396b30b8a530a9537b72
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="开始使用 Xamarin iOS 应用程序的移动服务"
    description="按照本教程中若要开始使用 Azure Xamarin iOS 开发的移动服务。"
    services="mobile-services"
    documentationCenter="xamarin"
    authors="conceptdev"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="07/09/2015"
    ms.author="craig.dunn@xamarin.com"/>

# <a name="getting-started"> </a>开始使用移动服务

[AZURE.INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]

本教程展示如何使用 Azure 移动服务 Xamarin.iOS 应用程序添加一个基于云的后端服务。 在本教程中，您将创建新的移动服务和一个简单的<em>的待办事项列表</em>应用程序，将应用程序数据存储在新的移动服务。

如果您更喜欢观看视频，剪辑下面遵循本教程中的相同步骤。

视频:"获取启动与 Xamarin 和 Azure 移动服务"与 Craig Dunn，开发推广人员为 Xamarin (持续时间︰ 10:05 分)

> [AZURE.VIDEO getting-started-with-xamarin-and-mobile-services]



下面是从已完成的应用程序的一个屏幕快照︰

![][0]

学完本教程需要 XCode 和[Xamarin Studio] OS X 或插件 Xamarin Visual Studio 为 Windows 上的 Visual Studio。 该示例将在 iOS 5.0 及更高版本上运行。

> [AZURE.IMPORTANT] 若要完成本教程，您需要一个 Azure 帐户。 如果您没有帐户，可以注册 Azure 的试验，并获得最多 10 个免费的移动服务，您可以继续使用您试用结束后甚至。 有关详细信息，请参阅[Azure 免费试用版](http://azure.microsoft.com/pricing/free-trial/)。

## <a name="create-new-service"> </a>创建新的移动服务

[AZURE.INCLUDE [mobile-services-create-new-service](../../includes/mobile-services-create-new-service.md)]

## 创建一个新的 Xamarin.iOS 应用程序

一旦您已经创建了您的移动服务，则可以按照在管理门户中创建新的应用程序或修改现有应用程序连接到您的移动服务方便快速入门。

本部分中，您将创建一个新的 Xamarin.iOS 应用程序连接到您的移动服务。

1.  在管理门户中，单击**移动服务**，然后单击您刚刚创建的移动服务。

2. 在快速启动选项卡，单击**选择平台**下的**Xamarin.iOS**和展开**创建一个新的 Xamarin.iOS 应用程序**。

    ![][6]

    这将显示三个简单的步骤来创建 Xamarin.iOS 的应用程序连接到您的移动服务。

    ![][7]

3. 如果您还没有这样做，下载并安装 （我们建议使用最新版本，Xcode 6.0，或更高版本） 的 Xcode 和[Xamarin 工作室]。

4. 单击**创建 TodoItems 表**创建一个表来存储应用程序数据。

5. 在**下载和运行应用程序**，请单击**下载**。

    这会将下载的示例_的待办事项列表_应用程序连接到您的移动服务和引用的 Xamarin.iOS Azure 移动服务组件的项目。 压缩的项目文件保存到本地计算机，并记下的保存位置。

## 运行新的 Xamarin.iOS 应用程序

本教程的最后阶段是生成并运行您的新应用程序。

1. 浏览到保存压缩的项目文件的位置、 展开您计算机上的文件和打开使用 Xamarin Studio 或 Visual Studio **XamarinTodoQuickStart.iOS.sln**解决方案文件。

    ![][8]

    ![][9]

2. 按**运行**按钮以生成项目，然后启动应用程序在 iPhone 的仿真程序，这是此项目的默认值。

3. 在应用程序中，键入有意义的文本，例如，_完成本教程_，然后单击加号 (**+**) 图标。

    ![][10]

    将 POST 请求发送到 Azure 中承载新的移动服务。 从请求的数据插入到 TodoItem 表中。 在表中存储的项目由手机信息服务，以及数据显示在列表中。

    > [AZURE.NOTE] 您可以查看您的移动服务来查询、 插入 TodoService.cs C# 文件中找到的数据访问的代码。

4. 回在管理门户中，单击**数据**选项卡，然后单击**TodoItems**表。

    ![][11]

    这允许您浏览该应用程序插入表的数据。

    ![][12]


## 下一步行动
现在，您已完成快速入门，学会如何在移动服务中执行其他重要的任务︰

* [有关数据入门]
    <br/>了解如何将新表添加到一个移动的服务，然后读取和编写针对该表格。

* [开始使用脱机数据同步]
  <br/>了解如何快速入门使用脱机数据同步以使应用程序响应迅速、 可靠。

* [开始使用身份验证]
  <br/>了解如何标识提供程序与应用程序的用户进行身份验证。

* [开始使用推式通知]
  <br/>了解如何将非常基本的推式通知发送到您的应用程序。

<!-- Anchors. -->
[移动服务入门]:#getting-started
[创建新的移动服务]:#create-new-service
[定义移动服务实例]:#define-mobile-service-instance
[下一步行动]:#next-steps

<!-- Images. -->
[0]: ./media/partner-xamarin-mobile-services-ios-get-started/mobile-quickstart-completed-ios.png
[6]: ./media/partner-xamarin-mobile-services-ios-get-started/mobile-portal-quickstart-xamarin-ios.png
[7]: ./media/partner-xamarin-mobile-services-ios-get-started/mobile-quickstart-steps-xamarin-ios.png
[8]: ./media/partner-xamarin-mobile-services-ios-get-started/mobile-xamarin-project-ios-xs.png
[9]: ./media/partner-xamarin-mobile-services-ios-get-started/mobile-xamarin-project-ios-vs.png
[10]: ./media/partner-xamarin-mobile-services-ios-get-started/mobile-quickstart-startup-ios.png
[11]: ./media/partner-xamarin-mobile-services-ios-get-started/mobile-data-tab.png
[12]: ./media/partner-xamarin-mobile-services-ios-get-started/mobile-data-browse.png


<!-- URLs. -->
[有关数据入门]: partner-xamarin-mobile-services-ios-get-started-data.md
[开始使用脱机数据同步]: mobile-services-xamarin-ios-get-started-offline-data.md
[开始使用身份验证]: partner-xamarin-mobile-services-ios-get-started-users.md
[开始使用推式通知]: partner-xamarin-mobile-services-ios-get-started-push.md

[Xamarin Studio]: http://xamarin.com/download
[移动服务 iOS SDK]: https://go.microsoft.com/fwLink/p/?LinkID=266533

[管理门户]: https://manage.windowsazure.com/

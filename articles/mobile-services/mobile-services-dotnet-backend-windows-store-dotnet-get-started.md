---
ms.openlocfilehash: 33cbc74e725848e391df1bf9acb898493c6ec78a
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="开始使用移动服务为 Windows 通用应用程序 |Microsoft Azure"
    description="按照本教程开始在 C# 中使用 Azure 的通用的 Windows 应用程序开发的移动服务。"
    services="mobile-services"
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
    ms.date="08/08/2015"
    ms.author="glenga"/>


# <a name="getting-started"> </a>开始使用移动服务

[AZURE.INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]

本教程演示了如何将基于云的后端服务添加到通用 Windows 应用程序使用 Azure 移动服务。 Windows 应用程序的通用解决方案包括用于 Windows 商店 8.1 和 Windows Phone 商店 8.1 的应用程序的项目和公共的共享的项目。 有关详细信息，请参阅[该目标窗口和 Windows Phone 构建通用的 Windows 应用程序](http://msdn.microsoft.com/library/windows/apps/xaml/dn609832.aspx)。

在本教程中，您将创建新的移动服务和一个简单*的待办事项列表*的应用程序将应用程序数据存储在新的移动服务。 您将创建移动服务使用受支持的.NET 语言使用 Visual Studio 的服务器端业务逻辑和管理移动服务。 若要创建可以在 JavaScript 编写服务器端业务逻辑的移动服务，请参阅本主题的 JavaScript 后端版本。

>[AZURE.NOTE]本主题演示如何通过使用 Azure 管理门户创建一个新的移动服务项目和通用的 Windows 应用程序。 通过使用 Visual Studio 2013年更新 3，还可以向现有的 Visual Studio 解决方案中添加新的移动服务项目。 有关详细信息，请参阅[添加到现有应用程序的移动服务](mobile-services-dotnet-backend-windows-universal-dotnet-get-started-data.md)。

>若要添加到应用程序项目中 Windows Phone 8.0 或 Windows Phone Silverlight 8.1 移动服务，请参阅[添加到现有的 Windows Phone 应用程序的移动服务](mobile-services-dotnet-backend-windows-phone-get-started-data.md)。

[AZURE.INCLUDE [mobile-services-windows-universal-get-started](../../includes/mobile-services-windows-universal-get-started.md)]

若要完成本教程，您需要以下各项︰

* 活动的 Azure 帐户。 如果您没有帐户，可以注册 Azure 的试验，并获得最多 10 个免费的移动服务，您可以继续使用您试用结束后甚至。 有关详细信息，请参阅[Azure 免费试用版](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-services-dotnet-backend-windows-store-dotnet-get-started%2F)。
* [Visual Studio 2013年]。

## 创建新的移动服务

[AZURE.INCLUDE [mobile-services-dotnet-backend-create-new-service](../../includes/mobile-services-dotnet-backend-create-new-service.md)]

## 创建一个新的通用 Windows 应用程序

一旦您已经创建了您的移动服务，则可以按照在管理门户中创建新的应用程序或修改现有应用程序连接到您的移动服务方便快速入门。

在本节中，您将创建新的通用 Windows 应用程序连接到您的移动服务。

1. 在管理门户中，单击**移动服务**，然后单击您刚刚创建的移动服务。

2. 在快速启动选项卡，单击**窗口**下**选择平台**和展开**创建新的 Windows 应用商店应用程序**。

    这将显示三个简单的步骤来创建 Windows 应用商店应用程序连接到您的移动服务。

    ![移动服务快速入门步骤](./media/mobile-services-dotnet-backend-windows-store-dotnet-get-started/mobile-quickstart-steps.png)

3. 如果还没有这么做，下载并在您的本地计算机或虚拟机上安装[Visual Studio 2013年]。

4. 在**下载并运行您的应用程序和本地服务**，为您的 Windows 应用商店应用程序选择语言，然后单击**下载**。

    此下载解决方案包含项目对移动服务和示例_的待办事项列表_的应用程序连接到您的移动服务。 压缩的项目文件保存到本地计算机，并记下其保存的位置。

## 测试应用程序对本地移动服务

[AZURE.INCLUDE [mobile-services-dotnet-backend-test-local-service-dotnet](../../includes/mobile-services-dotnet-backend-test-local-service-dotnet.md)]

>[AZURE.NOTE]您可以查看您的移动服务来查询、 插入 MainPage.xaml.cs 文件中找到的数据访问的代码。


## 发布您的移动服务

[AZURE.INCLUDE [mobile-services-dotnet-backend-publish-service](../../includes/mobile-services-dotnet-backend-publish-service.md)]


<ol start="4">
<li><p>共享代码项目中打开 App.xaml.cs 文件，查找创建的代码<a href="http://msdn.microsoft.com/library/Windowsazure/microsoft.windowsazure.mobileservices.mobileserviceclient.aspx" target="_blank">MobileServiceClient</a>实例，创建使用此客户端的代码注释出<em>本地主机</em>和取消注释创建客户端使用的远程移动服务 URL，看上去像下面的代码︰</p>

        <pre><code>public static MobileServiceClient MobileService = new MobileServiceClient(
            "https://todolist.azure-mobile.net/",
            "XXXX-APPLICATION-KEY-XXXXX");</code></pre>

    <p>现在，客户端将访问移动服务发布到 Azure。</p></li>
</ol>

## 测试针对移动服务在 Azure 中承载的应用程序

既然来发布移动服务和客户端连接到 Azure 中承载远程移动服务，我们可以运行应用程序使用 Azure 存储项。

[AZURE.INCLUDE [mobile-services-windows-universal-test-app](../../includes/mobile-services-windows-universal-test-app.md)]


## 下一步行动
现在，您已完成快速入门，学会如何在移动服务中执行其他重要的任务︰

* [添加到现有应用程序的移动服务][有关数据入门]
  <br/>了解有关存储和查询数据使用移动服务。

* [开始使用脱机数据同步]
  <br/>了解如何使用脱机数据同步，让您的应用程序响应迅速可靠。

* [添加到您的移动服务应用程序的身份验证][开始使用身份验证]
  <br/>了解如何标识提供程序与应用程序的用户进行身份验证。

* [添加到您的应用程序的推式通知][开始使用推式通知]
  <br/>了解如何将非常基本的推式通知发送到您的应用程序。

* [解决移动服务.NET 后端]
  <br/> 了解如何诊断和解决与移动服务.NET 后端可能出现的问题。

有关通用的 Windows 应用程序的详细信息，请参阅[支持多个设备的平台，从单一的移动服务](mobile-services-how-to-use-multiple-clients-single-service.md#shared-vs)。

<!-- Anchors. -->

<!-- Images. -->



<!-- URLs. -->
[Visual Studio 2013 年]: https://go.microsoft.com/fwLink/p/?LinkID=257546
[有关数据入门]: mobile-services-dotnet-backend-windows-universal-dotnet-get-started-data.md
[有关数据入门]: ../mobile-services-dotnet-backend-windows-store-dotnet-get-started-data.md
[开始使用脱机数据同步]: mobile-services-windows-store-dotnet-get-started-offline-data.md
[开始使用身份验证]: ../mobile-services-dotnet-backend-windows-store-dotnet-get-started-users.md
[开始使用推式通知]: ../mobile-services-dotnet-backend-windows-store-dotnet-get-started-push.md
[Visual Studio 专业版 2013]: https://go.microsoft.com/fwLink/p/?LinkID=257546
[移动服务 SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[JavaScript 和 HTML]: mobile-services-win8-javascript/
[管理门户]: https://manage.windowsazure.com/
[JavaScript 后端版本]: ../mobile-services-windows-store-get-started.md
[开始使用 Visual Studio 2012 的移动服务中的数据]: ../mobile-services-windows-store-dotnet-get-started-data-vs2012.md
[解决移动服务.NET 后端]: mobile-services-dotnet-backend-how-to-troubleshoot.md
 
测试

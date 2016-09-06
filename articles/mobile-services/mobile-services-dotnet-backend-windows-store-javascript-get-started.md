---
ms.openlocfilehash: 58f0ccf39838970a877ad247c439be10632053a7
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="开始使用移动服务为 Windows 应用商店应用程序 |移动开发人员中心"
    description="按照本教程中使用 Azure 移动服务为 Windows 应用商店开发 C#、 VB 或 JavaScript 中的开始。"
    services="mobile-services"
    documentationCenter="windows"
    authors="ggailey777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="javascript"
    ms.topic="article" 
    ms.date="08/08/2015"
    ms.author="glenga"/>


# <a name="getting-started"> </a>开始使用移动服务

[AZURE.INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]

##概述
本教程演示了如何将基于云的后端服务添加到通用 Windows 应用程序使用 Azure 移动服务。 在本教程中，您将创建新的移动服务和一个简单*的待办事项列表*的应用程序用 HTML 和 JavaScript 将应用程序数据存储在新的移动服务。 您创建的移动服务使用受支持的.NET 语言使用 Visual Studio 的服务器端业务逻辑和管理移动服务。 若要创建可以在 JavaScript 编写服务器端业务逻辑的移动服务，请参阅本主题的 JavaScript 版本。

[AZURE.INCLUDE [mobile-services-windows-universal-get-started](../../includes/mobile-services-windows-universal-get-started.md)]

##先决条件

若要完成本教程，您需要以下各项︰

* 活动的 Azure 帐户。 如果您没有帐户，您可以在几分钟创建免费的试用帐户。 有关详细信息，请参阅[Azure 免费试用版](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-services-javascript-backend-windows-store-javascript-get-started%2F)。
* <a href="https://go.microsoft.com/fwLink/p/?LinkID=257546" target="_blank">Visual Studio 专业版 2013年</a>。 免费试用版是可用的。


## 创建新的移动服务

[AZURE.INCLUDE [mobile-services-dotnet-backend-create-new-service](../../includes/mobile-services-dotnet-backend-create-new-service.md)]

## 创建一个新的通用 Windows 应用程序

一旦您已经创建了您的移动服务，则可以按照在管理门户中创建新的应用程序或修改现有应用程序连接到您的移动服务方便快速入门。

在本节中，您将创建新的通用 Windows 应用程序连接到您的移动服务。

1. 在管理门户中，单击**移动服务**，然后单击您刚刚创建的移动服务。

2. 在快速启动选项卡，单击**窗口**下**选择平台**和展开**创建新的 Windows 应用商店应用程序**。

    ![][6]

    这将显示三个简单的步骤来创建 Windows 应用商店应用程序连接到您的移动服务。

    ![][7]

3. 如果您还没有这样做，下载并安装<a href="https://go.microsoft.com/fwLink/p/?LinkID=257546" target="_blank">Visual Studio 专业版 2013年</a>在您的本地计算机或虚拟机上。

4. 在**下载并运行您的应用程序和本地服务**，为您的 Windows 应用商店应用程序选择语言，然后单击**下载**。

    此下载解决方案包含项目对移动服务和示例_的待办事项列表_的应用程序连接到您的移动服务。 压缩的项目文件保存到本地计算机，并记下其保存的位置。


## 测试应用程序对本地移动服务

[AZURE.INCLUDE [mobile-services-dotnet-backend-test-local-service-dotnet](../../includes/mobile-services-dotnet-backend-test-local-service-dotnet.md)]

>[AZURE.NOTE]您可以查看您的移动服务来查询、 插入 default.js 文件中找到的数据访问的代码。

## 发布您的移动服务

[AZURE.INCLUDE [mobile-services-dotnet-backend-publish-service](../../includes/mobile-services-dotnet-backend-publish-service.md)]

&nbsp;&nbsp;4.共享代码项目中打开 default.js 文件创建一个[WindowsAzure.MobileServiceClient](http://msdn.microsoft.com/library/azure/jj554219.aspx)实例的代码、 注释出创建此使用*本地主机*的客户端的代码，请取消注释创建客户端使用的远程移动服务 URL，如下所示的代码︰

    var client = new WindowsAzure.MobileServiceClient(
        "https://todolist.azure-mobile.net/",
        "XXXXXX-APPLICATION-KEY-XXXXXX"
    );

&nbsp;&nbsp;现在，客户端将访问移动服务发布到 Azure。

&nbsp;&nbsp;5.请按**F5**键以重新生成该项目并启动应用程序。

&nbsp;&nbsp;6.在该应用程序中**插入 TodoItem**，请键入有意义的文本，例如，*完成本教程*，，然后单击**保存**。

&nbsp;&nbsp;将 POST 请求发送到 Azure 中承载新的移动服务。

&nbsp;&nbsp;7.（可选） 在通用的 Windows 解决方案，到其他应用程序中更改默认启动项目和再次按**F5** ，并注意到上一步中保存的数据将从移动服务加载，应用程序启动后。

有关通用的 Windows 应用程序的详细信息，请参阅[支持多个设备的平台，从单一的移动服务](mobile-services-how-to-use-multiple-clients-single-service.md#shared-vs)。

<!-- Anchors. -->
[移动服务入门]:#getting-started
[创建新的移动服务]:#create-new-service
[定义移动服务实例]:#define-mobile-service-instance
[下一步行动]:#next-steps

<!-- Images. -->
[0]: ./media/mobile-services-dotnet-backend-windows-store-javascript-get-started/mobile-quickstart-completed.png

[6]: ./media/mobile-services-dotnet-backend-windows-store-javascript-get-started/mobile-portal-quickstart.png
[7]: ./media/mobile-services-dotnet-backend-windows-store-javascript-get-started/mobile-quickstart-steps.png
[8]: ./media/mobile-services-dotnet-backend-windows-store-javascript-get-started/mobile-service-startup.png

[10]: ./media/mobile-services-dotnet-backend-windows-store-javascript-get-started/mobile-quickstart-startup.png
[11]: ./media/mobile-services-dotnet-backend-windows-store-javascript-get-started/mobile-quickstart-publish.png
[12]: ./media/mobile-services-dotnet-backend-windows-store-javascript-get-started/mobile-data-browse.png


<!-- URLs. -->
[有关数据入门]: ../mobile-services-dotnet-backend-windows-universal-javascript-get-started-data.md
[开始使用身份验证]: ../mobile-services-dotnet-backend-windows-store-javascript-get-started-users.md
[开始使用推式通知]: ../mobile-services-dotnet-backend-windows-store-javascript-get-started-push.md
[Visual Studio 专业版 2013]: https://go.microsoft.com/fwLink/p/?LinkID=257546
[移动服务 SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[JavaScript 和 HTML]: mobile-services-win8-javascript/
[管理门户]: https://manage.windowsazure.com/
[JavaScript 版本]: ../mobile-services-windows-store-get-started.md
[开始使用 Visual Studio 2012 的移动服务中的数据]: ../mobile-services-windows-store-dotnet-get-started-data-vs2012.md
 
测试

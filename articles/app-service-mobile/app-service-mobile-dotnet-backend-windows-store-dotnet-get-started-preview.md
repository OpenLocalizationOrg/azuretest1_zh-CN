---
ms.openlocfilehash: 5a7b275a6fc3cc6c83584efe41f75c0b87dc625c
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="在 Azure 应用程序服务移动应用程序上创建一个 Windows 运行时 8.1 通用应用程序 |Microsoft Azure"
    description="按照本教程中使用 Azure 的移动应用程序 backends C#、 Visual Basic 或 JavaScript 中的 Windows 应用商店开发入门。"
    services="app-service\mobile"
    documentationCenter="windows"
    authors="ggailey777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="08/14/2015"
    ms.author="glenga"/>

#创建一个 Windows 应用程序

[AZURE.INCLUDE [app-service-mobile-selector-get-started-preview](../../includes/app-service-mobile-selector-get-started-preview.md)]
&nbsp;
[AZURE.INCLUDE [app-service-mobile-note-mobile-services-preview](../../includes/app-service-mobile-note-mobile-services-preview.md)]

##概述

本教程展示如何使用 Azure 的移动应用程序的后端添加到 Windows 运行时 8.1 通用应用程序的基于云的后端服务。 Windows 应用程序的通用解决方案包括用于 Windows 商店 8.1 和 Windows Phone 商店 8.1 的应用程序，除了常见的共享项目的项目。

[AZURE.INCLUDE [app-service-mobile-windows-universal-get-started-preview](../../includes/app-service-mobile-windows-universal-get-started-preview.md)]

##先决条件

若要完成本教程，您需要以下各项︰

* 活动的 Azure 帐户。 如果您没有帐户，可以注册 Azure 的试验，并获得最多 10 个自由移动应用程序，您可以继续使用您试用结束后甚至。 有关详细信息，请参阅[Azure 免费试用版](http://azure.microsoft.com/pricing/free-trial/)。

* [Visual Studio 社区 2013年]或更高版本。

>[AZURE.NOTE] 如果您想要开始使用 Azure 应用程序服务注册 Azure 帐户之前，请转到[尝试应用程序服务](http://go.microsoft.com/fwlink/?LinkId=523751&appServiceName=mobile)。 那里，您立即可以应用程序服务中创建短期的初学者移动应用程序 — 需要，没有信用卡，没有承诺。

##创建新 Azure 的移动应用程序的后端

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service-preview](../../includes/app-service-mobile-dotnet-backend-create-new-service-preview.md)]

## 下载服务器项目

1. 在[Azure 的门户网站]中，请单击**浏览所有** > **Web 应用程序**，然后单击您刚刚创建的移动应用程序后端。

2. 在移动应用程序的后端，单击**所有设置**，然后在**移动应用程序**，单击**快速启动** > **窗口 (C#)**。

3. 在**下载并运行服务器项目**中**创建新的应用程序**，单击**下载**，压缩的项目将文件解压缩到您的本地计算机，在 Visual Studio 中打开该解决方案。

4. 生成项目以恢复 NuGet 程序包。

##发布到 Azure 服务器项目

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-publish-service-preview](../../includes/app-service-mobile-dotnet-backend-publish-service-preview.md)]

##下载并运行客户端项目

一旦您创建了移动应用程序端，您可以按照简单快速入门 Azure 门户中创建新的应用程序或修改现有应用程序连接到您的移动应用程序后端。

在本节中，您将下载自定义连接到 Azure 的移动应用程序端的通用 Windows 应用程序模板项目。

1. 重新在您的移动应用程序后端的刀片，单击**所有设置**，然后在**移动应用程序**，单击**快速启动** > **窗口 (C#)**。

2.  在**下载和运行 Windows 项目**中**创建新的应用程序**，请单击**下载**，和压缩的项目将文件解压缩到您的本地计算机。

3. （可选）将通用的 Windows 应用程序项目添加到服务器项目的解决方案。 这使得它容易调试和测试应用程序和后端在相同的 Visual Studio 解决方案中，如果您选择这样做。

4. Windows 应用商店应用程序作为启动项目，然后按 F5 键以重新生成该项目并启动 Windows 应用商店应用程序。

5. 在应用程序中，**将 TodoItem 插入**的文本框中键入有意义的文本，例如，*完成本教程*，，然后单击**保存**。

    ![](./media/app-service-mobile-dotnet-backend-windows-store-dotnet-get-started-preview/mobile-quickstart-startup.png)

    这将 POST 请求发送到位于 Azure 中的新的移动应用程序后端。

6. 停止调试，请用鼠标右键单击`<your app name>.WindowsPhone`项目，单击**设为启动项目**，然后再次按 F5。

    ![](./media/app-service-mobile-dotnet-backend-windows-store-dotnet-get-started-preview/mobile-quickstart-completed-wp8.png)

    注意到 Windows 应用程序启动后，从移动应用程序加载上一步中保存的数据。

##下一步行动

* [添加到您的应用程序的身份验证 ](app-service-mobile-dotnet-backend-windows-store-dotnet-get-started-users-preview.md)
  <br/>了解如何标识提供程序与应用程序的用户进行身份验证。

* [将推式通知添加到您的应用程序](app-service-mobile-dotnet-backend-windows-store-dotnet-get-started-push-preview.md)
  <br/>了解如何将非常基本的推式通知发送到您的应用程序。

<!-- Anchors. -->
<!-- Images. -->
<!-- URLs. -->
[开始使用身份验证]: app-service-mobile-dotnet-backend-windows-store-dotnet-get-started-users-preview.md
[移动应用程序 SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[Azure 门户]: https://portal.azure.com/
[Visual Studio 社区 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203

测试

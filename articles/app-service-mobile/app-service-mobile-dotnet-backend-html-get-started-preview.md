---
ms.openlocfilehash: 7fff563af44b36ca6c5e2f8ef153bd312c41731b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="开始使用 HTML/JavaScript 应用程序的移动应用程序 backends |Azure 应用程序服务移动应用程序"
    description="按照本教程中使用 Azure 的移动应用程序 backends HTML5 和 JavaScript 中的 web 应用程序开发的入门。"
    services="app-service\mobile"
    documentationCenter=""
    authors="ggailey777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-html5"
    ms.devlang="javascript"
    ms.topic="get-started-article"
    ms.date="08/11/2015"
    ms.author="glenga"/>


#创建一个 HTML 应用程序

[AZURE.INCLUDE [app-service-mobile-selector-get-started-preview](../../includes/app-service-mobile-selector-get-started-preview.md)]
&nbsp;  
[AZURE.INCLUDE [app-service-mobile-note-mobile-services-preview](../../includes/app-service-mobile-note-mobile-services-preview.md)]

##概述

本教程展示了如何将基于云的后端服务添加到 HTML5/JavaScript web 应用程序使用 Azure 的移动应用程序的后端。 您将创建新的移动应用程序后端和一个简单*的待办事项列表*web 应用程序在 Azure 存储应用程序数据。

下面是从已完成的应用程序的一个屏幕快照︰

![已完成应用程序的屏幕抓图](./media/app-service-mobile-dotnet-backend-html-get-started-preview/mobile-quickstart-completed-html.png)

学完本教程是为 HTML 应用程序的所有其他移动应用程序教程的先决条件。 

##先决条件

若要完成本教程，您需要以下各项︰

* 活动的 Azure 帐户。 如果您没有帐户，可以注册 Azure 试用并获得最多 10 自由移动应用程序，您可以继续使用您试用结束后甚至。 有关详细信息，请参阅[Azure 免费试用版](http://azure.microsoft.com/pricing/free-trial/)。

* [Visual Studio 社区 2013年]或更高版本。

>[AZURE.NOTE] 如果您想要怎样的 Azure 帐户之前开始使用 Azure 应用程序服务，请转到[尝试应用程序服务](http://go.microsoft.com/fwlink/?LinkId=523751&appServiceName=mobile)，立即可以在此创建短期的初学者移动应用程序在应用程序服务。 没有信用卡，所需;没有承诺。

##创建新的移动应用程序后端

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service-preview](../../includes/app-service-mobile-dotnet-backend-create-new-service-preview.md)]

## 下载服务器项目

1. 在[Azure 门户网站]中，请单击**浏览所有** > **Web 应用程序**，然后单击您刚刚创建的移动应用程序后端。 

2. 在移动应用程序的后端，单击**所有设置**并都单击**移动应用程序**下的**快速入门** > **HTML/JavaScript**。

3. 在**下载并运行服务器项目**中**创建新的应用程序**，单击**下载**，压缩的项目将文件解压缩到您的本地计算机，在 Visual Studio 中打开该解决方案。

4. 生成项目以恢复 NuGet 程序包。

##在服务器项目中启用 CORS

跨来源的资源共享 (CORS) 是基于 web 的应用程序若要指示从哪个域请求是安全的一种方式，应允许浏览器。 您必须添加您移动应用程序的后端访问每个网站的 CORS 条目。 您可以通过使用标准的 ASP.NET Web API 行为控制 CORS 设置。 有关详细信息，请参见[ASP.NET Web API 中启用跨原始请求](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api#enable-cors)。

默认情况下，您将从该门户下载的客户端快速入门项目上运行本地主机端口 8000。 因此下, 一步将使 CORS 的`http://localhost:8000`服务器项目中。  

1. 在 Visual Studio 中的工具菜单中，单击**NuGet 程序包管理器** > **程序包管理器控制台**，选择 Nuget.org 作为**程序包源**，并在控制台窗口中执行以下命令︰
 
        Install-Package Microsoft.AspNet.WebApi.Cors  

    这将安装所需的后端的 CORS 支持。 

2. 打开 App_Start/WebApiConfig.cs 项目文件，然后添加以下 using 语句︰

        using System.Web.Http.Cors;

3. 接下来，添加下面的代码**WebApiConfig.Register**方法创建**HttpConfiguration**后︰

        // Enable CORS support for localhost port 8000, all headers and methods.
        var cors = new EnableCorsAttribute("http://localhost:8000", "*", "*");
        config.EnableCors(cors);

4. 保存您的更新。

接下来，您将启用了 CORS 的将项目部署到 Azure。

##发布到 Azure 服务器项目

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-publish-service-preview](../../includes/app-service-mobile-dotnet-backend-publish-service-preview.md)]

##下载并运行客户端项目

1. 回在您移动应用程序的后端的刀片式服务器，请单击**所有设置**并都单击**移动应用程序**下的**快速入门** > **HTML/JavaScript**。 

2.  在**下载和运行 HTML/Javascript 项目****创建新的应用程序**中，单击**下载**并将压缩的项目文件保存到本地计算机。

3. 浏览到保存压缩的项目文件的位置、 展开您计算机上的文件和启动**服务器**子文件夹中的以下命令文件之一。

    + **启动 windows**（Windows 计算机）
    + **发布 mac.command** （Mac OS X 计算机）
    + **发布 linux.sh** （Linux 计算机）

    > [AZURE.NOTE] 在 Windows 计算机上，键入`R`PowerShell 当要求您确认您想要运行的脚本。 Web 浏览器可能会警告您不运行脚本，因为它从互联网下载。 这种情况下，您必须请求浏览器继续加载该脚本。

    这将启动 web 服务器上本地计算机新的应用程序的宿主。

4. 打开 URL <a href="http://localhost:8000/" target="_blank">http://localhost:8000 /</a> web 浏览器中启动该应用程序。 客户端应用程序被配置为连接到您的移动应用程序端 Azure 中。

5. 在应用程序中，在**输入新任务**中，键入有意义的文本，例如，_完成本教程_，，然后单击**添加**。

    ![运行应用程序](./media/app-service-mobile-dotnet-backend-html-get-started-preview/mobile-quickstart-startup-html.png)

    这将 POST 请求发送到 Azure 中承载新的移动应用程序后端。 从请求的数据移动应用程序架构中 TodoItem 表中插入。 在表中存储的项目由服务，以及数据显示在应用程序中的第二列中。

    > [AZURE.TIP] 您可以查看您的移动服务来查询、 插入 app.js 文件中找到的数据访问的代码。 

<!-- Anchors. -->
<!-- Images. -->
<!-- URLs. -->
[开始使用身份验证]: app-service-mobile-dotnet-backend-windows-store-dotnet-get-started-users-preview.md
[移动应用程序 SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[Azure 门户]: https://portal.azure.com/

[Visual Studio 社区 2013]: https://www.visualstudio.com/downloads
 

测试

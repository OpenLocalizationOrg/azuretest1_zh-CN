---
ms.openlocfilehash: 9348743138ad0656f4e5a2d09d8f0802dfeea877
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="通过在 Visual Studio 中使用连接服务添加移动服务 |Microsoft Azure"
   description="通过使用 Visual Studio 中添加连接服务对话框中添加移动服务"
   services="visual-studio-online"
   documentationCenter="na"
   authors="patshea123"
   manager="douge"
   editor="tlee" />
<tags 
   ms.service="visual-studio-online"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="mobile"
   ms.date="08/12/2015"
   ms.author="patshea" />

# 通过使用 Visual Studio 连接服务添加移动服务

与 Visual Studio 2015，您可以连接到 Azure 移动服务使用对话框中**添加连接服务**。 您可以从任何 C# 客户端应用程序、 任何 JavaScript 应用程序中或跨平台 Cordova 应用程序进行连接。 连接后，可以创建和访问数据、 创建自定义的 Api 和计划的作业，或添加对推式通知的支持。  所有适当的引用和连接的代码将添加连接的服务操作。 您还可以利用内置的支持，以便进行身份验证各种流行的标识方案，Azure 的广告，如 Facebook、 Twitter，以及 Microsoft 帐户。

## 支持的项目类型

>[AZURE.NOTE] 在 Visual Studio 2015，将 Azure 移动服务添加到 Windows 通用 (Windows 10) 项目，通过使用对话框中添加连接服务不支持。 您可以通过安装为您的项目使用 NuGet 程序包管理器的相应软件包添加 Azure 移动服务。

可以使用连接服务对话框中的下列项目类型连接到 Azure 移动服务。

- 8.1.NET Windows 应用商店、 电话和通用应用程序项目

- 8.1 JavaScript Windows 应用商店、 电话和通用应用程序项目

- 对于 Apache Cordova 使用 Visual Studio 工具创建的项目


## 连接到 Azure 移动服务使用对话框中添加连接服务

1. 请确保您有一个 Azure 帐户。 如果您没有 Azure 帐户，您可以注册[免费试用版](http://go.microsoft.com/fwlink/?LinkId=518146)。

1. 打开**添加连接服务**对话框。
 - 用于.NET 的应用程序，在 Visual Studio 中打开您的项目，在解决方案资源管理器中打开上下文菜单中的**引用**节点，然后选择**添加连接服务**
 
        ![Connecting to Azure Mobile Service](./media/vs-azure-tools-connected-services-add-mobile-services/IC797635.png)

 - 对于 Apache Cordova 应用程序项目，在 Visual Studio 中打开您的项目，在解决方案资源管理器中打开该项目节点的上下文菜单，然后选择**添加连接服务**。

1. 在**添加连接服务**对话框中，选择**Azure 移动服务**，然后选择**配置**按钮。 系统可能会提示您登录到 Azure，如果还没有这么做。

    ![添加 Azure 的移动服务](./media/vs-azure-tools-connected-services-add-mobile-services/IC797636.png)

1. 在**Azure 移动服务**对话框中，选择现有的移动服务，如果有的话。 如果您需要创建一个新的 Azure 移动服务，请按照下列过程，这样做。 否则，请跳到下一步。

    若要创建一个新的手机信息服务帐户︰
    1. 选择底部的对话框中**创建服务**链接。
        ![添加新连接的移动服务](./media/vs-azure-tools-connected-services-add-mobile-services/IC797637.png)




    2. 在**创建移动服务**对话框中，可以选择.NET 后端移动服务或 JavaScript 后端移动服务，从**运行时**下拉列表。 
  
        ![创建移动服务](./media/vs-azure-tools-connected-services-add-mobile-services/IC797638.png)

        JavaScript 后端服务是简单且功能强大。 如果您创建一个 JavaScript 后端移动服务，服务器端 JavaScript 代码存储在云中，但您可以通过使用服务器资源管理器或 Azure 管理门户网站编辑服务器脚本。 

        .NET 后端移动服务为您提供完全的权力和 Web API 和实体框架的灵活性。 创建.NET 后端移动服务时，如果项目是为您创建并添加到解决方案中。 

    1. 选择要移动的服务，该**地区**，然后输入服务器的用户名和密码。
 
    1. 在输入了所有必需的信息后，选择**创建**按钮创建移动服务。
    2. 新的移动服务应出现在**Azure 移动服务**对话框中的服务列表中。 在列表中选择新的移动服务，然后选择**添加**按钮添加到您的项目的服务。
    

1. 审查获得入门页面显示并找出如何修改您的项目。 无论何时您添加连接的服务，入门页面出现在浏览器中。 您可以查看建议的下一个步骤和代码示例，或切换到怎么页后，可以看到哪些引用已添加到项目中，以及如何修改您的代码和配置文件。

1. 使用代码示例作为参考，开始编写代码以访问您的移动服务 ！

## 如何修改项目

Visual Studio 如何修改项目取决于项目类型。 C# 客户端应用程序，请参阅[怎么 – C# 项目](http://go.microsoft.com/fwlink/p/?LinkId=513119)。 JavaScript 的客户端应用程序，请参阅[怎么 – JavaScript 项目](http://go.microsoft.com/fwlink/p/?LinkId=513120)。 Cordova 应用程序，请参阅[怎么 – Cordova 项目](http://go.microsoft.com/fwlink/p/?LinkId=513116)。


##下一步行动

提出问题并获取帮助︰ 

 - [MSDN 论坛︰ Azure 的移动服务](https://social.msdn.microsoft.com/forums/azure/home?forum=azuremobile)

 - [在 Microsoft Azure 团队博客的 azure 移动服务](http://azure.microsoft.com/blog/topics/mobile/)

 - [在 azure.microsoft.com azure 移动服务](http://azure.microsoft.com/services/mobile-services/)

 - [Azure 的移动服务文档，网址 azure.microsoft.com](http://azure.microsoft.com/documentation/services/mobile-services/)




测试

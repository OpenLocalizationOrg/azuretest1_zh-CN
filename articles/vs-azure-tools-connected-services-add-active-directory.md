---
ms.openlocfilehash: eca8c775e43f997ef49da15b31a6f5ebe5d65903
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="通过在 Visual Studio 中使用连接服务添加 Azure Active Directory |Microsoft Azure"
   description="通过使用 Visual Studio 中添加连接服务对话框中添加 Azure 活动目录"
   services="visual-studio-online"
   documentationCenter="na"
   authors="patshea123"
   manager="douge"
   editor="tlee" />
<tags 
   ms.service="visual-studio-online""
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="visual-studio-online"
   ms.date="08/12/2015"
   ms.author="patshea" />

# 通过在 Visual Studio 中使用连接服务添加 Azure 活动目录 

##概述
通过使用 Azure 活动目录 (AD Azure)，您可以为 ASP.NET MVC web 应用程序或在 Web API 服务 AD 身份验证支持单一登录 (SSO)。 使用 Azure AD 身份验证，您的用户可以使用从 Azure 的广告其帐户连接到您的 web 应用程序。 公开从 web 应用程序的 API 时，Azure AD 身份验证与 Web API 的优点包括增强的数据的安全性。 使用 Azure 的广告，不需要管理自己的帐户和用户管理的单独的身份验证系统。

## 支持的项目类型

可以使用连接服务对话框以连接到 Azure AD 中以下项目类型。

- ASP.NET MVC 项目

- ASP.NET Web API 项目


### 连接到 Azure 的广告使用服务连接对话框

1. 请确保您有一个 Azure 帐户。 如果您没有 Azure 帐户，您可以注册[免费试用版](http://go.microsoft.com/fwlink/?LinkId=518146)。

1. 在 Visual Studio 中，打开您项目中的快捷菜单上的**引用**节点并选择**添加连接服务**。
1. 选择**Azure AD 身份验证**，然后选择**配置**。

    ![选择添加 Azure AD 身份验证](./media/vs-azure-tools-connected-services-add-active-directory/connected-services-add-active-directory.png)

1. **配置 Azure AD 身份验证**的第一页，请检查**配置单一登录使用 Azure 的广告**。

    如果您的项目配置了其他身份验证配置，向导将警告您继续操作将禁用以前的配置。

    ![在该向导中配置 Azure 的广告](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-1.png)

1.  在第二页上，从**域**下拉列表中选择一个域。 域列表包含在帐户设置对话框中列出的帐户可以访问的所有域。 或者，如果您没有找到您正在寻找的如 mydomain.onmicrosoft.com 的一个可以输入一个域名。 您可以选择创建一个新的选项 Azure 广告应用程序，或使用从现有的 AD Azure 应用程序的设置。 

    ![在该向导中配置 Azure 的广告](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-2.png)


1. 在向导的第三页上，请确保选中**读取目录数据**。 该向导将填写**客户机密**。 

    ![在该向导中配置 Azure 的广告](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-3.png)

1. 选择**完成**按钮。 对话框中添加必要的配置代码和启用 Azure AD 身份验证为您的项目的引用。 在 Azure 的门户网站上，可以看到 AD 域。

1. 查看开始页面出现在浏览器中的下一步行动，在创意和怎么页后，可以看到如何修改您的项目。 如果您想要检查一切运行，打开的一个修改后的配置文件并验证出了什么问题中提到的设置都有。 例如，主 web.config ASP.NET MVC 项目中的会添加这些设置︰

        <appSettings> 
            <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
            <add key="ida:AADInstance" value="https://login.windows.net/" />
            <add key="ida:Domain" value="Your selected domain" />
            <add key="ida:TenantId" value="The Id of your selected Azure AD Tenant" />
            <add key="ida:PostLogoutRedirectUri" value="The default redirect URI from the project" />
        </appSettings>

## 如何修改项目

运行该向导时，Visual Studio 将添加 Azure 的广告，并关联到项目的引用。 配置文件和代码文件，您的项目中还修改以添加对 Azure 广告的支持。 Visual Studio 可使特定的修改取决于该项目类型。 有关如何修改 ASP.NET MVC 项目的详细信息，请参阅[什么发生了变化 — MVC 项目](http://go.microsoft.com/fwlink/p/?LinkID=513809)。 对于 Web API 项目，看到[发生了什么变化 — Web API 项目](http://go.microsoft.com/fwlink/p/?LinkId=513810)。

##下一步行动

提出问题并获得的帮助。

 - [MSDN 论坛︰ Azure 广告](https://social.msdn.microsoft.com/forums/azure/home?forum=WindowsAzureAD)

 - [Azure AD 文档](http://azure.microsoft.com/documentation/services/active-directory/)

 - [对 Azure AD 的博客文章︰ 简介](http://blogs.msdn.com/b/brunoterkaly/archive/2014/03/03/introduction-to-windows-azure-active-directory.aspx)


测试

---
ms.openlocfilehash: 503a8acc29903da62c52f240600e124414e3f9f5
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="从 Azure 市场创建一个果园 CMS web 应用程序" 
    description="教您如何在 Azure 中创建新的 web 应用程序的教程。 此外学习如何启动和管理您的 web 应用程序使用 Azure 门户。" 
    tags="azure-portal"
    services="app-service\web" 
    documentationCenter=".net" 
    authors="tfitzmac" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/03/2015" 
    ms.author="tomfitz"/>

# 从 Azure 市场创建一个果园 CMS web 应用程序

## 概述

市场上提供范围广泛的流行的 web 应用程序由 Microsoft、 第三方公司和开放源码软件计划开发。 从市场创建的 web 应用程序不需要安装任何软件，不是用于连接到[Azure 预览门户网站](http://go.microsoft.com/fwlink/?LinkId=529715)的浏览器。 在市场上的 web 应用程序有关的详细信息，请参阅[Azure 市场](/marketplace/)。

在本教程中，您将学习︰

- 如何从市场中创建新的 web 应用程序

- 如何启动和管理您的 web 应用程序从 Azure 门户
 
将构建果园 CMS web 应用程序使用的默认模板。 [果园](http://www.orchardproject.net/)是免费，开源的。基于网络的 CMS 允许您创建自定义的应用程序、 内容驱动的 web 应用程序和网站。 果园 CMS 包含扩展性框架，通过它您可以[下载附加模块和主题](http://gallery.orchardproject.net/)自定义您的 web 应用程序。 下图显示在[Azure 应用程序服务](http://go.microsoft.com/fwlink/?LinkId=529714)，您将创建使果园 CMS web 应用程序。

![果园的博客][13]

[AZURE.INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

>[AZURE.NOTE] 如果您想要怎样的 Azure 帐户之前开始使用 Azure 应用程序服务，请转到[尝试应用程序服务](http://go.microsoft.com/fwlink/?LinkId=523751)，立即可以在此创建短期的初学者 web 应用程序在应用程序服务。 没有信用卡，所需;没有承诺。

## 从市场创建一个果园 web 应用程序

1. 登录到[Azure 预览门户](http://portal.azure.com)。

2. 单击**新建** > **Web + 移动** > **Azure 的市场**。
    
    ![创建新的][1]

3. 单击**Web 应用程序** > **果园 CMS**。 在下一步的刀片式服务器，请单击**创建**。
    
    ![从市场创建][2]

4. 配置 web 应用程序的 URL、 应用程序服务计划、 资源组和位置。 当您完成时，请单击**创建**。
    
    ![配置应用程序][3]

    创建 web 应用程序后，**通知**按钮将显示一个绿色的"成功"，将显示您的 web 应用程序的刀片式服务器。

## 启动和管理果园 web 应用程序

1. 在 web 应用程序的刀片式服务器，请单击**浏览**以打开 web 应用程序的欢迎页面。

    ![浏览按钮][12]

2. 输入所需的果园，配置信息，然后单击**完成安装程序**以完成配置，然后打开该 web 应用程序的主页。

    ![登录到果园][7]

    您将有新果园 web 应用程序看起来类似于下面的屏幕截图。  

    ![果园的 web 应用程序][13]

3. 按照中[果园文档](http://docs.orchardproject.net/)以了解更多有关果园和配置新的 web 应用程序的详细信息。

## 下一步行动

* [创建一个 ASP.NET MVC 应用程序使用身份验证和 SQL 数据库并将其部署到 Azure 应用程序服务](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md)--了解如何在 Azure 应用程序服务从 Visual Studio 中创建新的 web 应用程序。

## 会发生什么变化
* 有关更改网站为应用程序服务的指南，请参阅︰ [Azure 应用程序服务，并对现有的 Azure 服务及其影响](http://go.microsoft.com/fwlink/?LinkId=529714)
* 更改门户与预览门户的指南，请参阅︰[用于预览门户导航的引用](http://go.microsoft.com/fwlink/?LinkId=529715)

[1]: ./media/web-sites-dotnet-orchard-cms-gallery/orchardgallery-01.png
[2]: ./media/web-sites-dotnet-orchard-cms-gallery/orchardgallery-02.png
[3]: ./media/web-sites-dotnet-orchard-cms-gallery/orchardgallery-03.png
[4]: ./media/web-sites-dotnet-orchard-cms-gallery/orchardgallery-04.png
[5]: ./media/web-sites-dotnet-orchard-cms-gallery/orchardgallery-05.png
[7]: ./media/web-sites-dotnet-orchard-cms-gallery/orchardgallery-07.png
[12]: ./media/web-sites-dotnet-orchard-cms-gallery/orchardgallery-12.png
[13]: ./media/web-sites-dotnet-orchard-cms-gallery/orchardgallery-08.png


 

测试

---
ms.openlocfilehash: 610c252b000126f115fa4b64c82f9eb2ede58984
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="从 Microsoft Azure 中市场创建一个 Umbraco web 应用程序" 
    description="创建一个 Umbraco 内容管理系统并将其部署到 Azure 应用程序服务 Web 应用程序。" 
    tags="azure-portal"
    services="app-service\web" 
    documentationCenter="" 
    authors="tfitzmac" 
    manager="wpickett" 
    editor="mollybos"/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/03/2015" 
    ms.author="tomfitz"/>

#从 Microsoft Azure 中市场创建一个 Umbraco web 应用程序#

Umbraco CMS 是一个完备的开源内容管理系统，可用于创建从小型到复杂的各种应用程序。 Azure 市场提供范围广泛的流行的 web 应用程序由 Microsoft，第三方公司，开发并打开源软件计划。 效果库允许您在几分钟内创建 Umbraco CMS 应用程序，通过应用的初学者工具包或通过集成自己设计。 

这篇文章展示了 Azure 预览门户，极大地简化了资源管理。 Azure 预览门户网站旨在通过将跨平台的工具、 技术和服务从 Microsoft 和它的合作伙伴在单个工作区中加快您的软件交付过程。 而不是使用类似[Azure 应用程序服务](http://go.microsoft.com/fwlink/?LinkId=529714)Web 应用程序，Visual Studio 项目或数据库的独立资源，您可以创建、 管理和分析整个应用程序作为一个资源组。 

在本教程中，您将学习︰

- 如何创建新 web 应用程序通过使用 Azure 预览门户 Markeplace
- 如何通过使用 Umbraco CMS 构建博客网站 

##从市场 Azure 预览门户中创建 web 应用程序

1. 登录到[Azure 预览门户](https://portal.azure.com/)。

2. **市场上**的图标中选择。
    
3. 在**市场上**，选择**Web + 移动**选项卡，然后选择**Umbraco CMS**。
    
4. 若要创建一个新的 Umbraco CMS web 应用程序，请单击**创建**。
    
5. 下一步是配置与 Umbraco CMS 相关联的所有资源。 在这种情况下，这些资源包括一个 web 应用程序和 SQL Server 数据库。 首先，选择要配置的 web 应用程序设置，例如**URL**、**应用程序服务计划**、 **Web 应用程序设置**和**位置**的**Web 应用程序**。 
    
    ![配置资源][04AppSettings]
    
6. 配置数据库。 选择**数据库**，然后选择**服务器**。 本示例在 Azure 创建 SQL Server 数据库。
    
    ![在 Azure 上创建 SQL Server][05NewServer]
    
7. 现在，配置 web 应用程序和数据库，您可以开始部署应用程序，通过单击底部的第一个**Umbraco CMS**刀片式服务器上图所示的**创建**。
    
    ![单击创建][06UmbracoCMSGroup]
    
部署完成后，门户将显示刀片式服务器 Umbraco CMS web 应用程序的资源组。 在**摘要**部分中，单击 web 应用程序名称，以查看您的 web 应用程序的属性。 此外在**摘要**部分中，可以选择要查看的属性相关联的数据库的数据库资源。

## 启动和配置 Umbraco CMS web 应用程序 ##

1. 在您的 web 应用程序的详细信息刀片式服务器，请单击**浏览**以浏览您的 web 应用程序。
    
    ![浏览到您的站点][08UmbracoCMSGroupRunning]
    
2. 当您浏览 web 应用程序时，Umbraco CMS 启动其安装程序向导。 输入请求的信息，然后单击**自定义**。
    
    ![安装 Umbraco 向导][09InstallUmbraco7]
    
3. Umbraco 将使用的数据库中输入您的连接和身份验证详细信息。 选择**Microsoft SQL Azure**数据库类型。  您可以从您的 web 应用程序的**站点设置**部分数据库获取连接字符串。
    
    ![配置您的数据库][10ConfigureYourDatabase] 
    
4. 如果您不熟悉 Umbraco CMS，您可以选择网站初学者工具包。 否则，请单击**不，谢谢，我不想安装一个初学者网站**。
    
    ![安装一个初学者网站][11InstallAStarterWebsite]
    
5. Umbraco 安装程序将完成您的应用程序的设置。 在配置您的应用程序后，您将重定向到 Umbraco CMS 管理仪表板。
    
    ![Umbraco CMS 仪表板][14FriendlyCMS]
    
6. 现在，您将创建将发布的示例文本页。 选择**内容**、**溢出**，选择，然后选择**TextPage**。
    
    ![创建文本页面][15CreateItemUnderOverflow]
    
7. 输入标题和内容文本页面，如下所示。 完成后，单击**保存并发布**。
    
    ![输入一个标题和一些内容][16EnterAName]
    
8. 发布您的页时，请稍候。 当发布完成时，您将收到一个小警报在屏幕的右下角。 您现在可以在您的 web 应用程序上浏览新的内容。 
    
    ![已发布的 web 站点页][17MyPage]
    

就这么简单 ！ 您已成功创建博客 web 应用程序使用 Umbraco CMS，只需几分钟。 

##其他资源

[Umbraco 文档](http://our.umbraco.org/documentation)

[Umbraco 视频教程](https://umbraco.com/help-and-support/video-tutorials.aspx)

[Azure 门户文档](../preview-portal.md)

[Azure 门户 （第 9 频道）](http://channel9.msdn.com/Blogs/Windows-Azure/Azure-Preview-portal) 

[Azure 应用程序服务 Web 应用程序文档](/documentation/services/websites/)

## 会发生什么变化
* 有关更改网站为应用程序服务的指南，请参阅︰ [Azure 应用程序服务，并对现有的 Azure 服务及其影响](http://go.microsoft.com/fwlink/?LinkId=529714)
* 更改门户与预览门户的指南，请参阅︰[用于预览门户导航的引用](http://go.microsoft.com/fwlink/?LinkId=529715)

>[AZURE.NOTE] 如果您想要怎样的 Azure 帐户之前开始使用 Azure 应用程序服务，请转到[尝试应用程序服务](http://go.microsoft.com/fwlink/?LinkId=523751)，立即可以在此创建短期的初学者 web 应用程序在应用程序服务。 没有信用卡，所需;没有承诺。


<!-- IMAGES -->
[01Startboard]: ./media/web-sites-gallery-umbraco/01Startboard.PNG
[02WebGallery]: ./media/web-sites-gallery-umbraco/02WebGallery.PNG
[03UmbracoCMS]: ./media/web-sites-gallery-umbraco/03UmbracoCMS.PNG
[04AppSettings]: ./media/web-sites-gallery-umbraco/04AppSettings.PNG
[05NewServer]: ./media/web-sites-gallery-umbraco/05NewServer.PNG
[06UmbracoCMSGroup]: ./media/web-sites-gallery-umbraco/06UmbracoCMSGroup.PNG
[07UmbracoCMSGroupBlade]: ./media/web-sites-gallery-umbraco/07UmbracoCMSGroupBlade.PNG
[08UmbracoCMSGroupRunning]: ./media/web-sites-gallery-umbraco/08UmbracoCMSGroupRunning.PNG
[09InstallUmbraco7]: ./media/web-sites-gallery-umbraco/09InstallUmbraco7.png
[10ConfigureYourDatabase]: ./media/web-sites-gallery-umbraco/10ConfigureYourDatabase.png
[11InstallAStarterWebsite]: ./media/web-sites-gallery-umbraco/11InstallAStarterWebsite.png
[12ConfigureYourDatabase]: ./media/web-sites-gallery-umbraco/12ConfigureYourDatabase.png
[14FriendlyCMS]: ./media/web-sites-gallery-umbraco/14FriendlyCMS.PNG
[15CreateItemUnderOverflow]: ./media/web-sites-gallery-umbraco/15CreateItemUnderOverflow.PNG
[16EnterAName]: ./media/web-sites-gallery-umbraco/16EnterAName.PNG
[17MyPage]: ./media/web-sites-gallery-umbraco/17MyPage.PNG
 

测试

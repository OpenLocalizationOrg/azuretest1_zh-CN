---
ms.openlocfilehash: 237400ac292a4a5d06a82b050e1d05a79cd78d40
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="部署 web 应用程序在 Azure 应用程序服务"
    description="了解哪些方法可用于将内容部署到 Web 应用程序。"
    services="app-service\web"
    documentationCenter=""
    authors="tdykstra"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/14/2015"
    ms.author="tdykstra"/>

#部署 web 应用程序在 Azure 应用程序服务

## 概述

您必须将您自己的内容部署到[应用程序服务 Web 应用程序](http://go.microsoft.com/fwlink/?LinkId=529714)的多个选项。 本主题提供的简要概述，以及每个选项的详细信息的链接。


###<a name="cloud"></a>从云托管源代码管理系统部署

若要部署 web 应用程序的最佳方法是设置与您的[源代码管理系统](http://asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control)集成工作[不断提供工作流](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery)。 自动化不仅使开发过程更有效，但还可以使您的备份和恢复过程更易于管理和可靠。

如果您没有尚未设置的源代码管理，入门最简单的方法是使用云托管源代码管理系统。

* [Visual Studio 联机](#vso)
* [使用 Git 存储库网站](#git)
* [使用 Mercurial 资源库网站](#mercurial)
* [收存箱](#dropbox)

###<a name="ide"></a>从 IDE 部署

[Visual Studio](http://www.visualstudio.com/)和[WebMatrix](http://www.microsoft.com/web/webmatrix/)是 Microsoft 可用于 web 开发的 Ide （集成的开发环境）。 两者都提供了内置功能，使您可以轻松地部署 web 应用程序。  两者都可以使用[Web 部署](http://www.iis.net/downloads/microsoft/web-deploy)自动化部署相关的其他任务，如数据库部署和连接字符串更改。 同时还可以通过使用[FTP 或 FTPS](http://en.wikipedia.org/wiki/File_Transfer_Protocol)）。

WebMatrix 快速安装和简单易学，但 Visual Studio 提供了许多用于处理 Web 应用程序的功能。 从 Visual Studio ide 可以创建、 停止、 启动和删除 web 应用程序，您可以查看日志中实时创建时，您可以调试远程，并要多得多。 Visual Studio 也与例如[Visual Studio 在线](#vso)、 [Team Foundation Server](#tfs)和[Git 存储库](#git)的源代码管理系统集成。

* [Visual Studio](#vs)
* [WebMatrix](#webmatrix)

###<a name="ftp"></a>使用 FTP 实用程序部署

无论您使用何种 IDE 中，您还可以部署的内容到您的应用程序通过使用[FTP](http://en.wikipedia.org/wiki/File_Transfer_Protocol)将文件复制。 它可以很容易地创建 FTP 凭据的 web 应用程序，并可以使用 FTP，包括如 Internet Explorer 浏览器和齐全如[FileZilla](https://filezilla-project.org/)的免费实用程序的任何应用程序中使用它们。  Web 应用程序还支持更安全的 FTPS 协议。

虽然它可以很容易地将您的 web 应用程序的文件复制到 Azure 使用 FTP 实用程序，它们并不自动负责或协调相关的部署任务，如部署的数据库或更改连接字符串。 此外，许多 FTP 工具不跳过复制未更改的文件以比较源文件和目标文件。 对于大型应用程序，始终复制所有的文件可能会导致次要更新甚至长时间的部署时间由于始终复制的所有文件。

###<a name="onpremises"></a>从内部版本控制系统部署

如果您使用 TFS，Git 或 Mercurial 部署 （不云托管） 存储库中，您可以部署直接从存储库到 web 应用程序。

* [Team Foundation Server (TFS)](#tfs)
* [内部部署 Git 或 Mercurial 资料库](#onpremises)

###<a name="commandline"></a>使用命令行工具和 API 的 Azure 其余管理部署

总是最适合用来自动执行您开发的工作流，但如果不能直接在源代码控制系统中做到这一点，您可以设置它手动使用命令行工具。 通常，这涉及使用多个工具或框架，因为部署通常涉及到执行站点管理功能，以及将内容复制。

您可能需要通过提供其他管理 API 来执行部署的站点管理任务和更方便地使用该 API 的几个框架简化了 azure。

* [FTP](#ftp)
* [MSBuild](#msbuild)
* [FTP 脚本](#ftp2)
* [Windows PowerShell](#powershell)
* [.NET 管理 API](#api)
* [Azure 命令行接口 (CLI Azure)](#cli)
* [Web 部署命令行](#webdeploy)
 
###<a name="octopus"></a>Octopus 部署

可与应用程序服务 Web 应用程序使用[octopus 部署](http://en.wikipedia.org/wiki/Octopus_Deploy)。 有关详细信息，请参阅[部署 ASP.NET 应用程序到 Azure 网站](https://octopusdeploy.com/blog/deploy-aspnet-applications-to-azure-websites)。


##<a name="vso"></a>Visual Studio 联机

[Visual Studio 联机](http://www.visualstudio.com/)（以前称为团队基础服务） 是微软源控制和团队协作的基于云的解决方案。 该服务是免费为最多 5 个开发人员的团队。 要做到连续传递到应用程序服务 web 应用程序和存储库可以使用[Git 或 TFVC](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control#gittfs)。

有关详细信息，请参阅以下资源︰

* [连续传递到 Azure 使用 Visual Studio 在线和 TFVC](../cloud-services-continuous-delivery-use-vso.md)。 演示如何设置从 Visual Studio 在线的连续传递到 web 应用程序，使用 TFVC 的分步教程。 TFVC 是集中的信息源控制选项，而不是 Git，即分布式的源代码控制选项。
* [连续传递到 Azure 使用 Visual Studio 在线和 Git](../cloud-services-continuous-delivery-use-vso-git.md)。 类似于前面的教程但使用 Git，而不是 TFVC。

##<a name="git"></a>使用 Git 存储库网站

[Git](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control#gittfs)是流行的分布式的版本控制系统。 Azure 的内置功能，便于自动部署到 web 应用程序执行从流行的基于 web 的存储库中的网站中存储包括[GitHub](http://www.github.com)、 [CodePlex](http://www.codeplex.com/)和[BitBucket](https://bitbucket.org/)的 Git 存储库。 使用 Git 部署的优点是相对容易地回滚到较早的部署，如果以往任何时候都变得有必要。

有关详细信息，请参阅以下资源︰

* [发布到 Web 应用程序使用 Git 的源代码管理中](web-sites-publish-source-control.md)。 如何使用 Git 来从您的本地计算机直接发布到 Web 应用程序 （在 Azure，这种发布的方法称为本地 Git）。 此外演示如何启用连续部署从 GitHub、 CodePlex 或 BitBucket 的 Git 存储库。
* [部署到使用 GitHub 的 Web 应用程序使用 Kudu](http://azure.microsoft.com/documentation/videos/deploying-to-azure-from-github/)。 Scott Hanselman 和 David Ebbo 视频，演示如何部署 web 应用程序直接从 GitHub 到 Web 应用程序。
* [将部署到 Azure 按钮 Web 应用程序](http://azure.microsoft.com/blog/2014/11/13/deploy-to-azure-button-for-azure-websites-2/)。 有关触发的 Git 存储库中的部署方法的博客。
* [Git，Mercurial，和收存箱 azure 论坛](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=azuregit)。

##<a name="mercurial"></a>使用 Mercurial 资源库网站

如果[Mercurial](http://mercurial.selenic.com/)用作源代码管理系统，将您的存储库存储在[CodePlex](http://www.codeplex.com/)或[BitBucket](https://bitbucket.org/)，可以在 Azure 应用程序服务使用内置功能，自动部署您的内容。

有关如何部署使用 Mercurial 的信息，请参阅以下资源︰

* [发布到 Web 应用程序使用 Git 的源代码管理中](web-sites-publish-source-control.md)。 虽然本教程展示如何将发布一个 Git 存储库，寄宿在 CodePlex 或 BitBucket 的 Mercurial 资料库的过程是类似。
* [Git，Mercurial，和收存箱 azure 论坛](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=azuregit)。

##<a name="dropbox"></a>收存箱

[收存箱](https://www.dropbox.com/)不是源代码管理系统，但如果您将您的源代码存储在收存箱可以使部署自动化从收存箱帐户。

* [将部署到 Web 应用程序从收存箱](http://blogs.msdn.com/b/windowsazure/archive/2013/03/19/new-deploy-to-windows-azure-web-sites-from-dropbox.aspx)。 如何使用[Azure 门户](http://go.microsoft.com/fwlink/?LinkId=529715)设置收存箱部署。
* [收存箱部署到 Web 应用程序](http://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/Dropbox-Deployment-to-Windows-Azure-Web-Sites)。 该视频演示了将收存箱文件夹连接到一个 web 应用程序和如何快速您能够快速安装的 web 应用程序的显示和运行的过程或维护它使用简单的拖放部署。
* [Git，Mercurial，和收存箱 azure 论坛](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=azuregit)。

##<a name="vs"></a>Visual Studio

有关如何从 Visual Studio 将部署到 Web 应用程序的信息，请参阅以下资源︰

* [开始使用 Azure 和 ASP.NET](web-sites-dotnet-get-started.md)。 如何创建和部署一个简单的 ASP.NET MVC web 项目使用 Visual Studio 和 Web 部署。
* [如何使用 Visual Studio 部署 Azure WebJobs](websites-dotnet-deploy-webjobs.md)。 如何配置控制台应用程序项目，以便它们将部署为 WebJobs。  
* [部署与成员资格、 OAuth 和 SQL 数据库与 Web 应用程序安全的 ASP.NET MVC 5 应用程序](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md)。 如何创建和部署使用 Visual Studio、 Web 部署，和实体框架代码第一个迁移 SQL 数据库，ASP.NET MVC web 项目。
* [Visual Studio 和 ASP.NET web 部署概述](http://msdn.microsoft.com/library/dd394698.aspx)。 对 web 部署使用 Visual Studio 的基本介绍。 发布日期，但包括仍然相关，包括选项的概述，为部署数据库和 web 应用程序和其他部署一组任务可能需要执行操作，或者手动配置 Visual Studio 为您执行的信息。 本主题是关于一般而言，不仅部署到 Web 应用程序的部署。
* [ASP.NET Web 部署使用 Visual Studio](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/introduction)。 12 部分教程系列，包括比其他在此列表中更完整的部署任务范围。 由于编写本教程，但后来添加的注释解释了什么是缺少上添加了一些 Azure 的部署功能。
* [部署到 Azure Git 存储库直接从 Visual Studio 2012 中的 ASP.NET 网站](http://www.dotnetcurry.com/ShowArticle.aspx?ID=881)。 说明如何部署 ASP.NET web 项目中使用插件 Git 提交代码到 Git，Git 存储库的连接 Azure 的 Visual Studio。 在 Visual Studio 2013年启动，是 Git 支持内置不需要插件的安装。

##<a name="webmatrix"></a>WebMatrix

有关如何从 WebMatrix 部署到 Web 应用程序的信息，请参阅以下资源︰

* [生成和部署到 Azure 使用 WebMatrix Node.js 网站](web-sites-nodejs-use-webmatrix.md)。
* [创建和部署一个 PHP MySQL web 应用程序使用 WebMatrix](web-sites-php-mysql-use-webmatrix.md)。
* [WebMatrix 3︰ 集成 Git 和部署到 Azure](http://www.codeproject.com/Articles/577581/Webmatrixplus3-3aplusIntegratedplusGitplusandplusD)。 WebMatrix 用于部署从 Git 版本控制库的方式。

有关详细信息，请参阅以下资源︰

* [创建一个 PHP MySQL web 应用程序和部署使用 FTP](web-sites-php-mysql-deploy-use-ftp.md)。

##<a name="tfs"></a>Team Foundation Server (TFS)

Team Foundation Server 是 Microsoft 的内部解决方案源控制和团队协作。 您可以设置 TFS 做连续传递到 web 应用程序。

有关详细信息，请参阅以下资源︰

* [不断在 Azure 的云服务的提供](../cloud-services-dotnet-continuous-delivery.md)。 本文档是对 Azure 的云服务，但其部分内容相关的 Web 应用程序。

##<a name="gitmercurial"></a>内部部署 Git 或 Mercurial 资料库

在 Azure 可以输入任何存储库使用 Git 或 Mercurial 为了部署从该位置的 URL。 为 web 应用程序，可以直接推从本地 Git 存储库。

有关详细信息，请参阅以下资源︰

* [发布到 Web 应用程序使用 Git 的源代码管理中](web-sites-publish-source-control.md)。 如何使用 Git 直接从您的本地计算机发布到 web 应用程序 （在 Azure，这种发布的方法称为本地 Git）。 此外演示如何启用连续部署从 GitHub、 CodePlex 或 BitBucket 的 Git 存储库。
* [发布到任何 git/hg repo 从 Web 应用程序](http://blog.davidebbo.com/2013/04/publishing-to-azure-web-sites-from-any.html)。 介绍了 Web 应用程序中的"外部资源库"功能的博客。
* [Git，Mercurial，和收存箱 azure 论坛](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=azuregit)。
* [部署两个网站到 Azure 一个 Git 存储库](http://www.hanselman.com/blog/DeployingTWOWebsitesToWindowsAzureFromOneGitRepository.aspx)。 Scott Hanselman 的博客文章。


##<a name="msbuild"></a>MSBuild

如果您使用[Visual Studio IDE](#vs)的开发时，可以使用[MSBuild](http://msbuildbook.com/)自动化可以在 IDE 中执行的任何操作。 您可以配置 MSBuild 使用[Web 部署](#webdeploy)或[FTP/FTPS](#ftp)来复制文件。 Web 部署也可以自动执行许多其他与部署相关的任务，例如部署数据库。

有关使用 MSBuild 命令行部署的详细信息，请参阅以下资源︰

* [使用 Visual Studio 的 ASP.NET Web 部署︰ 命令行部署](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/command-line-deployment)。 有关部署到 Azure 使用 Visual Studio 的教程系列中的第十。 演示如何使用命令行来部署设置发布配置文件在 Visual Studio 中之后。
* [在 Microsoft 生成引擎︰ 使用 MSBuild 和基础的团队项目生成](http://msbuildbook.com/)。 包含有关如何部署使用 MSBuild 章节的硬拷贝的书籍。

##<a name="ftp2"></a>FTP 脚本

它可以很容易地创建[FTP/FTPS](http://en.wikipedia.org/wiki/File_Transfer_Protocol)凭据的 web 应用程序，并使用 FTP 批处理脚本然后使用这些凭据。

有关详细信息，请参阅以下资源︰

* [使用 FTP 批处理脚本](http://support.microsoft.com/kb/96269)。

##<a name="powershell"></a>Windows PowerShell

您可以从[Windows PowerShell](http://msdn.microsoft.com/library/dd835506.aspx)执行 MSBuild 或 FTP 部署功能。 如果您这样做时，您还可以使用轻松地调用 Azure 其余管理 API 的 Windows PowerShell cmdlet 的集合。

有关详细信息，请参阅以下资源︰

* [部署 web 应用程序链接到 GitHub 存储库](app-service-web-arm-from-github-provision.md)
* [设置 web 应用程序与 SQL 数据库](app-service-web-arm-with-sql-database-provision.md)
* [配置和部署 microservices Azure 中预知](app-service-deploy-complex-application-predictably.md)
* [使用 Azure，构建真实的云应用程序自动化的一切](http://asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything)。 介绍了电子书中所示的示例应用程序如何使用 Windows PowerShell 的脚本来创建 Azure 的测试环境并将其部署到它的电子书章。 请参阅[资源](http://asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything#resources)部分的其他 Azure PowerShell 文档的链接。
* [使用 Windows PowerShell 脚本要发布到开发和测试环境](http://msdn.microsoft.com/library/dn642480.aspx)。 如何使用 Windows PowerShell 部署脚本的生成 Visual Studio。

##<a name="api"></a>.NET 管理 API

您可以编写代码 C# 部署执行 MSBuild 或 FTP 的功能。 如果您这样做时，您可以访问 Azure 管理 REST API 来执行网站管理功能。

有关详细信息，请参阅以下资源︰

* [自动化的 Azure 管理库和.NET 的所有内容](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx)。 介绍.NET 管理 API 和多个文档的链接。

##<a name="cli"></a>Azure 命令行接口 (CLI Azure)

可以在 Windows、 Mac 或 Linux 机器中使用命令行来使用 FTP 部署的。 如果这样做，您也可以访问 Azure 其余管理 API 使用 Azure CLI。

有关详细信息，请参阅以下资源︰

* [Azure 命令行工具](/downloads/#cmd-line-tools)。 在 Azure.com 命令行工具的信息的门户页。

##<a name="webdeploy"></a>Web 部署命令行

[Web 部署](http://www.iis.net/downloads/microsoft/web-deploy)是部署到 IIS，不仅提供了智能文件同步功能，但也可以执行或协调许多其他与部署相关的任务使用 FTP 时无法自动的 Microsoft 软件。 例如，Web 部署可以部署新数据库或数据库更新，以及您的 web 应用程序。 Web 部署可以还将更新现有的站点，因为它可以智能地将复制已更改的文件所需的时间降至最低。 Microsoft WebMatrix，Visual Studio，Visual Studio 在线学习，以及 Team Foundation Server 操作系统都支持 Web 部署内置，但您还可以使用 Web 部署直接从命令行以使部署自动化。 Web 部署命令的功能非常强大，但学习曲线很陡。

有关详细信息，请参阅以下资源︰

* [简单的 Web 应用程序︰ 部署](http://azure.microsoft.com/blog/2014/07/28/simple-azure-websites-deployment/)。 通过关于他写来方便地使用 Web 部署工具的 David Ebbo 的博客。
* [Web 部署工具](http://technet.microsoft.com/library/dd568996)。 官方 Microsoft TechNet 站点上的文档。 但仍良好开端的发布日期。
* [使用 Web 部署](http://www.iis.net/learn/publish/using-web-deploy)。 访问 IIS.NET Microsoft 网站上的正式文档。 但良好开端还发布日期。
* [StackOverflow](http://www.stackoverflow.com)。 有关如何从命令行使用 Web 部署的最新信息的最佳位置。
* [使用 Visual Studio 的 ASP.NET Web 部署︰ 命令行部署](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/command-line-deployment)。 MSBuild 将使用 Visual Studio，则生成引擎，它还可用于从命令行部署到 Web 应用程序的 web 应用程序。 本教程是关于 Visual Studio 部署主要是一系列的一部分。

##<a name="nextsteps"></a>下一步行动

在某些情况下可能需要能够轻松地转移到您的 web 应用程序的产品版本之间来回切换。 有关详细信息，请参阅[在 Web 应用程序上分阶段部署](web-sites-staged-publishing.md)。

具有备份和还原计划到位是任何部署工作流的一个重要组成部分。 有关 Web 应用程序的信息备份和还原功能，请参见[Web 应用程序的备份](web-sites-backup.md)。  

有关如何使用 Azure 的基于角色的访问控制来管理访问 Web 应用程序部署的信息，请参阅[RBAC 和 Web 应用程序发布](http://azure.microsoft.com/blog/2015/01/05/rbac-and-azure-websites-publishing)。

有关部署的其他主题的信息，请参阅[Web 应用程序文档](/documentation/services/web-sites/)中的部署部分。

## 会发生什么变化
* 有关更改网站为应用程序服务的指南，请参阅︰ [Azure 应用程序服务，并对现有的 Azure 服务及其影响](http://go.microsoft.com/fwlink/?LinkId=529714)
* 旧的门户与新门户的更改的指南，请参阅︰[用于预览门户导航的引用](http://go.microsoft.com/fwlink/?LinkId=529715)
 

测试

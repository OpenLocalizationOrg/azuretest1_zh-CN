---
ms.openlocfilehash: a07d7957fe57d56aeec864b069944844d9720958
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="将项目存储在源代码管理 |Microsoft Azure"
    description="了解如何存储在后端.NET 项目并从您的计算机上本地 Git repo 发布。"
    services="mobile-services"
    documentationCenter=""
    authors="ggailey777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="na" 
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="06/16/2015"
    ms.author="glenga"/>

# 将项目存储在源代码管理中

> [AZURE。选择器列表 (平台 |后端）]
- [(任何 |.NET)](mobile-services-dotnet-backend-store-code-source-control.md)
- [(任何 |) Javascript](mobile-services-store-scripts-source-control.md)

本主题演示如何使用 Azure 移动服务所提供的源控件来存储您的.NET 后端服务项目。 您的项目可以通过简单地从本地 Git 存储库中上载到生产移动服务发布。

若要完成本教程，必须已完成[移动服务入门]或[添加到现有应用程序的移动服务]本教程创建移动服务。

##<a name="enable-source-control"></a>启用版本控制，在您的移动服务

[AZURE.INCLUDE [mobile-services-enable-source-control](../../includes/mobile-services-enable-source-control.md)]

##<a name="clone-repo"></a>安装 Git 并创建本地资源库中

1. 在您的本地计算机上安装 Git。

    Git 安装所需的步骤因操作系统而异。 有关操作系统特定的分发和安装指南，请参阅[安装 Git] 。

    > [AZURE.NOTE]
    > 在某些操作系统中，命令行和 GUI 的 Git 版本都可用。 本文中提供的说明进行操作使用的命令行版本。

2. 打开命令行，如**GitBash** (Windows) 或**狂欢**（Unix 外壳）。 在 OS X 系统上可以访问命令行通过**终端**应用程序。

3. 从命令行更改将用来储存您的脚本的目录。 例如， `cd SourceControl`。

4. 使用下面的命令来创建新的 Git 存储库中，一个本地副本替换`<your_git_URL>`您的移动服务的 Git 存储库的 url:

        git clone <your_git_URL>

5. 出现提示时，键入用户名和密码，那么当您在您的移动服务中启用源代码管理设置。 身份验证成功后，您将看到一系列类似下面的响应︰

        remote: Counting objects: 8, done.
        remote: Compressing objects: 100% (4/4), done.
        remote: Total 8 (delta 1), reused 0 (delta 0)
        Unpacking objects: 100% (8/8), done.

6. 浏览到运行目录`git clone`命令，并请注意，移动服务的名称创建一个新目录。 对于.NET 后端移动服务，git 存储库是空的初始。

现在，您已经创建了您的本地存储库中，您可以发布.NET 后端服务项目从该存储库。

##<a name="deploy-scripts"></a>使用 Git 发布您的项目

1. 在 Visual Studio 2013，创建新的.NET 后端移动服务项目，或将现有项目移动到您新的本地存储库。  

    对于快速测试，下载并保存到此文件夹的移动服务快速启动项目。

2. 删除任何 NuGet 程序包文件夹离开 packages.config 文件。

    移动服务将自动还原 NuGet 程序包根据 packages.confign 文件。 您还可以定义要防止被添加的软件包目录的.gitignore 文件。

3. 在 Git 命令提示符处，键入以下命令以开始跟踪新的脚本文件︰

        $ git add .

4. 键入下列命令以提交更改︰

        $ git commit -m "adding the .NET backend service project"

5. 键入以下命令可将所做的更改上载到远程资源库中，并提供您的凭据︰

        $ git push origin master

    您应该看到一系列命令，指示将项目部署到移动服务、 添加的软件包，并重新启动该服务。

6. 浏览到您的.NET 后端移动服务的 URL，您应该会看到如下︰

    ![移动服务启动页](./media/mobile-services-dotnet-backend-store-code-source-control/mobile-service-startup.png)

现在您的移动服务项目保持在源代码管理中，您可以通过简单地从本地资源库推送更新发布服务更新。 关于数据模型更改.NET 后端移动服务使用 SQL 数据库中的信息，请参阅[如何到.NET 后端移动服务数据模型更改]。

<!-- Anchors. -->

<!-- Images. -->

<!-- URLs. -->
[Git 网站]: http://git-scm.com
[源代码管理]: http://msdn.microsoft.com/library/windowsazure/c25aaede-c1f0-4004-8b78-113708761643
[安装 Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[开始使用移动服务]: mobile-services-dotnet-backend-ios-get-started.md
[添加到现有应用程序的移动服务]: mobile-services-dotnet-backend-ios-get-started-data.md
[Azure 的管理门户]: https://manage.windowsazure.com/
[从客户端调用一个自定义的 API]: mobile-services-dotnet-backend-ios-call-custom-api.md
[如何使数据模型更改为.NET 后端移动服务]: mobile-services-dotnet-backend-how-to-use-code-first-migrations.md

测试

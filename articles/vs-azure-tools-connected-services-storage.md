---
ms.openlocfilehash: c44c359121c5eb780909da21a94dc4d117b273ad
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="通过在 Visual Studio 中使用连接服务添加 Azure 存储 |Microsoft Azure"
   description="通过使用 Visual Studio 中添加连接服务对话框中将 Azure 存储添加到您的应用程序"
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

# 通过使用 Visual Studio 连接服务添加 Azure 存储

## 概述

与 Visual Studio 2015，您可以连接任何 C# 云服务、.NET 后端移动服务，ASP.NET 网站或服务，ASP.NET 5 服务或 Azure WebJob 服务到 Azure 存储通过使用**添加连接服务**对话框。 连接的服务功能添加了所有所需的引用和连接代码，并适当地修改配置文件。 该对话框还将告诉您下一步是启动 blob 存储、 队列和表的文档。

## 支持的项目类型

可以使用连接服务对话框以连接到 Azure 存储中以下项目类型。

- ASP.NET Web 项目

- ASP.NET 5 的项目

- .NET 云服务 Web 角色和工作角色项目

- .NET 的移动服务项目

- Azure WebJob 项目


## 连接到 Azure 存储使用服务连接对话框

1. 请确保您有一个 Azure 帐户。 如果您没有 Azure 帐户，您可以注册[免费试用版](http://go.microsoft.com/fwlink/?LinkId=518146)。 Azure 帐户之后，您可以创建存储帐户、 创建移动服务，和配置 Azure Active Directory。

1. 在 Visual Studio 中打开您的项目，在解决方案资源管理器中打开上下文菜单中的**引用**节点，然后选择**添加连接服务**。

    ![添加连接的服务](./media/vs-azure-tools-connected-services-storage/IC796702.png)

1. 在**添加连接服务**对话框中，选择**Azure 存储**，然后选择**配置**按钮。 系统可能会提示您登录到 Azure，如果还没有这么做。

    ![添加连接服务对话框中的存储](./media/vs-azure-tools-connected-services-storage/IC796703.png)

1. 在**Azure 存储**对话框中，选择现有的存储帐户，然后单击**添加**。

    如果您需要创建一个新的存储帐户，请转到下一步。 否则，请跳到步骤 6。

    ![Azure 存储对话框](./media/vs-azure-tools-connected-services-storage/IC796704.png)

1. 若要创建新的存储帐户︰ 

    1. 选择 Azure 存储对话框底部的**创建新的存储帐户**按钮。

    1. 填写**创建存储帐户**对话框，然后选择**创建**按钮。
    
        ![Azure 存储对话框](./media/vs-azure-tools-connected-services-storage/create-storage-account.png)

        回在**Azure 存储**对话框中时，列表中将显示新的存储。

    1. 从列表中选择新的存储，然后单击**添加**。

1. 在 WebJob 项目的服务引用节点下将显示连接的存储服务。

    ![Azure 存储在 web 项目中的作业](./media/vs-azure-tools-connected-services-storage/IC796705.png)

1. 查看出现的入门页面，了解如何修改您的项目。 无论何时您添加连接的服务，入门页面出现在浏览器中。 您可以查看建议的下一个步骤和代码示例，或切换到怎么页后，可以看到哪些引用已添加到项目中，以及如何修改您的代码和配置文件。

## 如何修改项目

完成该对话框后，Visual Studio 将添加引用和修改某些配置文件。 具体更改取决于项目类型。 

 - 对于 ASP.NET 项目，看到[发生了什么变化 – ASP.NET 项目](http://go.microsoft.com/fwlink/p/?LinkId=513126)。 
 - 对于 ASP.NET 5 项目，看到[发生了什么变化 – ASP.NET 5 项目](http://go.microsoft.com/fwlink/p/?LinkId=513124)。 
 - 对于云服务项目 （web 角色和辅助角色），看到[发生了什么变化 – 云服务项目](http://go.microsoft.com/fwlink/p/?LinkId=516965)。 
 - WebJob 项目请参见[怎么了-WebJob 项目](vs-storage-webjobs-what-happened/)。

## 下一步行动

1. 使用快速入门代码示例作为参考，创建存储所需的类型，然后开始编写代码以访问您的存储帐户 ！

1. 提出问题并获得的帮助
     - [MSDN 论坛︰ Azure 存储](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata)

     - [Azure 存储团队博客](http://blogs.msdn.com/b/windowsazurestorage/)

     - [在 azure.microsoft.com 的存储](http://azure.microsoft.com/services/storage)

     - [存储文档，网址 azure.microsoft.com](http://azure.microsoft.com/documentation/services/storage/)


测试

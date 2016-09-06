---
ms.openlocfilehash: 13dc2ccdb10e990edaea1716e6d38cd963c59872
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="Azure 应用程序服务 API 的应用程序部署" 
    description="了解如何将一个 API 的应用程序项目部署到 Azure 订购。" 
    services="app-service\api" 
    documentationCenter=".net" 
    authors="bradygaster" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-api" 
    ms.workload="web" 
    ms.tgt_pltfrm="dotnet" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/19/2015" 
    ms.author="bradyg;tarcher"/>

# Azure 应用程序服务 API 的应用程序部署

## 概述

如果您正在积极开发自己的 API 应用程序使用 Visual Studio，并且需要在云环境中测试您的 API，可以在 Azure 订阅中创建一个新的 API 应用程序和部署代码使用 Visual Studio 方便应用程序服务的部署功能。 

这是三个系列中的第二个教程︰

1. [创建 API 应用程序](app-service-dotnet-create-api-app.md)中创建的是 API 的应用程序项目。 
* 在本教程中，您将 Azure 订阅部署 API 的应用程序。
* 在[调试 API 的应用程序](../app-service-dotnet-remotely-debug-api-app.md)，您将使用 Visual Studio 远程调试的代码运行在 Azure 时。

## API 的应用程序部署 

在**解决方案资源管理器**中右击该项目 （而不是解决方案），然后单击**发布...**。 

![项目发布菜单选项](./media/app-service-dotnet-publish-api-app/20-publish-gesture-v3.png)

单击**配置文件**选项卡并单击**Microsoft Azure API 应用程序 （预览）**。 

![发布 Web 对话框](./media/app-service-dotnet-publish-api-app/21-select-api-apps-for-deployment-v2.png)

单击**新建**设置 Azure 订购新 API 应用程序。

![选择现有的 API 服务对话框](./media/app-service-dotnet-publish-api-app/23-publish-to-apiapps-v3.png)

在**创建 API 的应用程序**对话框中，输入以下命令︰

- 在**API 的应用程序名称**，输入应用程序的名称。 
- 如果您有多个 Azure 的订阅，则选择要使用的一个。
-应用程序服务在计划下，从您现有的应用程序服务计划、 选择或选择**创建新的应用程序服务计划**并输入一个新的计划的名称。 
- 在**资源组**中，从您现有的资源组中选择或**创建新的资源组**中选择并输入一个名称。 该名称必须是唯一的。请考虑使用的应用程序名作为前缀，并追加一些个人信息，例如您的 Microsoft ID (不带 @ 符号)。  
- 在**访问级别**下选择**可用于任何人**。 此选项将使您的 API 完全公开，本教程是很好。 您可以限制访问以后通过 Azure 门户。
- 选择一个区域。  

![配置 Microsoft Azure Web 应用程序对话框](./media/app-service-dotnet-publish-api-app/24-new-api-app-dialog-v3.png)

单击**确定**以创建 API 的应用程序在您的订阅。 过程可能需要几分钟的时间，因此 Visual Studio 将显示一个对话框，通知您该进程已开始。 

![开始的 API 服务创建确认消息](./media/app-service-dotnet-publish-api-app/25-api-provisioning-started-v3.png)

设置过程在 Azure 订阅创建资源组和 API 的应用程序。 Visual Studio 在**Azure 服务活动应用程序**窗口中将显示进度。 

![通过 Azure 应用程序服务活动窗口的状态通知](./media/app-service-dotnet-publish-api-app/26-provisioning-success-v3.png)

一旦配置 API 的应用程序，请右击解决方案资源管理器中的项目并选择**发布**以重新打开发布对话框中。 在上一步中创建的发布配置文件应预先选定。 单击**发布**着手部署过程。 

![API 的应用程序部署](./media/app-service-dotnet-publish-api-app/26-5-deployment-success-v3.png)

**Azure 服务活动应用程序**窗口将显示部署进度。 

![Azure 应用程序服务活动窗口的状态通知](./media/app-service-dotnet-publish-api-app/26-5-deployment-success-v4.png)

## 在 Azure 门户中查看应用程序

在本节中，您将导航到门户网站来查看用于 API 的应用程序的基本设置和更改您的 API 应用程序的迭代。 每个部署中，门户将反映于 API 应用程序要做的更改。 

在浏览器中，导航到[Azure 门户](https://portal.azure.com)。 

单击侧栏上的**浏览**按钮并选择**API 的应用程序**。

![浏览 Azure 的门户网站上的选项](./media/app-service-dotnet-publish-api-app/27-browse-in-portal-v3.png)

选择从列表 API 应用程序中创建您的订阅中的 API。

![API 的应用程序列表](./media/app-service-dotnet-publish-api-app/28-view-api-list-v3.png)

单击**API 定义**。 应用程序的**API 定义**刀片式服务器显示时创建该应用程序定义的 API 操作列表中。 

![API 定义](./media/app-service-dotnet-publish-api-app/29-api-definition-v3.png)

现在返回到 Visual Studio 中的项目。 将下面的代码添加到**ContactsController.cs**文件中。  

    [HttpPost]
    public HttpResponseMessage Post([FromBody] Contact contact)
    {
        // todo: save the contact somewhere
        return Request.CreateResponse(HttpStatusCode.Created);
    }

此代码将添加可用于新张贴的**发布**方法`Contact`api 的实例。 

![将 Post 方法添加到控制器](./media/app-service-dotnet-publish-api-app/30-post-method-added-v3.png)

在**解决方案资源管理器**中右键单击项目并选择**发布**。 

![项目发布上下文菜单](./media/app-service-dotnet-publish-api-app/31-publish-gesture-v3.png)

从**配置**下拉列表中选择**调试**配置，单击**发布**以重新部署 API 的应用程序。 

![发布站点设置](./media/app-service-dotnet-publish-api-app/36.5-select-debug-option-v3.png)

在**Web 发布**向导的**预览**选项卡上，单击**发布**。  

![发布 Web 对话框](./media/app-service-dotnet-publish-api-app/39-re-publish-preview-step-v2.png)

发布过程完成后，请返回到门户，并关闭并重新打开**API 定义**刀片式服务器。 您将看到新的 API 端点刚创建和部署直接到 Azure 订购。

![API 定义](./media/app-service-dotnet-publish-api-app/38-portal-with-post-method-v4.png)

## 下一步行动

您已经看到如何在 Visual Studio 中的直接部署功能方便地循环访问和快速部署和测试您的 API 能够正常工作。 在[下一步的教程](../app-service-dotnet-remotely-debug-api-app.md)中，您将看到如何在 Azure 中运行时调试 API 的应用程序。


 
测试

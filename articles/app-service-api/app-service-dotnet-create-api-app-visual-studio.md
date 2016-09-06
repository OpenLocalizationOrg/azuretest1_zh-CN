---
ms.openlocfilehash: 9132e8b412c6c7711a7e6933b78d34a1b3f32723
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="作为一个 API 应用程序中配置 Web API 项目" 
    description="了解如何配置 Web API 项目作为 API 的应用程序，使用 Visual Studio 2013 " 
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
    ms.date="08/14/2015" 
    ms.author="tdykstra"/>

# 作为一个 API 应用程序中配置 Web API 项目

## 概述

本教程演示如何采用现有的 Web API 项目并将其配置为在[Azure 应用程序服务](../app-service/app-service-value-prop-what-is.md) [API 的应用程序](app-service-api-apps-why-best-platform.md)的部署。 系列中的后续教程将说明如何为[部署](app-service-dotnet-deploy-api-app.md)和[调试](../app-service-dotnet-remotely-debug-api-app.md)API 的应用程序项目，您在本教程中创建。

有关 API 的应用程序的信息，请参阅[API 的应用程序是什么？](app-service-api-apps-why-best-platform.md)。

[AZURE.INCLUDE [install-sdk-2015-2013](../../includes/install-sdk-2015-2013.md)]

本教程需要 2.6 或更高版本的 Azure sdk 版本的.NET。

## 配置 Web API 项目 

本节说明如何将现有的 Web API 项目配置为 API 的应用程序。 通过使用 Web API 项目模板来创建 Web API 项目，您就会开始，然后您可以将其配置为 API 的应用程序。

1. 打开 Visual Studio 2015年或 Visual Studio 2013年。

2. 单击**文件 > 新建项目**。 

3. 选择**ASP.NET Web 应用程序**模板。

4. 请确保清除**添加到项目的应用程序见解**复选框。 

4. 名为*ContactsList*的项目

    ![](./media/app-service-dotnet-create-api-app-visual-studio/01-filenew-v3.png)

5. 单击**确定**。

6. 在**新的 ASP.NET 项目**对话框中，选择**空**项目模板。

7. 单击**Web API**复选框。

8. 清除**在云环境中的主机**选项。

    ![](./media/app-service-dotnet-create-api-app-visual-studio/webapinewproj.png)

9. 单击**确定**以生成项目。

    ![](./media/app-service-dotnet-create-api-app-visual-studio/sewebapi.png)

10. 在**解决方案资源管理器**中的项目 （不是解决方案），用鼠标右键单击，然后选择**添加 > Azure API 应用程序 SDK**。

    ![](./media/app-service-dotnet-create-api-app-visual-studio/addapiappsdk.png)

11. 在**选择的 API 的应用程序元数据源**对话框中，单击**自动生成的元数据**。 

    ![](./media/app-service-dotnet-create-api-app-visual-studio/chooseswagger.png)

    此选项使动态 Swagger 用户界面，您可以看到在本教程后面部分。 如果您选择上载 Swagger 元数据文件，它将与保存文件名称*apiDefinition.swagger.json*，如下面一节中所述。 

12. 单击**确定**。 
 
    在这种情况下，Visual Studio 安装 API NuGet 程序包的应用程序，并将 API 的应用程序元数据添加到 Web API 项目。  

[AZURE.INCLUDE [app-service-api-review-metadata](../../includes/app-service-api-review-metadata.md)]

[AZURE.INCLUDE [app-service-api-define-api-app](../../includes/app-service-api-define-api-app.md)]

[AZURE.INCLUDE [app-service-api-direct-deploy-metadata](../../includes/app-service-api-direct-deploy-metadata.md)]

## 下一步行动

API 应用程序现已准备好进行部署，您可以按照[部署 API 应用程序](app-service-dotnet-deploy-api-app.md)教程来做到这一点。
 
测试

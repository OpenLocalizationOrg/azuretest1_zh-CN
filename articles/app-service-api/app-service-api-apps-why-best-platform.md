---
ms.openlocfilehash: d21543d1aada4f6ccd344f7f6d7528b33a9669fe
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="什么是 API 的应用程序？" 
    description="了解为什么 Azure 应用程序服务是制定、 发布和承载 rest 风格的 Api 的最佳平台。" 
    services="app-service\api" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-api" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/30/2015" 
    ms.author="tdykstra"/>

# 什么是 API 的应用程序？

API 的应用程序提供丰富的平台和生态系统的构建、 承载、 过多占用，并在云和内部分发的 Api。 部署您的 API 作为 API 的应用程序并从中获益企业级安全、 简单的访问控制、 混合和 SaaS 连接。 自动 SDK 生成，并与[逻辑应用程序](../app-service-logic/app-service-logic-what-are-logic-apps.md)无缝集成。

API 的应用程序是[Azure 应用程序服务](../app-service/app-service-value-prop-what-is.md)，其中还包括 Web 应用程序、 移动应用程序和逻辑的应用程序的一部分。 

![](./media/app-service-api-apps-why-best-platform/appservicesuite.png)

## 为什么 API 的应用程序？

API 的应用程序提供了用于开发、 部署、 发布、 消耗和管理 rest 风格 web Api 的功能。 应用程序服务提供了目前公共预览中提供的以下功能︰

- **很容易消耗**- [Swagger](http://swagger.io/)集成支持使您的 Api 方便地可由各种客户端。  API 的应用程序 SDK 可以为多种语言，包括 C#、 Java 和 Javascript 中的 Api 在您生成客户端代码。

- **简单的访问控制**-内置身份验证服务支持 Azure Active Directory 或 Facebook 和 Twitter 等第三方服务。 您可以到您的代码保护未更改的 API 应用程序 fgrom 未经身份验证的访问。 如果您熟悉[Azure 移动服务](../mobile-services-windows-dotnet-how-to-use-client-library.md#authentication)所提供的身份验证服务，API 的应用程序框架为基础，将其扩展到承载 API 的应用程序的 Api。  应用程序服务 SDK 还可以简化的语法用于授权码。 有关详细信息，请参阅[API 的应用程序和移动应用程序在 Azure 应用程序服务的身份验证](../app-service/app-service-authentication-overview.md)。

- **轻松连接到 SaaS 平台** - Azure 市场中的[连接器 API 的应用程序](../app-service-logic/app-service-logic-what-are-biztalk-api-apps.md)不是 Microsoft 和第三方来简化代码编写队伍、 Office 365、 Twitter、 Facebook、 收存箱，和许多其他与进行交互提供。

- **与逻辑应用程序集成**的 API 的应用程序创建可以由[应用程序服务的逻辑应用程序](../app-service-logic/app-service-logic-what-are-logic-apps.md)使用。    

- **Visual Studio 集成**-在 Visual Studio 中的专用工具简化[创建](app-service-dotnet-create-api-app.md)、[部署](app-service-dotnet-deploy-api-app.md)、[调试](app-service-dotnet-remotely-debug-api-app)和管理 API 应用程序的工作。

您可以将您现有的 API，为的是︰ 您不需要更改任何您现有的 Api，利用 API 的应用程序的功能，只需将您的代码部署到 API 应用程序中的代码。 为您的 Api，可以使用 ASP.NET、 Java、 PHP、 Node.js 或 Python。

此外，API 的应用程序包括[应用程序服务 Web 应用程序的功能](../app-service-web/app-service-web-overview.md)。

>[AZURE.NOTE] [Azure API 管理](/services/api-management/)是独立的服务提供终结点整合和调节等功能。 您可以使用 API 的应用程序使用 API 管理。
>
>正在公共预览 API 的应用程序。 [应用程序服务 Web 应用程序](../app-service-web/app-service-web-overview.md)是用于构建和承载在全球范围内的安全任务关键型应用程序通常可用 (GA) 服务。 如果您正在寻找用于构建您的 API 今天正式推出服务，Web 应用程序是很好的选择。 在 API 的应用程序发生后正式上市，我们将执行现有的 web 应用程序和利用 API 的应用程序的其他功能提供路径。

### API 的应用程序功能将来出现

在不久的将来，API 的应用程序平台还将创建一个丰富的生态系统的 Api，通过轻松地共享您的代码︰  

- **公共和私有市场**的[Azure 市场](http://azure.microsoft.com/marketplace/)很容易找到并部署到 Azure 订阅预打包 API 应用程序由 Microsoft 和第三方开发的。 然后您可以打包和发布您自己的 API 应用程序开发过程中，以便其他开发人员可以将它们部署到它们 Azure 的预订。 Azure 市场发布您的 Api 时，您将能够使其仅对您的组织中的其他成员可见。 

- **自动依赖项部署**-只要部署 API 应用程序从市场到 Azure 订购，Azure 将自动部署相关 API 应用程序并创建所需的资源。 API 的应用程序软件包将指定 API 的应用程序，这取决于和 Azure 它需要的资源。

- **自动更新**-一个您已经共享的 API 应用程序软件包更新代码时，您将能够将更新推送到所有已安装并且正在运行 API 的应用程序的用户。 这也适用于非重大更改和用户已选择进入到接收更新。

其中的许多功能，这种公共市场并自动更新时，都已由 Microsoft 提供的 API 应用程序可用。

## API 的应用程序的概念 ##

- **网关**-处理 API 管理功能和资源组中的所有 API 应用程序的身份验证的 web 应用程序。 
- **Swagger** -一个用于交互式文档和发现 rest 风格的 API 中，默认情况下，API 应用程序中使用的框架。 有关详细信息，请参阅[http://swagger.io/](http://swagger.io/)。
- **连接器**的 API 的应用程序，可以轻松地连接到 SaaS 平台如队伍和 Office 365 的类型。 有关详细信息，请参阅[什么是连接器和 BizTalk API 的应用程序](../app-service-logic/app-service-logic-what-are-biztalk-api-apps.md)。
- **触发器**的其余部分的[逻辑应用程序](../app-service-logic/app-service-logic-what-are-logic-apps.md)API 调用它来满足特定条件时启动工作流进程。 例如，API 的应用程序可以提供逻辑应用程序定期调用来寻找某些短语中的 Twitter 订阅源的方法。 有关详细信息，请参阅[API 应用触发器](app-service-api-dotnet-triggers.md)。
- 通过触发器来启动工作流后的流程数据可以调用**操作**的其余部分的[逻辑应用程序](../app-service-logic/app-service-logic-what-are-logic-apps.md)API。 例如，API 的应用程序可以提供逻辑应用程序调用以响应通过 Twitter 触发器找到 tweet 的方法。 操作是由 Swagger API 定义的 API 方法。

## 入门教程

若要开始使用 API 的应用程序，请按照[创建 API 应用程序教程](app-service-dotnet-create-api-app.md)。

若要查看 API 应用程序使用的已知问题列表，请参阅[MSDN 论坛张贴](https://social.msdn.microsoft.com/Forums/en-US/7f8b42f2-ac0d-48b8-a35e-3b4934e1c25e/api-app-known-issues?forum=AzureAPIApps)。

关于 Azure 应用程序服务平台的详细信息，请参阅[Azure 应用程序服务](../app-service/app-service-value-prop-what-is.md)。

 

测试

---
ms.openlocfilehash: aea06fa7b1aaa1ccce399e62aa73c67f89501aec
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="管理 API 的应用程序" 
    description="了解如何使用 Azure 的门户和 Visual Studio 的服务器资源管理器来管理 Azure 应用程序服务 API 的应用程序。" 
    services="app-service\api" 
    documentationCenter="" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-api" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na"
    ms.topic="article" 
    ms.date="06/17/2015" 
    ms.author="tdykstra"/>

# 管理 API 在 Azure 应用程序服务的应用程序

本文演示如何使用[Azure 预览门户](https://portal.azure.com/)执行 API 的应用程序管理和监视任务。 以下是一些可以执行的任务︰

- 配置身份验证 
- 启用自动缩放
- 查看日志
- 请参阅多少请求进行和 API 的应用程序正在使用的数据量，请参阅
- 备份 API 的应用程序和创建通知
- 配置基于角色的访问安全

文章还演示了如何执行某些管理任务在 Visual Studio 中的服务器资源管理器窗口中。

## 在 Azure 中的 API 的应用程序和网关刀片预览门户

在 Azure 应用程序服务 API 的应用程序是具有承载 web 服务的其他功能的[web 应用程序](../app-service-web/app-service-web-overview.md)。 在 Azure 的门户中，没有用于管理 API 特定的功能和管理基础的 web 应用程序的**API 应用程序主机**刀片式服务器**API 应用程序**刀片式服务器。 

每个资源组，其中包含至少一个 API 应用程序还包括一个*网关*。 网关作为代理，处理身份验证和所有 API 应用程序的资源组中的其他管理功能。 API 的应用程序，如网关是额外的功能，具有一个 web 应用程序，因此也有两个门户刀片管理网关︰ 特定于网关的功能，为**网关**刀片和管理基础的 web 应用程序**网关主机**刀片式服务器。

### 您可以仅在应用程序 API 刀片式服务器完成任务

刀片式服务器**API 的应用程序**用于下列任务︰

- 单击配置访问级别-**设置 > 应用程序设置**。 默认值是内部的这意味着同一资源组中的只有 API 应用程序可调用 API 的应用程序。 有关详细信息，请参阅[保护 API 的应用程序](app-service-api-dotnet-add-authentication.md)。   
- 配置更新策略-单击**设置 > 应用程序设置**。 默认值为**上**。 这意味着，市场发布 API 的应用程序的新版本时，您的 API 应用程序将自动更新到最新版本是否非重大更改。  
- 配置身份验证，对于传出调用 API 的应用程序-单击**设置 > 验证**。  如果 API 应用程序调用外部服务要求身份验证，在此处输入所需的配置值。 例如，收存箱连接器所需的客户端 ID 和客户端密钥访问收存箱服务。
- 单击配置[RBAC](../role-based-access-control-configure.md)的**设置 > 用户**。 您在此处配置的用户访问权限决定谁可以在只访问 API 的应用程序特定的功能。 若要将 RBAC 配置 web 应用程序功能，请使用**API 的应用程序主机**刀片式服务器。 通常情况下您想要保持 API 的应用程序和 API 的应用程序宿主的 RBAC 设置同步。 如果您授予某人访问 API 的应用程序而不是 API 的应用程序主机，他们也不能在实际上与 API 应用程序宿主的刀片式服务器**API 的应用程序**使用的功能。
- 查看 API 定义-单击**API 定义**中的**摘要**部分要公开的 API 的应用程序的方法的列表，请参阅。
- [安装混合连接管理器](../app-service-logic/app-service-logic-hybrid-connection-manager.md)。 混合连接管理器使您能够连接到内部部署的系统，如 SQL Server 或 SAP。 此混合连接使用 Azure 服务总线来连接并控制之间 Azure 资源和内部资源的安全。

### API 的应用程序刀片和 API 的应用程序主机刀片式服务器可以的任务 

刀片式服务器**API 的应用程序**，您可以执行特定于基础的 web 应用程序的许多任务。 例如，您可以执行以下任务︰

* 停止、 启动和重新启动 web 应用程序，该 API 的主机应用程序。  
- 选择**请求和错误**添加不同的性能指标包括常为人所知的 HTTP 错误代码，如 200、 400 或 500 的 HTTP 状态代码。
- 请参阅响应时间多少请求对 API 的应用程序，并可以看到和多少数据后，数据量熄灭。 
- 如果跃点数超过某个阈值选择的，创建电子邮件通知。 
- 请参阅多少**CPU**使用的 API 的应用程序、 查看当前**使用率配额**以 mb 为单位，并了解您基于成本层的最大数据使用情况。
- 请参阅**估计花费**来确定运行 API 的应用程序的潜在成本。
- 查看应用程序日志和其他 IIS 日志，包括 web 服务器日志和 FREB 日志。
- 选择要打开进程浏览器的**进程**。 这将显示您的网站实例和它们的属性，包括线程计数和内存使用情况。

这些任务也可以通过**API 应用程序主机**刀片式服务器。  这是两个刀片式服务器共享相同的用户界面的很多的原因。 例如，**停止**、**启动**和**重新启动****应用程序 API**刀片式服务器上的按钮具有完全相同的效果**停止**、**启动**和**重新启动**按钮**API 应用程序主机**刀片式服务器上。 同样，提供**API 的应用程序**刀片式服务器上的监视信息是什么**API 应用程序主机**刀片式服务器显示相同。 

上一节中列出刀片式服务器**API 的应用程序**提供的唯一函数，不是**API 的应用程序主机**刀片式服务器中的重复项。

### 仅在 API 应用程序主机刀片式服务器可以的任务

所有任务的任何 web 应用程序，请使用**API 的应用程序主机**刀片式服务器。

### 只有在网关刀片式服务器可以的任务

**网关**刀片式服务器用于下列任务︰

- 身份验证提供程序配置的传入调用 API 的应用程序-单击**设置 > 标识**。 如果网关需要验证用户身份之前，从而使它们可以调用 API 的应用程序的资源组中，此处输入所需的配置值。 有关详细信息，请参阅[配置和测试 SaaS 连接器在 Azure 应用程序服务](app-service-api-connnect-your-app-to-saas-connector.md)。 
- 单击配置[RBAC](../role-based-access-control-configure.md)的**设置 > 用户**。 此处配置的用户访问权限决定谁可以在只访问特定于网关的功能，不是所有的 web 应用程序与共享功能。

### 您可以在网关刀片式服务器和网关主机刀片式服务器完成任务 

网关和网关主机刀片式服务器共享用户界面相同的 API 的应用程序和 API 的应用程序主机刀片。

### 您可以仅在网关主机刀片式服务器的任务

所有的任务的任何 web 应用程序，请使用**网关主机**刀片式服务器。

## <a id="navigate"></a>如何导航到 API 的应用程序和网关刀片 

转到刀片式服务器**API 的应用程序**的一种方法是通过 Azure 门户的浏览功能。  在门户的主页上，单击**浏览 > API 应用程序**来查看您可管理的所有 API 应用程序。 

![](./media/app-service-api-manage-in-portal/browse.png)

![](./media/app-service-api-manage-in-portal/apiappslist.png)

### 导航到 API 应用程序刀片式服务器

当单击**API 的应用程序**列表刀片式服务器中的行时，门户会显示刀片式服务器**API 的应用程序**。

![](./media/app-service-api-manage-in-portal/apiappblade.png)

### 导航到 API 应用程序主机刀片式服务器

要获得**API 应用程序主机**刀片式服务器，单击刀片式服务器**API 应用程序**中的**API 的应用程序主机**。

![](./media/app-service-api-manage-in-portal/apiappbladetohost.png)

![](./media/app-service-api-manage-in-portal/apiapphostbladenocallouts.png)

### 导航到网关刀片式服务器

要获得**网关**刀片式服务器，请单击刀片式服务器**API 应用程序**中的**网关**链接。
   
![](./media/app-service-api-manage-in-portal/apiappbladegotogateway.png)

![](./media/app-service-api-manage-in-portal/gatewaybladenocallout.png)

### 导航到网关主机刀片式服务器

要获得**网关主机**刀片式服务器，请单击**网关**刀片式服务器中的**网关主机**链接。
   
![](./media/app-service-api-manage-in-portal/gatewaybladetohost.png)

![](./media/app-service-api-manage-in-portal/gatewayhost.png)

## 在服务器资源管理器在 Visual Studio 中的 API 访问应用程序

在**服务器资源管理器**在 Visual Studio 可以启动远程调试会话中查看流式的日志，这些日志并单击一个菜单项在门户网站中打开 API 应用程序刀片式服务器。

若要获得在服务器资源管理器 API 的应用程序，请单击**Azure > 应用程序服务 > [资源组名称] > [API 的应用程序名称]**，如图所示。

![](./media/app-service-api-manage-in-portal/se.png)

## 下一步行动

本文已经展示了如何使用 Azure 门户用于 API 的应用程序中执行管理任务。 用于 API API 的应用程序库中安装的应用程序，请参阅[管理和监视您的内置 API 应用程序和接口](../app-service-logic/app-service-logic-monitor-your-connectors.md)。

有关如何使用命令行管理 API 的应用程序的信息，（在各种浏览器窗口） 文章左侧看到**--自动执行**部分中显示的菜单上的文章或在顶部 （上狭窄的浏览器窗口） 的文章。

测试

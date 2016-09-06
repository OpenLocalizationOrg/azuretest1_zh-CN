---
ms.openlocfilehash: 591dd0d70b79d146679f1c1b9cd46612cd2b89a8
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="开始使用 Azure 搜索管理 REST API，" 
    description="开始使用 Azure 搜索管理 REST API，" 
    services="search" 
    documentationCenter="" 
    authors="HeidiSteen" 
    manager="mblythe" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="07/08/2015" 
    ms.author="heidist"/>

# 开始使用 Azure 搜索管理 REST API，

Azure 搜索其他管理 API 是在门户中执行管理任务编程式替代方法。 服务管理操作包括创建或删除服务、 扩展服务，并管理密钥。 本教程附带的示例客户端应用程序演示服务管理 API。 它还包括在本地开发环境中运行该示例所需的配置步骤。

若要完成本教程，您需要︰

- Visual Studio 2012 或 2013
- 示例客户端应用程序下载

在完成本教程后，请将提供两种服务︰ Azure 搜索和 Azure 活动目录 (AD)。 此外，您将创建的广告应用程序建立在 Azure 中的客户端应用程序和资源管理器终结点之间的信任。

您将需要一个 Azure 帐户来完成本教程。


##下载示例应用程序

本教程基于 Windows 控制台应用程序编写的 C#，您可以编辑并在运行 Visual Studio 2012 或 2013

您可以在[Azure 搜索管理 API 演示](https://azuresearchmgmtapi.codeplex.com/)在 Codeplex 上找到客户端应用程序。


##配置应用程序

在运行示例应用程序之前，必须启用身份验证，以便客户端应用程序中对资源管理器终结点发送的请求可以被接受。 身份验证要求发起[Azure 资源管理器](http://msdn.microsoft.com/library/azure/dn790568.aspx)，它是门户网站相关的所有操作通过 API，包括那些与管理搜索服务请求的基础。 Azure 搜索服务管理 API 是只是一种扩展的 Azure 资源管理器中，并因此会继承其依赖项。  

Azure 的资源管理器需要 Azure Active Directory 服务作为其身份标识提供程序。 

若要获取一个访问令牌，将允许请求访问资源管理器，客户端应用程序包括调用活动目录的代码段。 从这篇文章借用了代码段中，加上使用代码段的必备步骤︰[验证 Azure 资源管理器的请求](http://msdn.microsoft.com/library/azure/dn790557.aspx)。

可以按照上面的链接中的说明，或使用本文档中的步骤，如果您更喜欢通过本教程逐步转。

在本节中，您将执行以下任务︰

1. 创建广告服务
1. 创建 AD 应用程序
1. 通过注册您下载的示例客户端应用程序有关的详细信息配置 AD 应用程序
1. 加载示例客户端应用程序的值将用于获得对其请求的授权

> [AZURE.NOTE] 这些链接提供给资源管理器的客户端请求身份验证使用 Azure Active Directory 背景︰ [Azure 资源管理器](http://msdn.microsoft.com/library/azure/dn790568.aspx)、[验证 Azure 资源管理器请求](http://msdn.microsoft.com/library/azure/dn790557.aspx)和[Azure 活动目录](http://msdn.microsoft.com/library/azure/jj673460.aspx)。

###创建一个 Active Directory 服务

1. 登录到[Azure 的管理门户](https://manage.windowsazure.com)。

2. 沿左侧的导航窗格中向下滚动并单击**活动目录**。

4. 单击**新建**以打开**服务应用程序** | **活动目录**。 在此步骤中，您将创建新的 Active Directory 服务。 此服务将举办广告应用程序，您将定义从现在起的几个步骤。 创建新的服务可帮助隔离从其它应用程序，您可能已经主持在 Azure 教程。

5. 单击**目录** | **自定义创建**。

6. 输入服务名称、 域和地理位置。 域必须是唯一的。 单击复选标记以创建服务。

     ![][5]

###创建一个新的广告应用程序，这项服务

1. 选择您刚创建的"SearchTutorial"Active Directory 服务。

2. 在菜单上，单击**应用程序**。 
 
3. 单击**添加应用程序**。 AD 应用程序存储的客户端应用程序将使用它作为标识提供程序的信息。  
 
4. 选择**添加我的公司正在开发的应用程序**。 此选项提供了应用程序不会发布到应用程序库的注册设置。 由于客户端应用程序不是应用程序库的一部分，这是正确的选择本教程。

     ![][6]
 
5. 输入一个名称，例如"Azure 搜索经理"。

6. 选择的应用程序类型的**本机客户端应用程序**。 这是正确的示例应用程序;它正好是一个 Windows 客户端 （控制台） 应用程序不是 web 应用程序。

     ![][7]
 
7. 在重定向 URI，请输入"http://localhost/Azure-Search-Manager-App"。 这对哪个 Azure Active Directory 的 URI 将重定向响应 OAuth 2.0 授权请求的用户代理。 值不需要物理的终结点，但必须是有效的 URI。 

    出于本教程的目的，值可以是任何内容，但您所输入的任何内容将成为必需的输入示例应用程序中管理连接。 
 
7. 单击复选标记以创建活动目录的应用程序。 在左侧的导航窗格中，您应该看到"Azure 的搜索-管理器-App"。

###配置 AD 应用程序
 
9. 广告应用程序，请单击"Azure 的搜索-管理器-App"，您刚刚创建的。 您应看到它在左侧的导航窗格中列出。

10. 顶部的菜单中，单击**配置**。
 
11. 向下滚动到的权限，然后选择**Azure 管理 API**。 在此步骤中，可以指定 （在此例中，Azure 资源管理器 API） API，客户端应用程序必须具有访问权限的访问级别以及其需要。

12. 在委派权限，请单击下拉列表，然后选择**访问 Azure 服务管理 (预览**)。
 
     ![][8]
 
13. 保存所做的更改。 

保持打开状态的应用程序配置页。 下一步，从此页面复制值，并将示例应用程序中输入这些信息。

###加载示例应用程序的注册和订阅值

在本节中，您将编辑中 Visual Studio，替换从门户中获得的值有效的解决方案。
附近的 Program.cs 顶部显示要添加的值︰

        private const string TenantId = "<your tenant id>";
        private const string ClientId = "<your client id>";
        private const string SubscriptionId = "<your subscription id>";
        private static readonly Uri RedirectUrl = new Uri("<your redirect url>");

如果您还没有[下载示例应用程序从 Codeplex](https://azuresearchmgmtapi.codeplex.com/)，您需要此步骤。

1. 在 Visual Studio 中打开**ManagementAPI.sln** 。

2. 打开 Program.cs。

3. 提供`ClientId`。 从广告应用程序配置页面左侧从上一步中打开、 复制广告应用程序配置页面中的门户中的客户机 ID 并粘贴到 Program.cs。

4. 提供`RedirectUrl`。 将重定向 URI 从同一门户页面中，复制并粘贴到 Program.cs。

    ![][9]

5. 提供 `TenantID.` 
    - 返回到活动目录 |SearchTutorial （服务）。 
    - 从顶栏中单击**应用程序**。 
    - 单击底部的页上**查看终结点**。 
    - 复制 OAUTH 2.0 授权的终结点列表的底部。 
    - 将该终结点粘贴到 TenantID，修整租户 ID 以外的所有 URI 参数的值

    给出"https://login.windows.net/55e324c7-1656-4afe-8dc3-43efcd4ffa50/oauth2/authorize?api-version=1.0"，删除"55e324c7-1656-4afe-8dc3-43efcd4ffa50"之外的全部内容。

    ![][10]

6. 提供`SubscriptionID`。
    - 转到门户主页。
    - 单击底部的左侧的导航窗格中的**设置**。
    - 从订阅选项卡，将复制的订阅 ID 并将其粘贴到 Program.cs。

7. 保存，然后生成该解决方案。


##使用此应用程序

在第一个方法调用中添加断点，以便您可以逐步执行程序。 按**f5 键**以运行该应用程序，然后按**F11**来逐句通过代码。

示例应用程序创建现有 Azure 订阅免费 Azure 搜索的服务。 如果您的订阅已经存在一项免费服务，示例应用程序将失败。 允许每个订阅中只有一个可用的搜索服务。

1. 从解决方案资源管理器中打开 Program.cs，转到 (void 的 string [] Main 函数。 
 
3. 请注意，使用**ExecuteArmRequest**来执行请求对 Azure 资源管理器终结点、`https://management.azure.com/subscriptions`为指定`subscriptionID`。 在整个程序使用此方法使用 Azure 资源管理器 API 或搜索管理 API 来执行操作。

3. 必须进行身份验证和授权请求到 Azure 资源管理器中。 这是使用**GetAuthorizationHeader**方法中，调用**ExecuteArmRequest**方法，从[身份验证 Azure 资源管理器请求](http://msdn.microsoft.com/library/azure/dn790557.aspx)借用的。 请注意，调用**GetAuthorizationHeader** `https://management.core.windows.net`以获取一个访问令牌。

4. 会提示您使用用户名和密码有效的订阅进行登录。

5. 接下来，使用 Azure 资源管理器提供程序注册新的 Azure 搜索服务。 同样，这也是**ExecuteArmRequest**使用的方法，这一次在 Azure 上创建搜索服务，通过订阅`providers/Microsoft.Search/register`。 

6. 该程序的其余部分使用[Azure 搜索管理 REST API](http://msdn.microsoft.com/library/dn832684.aspx)。 请注意，`api-version`此 API 是 Azure 资源管理器 api 版本不同。 例如，`/listAdminKeys?api-version=2014-07-31-Preview`显示了`api-version`的 Azure 搜索管理 REST API。

    下一步的系列操作检索服务定义刚刚创建管理 api 键、 再生和检索键、 副本和分区，更改和最后删除该服务。

    更改服务副本或分区计数时, 预计，此操作将会失败，如果您使用的免费版。 只有标准版可使用的其他分区和复制副本。

    删除服务是最后一道工序。

##下一步行动

在完成本教程后后, 可能想要了解更多有关服务管理或使用 Active Directory 服务进行身份验证︰

- 了解有关使用 Active Directory 集成客户端应用程序。 请参阅[将 Azure 的 Active Directory 中的应用程序集成](http://msdn.microsoft.com/library/azure/dn151122.aspx)。
- 了解如何在 Azure 中其他服务管理操作。 请参阅[管理服务](http://msdn.microsoft.com/library/azure/dn578292.aspx)。

<!--Anchors-->
[下载示例应用程序]: #Download
[配置应用程序]: #config
[使用此应用程序]: #explore
[下一步行动]: #next-steps

<!--Image references-->
[5]: ./media/search-get-started-management-api/Azure-Search-MGMT-AD-Service.PNG
[6]: ./media/search-get-started-management-api/Azure-Search-MGMT-AD-App.PNG
[7]: ./media/search-get-started-management-api/Azure-Search-MGMT-AD-App-prop.PNG
[8]: ./media/search-get-started-management-api/Azure-Search-MGMT-AD-ConfigPermissions.PNG
[9]: ./media/search-get-started-management-api/Azure-Search-MGMT-AD-ConfigPage.PNG
[10]: ./media/search-get-started-management-api/Azure-Search-MGMT-AD-OAuthEndpoint.PNG

<!--Link references-->
[管理解决方案在 Microsoft Azure 中搜索]: search-manage.md
[Azure 搜索开发工作流]: search-workflow.md
[创建第一个 azure 搜索解决方案]: search-create-first-solution.md
[创建一个使用 Azure 搜索的地理空间搜索应用程序]: search-create-geospatial.md


 
---
ms.openlocfilehash: 8c18246f4226f7c20045b443d7e06058fbcfdd12
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="部署并配置一个 SaaS 连接器 API 应用程序" 
    description="了解如何配置安装在 Azure 订购从 Azure 市场 SaaS 连接器。" 
    services="app-service\api" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-api" 
    ms.workload="web" 
    ms.tgt_pltfrm="dotnet" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/06/2015" 
    ms.author="tdykstra"/>

# 部署并配置一个 SaaS 连接器 API 应用程序在 Azure 应用程序服务

## 概述

本教程介绍如何安装、 配置和测试[软件作为-服务 (SaaS) 连接器](../app-service-logic-what-are-bizTalk-api-apps.md) [Azure 应用程序服务](/documentation/services/app-service/)中调用此方法以编程方式，如移动应用程序。 SaaS 连接器是简化与 Office 365、 销售队伍、 Facebook 和收存箱等的 SaaS 平台进行交互的[API 的应用程序](app-service-api-apps-why-best-platform.md)。 如果不使用您想要创建一个自定义的.NET API 应用程序的预组包的连接器，请参阅[连接到 ASP.NET API 的应用程序从一个 SaaS 平台](app-service-api-dotnet-connect-to-saas.md)。 

例如，如果您想要的代码来读取和写入收存箱帐户中文件的 HTTP 请求，收存箱直接使用的身份验证过程是复杂的。 收存箱接头负责身份验证的复杂性，使您可以集中精力编写业务特定代码。

> [AZURE.NOTE] 如果您想要使用 SaaS 连接器从逻辑应用程序，则不需要说明进行操作。 有关如何使用 SaaS 连接器内的逻辑应用程序的信息，请参阅[创建新的逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)和[使用自定义 OAUTH 应用程序连接器](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps&announcementId=4af1e4c5-d220-4457-97d8-d08e427ae6c1)。
 
本教程使用收存箱接头为例，可指导您完成以下步骤︰

* 在 Azure 的订阅中的[资源组](../resource-group-overview.md)中安装的收存箱接头。 
* 配置收存箱接头，以便它可以连接到收存箱服务。 （若要完成此步骤需要收存箱帐户。）
* 配置资源组，以便只有通过身份验证的用户才可以访问 API 的应用程序包含在资源组中。
* 测试以确认用户身份验证并收存箱身份验证工作。

有关在应用程序服务的身份验证的详细信息，请参阅[API 的应用程序和移动应用程序的身份验证](../app-service/app-service-authentication-overview.md)。 

## 安装的收存箱接头

1. 转到[Azure 预览门户]的主页上，单击**市场**。

    ![在 Azure 预览门户市场](./media/app-service-api-connnect-your-app-to-saas-connector/marketplace.png)

2. 搜索下拉框，然后单击**收存箱连接器**图标。

    ![单击收存箱接头](./media/app-service-api-connnect-your-app-to-saas-connector/searchdb.png)
 
3. 单击**创建**。

    ![单击创建](./media/app-service-api-connnect-your-app-to-saas-connector/clickcreate.png)
 
5. 在**收存箱接头**刀片式服务器，**应用程序服务计划**下单击**创建新**的然后在**创建新的应用程序服务计划**中输入 DropBoxPlan。 

    有关应用程序服务计划的详细信息，请参阅[Azure 应用程序服务计划深度探讨](azure-web-sites-web-hosting-plans-in-depth-overview.md)。 

4. 在**资源组**中，单击**新创建**的然后在**创建新的资源组**框中输入 DropboxRG。

    有关资源组的详细信息，请参阅[使用资源组来管理 Azure 的资源](../resource-group-overview.md)。

7. 选择**定价层**的自由。 （如果您看不到它在列表中，单击**查看所有**。 单击**F1 免费**后，单击**选择**按钮。

    您可以使用收费定价层，但是本教程不需要。
 
11. 选择一个附近的**位置**。  

9. <a id="gateway"></a>保留默认值"DropboxConnector"作为连接器，该**名称**，然后单击**创建**。 

    ![单击创建](./media/app-service-api-connnect-your-app-to-saas-connector/createdropbox.png) 

    Azure 应用程序服务创建资源组，然后在资源组中创建收存箱连接器 API 应用程序中和一个*网关*的 web 应用程序。 网关的功能是管理对资源组中的所有 API 应用程序的访问。 

    您可以通过在 Azure 预览门户主页上单击**通知**检查资源创建的进度。

3. 当 Azure 完成创建连接器后时，请单击**浏览 > 资源组 > DropboxRG**。
 
    DropboxRG 为**资源组**刀片式服务器资源组中显示了连接器和网关。

    ![资源组图](./media/app-service-api-connnect-your-app-to-saas-connector/rgdiagram.png) 

## 配置您的收存箱帐户和收存箱接头

要启用到收存箱帐户的 API 访问，您必须收存箱开发人员网站上创建收存箱的应用程序。 然后到您收存箱的连接器，将客户机 ID 和客户端密钥值复制从该收存箱的应用程序和设置连接器接受经过验证的请求。

### <a id="createdbapp"></a>创建收存箱的应用程序

[AZURE.INCLUDE [app-service-api-create-dropbox-app](../../includes/app-service-api-create-dropbox-app.md)]

### <a id="copysettings"></a>复制到 Azure 收存箱接头，反之亦然的收存箱的应用程序设置 

[AZURE.INCLUDE [app-service-api-exchange-dropbox-settings](../../includes/app-service-api-exchange-dropbox-settings.md)]

### 设置收存箱连接器，需要经过身份验证的访问

默认情况下，连接器的**访问级别**设置为**内部**，意味着它只能调用由其他 API 的应用程序和 web 应用程序在同一个资源组中。 但收存箱仅允许经过身份验证的用户访问您收存箱的帐户，因此您必须更改设置，以要求用户身份验证的访问级别。

1. 请返回到**设置**刀片式服务器，并单击**应用程序设置**。

2. 在**应用程序设置**刀片式服务器，**访问级别**设置为**公共 （身份验证）**，，然后单击**保存**。 
    
    ![设置为公共 （身份验证）](./media/app-service-api-connnect-your-app-to-saas-connector/pubauth.png)

您现在已经配置收存箱接头，以便拨出呼叫可以访问您的收存箱帐户，并传入呼叫必须来自已通过身份验证的用户。 下一节中您可以指定要用来验证用户的身份验证提供程序。

## 配置网关

作为解释了[早期](#gateway)，网关可管理的资源组中的所有 API 应用程序访问一个特殊的 web 应用程序。 若要设置网关用户进行身份验证，您选择 Azure Active Directory，Google，如身份验证提供程序或使用 Twitter。 然后，用户将不得不之前他们可以成功调用收存箱接头与选定的提供程序验证身份。

- 若要执行此步骤，转到[配置网关](app-service-api-dotnet-add-authentication.md#configure-the-gateway)[保护 API 应用程序](app-service-api-dotnet-add-authentication.md)教程，明并按照说明进行操作那里 DropboxRG 资源组中配置的网关。

## 通过测试来验证用户和收存箱身份验证

网关配置中您要使用的身份验证提供程序的 DropboxRG 资源组后，您可以测试收存箱接头。

大多数情况下您将通过调用它的代码中，从使用连接器和我们也编写教程将介绍如何做到这一点。 但是，有时您需要验证连接器正在绑定的应用程序的其他部分之前。 本教程展示如何使用一个浏览器和简单的其余部分客户端工具来验证您可以通过收存箱连接器，您只需安装并配置的收存箱服务进行交互。

以下说明演示如何执行这些步骤，使用 Chrome 浏览器开发人员工具和把邮递员弄 REST 客户端工具。 这只是一个示例，并且您可以执行相同的过程与其他浏览器和工具。 "高级 REST 客户端"是另一个 Chrome 外接程序中可以使用。

### 最终用户身份登录

请执行以下步骤在新的浏览器窗口中。 这取决于您正在使用哪种身份验证提供程序，您可能会发现需要使用私有或 incognito 窗口。

2. 转到网关和您配置的身份验证提供程序的登录 URL。 该 URL 遵循以下模式︰ 

        http://[gatewayurl]/login/[providername]

    网关 URL 可以获得在[Azure 预览门户]**网关**刀片式服务器。 （要获得**网关**刀片式服务器，请单击**资源组**刀片上图中的网关。）

    ![网关的 URL](./media/app-service-api-connnect-your-app-to-saas-connector/gatewayurl.png)

    [提供程序名称] 值为"facebook"Facebook、 Twitter 的"twitter"、"aad"Azure Active directory，等等。

    这里是 Azure Active Directory 的示例登录 URL:

        https://dropboxrgaeb4ae60b7cb4f3d966dfa43.azurewebsites.net/login/aad/

3. 浏览器将显示一个登录页面时，请输入您的凭据。 
 
    如果您配置了 Azure Active Directory 登录，登录作为一个[Azure 的门户网站]，例如 admin@contoso.onmicrosoft.com 的 Azure 活动目录选项卡中创建的应用程序的**用户**选项卡中列出的用户。

    登录成功后，您将获得"登录完成"页。

    ![](./media/app-service-api-connnect-your-app-to-saas-connector/logindone.png)

### 为收存箱提供该用户的标识

若要获取使用收存箱 API 的收存箱授权，您必须为收存箱提供用户的凭据。  Azure 将启动该过程，但若要触发该进程必须转到特殊的网关在浏览器中的 URL。  若要获取该 URL 请到网关进行 HTTP Post 请求。

对网关的 HTTP Post 请求都必须包括 Azure 已经在您登录时所提供的身份验证令牌。 对于浏览器请求，包括标记是自动的因为该标记存储在 cookie 中，但可以从 cookie 获取令牌并将其放在 HTTP Post 请求的请求标头中使用其他客户端工具 HTTP Post 请求。

1. 在浏览器窗口中包含"登录完成"消息，请转到浏览器的开发人员工具和查找`x-zumo-auth`cookie。 此窗口保持打开状态以便下一步可复制 cookie 的值。
 
    若要获得在 Chrome 中的 cookie 值，请执行以下步骤︰

    - 按 f12 键以打开开发人员工具。
    - 请转至**资源**选项卡。
    - 为您的网关网站，查找 cookie 和连击的**值**`x-zumo-auth`选择它的所有 cookie。 （请确保您有所有 cookie 的值。 如果双击时，您可能会得到仅第的一部分。)  

    ![复制标记](./media/app-service-api-connnect-your-app-to-saas-connector/copytoken.png) 

4. 在新的浏览器选项卡或窗口中，创建和发送到网关来请求同意 URL 的 HTTP Post 请求。 包括`x-zumo-auth`令牌作为 HTTP 标头。

    该 URL 遵循以下模式︰

        [gatewayurl]/api/consent/list?api-version=2015-01-14&redirecturl=[a URL you want the browser to go to after you authenticate]

    例如︰

        https://dropboxrgaeb4ae60b7cb4f3d966dfa43.azurewebsites.net/api/consent/list?api-version=2015-01-14&redirecturl=https://portal.azure.com

    在 Chrome 中把邮递员弄发送请求，请执行以下步骤︰

    - 输入上面所述的**请求的 URL** 。
    - 设置开机**自检**方法。
    - 添加标题名为`x-zumo-auth`。
    - 标头的**值**字段中粘贴从复制的值`x-zumo-auth`cookie。
    - 单击**发送**。
     
    下面的插图显示在 Chrome 中把邮递员弄工具︰

    ![同意的 url 发送](./media/app-service-api-connnect-your-app-to-saas-connector/sendforconsent.png)

    该响应包括为了启动的收存箱与用户在登录过程中使用的 URL。 （如果您收到指出不受支持的 Get 方法，尽管有方法下拉列表设置为**开机自检**错误响应，请确保网关 URL 是 HTTPS，不是 HTTP。）

    ![同意的 URL](./media/app-service-api-connnect-your-app-to-saas-connector/getconsenturl.png)

2. 转到您收到响应 HTTP Post 请求的 URL。

    响应此 URL 将浏览器重定向到收存箱站点，在其中用户登录并授予应用程序访问的用户帐户的同意。
    
    当登录和同意的情况下完成后时，收存箱将浏览器重定向到您指定的重定向 URL （例如，Azure 预览门户如果遵循此示例，使用 https://portal.azure.com）。 如果您从 web 应用程序中调用，这是下一个页面将显示在 web 应用程序中。  应用程序应检查该 URL，因为如果登录或未经您同意的过程中出现错误，可能包括重定向 URL`error`查询字符串变量。

3. 此浏览器窗口保持打开状态，您将在下一节中使用它。

### 调用 API 的收存箱接头 

Azure 现在正在为您管理三个身份验证令牌︰

- 一个从您的网关，例如 Azure Active Directory 配置的身份验证提供程序。
- 收存箱中的一个。
- 一个 Azure 创建 （"zumo"标记）。

您必须使用发出 HTTP 请求使用收存箱时的 zumo 标记只有一个。 在后台 Azure 有关联的该标记与另两个和连接器提供的其它两个代表您当它调用收存箱。

在下面的步骤来看一看在收存箱帐户文件的收存箱接头进行 Get 请求。 由于 Get 请求，可以使用一个浏览器窗口，因为您的浏览器窗口已经有 zumo 标记，在 cookie 中，所有您需要做是转到的 URL，您会得到收存箱数据。 

1. 在浏览器窗口中具有 Azure 预览门户打开，回到收存箱连接器**应用程序 API**刀片式服务器。 

2. 复制 API 的应用程序的 URL。
 
4. 单击以查看可用连接器中的 API 方法的**API 定义**图标。

    ![刀片式服务器 API 的应用程序](./media/app-service-api-connnect-your-app-to-saas-connector/apiappblade.png) 

    ![API 定义](./media/app-service-api-connnect-your-app-to-saas-connector/apidef.png) 

2. 在网关到身份验证所使用的浏览器窗口中调用 GET 方法检索文件夹中文件的名称。 以下是 URL 模式︰

        [connectorurl]/folder/[foldername]
   
3. 例如，如果您想要查看您的网络日志的文件夹中的文件连接器 URL 是 https://dropboxrg784237ad3e7.azurewebsites.net，URL 将是︰

        https://dropboxrg784237ad3e7.azurewebsites.net/folder/blog

    不同的浏览器响应 API 调用以不同的方式;如果您使用 Chrome 会看到与以下示例类似的响应︰

    ![获取文件夹请求响应](./media/app-service-api-connnect-your-app-to-saas-connector/dbresponse.png) 

## 下一步行动

您已经看到如何安装、 配置和测试 SaaS 连接器。 有关详细信息，请参阅以下资源︰

* [使用连接器](../app-service-logic/app-service-logic-connectors-list.md)
* [API 的应用程序和移动应用程序的身份验证](../app-service/app-service-authentication-overview.md)  

[Azure 预览门户]: https://portal.azure.com/
[Azure 门户]: https://manage.windowsazure.com/
 

测试

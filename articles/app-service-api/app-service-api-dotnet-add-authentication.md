---
ms.openlocfilehash: b4fef3e86a1830ed021a5cdc5b2be99ee561ba52
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="保护 Azure API 的应用程序" 
    description="了解如何使用 Visual Studio 的 Azure API 应用程序保护。" 
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
    ms.date="08/05/2015" 
    ms.author="tdykstra"/>

# 保护应用程序 API︰ 添加 Azure Active Directory 或社会提供程序验证

## 概述

本教程展示如何保护应用程序 API，以便只有通过身份验证的用户才可以访问它。 本教程还显示代码，您可以使用 ASP.NET API 应用程序中检索登录用户的相关信息。

您将执行以下步骤︰

- 调用 API 的应用程序以验证它正常工作。
- 将身份验证规则应用于 API 的应用程序。
- 调用 API 应用程序再次确认它拒绝未经身份验证的请求。
- 登录到已配置的提供程序。
- 调用 API 应用程序再次验证身份验证的访问的工作。
- 编写并测试代码检索登录用户的声明。

有关在 Azure 应用程序服务的身份验证的详细信息，请参阅[API 的应用程序和移动应用程序的身份验证](../app-service/app-service-authentication-overview.md)。

## 先决条件

本教程使用的 API 应用程序[创建 API 应用程序](app-service-dotnet-create-api-app.md)中创建和[部署 API 应用程序](app-service-dotnet-deploy-api-app.md)中部署工作。

## 使用浏览器来调用 API 的应用程序 

若要验证您的 API 应用程序是可公开访问的最简单方法是从浏览器中调用它。

1. 在浏览器中，转到[Azure 预览门户]。

3. 从主页页面中单击**浏览 > API 应用程序**，然后单击您想要保护的 API 的应用程序的名称。

    ![浏览](./media/app-service-api-dotnet-add-authentication/browse.png)

    ![选择的 API 的应用程序](./media/app-service-api-dotnet-add-authentication/select.png)

3. 在**API 应用程序**刀片式服务器，请单击要打开浏览器窗口调用 API 的应用程序的**URL** 。

    ![刀片式服务器 API 的应用程序](./media/app-service-api-dotnet-add-authentication/chooseapiappurl.png)

2. 添加`/api/contacts/get/`到浏览器地址栏中的 URL。

    例如，如果您 API 的应用程序的 URL 是这样︰

        https://microsoft-apiappeeb5bdsasd744e188be7fa26f239bd4b.azurewebsites.net/

    为此的完整 URL:

        https://microsoft-apiappeeb5bdsasd744e188be7fa26f239bd4b.azurewebsites.net/api/contacts/get/

    不同的浏览器以不同的方式处理 API 调用。 从 Chrome 浏览器成功调用图。

    ![铬色 Get 响应](./media/app-service-api-dotnet-add-authentication/chromeget.png)

2. 保存的 URL 使用;以后在本教程中，将使用它。

## 保护 API 的应用程序

当部署 API 的应用程序时，您部署了它给资源组。 可以将 web 应用程序和其他 API 的应用程序添加到同一个资源组，并在资源组中每个 API 的应用程序可以具有三个辅助功能设置之一︰
<!--todo: diagram showing different accessibility settings-->

- **公共 （匿名）** -任何人都可以不被记录在调用该 API 应用程序从资源组外的。
- **公共 （身份验证）** -仅身份验证允许用户调用该 API 应用程序从资源组外的。
- **内部**-仅其他同一资源组 API 应用程序可以调用 API 的应用程序。 （从 web 应用程序的调用被视为外部即使 web 应用程序在同一个资源组。

当 Visual Studio 为您创建的资源组时，它还创建一个*网关*。  网关是处理所有请求发往 API 应用程序中的资源组中的一个特殊的 web 应用程序。

当您转到[Azure 预览门户网站]中的资源组的刀片式服务器时，您可以看到您 API 的应用程序和关系图中的网关。

![资源组图](./media/app-service-api-dotnet-add-authentication/rgdiagram.png)

### <a id="apiapp"></a>配置 API 应用程序要求身份验证

若要配置 API 应用程序接受经过验证的请求，将其可访问性为**Public （身份验证）** ，您将配置为要求身份验证从 Azure Active Directory、 Google 和 Facebook 提供者网关。

[AZURE.INCLUDE [app-service-api-config-auth](../../includes/app-service-api-config-auth.md)]

现在将 API 应用程序防止未经身份验证的访问。 接下来，您必须配置网关，可以指定要使用的身份验证提供程序。

### <a id="gateway"></a>配置网关使用的身份验证提供程序

[AZURE.INCLUDE [app-service-api-gateway-config-auth](../../includes/app-service-api-gateway-config-auth.md)]

## 验证身份验证正常工作

**注意︰**如果必须执行下列步骤时登录时遇到问题，请尝试打开专用或 incognito 窗口。
 
1. 打开一个浏览器窗口，并在地址栏中输入的 URL 调用 API 应用程序`Get`方法，因为您未前面。

    这一次尝试访问 API 的应用程序会导致出现错误信息。

    ![铬 Get 响应失效](./media/app-service-api-dotnet-add-authentication/chromegetfail.png)

2. 在浏览器中，转到登录 URL。 该 URL 遵循以下模式︰ 

        http://[gatewayurl]/login/[providername]

    网关 URL 可以获得在[Azure 预览门户]**网关**刀片式服务器。 （要获得**网关**刀片式服务器，请单击**资源组**刀片上图中的网关。）

    ![网关的 URL](./media/app-service-api-dotnet-add-authentication/gatewayurl.png)

    [提供程序名称] 必须是以下值之一︰
    
    * ""microsoftaccount
    * ""facebook
    * ""twitter
    * ""google
    * ""aad

    这里是 Azure Active Directory 的示例登录 URL:

        https://dropboxrgaeb4ae60b7cb4f3d966dfa43.azurewebsites.net/login/aad/

    请注意，与前面的 URL，此一不包括您的 API 的应用程序名称︰ 网关验证您没有 API 的应用程序。  网关处理资源组中的所有 API 应用程序的身份的验证。

3. 浏览器将显示一个登录页面时，请输入您的凭据。 
 
    如果您配置了 Azure Active Directory 登录，则使用[Azure 的门户网站]，例如 admin@contoso.onmicrosoft.com 的 Azure 活动目录选项卡中创建的应用程序的**用户**选项卡中列出的用户之一。

    ![AAD 的用户](./media/app-service-api-dotnet-add-authentication/aadusers.png)

    ![登录页](./media/app-service-api-dotnet-add-authentication/ffsignin.png)

4. 显示"登录完成"消息时，将 URL 输入到 API 应用程序获取方法再次。

    这一次因为您已经通过身份验证，则该调用不成功。 您是经过身份验证的用户，并将您的请求传递给 API 的应用程序，认识到网关。

    ![完成登录](./media/app-service-api-dotnet-add-authentication/logincomplete.png)

    ![铬色 Get 响应](./media/app-service-api-dotnet-add-authentication/chromeget.png)

    如果您已启用了 Swagger 用户界面，还可以到 Swagger 用户界面页现在。 但是，您将看到右下角的页，在一个红色**错误**图标，并且如果单击该图标，您将看到 Swagger 的 JSON 文件是不可访问的消息。 这是因为 Swagger 调用 AJAX 而不包括 Zumo 令牌，尝试检索的 JSON 文件。 这不防止 Swagger 用户界面页工作。

## 用于发送 Post 请求把邮递员弄

当您登录到网关时，网关发送回一个身份验证标记。  此标记必须包括与从外部源，通过网关的所有请求。 当您访问使用浏览器的 API 时，浏览器将通常存储在 cookie 中的标记，并将其发送以及所有后续调用 API。

这样就可以看到在后台发生了什么情况，本教程的这一部分中使用浏览器工具来创建和提交 Post 请求，并获得授权令牌从 cookie 并将其包含在 HTTP 标头。 此节是可选的︰ 在上一节已验证 API 的应用程序只接受经过身份验证的访问。

这些过程说明了如何把邮递员弄工具使用 Chrome 浏览器，但您无法执行相同的操作与任何其他客户端工具和浏览器的开发工具。

1. 在 Chrome 浏览器窗口中，显示上一节中进行身份验证，逐步转到，然后打开开发者工具 (F12)。

    ![转到资源选项卡](./media/app-service-api-dotnet-add-authentication/resources.png)

3. Chrome 开发工具的**资源**选项卡中找到您的网关，cookie 并连击**x-zumo-身份验证**cookie 的值来选择它的所有。

    **注意︰** 请确保您有所有 cookie 的值。 如果双击时，您将获得只有它的第一个部分。

5. **值**的**x-zumo-身份验证**cookie 中，用鼠标右键单击，然后单击**复制**。

    ![复制身份验证令牌](./media/app-service-api-dotnet-add-authentication/copyzumotoken.png)

4. 如果您还没有这样操作，把邮递员弄扩展安装 Chrome 浏览器中。

6. 打开把邮递员弄扩展。

7. 在请求 URL 字段中，输入的 URL 以前，使用的 API 应用程序获取方法，但请省略`get/`从结尾。
 
        http://[apiappurl]/api/contacts
    
8. 请单击**邮件头**，然后再添加一个*x-zumo-身份验证*标头。 将标记值从剪贴板粘贴到**值**字段。

9. 添加值*应用程序/json*与一个*内容类型*标头。

10. 单击**窗体数据**，然后添加一个*与*下面的值︰

        {   "Id": 0,   "Name": "Li Yan",   "EmailAddress": "yan@contoso.com" }

11. 单击发送。

    API 的应用程序返回*201 已创建*响应。

    ![添加页眉和正文](./media/app-service-api-dotnet-add-authentication/addcontact.png)

12. 要验证该请求者的身份验证令牌没有起作用，请删除身份验证标头，并再次单击发送。

    获得*403*响应。

    ![403 禁止访问响应](./media/app-service-api-dotnet-add-authentication/403forbidden.png)

## 获取有关登录用户的信息

在这一节中您更改 ContactsList API 应用程序中的代码，以便检索并返回登录的用户的姓名和电子邮件地址。  

1. 在 Visual Studio 中，在[部署 API 应用程序](app-service-dotnet-deploy-api-app.md)中打开 API 的应用程序项目部署，并呼吁本教程。

3. 打开 apiapp.json 文件，并添加行，指示该 API 应用程序使用 Azure Active Directory 身份验证。

        "authentication": [{"type": "aad"}]

    最终的 apiapp.json 文件将类似于下面的示例︰

        {
            "$schema": "http://json-schema.org/schemas/2014-11-01/apiapp.json#",
            "id": "ContactsList",
            "namespace": "microsoft.com",
            "gateway": "2015-01-14",
            "version": "1.0.0",
            "title": "ContactsList",
            "summary": "",
            "author": "",
            "endpoints": {
                "apiDefinition": "/swagger/docs/v1",
                "status": null
            },
            "authentication": [{"type": "aad"}]
        }

    本教程使用 Azure Active Directory 为例。 对于其他提供程序，您需要使用相应的标识符。 下面是有效的提供程序的值︰

    * ""aad
    * ""microsoftaccount
    * ""google
    * ""twitter
    * "facebook"。 

3. 在*ContactsController.cs*文件中，添加`using`语句在文件的顶部。

        using Microsoft.Azure.AppService.ApiApps.Service;

2. 中的代码替换`Get`用下面的代码的方法。

        var runtime = Runtime.FromAppSettings(Request);
        var user = runtime.CurrentUser;
        TokenResult token = await user.GetRawTokenAsync("aad");
        var name = (string)token.Claims["name"];
        var email = (string)token.Claims["http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn"];
        return new Contact[]
        {
            new Contact { Id = 1, EmailAddress = email, Name = name }
        };

    而不是三个示例联系人，代码也会返回已登录用户的联系信息。 

    该代码示例使用 Azure 活动目录。 对于其他提供程序将使用的适当标记的名称和声明标识符上, 一步中所示。

    Azure Active Directory 索赔所提供的信息，请参阅[支持令牌和声明类型](https://msdn.microsoft.com/library/dn195587.aspx)。

3. 添加一条 using 语句`Microsoft.Azure.AppService.ApiApps.Service`。

        using Microsoft.Azure.AppService.ApiApps.Service;

3. 重新部署项目。  

    Visual Studio 将记住从后面[部署](app-service-dotnet-deploy-api-app.md)教程同时部署项目时的设置。  用鼠标右键单击该项目，单击**发布**，然后单击**发布网站**对话框中的**发布**。

6. 按照您以前未向受保护的 API 应用程序发送一个 Get 请求的过程。 

    响应消息显示的名称和标识用来登录的 ID。

    ![使用登录用户的响应消息](./media/app-service-api-dotnet-add-authentication/chromegetuserinfo.png)

## 下一步行动

您已经看到如何通过要求 Azure Active Directory 或社会提供身份验证来保护 Azure API 的应用程序。 有关详细信息，请参阅[API 的应用程序和移动应用程序的身份验证](../app-service/app-service-authentication-overview.md)。 

[Azure 门户]: https://manage.windowsazure.com/
[Azure 预览门户]: https://portal.azure.com/

测试

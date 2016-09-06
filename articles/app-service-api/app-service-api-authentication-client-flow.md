---
ms.openlocfilehash: a441c866ac080afb5799260811ddd8cd967ce728
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="从已经过身份验证的客户端调用 API 的应用程序" 
    description="了解如何从 web 应用程序客户端通过 Azure Active Directory 验证调用 Azure API 的应用程序。" 
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
    ms.date="07/26/2015" 
    ms.author="tdykstra"/>

# 从 Azure Active Directory 验证 web 应用程序客户端调用 Azure API 的应用程序

## 概述

本教程展示如何调用 API 应用程序是从 web 应用程序，还受 AAD 保护通过 Azure 活动目录 (AAD)。 完成本教程后，您将有一个 web 应用程序和 API 应用程序运行在 Azure 订购。  使 web 应用程序将显示下面的屏幕快照中所示通过调用 API 的应用程序，它获取的数据。

![](./media/app-service-api-authentication-client-flow/aboutpage.png)

您将学习如何将 AAD 凭据上从 web 应用程序 API 的应用程序，以便不提示用户再次通过 API 的应用程序日志。

您将执行以下步骤︰

- 创建 API 的应用程序并将其配置为使用 AAD 身份验证。
- 创建 web 应用程序并将其配置为使用 AAD 身份验证。
- 将代码添加到 web 应用程序调用受保护的 API 应用程序中。
- 将 web 应用程序和 API 的应用程序部署到 Azure。
- 若要验证 web 应用程序可以调用 API 的应用程序的测试。

### 客户端流量身份验证

您将添加到 web 应用程序的代码将实施一个称为[客户端流量](../app-service/app-service-authentication-overview.md#client-flow)身份验证过程。 下面的关系图说明了获得 AAD 访问令牌和交换它的 API 的应用程序 (Zumo) 标记的过程。

![](./media/app-service-api-authentication-client-flow/clientflow.png)

如果你不熟悉的角色在身份验证中用于 API 的应用程序的 API 的应用程序网关，请参阅[API 的应用程序和移动应用程序的身份验证](../app-service/app-service-authentication-overview.md)。

## 先决条件

在开始学习本教程之前, 确保您具有[Visual Studio 2015年](https://www.visualstudio.com/)和[.NET 的 Azure SDK](http://go.microsoft.com/fwlink/?linkid=518003)安装。 相同的说明也适用于 Visual Studio 2013年。

本教程假设您知道如何使用 Visual Studio 中的 web 项目。

## 创建并使用 AAD 的 API 应用程序安全

1. 创建或下载 API 的应用程序项目。
 
    * 若要创建项目，请按照[创建 API 应用程序](app-service-dotnet-create-api-app.md)中的说明进行操作。

    * 下载的项目，请从[ContactsList GitHub 存储库](https://github.com/tdykstra/ContactsList)获得。

    您现在拥有 Web API 项目返回联系人数据。

    ![](./media/app-service-api-authentication-client-flow/swaggerlocal.png)

2. API 的应用程序项目部署到 Azure 中新 API 应用程序。

    按照[部署 API 应用程序](app-service-dotnet-deploy-api-app.md)中的说明进行操作。

    在[Azure 预览门户]，API 应用程序**API 定义**刀片式服务器现在显示您部署的 Web API 项目的 Get 和 Post 方法。

    ![](./media/app-service-api-authentication-client-flow/apiappinportal.png)

4. 为身份标识提供程序使用 Azure 活动目录 (AAD) 的安全 API 的应用程序。
 
    按照 AAD[保护 API 应用程序](app-service-api-dotnet-add-authentication.md)中的说明进行操作。 您不必执行本指南的**使用把邮递员弄来发送 Post 请求**部分。

    当您完成时， [Azure 预览门户]中的 API 的应用程序的**Azure Active Directory**刀片式服务器都有 AAD 应用程序的客户机 ID 和 AAD 租户。

    ![](./media/app-service-api-authentication-client-flow/aadblade.png)

    并且在[Azure 门户]AAD 应用程序的**配置**选项卡 API 的应用程序的应用程序 ID 的 URL 和回复 URL。

    ![](./media/app-service-api-authentication-client-flow/aadconfig.png)

## 创建并使用 AAD 一个 web 应用程序安全

这一节中下载和配置 AAD 身份验证为设置一个 web 项目。 该项目具有**关于**页面登录的用户的显示索赔信息。

![](./media/app-service-api-authentication-client-flow/aboutpagestart.png)

您将通过添加一节用于显示从该 API 应用程序中检索联系信息以后修改此页。

1. 从[WebApp-GroupClaims-最低存储库](https://github.com/AzureADSamples/WebApp-GroupClaims-DotNet/)下载的 web 项目
 
2. 请按照说明**如何运行该示例**[自述文件](https://github.com/AzureADSamples/WebApp-GroupClaims-DotNet/blob/master/README.md)，以下情况除外︰
 
    * 您可以使用 Visual Studio 2015年尽管自述文件说明要使用 Visual Studio 2013年。 

    * 使用已创建的 AAD 应用程序而不是创建一个新。
 
    * 保持相同的**应用程序 ID URI**已有 AAD 应用程序。 （不更改它的自述文件中指定的格式。 

在保持相同的应用程序 ID URI，这样就可以使用相同的 AAD 访问创建 API 应用程序标记为 web 应用程序和 API 的应用程序。 如果应用程序 ID URI 改由自述文件预定义的格式，，将能访问 web 应用程序而不是 API 的应用程序。  您不能将 AAD 令牌传递给 API 的应用程序网关来获取 Zumo 标记，因为网关应用程序 ID URI 由组成的网关 URL 需要一个标记以及"/ 登录/aad"。   

## 将生成的客户端 API 应用程序的代码添加

在本节中，您将添加类型化接口的调用 API 的应用程序自动生成的代码。 

8.  在 Visual studio**解决方案资源管理器**中，右击 WebApp-GroupClaims-最低项目，然后单击**添加 > Azure API 应用程序客户端**。

9.  在**添加 Microsoft Azure API 应用程序客户端**对话框中，选择 AAD 的受保护 API 应用程序。

    代码生成完成后，您将看到一个新的文件夹，可在**解决方案资源管理器**中使用 API 的应用程序的名称。 此文件夹包含实现的客户端类和数据模型的代码。 

    ![](./media/app-service-api-authentication-client-flow/aboutpagestart.png)

10. 解决不明确的引用由生成的代码在*ContactsList/ContactsExtensions.cs*︰ 更改两个`Task.Factory.StartNew`到`System.Threading.Tasks.Task.Factory.StartNew`。
 
## 添加代码以交换 Zumo 标记的 AAD 标记

1.  在 HomeController.cs，添加`using`应用程序服务 SDK 和 JSON 序列化程序的语句。

        using Microsoft.Azure.AppService;
        using Newtonsoft.Json.Linq;

2. 将常量添加网关 URL 以及 AAD 应用程序的应用程序 ID URI。 请确保该网关 URL https，不是 http。

        private const string GATEWAY_URL = "https://clientflowgroupaeb4ae60b7cb4f3d966dfa43b6607f30.azurewebsites.net/";
        private const string APP_ID_URI = GATEWAY_URL + "login/aad";

2.  添加一个方法来获取客户端对象调用 API 的应用程序。

        public async Task<AppServiceClient> GetAppServiceClient()
        {
            var appServiceClient = new AppServiceClient(GATEWAY_URL);
            string userObjectID = ClaimsPrincipal.Current.FindFirst
                ("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
        
            var authContext = new AuthenticationContext
                (ConfigHelper.Authority, new TokenDbCache(userObjectID));
        
            ClientCredential credential = new ClientCredential
                (ConfigHelper.ClientId, ConfigHelper.AppKey);
        
            // Get the AAD token.
            AuthenticationResult result = authContext.AcquireToken(APP_ID_URI, credential);
            var aadToken = new JObject();
            aadToken["access_token"] = result.AccessToken;
        
            // Send the AAD token to the gateway and get a Zumo token
            var appServiceUser = await appServiceClient.LoginAsync
                ("aad", aadToken).ConfigureAwait(false);
        
            return appServiceClient;
        }

    在此代码中`ConfigHelper.Authority`可解析为"https://login.microsoftonline.com/ {您租户}"，例如:"https://login.microsoftonline.com/contoso.onmicrosoft.com"。 

2.  之前立即添加代码`return View()`语句末尾处`About`方法可调用 API 的应用程序。 (在下面的步骤中，您将添加到代码`About`视图以显示返回的数据。)

        var appServiceClient = await GetAppServiceClient();
        var client = appServiceClient.CreateContactsList();
        var contacts = client.Contacts.Get();
        ViewData["contacts"] = contacts;

3. 在*Views/Home/About.cshtml*中，添加代码后立即`h2`标题可显示的联系人信息。

        <h3>Contacts</h3>
        <table class="table table-striped table-bordered table-condensed table-hover">
            <tr>
                <th>Name</th>
                <th>Email</th>
            </tr>
        
            @foreach (WebAppGroupClaimsDotNet.Models.Contact contact in (IList<WebAppGroupClaimsDotNet.Models.Contact>)ViewData["contacts"])
            {
                <tr>
                    <td>@contact.Name</td>
                    <td>@contact.EmailAddress</td>
                </tr>
            }
        
        </table>


3. 在应用程序 Web.config 文件中，添加下面的绑定重定向之后打开`<assemblyBinding>`标记。

        <dependentAssembly>
          <assemblyIdentity name="System.Net.Http.Primitives" publicKeyToken="b03f5f7f11d50a3a" culture="neutral"/>
          <bindingRedirect oldVersion="0.0.0.0-4.2.28.0" newVersion="4.2.28.0"/>
        </dependentAssembly>

    此绑定重定向，而该应用程序将失败，因为应用程序服务 SDK 包括 System.Net.Http.Primitives 1.5.0.0 版本的依赖，但项目所使用的版本是 4.2.28.0。
 
3. 按照**部署到 Azure 此示例**[自述文件](https://github.com/AzureADSamples/WebApp-GroupClaims-DotNet/)中的说明进行操作。

5. 运行应用程序。

    ![](./media/app-service-api-authentication-client-flow/homepage.png)

6. **登录**，请单击，然后输入您为此应用程序使用 AAD 域中的用户的凭据。

    ![](./media/app-service-api-authentication-client-flow/signedin.png)

7. 单击**关于**。

    代码添加到的关于控制器和视图运行，并显示您成功身份验证到 API 的应用程序，并调用其 Get 方法。

    ![](./media/app-service-api-authentication-client-flow/aboutpage.png)

## 疑难解答

### HTTP 400 错误 

请确保该网关 url https，不是 http。  在复制时直接从 Azure 预览门户网站，它可以指定 http。
 
### SQL Server 错误

应用程序需要访问 SQL Server 数据库由 GroupClaimContext 连接字符串。 请确保已部署站点中的连接字符串指向 SQL 数据库实例。  在已部署的 Web.config 或 Azure 的运行时配置设置，您可以将正确的连接字符串。

## 确认

Govind s。 Yadav ([@govindsyadav](https://twitter.com/govindsyadav)) 为感谢帮助开发本教程。 

## 下一步行动

您已经看到如何执行客户端流用于应用程序服务 API 的应用程序的身份验证。 有关处理 API 应用程序中的身份验证的其他方法的信息，请参阅[API 的应用程序和移动应用程序的身份验证](../app-service/app-service-authentication-overview.md)。 

[Azure 门户]: https://manage.windowsazure.com/
[Azure 预览门户]: https://portal.azure.com/

测试

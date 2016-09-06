---
ms.openlocfilehash: 9d5fb63e331672d5abf8eca8b9a79e535ae73432
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="应用程序模型 v2.0 |Microsoft Azure"
    description="如何生成.NET MVC Web 应用程序的调用 web 服务使用个人 Microsoft 客户和工作或学校的帐户登录。"
    services="active-directory"
    documentationCenter=".net"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/12/2015"
    ms.author="dastrock"/>

# 应用程序模型 v2.0 预览︰ 从.NET web 应用程序调用 web API

> [AZURE.NOTE]
    此信息适用于 v2.0 终结点公共预览。  有关说明如何与普遍适用的 Azure AD 集成服务，请参考[Azure 活动目录开发指南 》](active-directory-developers-guide.md)。

使用 2.0 版应用程序模型中，可以快速添加到 web 应用程序的身份验证和 web Api 对两个人的 Microsoft 客户支持和工作或学校的帐户。  在这里，我们将所构建的 MVC web 应用程序︰

- 有迹象用户使用 OpenID 连接，请从 Microsoft OWIN 中间件的一些帮助。
- 使用 ADAL 的 web API 获取 OAuth 2.0 的访问令牌。
- 创建，读取，并删除用户的"待办事项列表，这些位于 web 上的项目 api 和受到 OAuth 2.0。

本教程将主要致力于获得并使用的访问令牌中完全[此处](active-directory-v2-flows.md#web-apps)所述的 web 应用程序中。  作为系统必备组件，您可能需要首先了解如何[添加基本登录到 web 应用程序](active-directory-v2-devquickstarts-dotnet-web.md)或如何[适当保护 web API](active-directory-v2-devquickstarts-dotnet-api.md)。

若要从客户端调用待办事项列表 Web API 的基本步骤如下︰

1. 注册应用程序
2. 登录到 web 应用程序使用 OpenID 连接的用户
3. 使用 ADAL 来获取访问令牌在用户登录时
4. 调用待办事项列表 Web API 加装了一个访问令牌。

本教程中的代码[在 GitHub 上](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet)维护。  

要继续下去，您可以[下载应用程序的主干作为.zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/skeleton.zip)或克隆主干︰

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet.git```

或者，您可以[下载已完成的应用程序作为.zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip)或克隆已完成的应用程序︰

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet.git```

## 1.注册应用程序
在[apps.dev.microsoft.com](https://apps.dev.microsoft.com)，创建新的应用程序，或按照以下[详细的步骤](active-directory-v2-app-registration.md)。  请确保︰

- 复制下分配给您的应用程序的**应用程序 Id** ，您需要很快。
- 创建的**密码**类型，**应用程序的机密**和稍后复制下来其值
- 添加您的应用程序的**Web**平台。
- 请输入正确的**重定向 URI**。 此重定向 uri 指示到 Azure 广告位置应该将身份验证响应重定向-本教程中的默认值是`https://localhost:44326/`。


## 2.签署使用 OpenID 连接的用户
在这里，我们将配置为使用[OpenID 连接的身份验证协议](active-directory-v2-protocols.md#openid-connect-sign-in-flow)OWIN 中间件。  OWIN 将用于颁发登录和注销请求，管理用户的会话并获得用户有关的信息，在其他事情。

-   若要开始，请打开`web.config`文件的根目录中的`TodoList-WebApp`项目，然后输入您的应用程序中的配置值`<appSettings>`部分。
    -   `ida:ClientId`是分配给您的应用程序中注册门户的**应用程序 Id** 。
    - `ida:ClientSecret`注册门户中创建的**应用程序的机密**。
    -   `ida:RedirectUri`是在门户中输入的**重定向 Uri** 。
- 打开`web.config`文件的根目录中的`TodoList-Service`项目，和替换`ida:Audience`与上述相同的**应用程序 Id** 。


-   现在，添加到 OWIN 中间件 NuGet 程序包`TodoList-WebApp`使用程序包管理器控制台项目。

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect -ProjectName TodoList-WebApp
PM> Install-Package Microsoft.Owin.Security.Cookies -ProjectName TodoList-WebApp
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoList-WebApp
```

-   打开的文件`App_Start\Startup.Auth.cs`并添加`using`以上的库的语句。
- 在同一文件中，实现`ConfigureAuth(...)`方法。  您在中提供的参数`OpenIDConnectAuthenticationOptions`将作为您的应用程序坐标与 Azure 广告进行通信。

```C#
public void ConfigureAuth(IAppBuilder app)
{
    app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

    app.UseCookieAuthentication(new CookieAuthenticationOptions());

    app.UseOpenIdConnectAuthentication(
        new OpenIdConnectAuthenticationOptions
        {

                    // The `Authority` represents the v2.0 endpoint - https://login.microsoftonline.com/common/v2.0
                    // The `Scope` describes the permissions that your app will need.  See https://azure.microsoft.com/documentation/articles/active-directory-v2-scopes/
                    // In a real application you could use issuer validation for additional checks, like making sure the user's organization has signed up for your app, for instance.

                    ClientId = clientId,
                    Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, "common", "/v2.0"),
                    Scope = "openid offline_access",
                    RedirectUri = redirectUri,
                    PostLogoutRedirectUri = redirectUri,
                    TokenValidationParameters = new TokenValidationParameters
                    {
                        ValidateIssuer = false,
                    },

                    // The `AuthorizationCodeReceived` notification is used to capture and redeem the authorization_code that the v2.0 endpoint returns to your app.

                    Notifications = new OpenIdConnectAuthenticationNotifications
                    {
                        AuthenticationFailed = OnAuthenticationFailed,
                        AuthorizationCodeReceived = OnAuthorizationCodeReceived,
                    }

        });
}
...
```

## 3.使用 ADAL 来获取访问令牌在用户登录时
在`AuthorizationCodeReceived`通知，我们想要使用[OpenID 连接一同 OAuth 2.0](active-directory-v2-protocols.md#openid-connect-with-oauth-code-flow)兑换访问标记为待办事项列表服务的授权。  ADAL 可以简化此过程为您︰

- 首先，安装 ADAL 的预览版本︰

```PM> Install-Package Microsoft.Experimental.IdentityModel.Clients.ActiveDirectory -ProjectName TodoList-WebApp -IncludePrerelease```
- 并添加另一个`using`语句`App_Start\Startup.Auth.cs`ADAL 的文件。
- 现在，添加新的方法，`OnAuthorizationCodeReceived`事件处理程序。  此处理程序将使用 ADAL 来获取访问标记为待办事项列表 API，并将标记 ADAL 的标记在缓存中存储的后面︰

```C#
private async Task OnAuthorizationCodeReceived(AuthorizationCodeReceivedNotification notification)
{
        string userObjectId = notification.AuthenticationTicket.Identity.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
        string tenantID = notification.AuthenticationTicket.Identity.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;
        string authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenantID, string.Empty);
        ClientCredential cred = new ClientCredential(clientId, clientSecret);

        // Here you ask for a token using the web app's clientId as the scope, since the web app and service share the same clientId.
        var authContext = new Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext(authority, new NaiveSessionCache(userObjectId));
        var authResult = await authContext.AcquireTokenByAuthorizationCodeAsync(notification.Code, new Uri(redirectUri), cred, new string[] { clientId });
}
...
```

- 在 web 应用程序，ADAL 具有可扩展的令牌缓存，用于存储标记。  此示例实现`NaiveSessionCache`它使用 http 会话存储到缓存标记。

<!-- TODO: Token Cache article -->


## 4.调用待办事项列表 Web API
现在是时间来实际使用您在步骤 3 中获得 access_token。  打开 web 应用程序的`Controllers\TodoListController.cs`文件，这使得所有 CRUD 请求到待办事项列表 API。

- 您可以使用 ADAL 再次这里从 ADAL 缓存中提取 access_tokens。  首先，添加`using`ADAL 语句到此文件。

    `using Microsoft.Experimental.IdentityModel.Clients.ActiveDirectory;`

- 在`Index`操作，使用 ADAL`AcquireTokenSilentAsync`方法来获取 access_token 可以用来读取数据，从待办事项列表服务︰

```C#
...
string userObjectID = ClaimsPrincipal.Current.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
string tenantID = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;
string authority = String.Format(CultureInfo.InvariantCulture, Startup.aadInstance, tenantID, string.Empty);
ClientCredential credential = new ClientCredential(Startup.clientId, Startup.clientSecret);

// Here you ask for a token using the web app's clientId as the scope, since the web app and service share the same clientId.
AuthenticationContext authContext = new AuthenticationContext(authority, new NaiveSessionCache(userObjectID));
result = await authContext.AcquireTokenSilentAsync(new string[] { Startup.clientId }, credential, UserIdentifier.AnyUser);
...
```

- 该示例然后将生成的令牌添加到 HTTP GET 请求`Authorization`标头，将待办事项列表服务用于对请求进行身份验证。
- 如果待办事项列表服务返回`401 Unauthorized`响应，在 ADAL access_tokens 已经无效原因。  在这种情况下，应从 ADAL 缓存丢弃任何 access_tokens，并向用户显示一条消息，他们可能需要再次登录，这将重新启动标记购置流。

```C#
...
// If the call failed with access denied, then drop the current access token from the cache,
// and show the user an error indicating they might need to sign-in again.
if (response.StatusCode == System.Net.HttpStatusCode.Unauthorized)
{
        var todoTokens = authContext.TokenCache.ReadItems().Where(a => a.Scope.Contains(Startup.clientId));
        foreach (TokenCacheItem tci in todoTokens)
                authContext.TokenCache.DeleteItem(tci);

        return new RedirectResult("/Error?message=Error: " + response.ReasonPhrase + " You might need to sign in again.");
}
...
```

- 同样，如果 ADAL 不能出于任何原因返回 access_token，则应要求用户重新登录。  这很简单，只捕捉任何`AdalException`:

```C#
...
catch (AdalException ee)
{
        // If ADAL could not get a token silently, show the user an error indicating they might need to sign in again.
        return new RedirectResult("/Error?message=An Error Occurred Reading To Do List: " + ee.Message + " You might need to log out and log back in.");
}
...
```

- 精确的相同`AcquireTokenSilentAsync`调用是在 implementd `Create` ，`Delete`操作。  在 web 应用程序，可以使用此 ADAL 方法来获取 access_tokens，您需要在您的应用程序。  ADAL 将负责获取、 缓存和刷新令牌。

最后，生成并运行您的应用程序 ！  使用 Microsoft 帐户或 Azure AD 帐户，登录，并注意在顶部导航栏中如何反映用户的身份。  添加和删除一些项目，从用户的待办事项列表以查看 OAuth 2.0 安全操作的 API 调用。  现在，您可以同时使用行业标准协议，可以对其个人和工作/学校这两个帐户的用户进行身份验证保护 web 应用程序和 web API。

为了便于参考，[此处提供了](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip)完整的示例 （无需您配置的值）。  

## 下一步行动

更多的资源，请查阅︰
- [应用程序模型 v2.0 预览 >>](active-directory-appmodel-v2-overview.md)
- [StackOverflow"adal"标记 >>](http://stackoverflow.com/questions/tagged/adal)

测试

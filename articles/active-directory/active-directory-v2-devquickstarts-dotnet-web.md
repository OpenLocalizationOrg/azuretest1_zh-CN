---
ms.openlocfilehash: 7f01c88fa50dcd003252ff4e219e0f0a8e2b92f4
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="应用程序模型 v2.0 |Microsoft Azure"
    description="如何构建.NET MVC Web 应用程序进行签名以两个人的 Microsoft 帐户的用户和工作或学校的帐户。"
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

# 应用程序模型 v2.0 预览︰ 登录到.NET MVC web 应用程序中添加

使用 2.0 版应用程序模型中，可以快速添加到这两个人的 Microsoft 客户支持的 web 应用程序的身份验证和工作或学校的科目。  在 ASP.NET web 应用程序，您可以完成此操作使用 Microsoft 的.NET Framework 4.5 中包含的 OWIN 中间件。

  > [AZURE.NOTE]
    此信息适用于 2.0 版应用程序模型的公共预览。  有关说明如何与普遍适用的 Azure AD 集成服务，请参考[Azure 活动目录开发指南 》](active-directory-developers-guide.md)。

 这里我们将使用 OWIN 为︰
-   用户登录应用程序使用 Azure AD，2.0 版应用程序模型。
-   显示有关该用户的某些信息。
-   对用户在该应用程序进行签名。

为此，您需要︰

1. 注册应用程序。
2. 将您的应用程序设置为使用 OWIN 身份验证管道。
3. 使用 OWIN 到 Azure AD 颁发登录和注销请求。
4. 打印出与用户有关的数据。

本教程中的代码[在 GitHub 上](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet)维护。  要继续下去，您可以[下载应用程序的主干作为.zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/skeleton.zip)或克隆主干︰

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet.git```

同时本教程末尾提供了已完成的应用程序。

## 1.注册应用程序
在[apps.dev.microsoft.com](https://apps.dev.microsoft.com)，创建新的应用程序，或按照以下[详细的步骤](active-directory-v2-app-registration.md)。  请确保︰

- 复制下分配给您的应用程序的**应用程序 Id** ，您需要很快。
- 添加您的应用程序的**Web**平台。
- 请输入正确的**重定向 URI**。 此重定向 uri 指示到 Azure 广告位置应该将身份验证响应重定向-本教程中的默认值是`https://localhost:44326/`。

## 2.设置了您的应用程序使用 OWIN 身份验证管道
在这里，我们将配置为使用 OpenID 连接身份验证协议 OWIN 中间件。  OWIN 将用于颁发登录和注销请求，管理用户的会话并获得用户有关的信息，在其他事情。

-   若要开始，请打开`web.config`项目的根目录中的文件，然后输入您的应用程序中的配置值`<appSettings>`部分。
    -   `ida:ClientId`是分配给您的应用程序中注册门户的**应用程序 Id** 。
    -   `ida:RedirectUri`是在门户中输入的**重定向 Uri** 。

-   接下来，使用程序包管理器控制台向项目中添加 OWIN 中间件 NuGet 程序包。

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
PM> Install-Package Microsoft.Owin.Security.Cookies
PM> Install-Package Microsoft.Owin.Host.SystemWeb
```

-   添加"OWIN 启动类"项目到称为`Startup.cs`右项目单击-->**添加** --> **新项**--> 搜索"OWIN"。  OWIN 中间件将调用`Configuration(...)`在您的应用程序启动时的方法。
-   更改的类声明为`public partial class Startup`-已实施了此类的一部分为您在另一个文件中。  在`Configuration(...)`方法，生成一个 ConfigureAuth(...) 调用以设置您的 web 应用程序的身份验证  

```C#
public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

-   打开的文件`App_Start\Startup.Auth.cs`并实施`ConfigureAuth(...)`方法。  您在中提供的参数`OpenIdConnectAuthenticationOptions`将作为您的应用程序坐标与 Azure 广告进行通信。  您还需要设置 Cookie 身份验证-OpenID 连接中间件使用这背后的 cookie。

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
                                     RedirectUri = redirectUri,
                                     Scope = "openid",
                                     PostLogoutRedirectUri = redirectUri,
                                     TokenValidationParameters = new TokenValidationParameters
                                     {
                                             ValidateIssuer = false,
                                     },
                                     Notifications = new OpenIdConnectAuthenticationNotifications
                                     {
                                             AuthenticationFailed = OnAuthenticationFailed,
                                     }
                             });
             }
```

## 3.使用 OWIN 到 Azure AD 颁发登录和注销请求
您的应用程序现在正确配置与使用 OpenID 连接的身份验证协议的 2.0 版终结点进行通信。  OWIN 进行处理的所有丑陋精心编制的消息身份验证，验证令牌从 Azure 的广告，和维护用户会话的详细信息。  剩下的是要让您的用户登录和注销。

- 您可以使用授权您的控制器需要该用户登录才能访问某一页中的标记。  打开`Controllers\HomeController.cs`，并添加`[Authorize]`标记添加到关于控制器。

```C#
[Authorize]
public ActionResult About()
{
  ...
```

-   您还可以使用 OWIN 直接在代码中发出身份验证请求。  打开`Controllers\AccountController.cs`。  在 SignIn() 和 SignOut() 的动作，发出 OpenID 连接挑战和注销请求，分别。

```C#
public void SignIn()
{
    // Send an OpenID Connect sign-in request.
    if (!Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge(new AuthenticationProperties { RedirectUri = "/" }, OpenIdConnectAuthenticationDefaults.AuthenticationType);
    }
}

// BUGBUG: Ending a session with the v2.0 endpoint is not yet supported.  Here, we just end the session with the web app.  
public void SignOut()
{
    // Send an OpenID Connect sign-out request.
    HttpContext.GetOwinContext().Authentication.SignOut(CookieAuthenticationDefaults.AuthenticationType);
    Response.Redirect("/");
}
```

-   现在，打开`Views\Shared\_LoginPartial.cshtml`。  这是您将在这里向用户显示应用程序的登录和注销链接，并打印出在视图中的用户的名称。

```HTML
@if (Request.IsAuthenticated)
{
    <text>
        <ul class="nav navbar-nav navbar-right">
            <li class="navbar-text">

                @*The 'preferred_username' claim can be used for showing the user's primary way of identifying themselves.*@

                Hello, @(System.Security.Claims.ClaimsPrincipal.Current.FindFirst("preferred_username").Value)!
            </li>
            <li>
                @Html.ActionLink("Sign out", "SignOut", "Account")
            </li>
        </ul>
    </text>
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li>@Html.ActionLink("Sign in", "SignIn", "Account", routeValues: null, htmlAttributes: new { id = "loginLink" })</li>
    </ul>
}
```

## 4.显示用户信息
当使用 OpenID 连接的用户进行身份验证，2.0 版终结点包含[声明](active-directory-v2-tokens.md#id_tokens)或断言有关用户应用程序返回 id_token。  您可以使用这些声明来个性化您的应用程序︰

- 打开`Controllers\HomeController.cs`文件。  您可以通过在控制器中访问该用户的声明`ClaimsPrincipal.Current`安全主体对象。

```C#
[Authorize]
public ActionResult About()
{
    ViewBag.Name = ClaimsPrincipal.Current.FindFirst("name").Value;

    // The object ID claim will only be emitted for work or school accounts at this time.
    Claim oid = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier");
    ViewBag.ObjectId = oid == null ? string.Empty : oid.Value;

    // The 'preferred_username' claim can be used for showing the user's primary way of identifying themselves
    ViewBag.Username = ClaimsPrincipal.Current.FindFirst("preferred_username").Value;

    // The subject or nameidentifier claim can be used to uniquely identify the user
    ViewBag.Subject = ClaimsPrincipal.Current.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;

    return View();
}
```

最后，生成并运行您的应用程序 ！   使用 Microsoft 个人帐户或一个工作或学校的帐户，登录，并注意在顶部导航栏中如何反映用户的身份。  现在，您可以使用行业标准协议，可以对其个人和工作/学校这两个帐户的用户进行身份验证保护 web 应用程序。

为了便于参考，[作为.zip 此处提供](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/complete.zip)（无需您配置的值） 的完整的示例，或者您可以克隆它从 GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet.git```

## 下一步行动

您现在可以移动到更高级的主题。  您可能想要尝试︰

[安全的 Web API 2.0 版应用程序模型与 >>](active-directory-devquickstarts-webapi-dotnet.md)

更多的资源，请查阅︰
- [应用程序模型 v2.0 预览 >>](active-directory-appmodel-v2-overview.md)
- [StackOverflow"azure 活动目录"标签 >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

测试

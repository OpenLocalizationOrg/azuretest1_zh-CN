---
ms.openlocfilehash: fbf4067fe93bed4cda80598a970cafaa60a67110
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="应用程序模型 v2.0 |Microsoft Azure"
    description="应用程序和方案 Azure AD 的应用程序模型 v2.0 公共预览所支持的类型。"
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/12/2015"
    ms.author="dastrock"/>

# 应用程序模型 v2.0 预览︰ 类型的应用程序
2.0 版应用程序模型的各种现代应用程序体系结构中，所有这些都基于[OAuth 2.0](active-directory-v2-protocols.md#oauth2-authorization-code-flow)和/或[连接 OpenID](active-directory-v2-protocols.md#openid-connect-sign-in-flow)的行业标准协议支持身份验证。  此文档简要介绍了您可以生成应用程序的类型，独立的语言或平台的希望。  它将帮助您了解高级别方案之前您[跳转到代码的权限](active-directory-appmodel-v2-overview.md#getting-started)。

> [AZURE.NOTE]
    此信息适用于 2.0 版应用程序模型的公共预览。  有关说明如何与普遍适用的 Azure AD 集成服务，请参考[Azure 活动目录开发指南 》](active-directory-developers-guide.md)。

## 基础知识
每个应用程序使用 2.0 版应用程序模型将需要在[apps.dev.microsoft.com](https://apps.dev.microsoft.com)注册。  应用程序注册过程将收集和将几个值分配给您的应用程序︰

- 唯一地标识您的应用程序的**应用程序 Id**
- **重定向 URI** ，用于直接返回到您的应用程序响应
- 其他几个特定于方案的值。  有关详细信息，了解如何[注册应用程序](active-directory-v2-app-registration.md)。

注册后，应用程序可以与 Azure 广告通讯 v2.0 终结点到我发送的请求︰

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

每个应用程序的交互使用 2.0 版的终结点都将遵循类似高级别模式︰

1. 应用程序将最终用户用于登录的 2.0 版端点。
2. 用户通过验证，输入他们的用户名和密码或其他方式。
3. 用户授权其代表该应用程序，该应用程序授予的权限的请求。
4. 该应用程序从 v2.0 终结点接收某种安全的令牌。
5. 应用程序使用安全令牌来访问受保护的信息或资源。
6. 资源服务器验证安全令牌，以确保可以授予访问权限。
7. 该应用程序会定期刷新安全令牌。

<!-- TODO: Need a page for libraries to link to -->
每个七个步骤将不同稍有根据您正在构建的应用程序的类型，我们已经为您处理细节的开源库。  可以了解有关[权限、 许可以及范围](active-directory-v2-scopes.md)，或阅读以了解一些具体实例。

## Web 应用程序
为 web 应用程序 （.NET、 PHP、 Java、 拼音、 Python、 节点等），可以通过浏览器 2.0 版应用程序模型支持使用[OpenID 连接](active-directory-v2-protocols.md#openid-connect-sign-in-flow)用户登录访问。  在 OpenID 连接使 web 应用程序会接收到`id_token`，验证用户的身份，并提供了有关索赔表单中的用户信息的安全标记︰

```
// Partial raw id_token
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImtyaU1QZG1Cd...

// Partial content of a decoded id_token
{
    "name": "John Smith",
    "email": "john.smith@gmail.com",
    "oid": "d9674823-dffc-4e3f-a6eb-62fe4bd48a58"
    ...
}
```

您可以了解有关的所有类型的令牌和在[v2.0 令牌引用](active-directory-v2-tokens.md)的应用程序可用的声明。

在 web 服务器应用程序，登录身份验证流采用下列高级别步骤︰

![泳道的 web 应用程序的图像](../media/active-directory-v2-flows/convergence_scenarios_webapp.png)

Id_token 使用公用签名密钥从 v2.0 终结点接收到的验证足以确保该用户的标识，并设置可用于标识在后续页请求的用户的会话 cookie。

若要查看运行中的这种情况下，试验中我们[入门](active-directory-appmodel-v2-overview.md#getting-started)部分的 web 应用程序签入代码示例之一。

除了简单登录，可能还需要 web 服务器应用程序来访问 REST API，如一些其他 web 服务。  在这种情况下使 web 服务器应用程序可以进行组合 OpenID 连接和 OAuth 2.0 流，使用[OAuth 2.0 授权码流](active-directory-v2-protocols.md#oauth2-authorization-code-flow)。 这种情况下是在[Web Api 部分](#web-apis)，我们[WebApp WebAPI 入门主题](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md)中涵盖以下方面。

## Web Api
可以使用 2.0 版应用程序模型来保护 web 服务，如您的应用程序的 rest 风格的 Web API。  而不是 id_tokens 和会话 cookie，Web Api 使用 OAuth 2.0 access_tokens 来保护他们的数据和验证传入的请求。  Web API 的调用方追加了 access_token 中的 HTTP 请求的授权标头︰

```
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

然后，Web API 可以使用 access_token 来验证 API 调用者的身份并从编码为 access_token 的声明提取有关调用方的信息。  您可以了解有关的所有类型的令牌和在[v2.0 令牌引用](active-directory-v2-tokens.md)的应用程序可用的声明。

Web API 可以让用户选择-在/退出某些功能或数据的电源通过公开又称为[作用域](active-directory-v2-scopes.md)的权限。  用户必须同意的范围种在流动过程中，对于调用应用程序中获取权限范围内的。  询问用户的权限，以及在 Web API 接收的所有 access_tokens 中记录这些权限将负责 v2.0 终结点。  所有 Web API 需要担心是验证它接收每次调用 access_tokens 并执行适当的授权检查。

Web API 可以从所有类型的应用程序，包括 web 服务器应用程序、 桌面和移动应用程序，单个页面的应用程序、 服务器端守护程序和甚至是其他 Web Api 接收 access_tokens。  举一个例子，让我们看一下 web 服务器应用程序调用 Web API 的完整流程。

![Web 应用程序 Web API 泳道图像](../media/active-directory-v2-flows/convergence_scenarios_webapp_webapi.png)

要了解有关 authorization_codes、 refresh_tokens 和 access_tokens 的详细的步骤，请阅读有关[OAuth 2.0 协议](active-directory-v2-protocols.md#oauth2-authorization-code-flow)。

若要了解如何保护 Web API 2.0 版应用程序模型和 OAuth 2.0 access_tokens，取出中[快速入门部分](active-directory-appmodel-v2-overview.md#getting-started)我们的 Web API 代码示例。


## 移动和本机应用程序
安装在设备上，例如移动和桌面应用程序的应用程序经常需要访问后端服务或 Web Api 的数据存储和执行同一用户的各种功能。  这些应用程序可以将登录和授权添加到后端服务使用 v2.0 模型以及[OAuth 2.0 授权码流](active-directory-v2-protocols.md#oauth2-authorization-code-flow)。  

在此流程，应用程序接收授权从 v2.0 终结后用户登录，它代表应用程序的调用后端服务代表当前已登录的用户的权限。  应用程序然后可以交换在后台 OAuth 2.0 access_token 和 refresh_token authoriztion_code。  应用程序可以使用 access_token 来验证对 Web Api 在 HTTP 请求中，并且可以使用 refresh_token 来获取新的 access_tokens 较旧的到期时。

![本机应用程序泳道图像](../media/active-directory-v2-flows/convergence_scenarios_native.png)

## 当前预览限制
这些类型的应用程序目前不支持通过 2.0 版应用程序模型预览，但在正式供货及时支持的路线图。  [V2.0 预览限制文章](active-directory-v2-limitations.md)中描述的 2.0 版应用程序模型的公共预览其他的局限性和限制。

### 单个页面的应用程序 (Javascript)
许多现代的应用程序具有单个页面应用程序前端编写主要在 javascript 和经常使用 SPA 框架，如 AngularJS、 Ember.js、 Durandal 等。 通常可用 Azure 广告服务支持这些应用程序使用[OAuth 2.0 隐式流](active-directory-v2-protocols.md#oauth2-implicit-flow)-但是，这个流量尚不可用 2.0 版应用程序模型中。  它将以短订单。

如果你急切地想获得 SPA 使用 2.0 版应用程序模型，可以实现使用上述[web 服务器应用程序流](#web-apps)的身份验证。  但这不是推荐的方法，并为此方案的文档将被限制。  如果您想要体会 SPA 方案，您可以签出[普遍适用的 Azure AD SPA 代码示例](active-directory-devquickstarts-angular.md)。

### 守护程序/服务器端应用程序
包含长时间运行的流程或不存在的用户的情况下运行的应用程序也需要一种方法来访问受保护的资源，例如 Web Api。  这些应用程序可以进行身份验证并获取令牌使用应用程序的标识 （而不是用户的委派的身份） 使用[OAuth 2.0 客户端凭据流](active-directory-v2-protocols.md#oauth2-client-credentials-grant-flow)。  

2.0 版应用程序模型-这是说，应用程序只能获得令牌后发生非交互式用户签入流当前不支持此流。  在不久的将来将添加客户端凭据流。  如果您想要查看客户端凭据流中普遍适用的 Azure 广告服务， [GitHub 上的守护程序示例](https://github.com/AzureADSamples/Daemon-DotNet)检查。

### 链接的 Web Api （上代表的）
许多的体系结构包括 Web API 程序需要调用另一个下游 Web API，同时受 2.0 版应用程序模型。  该方案是在本机客户端具有 Web API 后端，后者又调用 Microsoft 联机服务例如 Office 365 或图形 API 中常见。

可以使用 OAuth 2.0 Jwt 载体凭据授予，否则称为[On-Behalf-Of 流](active-directory-v2-protocols.md#oauth2-on-behalf-of-flow)支持此链接的 Web API 方案。  不过，在代表的流量目前未实现 2.0 版应用程序模型的预览中。  若要查看此流原理在普遍适用的 Azure AD 服务，请检查[在 GitHub 上代表的代码示例](https://github.com/AzureADSamples/WebAPI-OnBehalfOf-DotNet)。

测试

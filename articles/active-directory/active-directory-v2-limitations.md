---
ms.openlocfilehash: e2d986847b9adc4ea5236f23f1308df903156951
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="应用程序模型 v2.0 |Microsoft Azure"
    description="一份限制和 Azure AD 2.0 版应用程序模型的限制条件。"
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

# 应用程序模型 v2.0 预览︰ 限制和限制

有几种功能与 2.0 版应用程序模型的公共预览期间中尚不支持的功能。  2.0 版应用程序模型达到一般的可用性，但如果您要构建的应用程序在公共预览过程中应注意它们之前，每个这些限制将被删除。

> [AZURE.NOTE]
    此信息适用于 2.0 版应用程序模型的公共预览。  有关说明如何与普遍适用的 Azure AD 集成服务，请参考[Azure 活动目录开发指南 》](active-directory-developers-guide.md)。

## 对生产应用程序的支持
与 2.0 版应用程序模型集成的应用程序才会向公众释放作为生产级应用程序。  2.0 版应用程序模型是在这次公共预览-重大更改可能会及时引入任何时候并没有 sla 保证的服务。  不会为任何可能发生的事件提供支持。  如果您不愿意接受的依赖关系从事的服务仍然处于开发阶段，风险，必须与我们联系 @AzureAD 讨论您的应用程序或服务的范围。

## 对应用程序的限制
当前在 2.0 版应用程序模型公共预览中不支持以下类型的应用程序。  有关受支持的应用程序类型的说明，请参阅[这篇文章](active-directory-v2-flows.md)。

##### 单个页面的应用程序 (Javascript)
许多现代的应用程序具有单个页面应用程序前端编写主要在 javascript 和经常使用 SPA 框架，如 AngularJS、 Ember.js、 Durandal 等。 通常可用 Azure 广告服务支持这些应用程序使用[OAuth 2.0 隐式流](active-directory-v2-protocols.md#oauth2-implicit-flow)-但是，这个流量尚不可用 2.0 版应用程序模型中。  它将以短订单。

如果你急切地想获得 SPA 使用 2.0 版应用程序模型，可以实现使用[web 应用程序流](active-directory-v2-flows.md#web-apps)的身份验证。  但这不是推荐的方法，并为此方案的文档将被限制。  如果您想要体会 SPA 方案，您可以签出[普遍适用的 Azure AD SPA 代码示例](active-directory-devquickstarts-angular.md)。

##### 守护程序/服务器端应用程序
包含长时间运行的流程或不存在的用户的情况下运行的应用程序也需要一种方法来访问受保护的资源，例如 Web Api。  这些应用程序可以进行身份验证并获取令牌使用应用程序的标识 （而不是用户的委派的身份） 使用[OAuth 2.0 客户端凭据流](active-directory-v2-protocols.md#oauth2-client-credentials-grant-flow)。  

2.0 版应用程序模型-这是说，应用程序只能获得令牌后发生非交互式用户签入流当前不支持此流。  在不久的将来将添加客户端凭据流。  如果您想要查看客户端凭据中通常可用的流 Azure AD 应用程序模型中，检出[在 GitHub 上的守护程序示例](https://github.com/AzureADSamples/Daemon-DotNet)。

##### 链接的 Web Api （上代表的）
许多的体系结构包括 Web API 程序需要调用另一个下游 Web API，同时受 2.0 版应用程序模型。  该方案是在本机客户端具有 Web API 后端，后者又调用 Microsoft 联机服务例如 Office 365 或图形 API 中常见。

可以使用 OAuth 2.0 Jwt 载体凭据授予，否则称为[On-Behalf-Of 流](active-directory-v2-protocols.md#oauth2-on-behalf-of-flow)支持此链接的 Web API 方案。  不过，在代表的流量目前未实现 2.0 版应用程序模型的预览中。  若要查看此流原理在普遍适用的 Azure AD 服务，请检查[在 GitHub 上代表的代码示例](https://github.com/AzureADSamples/WebAPI-OnBehalfOf-DotNet)。

##### 独立 Web Api
在 2.0 版应用程序模型预览中，您可以生成[使用 OAuth 标记，保护 Web API](active-directory-v2-flows.md#web-apis)从 v2.0 终结点。  但是，Web API 将只能接收标记从客户端共享相同的应用程序 id。  不支持从多个不同的客户端访问的第三方 web 服务的构造。

若要了解如何创建一个接受来自已知客户端具有相同的应用程序 Id 的标记的 Web API，请参阅我们[入门](active-directory-appmodel-v2-overview.md#getting-started)部分的 2.0 版应用程序模型 Web API 示例。

## 对用户的限制
当前使用 2.0 版应用程序模型构建的每个应用程序将向公众公开与 Microsoft 帐户或 Azure AD 帐户的所有用户。 具有两种类型的帐户的任何用户将能够成功地定位到或安装您的应用程序，输入其凭据，在 2.0 版应用程序模型中，并且表示同意在您的应用程序的权限。  您可以编写您的应用程序代码拒绝从某个组的用户的登录，但它不会阻止它们能够同意该应用程序。

实际上，您的应用程序可以不限制的用户可以登录到该应用程序的类型。  您将不能生成业务线 （限于一个组织中的用户） 的应用程序、 应用程序可用于仅企业用户 （使用 Azure 的广告帐户） 或只供消费者 （使用 Microsoft 帐户） 的应用程序。

## 限制对应用程序登记
在此时间点，想要与 2.0 版应用程序模型集成的所有应用程序必须在[apps.dev.microsoft.com](https://apps.dev.microsoft.com)创建新的应用程序注册。  任何现有的 Azure 广告或 Microsoft 客户应用程序将不兼容 2.0 版应用程序模型中，也不会在任何新的应用程序注册门户除了门户中注册应用程序。  没有迁移路径从上市 Azure 广告使应用程序服务对 2.0 版应用程序模型。

同样，在新的应用程序注册门户中注册的应用程序将使用专门 2.0 版应用程序模型。  您不可以使用应用程序注册门户创建将与 Azure 广告成功集成的应用程序或 Microsoft 客户服务。

在新的应用程序注册门户中注册的应用程序是当前限制为一组有限的 redirect_uri 值。  与方案开始必须为 web 应用程序和服务 redirect_uri 或`https`，而所有其他平台 redirect_uri 必须使用硬编码的值`urn:ietf:oauth:2.0:oob`。

若要了解如何在新的应用程序注册门户中注册应用程序，请参阅[该文章](active-directory-v2-app-registration.md)。

## 有关服务和 Api 的限制
2.0 版应用程序模型当前支持登录注册新的应用程序注册门户，提供在任何应用程序属于[受支持的身份验证流程](active-directory-v2-flows.md)列表。  但是，这些应用程序将只能获得非常有限的一组资源的 OAuth 2.0 的访问令牌。  V2.0 终结点只会颁发的 access_tokens:

- 应用程序请求令牌。  应用程序可以获取 access_token 本身，如果逻辑应用程序组成的若干个不同组件或层。  若要查看运行中的这种情况下，检查出我们[快速入门](active-directory-appmodel-v2-overview.md#getting-started)教程。
- Outlook 邮件、 日历和联系人 REST Api，它们都是位于 https://outlook.office.com。  要了解如何编写访问这些 Api 的应用程序，请参阅这些[办公室快速入门](https://www.msdn.com/office/office365/howto/authenticate-Office-365-APIs-using-v2)教程。

以及您自己的 Web Api 和服务支持，将在不久的将来，添加更多 Microsoft 联机服务。

## 库和 Sdk 的限制
不是所有的语言和平台必须支持 2.0 版应用程序模型预览库。  身份验证库集是当前限制为.NET、 iOS、 Android、 NodeJS 和 Javascript。  相应的代码示例和教程的每个部分提供了我们[开始工作](active-directory-appmodel-v2-overview.md#getting-started)。

如果您想要与使用其他语言或平台的 2.0 版应用程序模型集成的应用程序，请参阅[OAuth 2.0 和 OpenID 连接协议引用](active-directory-v2-protocols.md)它将指导您如何构建与 v2.0 终结点进行通信所需的 HTTP 消息。

## 协议的限制
2.0 版应用程序模型支持打开 ID 连接和 OAuth 2.0。  但是，并非所有特性和功能的每个协议都已都并入 2.0 版应用程序模型。  一些示例包括︰

- 完全支持 OpenID 连接`prompt`参数
- OpenID 连接`login_hint`参数
- OpenID 连接`domain_hint`参数
- OpenID 连接 `end_sesssion_endpoint`

为了更好地理解协议中支持的功能的 2.0 版应用程序模型的范围，阅读我们的[OpenID 连接和 OAuth 2.0 协议参考](active-directory-v2-protocols.md)。

测试

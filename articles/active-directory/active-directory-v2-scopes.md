---
ms.openlocfilehash: 2626521904e1e7cd60bbf5e8b955beecce75716b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="应用程序模型 v2.0 |Microsoft Azure"
    description="在 Azure AD 2.0 版应用程序模型中，包括范围、 权限和许可授权的说明。"
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

# 应用程序模型 v2.0 预览︰ 范围、 权限和许可

将与 Azure AD 集成的应用程序按照特定的授权模型，使用户能够控制应用程序如何访问它们的数据。  在应用程序模型 2.0 版，此授权模型的实现进行了更新，更改应用程序必须使用 Azure AD 的交互方式。  本主题涵盖包括范围、 权限和同意此授权模型的基本概念。

> [AZURE.NOTE]
    此信息适用于 2.0 版应用程序模型的公共预览。  有关说明如何与普遍适用的 Azure AD 集成服务，请参考[Azure 活动目录开发指南 》](active-directory-developers-guide.md)。

## 范围和权限

应用程序模型 2.0 版实现[OAuth 2.0](active-directory-v2-protocols.md)授权协议，这是一种允许第三方应用程序代表用户访问 web 承载资源的方法。  资源标识符或**应用程序 ID URI**必须与 Azure AD 集成的任何 web 承载的资源。  例如，一些 Microsoft 的 web 托管资源包括︰

- Office 365 统一邮件 API: `https://outlook.office.com`
- Azure 的资源管理器 API: `https://management.azure.com`
- Azure 广告图形 API: `https://graph.windows.net`

同样适用于使用 Azure AD 集成了任何第三方资源。  这些资源还可以定义一组可用于划分成更小块的功能的资源的权限。  例如，Office 365 统一邮件 API 定义了这些基本权限︰

- 读取用户的邮箱
- 写入到用户的邮箱
- 发送邮件的用户

通过定义这些权限，则资源可以有细粒度地控制其数据和如何向外界公开。  一个第三方应用程序然后可以从最终用户的请求这些权限，最终用户必须批准权限之前应用程序可对代表他们。  通过分块资源的功能集成到更小的权限集，可以生成第三方应用程序请求只为执行其职责所需的特定权限。  它还使最终用户能够知道完全的应用程序将如何使用他们的数据，以使它们更加确信，该应用程序行为不心怀恶意企图。

在 Azure 广告和 OAuth，这些权限被称为**作用域**。  您可能会看到它们被称为**oAuth2Permissions**。  作用域 Azure AD 中表示为一个字符串值。  粘连与 Office 365 统一邮件 API 的示例，每个权限的范围值是︰

- 读取用户的邮箱︰ `Mail.Read`
- 写入到用户的邮箱︰ `Mail.ReadWrite`
- 作为用户发送邮件︰ `Mail.Send`

应用程序可以通过指定作用域到 2.0 版的端点，请求在请求这些权限，如下所述。

## 同意

在[OpenID 连接或 OAuth 2.0](active-directory-v2-protocols.md)的授权请求，应用程序可以请求所需使用的权限`scope`查询参数。  例如，当用户应用程序进行签名，该应用程序将发送的请求如下所示 （使用的换行以提高可读性）︰

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&scope=
https%3A%2F%2Foutlook.office.com%2Fmail.read%20
https%3A%2F%2Fgraph.windows.net%2Fmail.send
&state=12345
```

`scope`参数是以空格分隔列表的应用程序正在请求的范围。  每个单个作用域将范围值附加到资源的标识符 (应用程序 ID URI) 来表示。  上述请求指示该应用程序需要读取用户的邮箱，并充当用户发送邮件的权限。

用户输入其凭据后，将检查 v2.0 终结点匹配记录的**用户同意**。  如果用户已不同意所请求的任何的权限在过去，2.0 版终结点将询问用户授予所请求的权限。  

![工作帐户同意屏幕抓图](../media/active-directory-v2-flows/work_account_consent.png)

如果用户审批权限，同意将记录，以便用户不必重新同意在后续的登录。

## 增量的同意

您的应用程序不需要请求它需要在初始用户登录或注册每个权限。 因为您可以基于每个请求指定作用域，您的应用程序可以执行"增量的同意"，并选择何时可以要求用户同意的情况下的最佳时机。  有几个常见的原因应用程序不可能一次要求提供的所有权限︰

- 需要许多权限的应用程序。  如果您的应用程序访问许多不同的资源，并在每个执行丰富的功能，权限列表中的您的应用程序需要可以很快就变得很长。  从历史上看，较长的列表的权限有不鼓励用户注册应用程序并且会降低获得用户的认可。  而不是同时请求这些权限，您的应用程序可以询问时初始登录，部分并以增量方式询问附加权限的用户尝试使用高级的功能。
- 不断增长的应用程序。  几乎每个应用程序始于较小的功能集，并随时间来引入新的功能增长。  与增量同意的情况下，可以轻松地更改您的应用程序并顺利地向用户请求新的权限。  您只需更新代码，以授权请求发送的其他作用域。
- 仅限管理员的权限。  一些资源将该**只**同意，或审核组织的管理员可以定义的权限。  例如，Azure 广告图形 API 定义了`Directory.Write`权限，它允许应用程序创建、 更新和删除用户和组，amognst 其他事情。  可以想象为什么该权限可能需要高特权的管理员的批准  通过使用增量同意的情况下，您的应用程序可以公开一组基本的功能，任何用户可以注册而无需管理员的批准。  但是，一旦他们尝试执行高权限的操作时，可以请求仅限管理员的权限，并需要访问您的应用程序的部分之前管理员报出。

## 使用权限

为您的应用程序权限的用户同意后，您的应用程序可以获取表示您的应用程序中某些容量的资源的访问权限的访问令牌。  一个给定的访问令牌只能用于单个 resorce，但其内部编码将是您的应用程序已被授予对该资源的所有权限。  获取一个访问令牌，您的应用程序可以到 v2.0 令牌终结点发出一个请求︰

```
POST common/v2.0/oauth2/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/json

{
    "grant_type": "authorization_code",
    "client_id": "2d4d11a2-f814-46a7-890a-274a72a7309e",
    "scope": "https://outlook.office.com/mail.read https://outlook.office.com/mail.send",
    "code": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq..."
    "client_secret": "zc53fwe80980293klaj9823"  // NOTE: Only required for web apps
}
```

然后可以到资源的 HTTP 请求中使用生成的访问令牌-它将可靠地指示资源，您的应用程序具有适当的权限来执行给定的任务。  

OAuth 2.0 协议以及如何获取访问令牌的详细信息，请参阅该[应用程序模型 2.0 版协议参考](active-directory-v2-protocols.md)。

## OpenId 和 Offline_Access

应用程序模型 2.0 版有两个很好定义的作用域不适用于对特定资源的`openid`， `offline_access`。

#### OpenId

如果应用程序执行登录使用[OpenID 连接](active-directory-v2-protocols.md#openid-connect-sign-in-flow)，则必须请求`openid`范围。  `openid`范围将显示在工作帐户同意屏幕为"登录"的权限，并在个人 Microsoft 客户同意的情况下屏幕与"查看您的配置文件和连接到应用程序和服务使用 Microsoft 帐户"权限。  此权限允许应用程序访问的 OpenID 连接用户信息终结点，并因此要求用户批准。  `openid`范围也可 v2.0 令牌端点处获得 id_tokens，可用于保护应用程序的不同组件之间的 HTTP 调用。

#### Offline_Access

`offline_access`作用域允许您访问资源的应用程序代表用户的时间较长。  在工作帐户的同意屏幕中，该范围将显示与"随时访问您的数据"权限。  在个人 Microsoft 客户同意的情况下屏幕，它将显示为"随时访问您的信息"权限。  当用户批准`offline_access`的范围内，您的应用程序将能够从 v2.0 令牌终结点接收刷新令牌。  刷新令牌将长期存在和允许您的应用程序，以获取新的访问令牌，较旧的或使其过期。

如果您的应用程序不会请求`offline_access`的范围内，它将不会收到 refresh_tokens。  这意味着，当您兑换[OAuth 2.0 授权代码流](active-directory-v2-protocols.md#oauth2-authorization-code-flow)中的授权，您将只能收到回从 access_token`/token`终结点。  该 access_token 的时间 （通常是一个小时），短时间内将保持有效，但最终会到期。  此时，时间点，您的应用程序需要将用户重定向回`/authorize`检索新授权的终结点。  在此重定向用户可能会或可能不需要重新输入其凭据或重新同意访问权限，具体情况取决于应用程序的类型。

有关如何获取和使用刷新令牌，请参阅该[应用程序模型 2.0 版协议参考](active-directory-v2-protocols.md)。

测试

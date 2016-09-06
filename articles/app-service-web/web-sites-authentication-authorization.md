---
ms.openlocfilehash: 68a043af75e1368697434b8889cee61a1d1be74d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="使用 Active Directory 在 Azure 应用程序服务的身份验证" 
    description="了解业务线应用程序部署到 Azure 应用程序服务 Web 应用程序的不同身份验证和授权选项" 
    services="app-service\web" 
    documentationCenter="" 
    authors="cephalin" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-web" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="web" 
    ms.date="07/02/2015" 
    ms.author="cephalin"/>

# 使用 Active Directory 在 Azure 应用程序服务的身份验证 #

[Azure 应用程序服务 Web 应用程序](http://go.microsoft.com/fwlink/?LinkId=529714)可通过企业的业务线应用程序方案支持单一登录 (SSO) 的用户，无论他们从内部环境或公共 internet 访问应用程序。 它可以与集成， [Azure Active Directory](http://azure.microsoft.com/services/active-directory/) (AAD) 或内部安全令牌服务 (STS)，如 Active Directory 联合身份验证服务 (AD FS）)，以进行内部活动目录 (AD) 用户身份验证和授权它们正确。

## 零摩擦身份验证和授权 ##

一个按钮单击几下，您可以为您的 web 应用程序启用身份验证和授权。 每个 Azure 的 web 应用程序中的复选框样式配置业务线 web 应用程序提供基本的访问控制。 它是通过强制 HTTPS 和身份验证到 Azure AD 租户所选择的以前授予用户访问您的 web 应用程序内容的权限。 有关详细信息，请参阅[Web 应用程序的身份验证 / 授权](http://azure.microsoft.com/blog/2014/11/13/azure-websites-authentication-authorization/)。

>[AZURE.NOTE] 此功能目前在预览中。

## 手动实现身份验证和授权 ##

在许多情况下，您想要自定义的身份验证和授权的行为的应用程序，如登录和注销页，自定义授权逻辑，多租户应用程序的行为，等等。 在这些情况下，可能是更好的办法配置身份验证和授权功能手动为更大的灵活性。 下面是两个主要选项  

-   [Azure AD](web-sites-dotnet-lob-application-azure-ad.md) -您可以为 web 应用程序使用 Azure 广告实现身份验证和授权。 为身份标识提供程序使用 Azure AD 具有以下特征︰
    -   支持流行的身份验证协议，如[OAuth 2.0](http://oauth.net/2/)、 [OpenID 连接](http://openid.net/connect/)和[SAML 2.0](http://en.wikipedia.org/wiki/SAML_2.0)。 有关受支持协议的完整列表，请参阅[Azure 活动目录的身份验证协议](http://msdn.microsoft.com/library/azure/dn151124.aspx)。
    -   可以使用 Azure 专用标识提供程序没有任何内部部署基础结构。
    -   也可以使用内部部署配置目录同步 AD （托管内部）。
    -   Azure 广告与从您的内部目录同步 AD 域平滑的 SSO 体验到您的 web 应用程序时启用 AD 用户从 intranet 和 internet 访问。 内联网，从 AD 用户可以自动通过集成的身份验证来访问 web 应用程序。 从互联网中，AD 用户可以登录到使用其 Windows 凭据的 web 应用程序中。
    -   对[所有应用程序都由 Azure 的广告支持](/marketplace/active-directory/)，包括 Azure、 Office 365、 Dynamics CRM Online，Microsoft Intune 和数以千计的非 Microsoft 云应用程序提供 SSO。 
    -   Azure 广告委托给非管理员角色[依赖方](http://en.wikipedia.org/wiki/Relying_party)应用程序管理时仍必须由全局管理员配置对敏感目录数据的应用程序访问。
    -   发送所有信赖方应用程序的声明类型的通用组。 声明类型的列表，请参阅[支持令牌和声明类型](http://msdn.microsoft.com/library/azure/dn195587.aspx)。 索赔是不可自定义。
    -   [Azure 广告图形 API](http://msdn.microsoft.com/library/azure/hh974476.aspx)让应用程序访问目录数据可以在 Azure 的广告。
-   [内部安全令牌服务 (STS)，例如，AD FS](../web-sites-dotnet-lob-application-adfs/) — 可以实现身份验证和授权的 web 应用程序与内部 STS 如 AD FS。 AD FS 使用内部具有以下特征︰
    -   AD FS 拓扑必须部署的内部，使用成本和管理开销。
    -   公司政策要求 AD 数据会存储在部署时最佳。
    -   只有 AD FS 管理员可以配置[信赖方信任和声明规则](http://technet.microsoft.com/library/dd807108.aspx)。
    -   可以管理[索赔](http://technet.microsoft.com/library/ee913571.aspx)在每个应用程序的基础上。
    -   用于访问本地广告通过公司防火墙的数据必须具有单独的解决方案。

>[AZURE.NOTE] 如果您想要怎样的 Azure 帐户之前开始使用 Azure 应用程序服务，请转到[尝试应用程序服务](http://go.microsoft.com/fwlink/?LinkId=523751)，立即可以在此创建短期的初学者 web 应用程序在应用程序服务。 没有信用卡，所需;没有承诺。

## 会发生什么变化
* 有关更改网站为应用程序服务的指南，请参阅︰ [Azure 应用程序服务，并对现有的 Azure 服务及其影响](http://go.microsoft.com/fwlink/?LinkId=529714)
* 旧的门户与新门户的更改的指南，请参阅︰[用于预览门户导航的引用](http://go.microsoft.com/fwlink/?LinkId=529715)
 

测试

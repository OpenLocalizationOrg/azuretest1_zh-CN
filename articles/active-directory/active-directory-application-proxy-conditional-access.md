---
ms.openlocfilehash: 332091d4632c2c736886415bbb178e0cfce483b2
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="条件与 AD Azure 应用程序代理发布的应用程序访问"
    description="介绍如何设置条件发布使用 Azure AD 应用程序代理服务器进行远程访问的应用程序的访问权限。"
    services="active-directory"
    documentationCenter=""
    authors="rkarlin"
    manager="msStevenPo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/09/2015"
    ms.author="rkarlin"/>

# 使用条件访问
> [AZURE.NOTE] 应用程序代理服务器是仅当您升级到特优或 Azure Active directory 的基本版才可用的功能。 有关详细信息，请参见[Azure Active Directory 版本](https://msdn.microsoft.com/library/azure/dn532272.aspx)。

现在，您可以启用条件访问权限授予用户和组访问应用程序使用应用程序代理发布的访问规则。 这使您能够︰
- 要求每个应用程序的多因素身份验证
- 仅当用户不是在工作时需要多因素身份验证
- 阻止用户访问该应用程序，当它们不是在工作

这些规则可以应用到所有用户和组或仅对特定用户和组。
默认规则将应用于所有用户有权访问应用程序。 但是该规则也可以限制为用户指定的安全组的成员。  
当用户访问使用 OAuth 2.0，OpenID 连接，SAML 或 WS 联合身份验证联合应用程序评估访问规则。 此外，访问规则进行评估时的 OAuth 2.0 和 OpenID 连接时刷新令牌用于获取一个访问令牌。 

## 条件接收系统必备组件

- 对 Azure Active Directory 高级订阅 
- 联合或托管的 Azure Active Directory 租户 
- 联合的承租人要求启用多因素身份验证 (MFA) 

![](http://i.imgur.com/rv28onQ.png)

## 配置每个应用程序的多因素身份验证
1. 在 Azure 管理门户管理员身份登录。
2. 转到活动目录并选择要在其中启用应用程序代理的目录。
3. 单击**应用程序**并向下的滚动到**访问规则**部分。 使用访问规则部分只显示使用联合身份验证的使用应用程序代理发布的应用程序。
4. 通过选择**启用访问规则****上**启用该规则。
5. 指定的用户和组，这些规则将应用于人。 使用**添加组**按钮以选择一个或多个组的访问规则将应用于哪个。 此对话框还可用于删除所选的组。  当已选中的规则将应用于组时，访问规则将仅强制实施的属于指定的安全组的一个用户。 <br> 若要显式排除规则的安全组，**除**检查，指定一个或多个组。 除外列表中的组成员的用户将不需要执行多因素身份验证。 <br>如果用户被配置使用每用户多因素身份验证功能，则此设置将优先于应用程序的多因素身份验证规则。 这意味着已配置为每个用户的多因素身份验证的用户需要执行多因素身份验证，即使它们不受应用程序的多因素身份验证规则。 [了解更多有关多因素身份验证和基于用户的设置](../multi-factor-authentication/multi-factor-authentication.md)。 
6. 选择您要设置访问规则︰
- **需要多因素身份验证**︰ 向其应用访问规则的用户将需要完成之前访问该规则所应用到的应用程序的多因素身份验证。
- **不在工作时需要多因素身份验证**︰ 试图从受信任的 IP 地址访问该应用程序的用户不需要进行多因素身份验证。 多因素身份验证设置页面上，可以配置受信任的 IP 地址范围。
- **阻止不在工作时访问**︰ 试图访问该应用程序从公司网络外部的用户将无法访问该应用程序。


## 联合身份验证服务配置 MFA
多因素身份验证 (MFA) 可能对联盟的承租人，执行通过 Azure Active Directory 或内部部署 AD FS 服务器。 默认情况下，MFA 会承载 Azure Active Directory 的任何页面上。 为了配置部署 MFA，运行 Windows PowerShell 并且使用 – SupportsMFA 属性设置 Azure 的广告模块。
下面的示例演示如何通过[设置 MsolDomainFederationSettings cmdlet](https://msdn.microsoft.com/library/azure/dn194088.aspx) contoso.com 租户启用内部 MFA:`Set-MsolDomainFederationSettings -DomainName contoso.com -SupportsMFA $true `除了设置此标志，必须配置联盟的租户 AD FS 实例进行多因素身份验证。 按照说明进行[内部部署 Microsoft Azure 多因素身份验证](http://technet.microsoft.com/library/dn280946.aspx)。 
## 其他资源

* [作为一个组织注册 azure](..sign-up-organization.md)
* [Azure 的标识](..fundamentals-identity.md)

测试

---
ms.openlocfilehash: e3e402ed22717460d7cdcc26521466bd3d2aa6f0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="Azure 的条件访问 SaaS 应用程序的预览 |Microsoft Azure"
    description="在 Azure 广告的条件访问允许您配置每个应用程序的多因素身份验证的访问规则，并且能够禁止不受信任网络上的用户访问。 "
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="stevenpo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/12/2015"
    ms.author="femila"/>

# Azure 的条件访问 SaaS 应用程序的预览

Azure 的 SaaS 应用程序的条件访问仅供公共预览。 预览使您可以配置每个应用程序的多因素身份验证的访问规则，并能够阻止不受信任网络上的用户的访问权限。 

可以将多因素身份验证规则应用于指派给应用程序，或仅对用户指定的安全组内的所有用户。 如果他们从一个 IP 地址，在组织的内部网络访问应用程序，用户可能不从多因素身份验证的要求。
这些功能可用于购买 Azure 活动目录特优许可的客户。

## 方案的前提条件
* 对 Azure Active Directory 高级订阅

* 联合或管理 Azure Active Directory 租户

* 联合的承租人要求启用多因素身份验证 (MFA)。

## 在此预览版本中的已知的问题
此预览应用于预先集成联合 SaaS 应用程序，应用程序在使用密码单一登录、 注册开发和行的业务应用程序和 Azure AD 应用程序代理。 一些其他的应用程序仍然启用.

##配置每个应用程序访问规则

本部分介绍如何配置每个应用程序访问规则。

1. Microsoft Azure 门户以管理员身份登录。
2. 在左窗格中，选中**Active Directory**。
3. 在目录选项卡上，选择您的目录。
4. 选择**应用程序**选项卡。
5. 选择该规则将设置为该应用程序。
6. 选择**配置**选项卡。
7. 向下滚动到访问规则部分。 选择所需的访问规则。
8. 指定该规则将应用于的用户.
9. 通过选择**已启用，在**启用该策略。

##了解访问规则

本节提供在 Azure 条件应用程序访问预览中支持的访问规则的详细的说明。
### 指定的用户的访问规则应用于

默认情况下该策略将应用于对应用程序具有访问权限的所有用户。 但是还可以限制对指定的安全组成员的用户的策略。 **添加组**按钮用于从访问规则将应用于组选择对话框中选择一个或多个组。 此对话框还可用于删除所选的组。 当已选中的规则将应用于组时，访问规则将仅强制实施的属于指定的安全组的一个用户。

安全组也可以显式排除策略中选择除外选项，并指定一个或多个组。 是除外列表中的组成员的用户将不受多因素身份验证需求，即使它们的访问规则应用于某个组的成员。
所示的访问规则需要以下中经理组访问应用程序时使用多因素身份验证的所有用户。

![设置与 MFA 的条件访问规则](./media/active-directory-conditional-access/conditionalaccess-saas-apps.jpg)

##MFA 条件访问规则
如果用户已使用每用户多因素身份验证功能配置，用户在此设置将优先于应用程序多因素身份验证规则。 这意味着需要已配置为每个用户的多因素身份验证的用户来执行多因素身份验证，即使它们不受应用程序的多因素身份验证规则。 了解有关多因素身份验证和每个用户设置的详细信息。

###访问规则选项
预览当前支持以下选项︰

* **需要多因素身份验证**︰ 使用此选项的访问规则应用到的用户将需要完成之前访问该策略将应用于应用程序的多因素身份验证。

* **不在工作时需要多因素身份验证**︰ 使用此选项是否来自受信任的 IP 的用户将不需要执行多因素身份验证。 可以在多因素身份验证设置页配置受信任的 IP 地址范围

* **阻止不在工作时访问**︰ 此选项不来自受信任的 IP 的用户将被阻止。 多因素身份验证设置页面上，可以配置受信任的 IP 地址范围。

###设置规则状态
访问规则状态允许打开或关闭这些规则。 关闭的多因素身份验证要求的访问规则时将不执行。

### 评估访问规则

当用户访问使用 OAuth 2.0，OpenID 连接，SAML 或 WS 联合身份验证联合应用程序评估访问规则。 除了访问规则进行评估时的 OAuth 2.0 和 OpenID 连接时刷新令牌用于获取一个访问令牌。 如果使用刷新令牌时，策略评估失败，将返回错误 invalid_grant，这表明用户需要对客户端重新进行身份验证。
通过配置联合身份验证服务提供多因素身份验证

多因素身份验证 (MFA) 可能对联盟的承租人，执行通过 Azure Active Directory 或内部部署 AD FS 服务器。

默认情况下，MFA 会承载的 Azure Active Directory 的网页。 要配置部署 MFA，– SupportsMFA 属性必须设为 Azure Active Directory，通过使用 Windows PowerShell Azure AD 模块中，则返回 true。

下面的示例演示如何通过[设置 MsolDomainFederationSettings cmdlet](https://msdn.microsoft.com/library/azure/dn194088.aspx) contoso.com 租户启用内部 MFA:

    Set-MsolDomainFederationSettings -DomainName contoso.com -SupportsMFA $true

除了设置此标志，必须配置联盟的租户 AD FS 实例进行多因素身份验证。 按照部署 Azure 多因素身份验证的说明。


测试

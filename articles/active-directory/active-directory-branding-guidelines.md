---
ms.openlocfilehash: 03a9f41fcf193fc87c7a54c8e246f9227623bfc6
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="商标使用准则的应用程序"
   description="全面的 Azure Active Directory 面向开发人员的资源指南"
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="msmbaldwin"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/25/2015"
   ms.author="mbaldwin"/>


# 商标使用准则的应用程序


本主题讨论使用 Azure Active Directory 开发应用程序时，应使用商标使用准则。 这些准则将有助于直接您的客户，当他们想要使用他们工作或学校的帐户，在 Azure 广告，注册和登录到您的应用程序中管理。

## 个人帐户与工作或 microsoft 的学校科目

Microsoft 管理两种类型的用户帐户︰

- **个人帐户**（以前称为 Windows Live™ ID）。 这些帐户表示*单个*用户和微软之间的关系，并用于访问 Microsoft 中的使用者的设备和服务。 这些帐户被供个人使用。

- **工作或学校的帐户。** 这些帐户是由 Microsoft 管理使用 Azure Active Directory 的组织代表。 这些帐户用于从 Microsoft Office 365 和其他商业服务登录。

Microsoft 工作或学校的帐户通常分配给最终用户 （员工、 学生、 联邦雇员） 通过其组织 （公司、 学校、 政府机关）。 这些帐户在云中，在 Azure 的广告平台，直接掌握了，或者从本地目录中，如 Windows 服务器的 Active Directory 同步到 Azure 的广告。 Microsoft 是*管理员*的工作或学校的科目，但拥有和控制的组织帐户。

## 指的 Azure 的广告在您的应用程序中的帐户

Microsoft 不公开最终用户对 Azure 或 Active Directory 品牌名称，既不应该您。

- 一旦用户登录，应使用组织的名称和徽标尽可能多地。 这是优于使用通用术语"组织"等。

- 当用户没有登录时，您就应当为其帐户"工作或学校帐户"，然后使用 Microsoft 徽标来传达这些帐户由 Microsoft 管理。 不要使用如"企业客户"、"企业往来帐户"或"公司帐户"的条款，它创建用户混淆。

## 用户帐户 pictogram
在早期版本的这些原则，我们建议使用"蓝色工牌"pictogram。 根据用户和开发者的反馈，我们现在建议使用 Microsoft 徽标的相反。 这将帮助用户了解，他们可以重复使用它们将使用具有 Office 365 或其他 Microsoft 业务服务以登录到您的应用程序的帐户。

## 注册和签名用 Azure 的广告

您的应用程序可能会显示注册和登录的单独的路径，以下各节提供了可视化指导，这两种情况。

**如果您的应用程序支持最终用户符号向上 （例如免费试用版或 freemium 模型）**︰ 可以显示**登录**按钮，使用户能够访问您的应用程序与他们的工作或学校帐户从 Microsoft。 Azure 的广告将显示在访问您的应用程序在第一次同意提示。

**如果您的应用程序需要的权限，只有管理员可以表示同意，或如果您的应用程序要求组织授权**︰ 应该分开，从用户登录的管理员购置。 **"获取此应用程序"按钮**将重定向管理员登录，然后要求他们授予代表组织中的用户同意的情况。 这有抑制到您的应用程序的最终用户同意的情况下提示的优点。

## 应用程序获取可视化指导

"获取该应用程序"链接必须将用户重定向到 Azure AD 授予访问权限 （授权） 页，允许组织管理员联系以授予您的应用程序有权访问由 Microsoft 的组织的数据。 [Azure Active Directory 集成应用程序](active-directory-integrating-applications.md)文章讨论了如何请求访问的详细信息。

管理员表示同意在您的应用程序后，他们可以选择将其添加到其用户的 Office 365 提供应用程序启动器体验 （从 waffle 和[https://portal.office.com/myapps](https://portal.office.com/myapps)的情况下可访问）。 如果您想要公布这种能力，可以使用术语像"将此应用程序添加到您的组织"，并显示此类按钮︰

![应用程序类型和方案](./media/active-directory-branding-guidelines/add-to-my-org.png)

但是，我们建议您编写说明性的文本，而不是依靠按钮。 例如︰
> *如果您已经使用 Office 365 或其他 microsoft 的业务服务，您可以只需授予 < your_app_name > 访问组织的数据。 这将允许您的用户可以访问其现有的工作帐户 < your_app_name >。*


## 登录程序的可视化指南
您的应用程序应显示一个符号按钮将用户重定向到登录端点对应于使用 Azure 广告与集成的协议中。 下一节详细说明该按钮应如下所示。

### Pictogram 和"工作或学校帐户"
它是 Microsoft 徽标的关联，"工作或学校"泛指唯一地表示其他标识之间的 Azure 广告提供商您的应用程序可能支持。 如果您没有足够的空间用于"工作或学校的帐户"，是可以缩短其"工作帐户"。

![应用程序类型和方案](./media/active-directory-branding-guidelines/work-or-school-account.png)

![应用程序类型和方案](./media/active-directory-branding-guidelines/work-account.png)

您还可以提供补充说明，以帮助最终用户识别是否他们可以使用此按钮︰

![应用程序类型和方案](./media/active-directory-branding-guidelines/work-account-with-explaination.png)

## 正确和错误的品牌
**** "工作或学校帐户"结合使用 Microsoft 徽标来表示登录使用 Azure 的广告。 如果磁盘空间非常珍贵，很确定地说"工作帐户"，但**不是**使用其他术语，如"企业客户"、"企业往来帐户"或"公司帐户"。

**不要**使用"Office 365 ID"或"Azure ID"。 Office 365 也是使用 microsoft 这不使用 Azure AD 身份验证提供者的名称。

**不**改变 Microsoft 徽标。

**不**公开到 Azure 或 Active Directory 品牌的最终用户。 它不过是确定与开发人员、 IT 专业人员和管理员使用这些术语。

## 导航须知

**请**提供便于用户注销并切换到另一个用户帐户。 大多数人有一个个人帐户从 Microsoft/Facebook/Google/Twitter，而人们往往与多个组织。 支持多个已登录的用户即将登场。

## 支持两个 Azure 的 AD 和 Microsoft 应用程序中的帐户

如果您的应用程序支持两个 Azure 的 AD 和 Microsoft 帐户，您需要在您的应用程序中包含两个单独登录按钮。 我们当前正在处理更新，您可以一次集成和支持这两个人并处理来自 Microsoft 的帐户。 当可用时，您可以在您的应用程序中显示一个"登录与 Microsoft"按钮。

测试

---
ms.openlocfilehash: 393fcabbf977f0de3ddd6c1f810ed405a013ab5e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="创建或编辑在 Azure AD 用户"
    description="解释如何创建或编辑用户帐户，在 Azure 广告的主题。"
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="stevepo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity" 
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/31/2015"
    ms.author="curtand"/>

# 创建或编辑在 Azure AD 用户

您必须为将访问 Microsoft 云服务的每个用户创建一个帐户。 此外可以更改用户帐户，或在不再需要时删除它们。 默认情况下用户不具有管理员权限，但也可以选择分配它们。

## 创建用户

1. **活动目录**，请单击，然后单击您单位的目录的名称。
2. 在**用户**页上，单击**添加用户**。
3. **请告诉我们有关此用户**在页上，为**类型的用户**，可以选择︰
    1. **您组织中的新用户**-表示所需新的用户帐户来创建和管理您的目录中。
    2. **与现有 Microsoft 客户用户**— 表示您想要添加到目录的现有 Microsoft 帐户，以便基于 Azure 资源与使用 Microsoft 帐户访问 Azure 的联管理员进行协作。
    3. **另一个 Azure 的广告目录中的用户**– 指示您要将用户帐户添加到您来自另一个 Azure 的广告目录的目录。 您需要为其成员的另一个目录中选择一个用户。
4. 这取决于您选择的选项，键入用户名称或微软此用户将登录的帐户名称。
5. 在用户**配置文件**页上，提供用户的姓，姓氏，用户友好名称和角色下拉列表菜单中的用户角色。 有关用户和管理员角色的详细信息，请参阅[在 Azure 的广告分配管理员角色](active-directory-assign-admin-roles.md)。 指定是否**启用多因素身份验证**。
6. 在**获得临时密码**页上，单击**创建**。

如果您的组织使用多个域，您应了解以下问题，当您创建用户帐户︰

- 如果您首次创建，例如，跟 geoffgrisso@contoso.com 的 geoffgrisso@contoso.onmicrosoft.com，您可以创建跨域用户帐户使用同一用户主体名称 (UPN)。
- 不能创建跟 geoffgrisso@contoso.onmicrosoft.com 的 geoffgrisso@contoso.com。

## 编辑用户

如果您正在编辑的用户与您的内部部署 Active Directory 服务同步，出现一条错误消息，并且您将不能编辑使用该过程的用户。 若要编辑用户，请使用您本地 Active Directory 管理工具。

若要编辑 Azure 管理门户中的用户︰

1. **活动目录**，请单击，然后单击您单位的目录的名称。
2. 在**用户**页中，单击您要编辑的用户的显示名称。
3. 完成所做的更改，然后单击**保存**。

## 重置用户密码

1. **活动目录**，请单击，然后单击您单位的目录的名称。
2. 在**用户**页中，单击您要编辑的用户的显示名称。
3. 在门户的底部，单击**重设密码**。
4. 在重置密码对话框中，单击**重置**。
5. 单击复选标记，以确认重置密码。

## 创建外部用户

在 Azure 广告还可以添加用户到 Azure 的广告目录从其他 Azure 的广告目录或使用 Microsoft 帐户的用户。 用户可以是成员的达 20 个不同的目录。

添加来自另一个目录的用户是外部用户。 外部用户可以与已经存在的目录，如在测试环境中，而不用要求他们使用新的帐户和凭据进行登录的用户进行协作。 外部用户进行身份验证及其主目录时他们登录和身份验证适用于它们的成员的所有其他目录。

若要创建外部用户，请在门户和**类型的用户**创建一个用户，选择**另一个 Azure 的广告目录中的用户**。

## 外部用户管理和限制

用户从一个目录添加到一个新目录时，该用户将是中新目录的外部用户。 最初，显示名称和用户名称是从用户的"主目录"复制并刻录到另一个目录中的外部用户。 从此以后，那些和外部用户对象的其他属性是完全独立的︰ 如果您更改用户的主目录，例如，更改用户的名称，在添加职务，等这些更改不会传播到另一个目录中的外部用户帐户。

两个对象之间的唯一联系是用户主目录或其 Microsoft 帐户始终验证。 这就是为什么看不到要重置密码或启用外部用户帐户的多因素身份验证选项︰ Microsoft 帐户的主目录的身份验证策略目前是仅有的一个用户登录时进行求值。

> [AZURE.NOTE]
> 您仍然可以通过禁用外部目录中的用户，这将阻止对您的目录的访问。

如果在他们的主目录中删除用户或他们取消其 Microsoft 客户，外部用户仍然存在于目录中。 但是，用户无法访问目录中的资源，因为用户不能再到他们的主目录或 Microsoft 客户身份验证。

多个目录是管理员的用户可以管理每个 Azure 管理门户中这些目录。 但是，Office 365 的其他应用程序当前不提供的体验，以分配和作为另一个目录中的外部用户访问服务。 今后，我们将为开发人员提供指导其应用程序如何使用用户属于多个目录。

目前有局限性，因为管理员只能授予同意其主目录中的多租户应用程序，并且仅对于 SaaS 应用程序，SSO 通过自己的主目录中访问面板资源调配。 Microsoft 帐户用户具有相同的限制，因为他们目前不能授予同意多租户应用程序中，或使用访问权限面板。

## 客人

**访客**是您已将用户类型设置为"来宾"的目录中的用户。 普通用户有一种用户类型的"成员"，以表明它们是属于您的目录。 当资源与人共用外部到您的目录，例如，Microsoft 帐户添加到 Azure 订购或与外部用户共享在 SharePoint 文档时创建的客人。

客人的目录中有一组有限的权限。 这些权限限制为客人发现同时仍能进行交互的用户和组关联的资源，他们正在着手在目录中其他用户的有关信息的能力。 例如，分配给订阅了 Azure 的访客将无法看到其他用户和组与 Azure 订阅关联。 他们还可以在应该被赋予访问订阅提供他们知道该用户的完整电子邮件地址目录中查找其他用户。 访客才可以看到一组有限的其他用户的属性。 这些属性被限制为显示名称、 电子邮件地址、 用户主体名称 (UPN) 和缩略图的照片。

## 配置用户访问策略

目录的**配置**选项卡包含选项来控制外部用户访问。 这些选项可以更改目录的全局管理员只在完整 Azure 门户中 （没有 Windows PowerShell 或者 API 方法） 的用户界面。
在 Azure 的门户打开**配置**选项卡，单击**撤销**，然后单击目录的名称。

![][1]

然后您可以编辑的选项用于控制外部用户访问。

![][2]

默认情况下，客人不能枚举目录，内容使他们看不到任何用户或组**成员列表**中。 他们可以通过键入用户的完整电子邮件地址，搜索用户，然后授予访问权限。 客人的默认限制设置为︰

- 他们无法枚举目录中用户和组。
- 如果他们知道该用户的电子邮件地址，他们可以看到有限的用户的详细信息。
- 当他们知道组名称时，他们可以看到有限的一组的详细信息。

客人以有限的用户或组的详细信息，请参阅的能力使他们能够邀请其他人并与他们协作的人员的某些详细信息，请参阅。  

## 接下来是什么

- [管理 Azure 的广告](active-directory-administer.md)
- [在 Azure AD 管理密码](active-directory-manage-passwords.md)
- [在 Azure AD 管理组](active-directory-manage-groups.md)

<!--Image references-->
[1]: ./media/active-directory-create-users/RBACDirConfigTab.png
[2]: ./media/active-directory-create-users/RBACGuestAccessControls.png

测试

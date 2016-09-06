---
ms.openlocfilehash: 2aed19ff64b385e4dd160487df24399e6f5a9abb
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="管理 Azure 的广告目录"
    description="解释的主题是什么 Azure AD 租户，以及如何管理 Azure 的广告目录。"
    services="active-directory"
    documentationCenter=""
    authors="Markusvi"
    writer="markvi"
    manager="stevenpo"
    editor="LisaToft"/>

<tags
    ms.service="active-directory"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="05/05/2015"
    ms.author="markvi"/>

# 管理 Azure 的广告目录

## 什么是 Azure AD 租户？

在物理场所，word 租户可以定义为组或占据建筑物的公司。 例如，您的组织可能拥有大楼中的办公室空间。 该建筑可能与若干其他组织街道上。 您的组织将被视为一个租户的这栋建筑物。 此建筑是贵组织的资产和提供安全并确保您可以安全地开展业务。 它还分开您的街道上的其他企业。 这可以确保您的组织资产其中是并独立于其他组织。

在支持云的场所中，租户可以定义为客户端或组织拥有并管理的云服务的特定实例。 使用标识平台由 Microsoft Azure，租户是 Azure 活动目录 (AD Azure) 组织接收，并拥有当 Microsoft 云服务，例如 Azure 或 Office 365 的其签署的只是专用的实例。

每个 Azure 的广告目录是截然不同且相互独立的其他 Azure 的广告目录。 就像公司的办公大楼是特定于您的组织的安全资产，Azure 的广告目录是也可使用的安全资产由您的组织。 Azure 广告体系结构隔离从共同结合客户数据和标识信息。 这意味着用户和管理员的一个 Azure 的广告目录不能意外地或恶意地访问另一个目录中的数据。

![][1]

## 如何获取 Azure 的广告目录？

Azure AD 提供核心目录和身份管理功能背后大部分微软的云服务，包括︰

- Azure
- Microsoft 365 Office
- Microsoft Dynamics CRM Online
- Microsoft Intune

这些 Microsoft 云服务的任何注册后，您将获得 Azure 的广告目录。 您可以根据需要创建其他目录。 例如，可能维护您的第一个目录作为生产目录，然后创建另一个目录，用于测试或临时。

> [AZURE.NOTE]
> 您的第一个服务注册后，我们建议您使用相同的管理员帐户，当您注册其他 Microsoft 云服务与组织相关联。

第一次注册 Microsoft 云服务，将提示您提供组织和贵组织的互联网域名注册的详细信息。 然后，使用此信息来创建一个新组织的 Azure 的广告目录实例。 该同一目录用于预订多个 Microsoft 云服务时尝试验证签名。

其他服务充分利用任何现有用户帐户、 策略、 设置或内部目录集成配置可帮助提高您的组织的身份基础结构内部和 Azure 的广告之间的效率。

例如，如果您最初注册 Microsoft Intune 订阅并完成进一步将与 Azure 的广告目录将内部部署 Active Directory 集成通过部署目录同步和/或单一登录服务器所必需的步骤，您可以注册其他 Microsoft 云服务，它还可以利用相同的目录集成好处与 Microsoft Intune 现在使用 Office 365。

有关与 Azure AD 集成您的内部目录的详细信息，请参阅[目录集成](active-directory-aadconnect.md)。

### Azure 的广告目录相关联的 Azure 的新订阅

可以将新的 Azure 订阅与同一对现有 Office 365 或 Microsoft Intune 订阅的登录进行身份验证的目录相关联。 登录到您的工作或学校帐户使用 Azure 管理门户。 管理门户返回一条消息，那是找不到该帐户的任何订阅。 选择**符号 Up For Azure**和您的目录将用于管理门户中可用。 有关详细信息，请参阅[管理 Office 365 订购 Azure 中的目录](active-directory-how-subscriptions-associated-directory.md#manage-the-directory-for-your-office-365-subscription-in-azure)。

有关常见的使用问题，Azure 的广告视频，请参阅[Azure Active Directory 的通用注册、 登录和使用的问题](http://channel9.msdn.com/Series/Windows-Azure-Active-Directory/WAADCommonSignupsigninquestions)。

### 注册后，Microsoft 云服务为组织创建 Azure 的广告目录

如果您还没有订阅 Microsoft 云服务，使用下面的链接之一注册。 注册您的第一个服务的操作将自动创建一个 Azure 的广告目录。

- [Microsoft Azure](https://account.windowsazure.com/organization)
- [365 office](http://products.office.com/business/compare-office-365-for-business-plans/)
- [Microsoft Intune](https://account.manage.microsoft.com/Signup/MainSignUp.aspx?OfferId=40BE278A-DFD1-470a-9EF7-9F2596EA7FF9&ali=1)

### 管理 Azure 设置默认目录

今天，当您注册 Azure 并且您的订阅已与该目录会自动创建目录。 但是，如果您最初注册 azure 在 2013 年 10 月之前，目录不会自动创建。 在这种情况下，Azure 可能通过为其提供一个默认的目录具有"回填"为您的帐户。 您的订购已再与该默认目录。

回填的目录已经在整体改进安全模型的一部分作为 2013 年 10 月为 Azure。 它有助于为所有 Azure 客户提供组织标识功能，并确保 Azure 的所有资源都在目录中的用户的上下文中都访问。 没有目录的情况下，不能使用 Azure。 要实现它，不得不已创建的任何用户都可以在 2013 年 7 月 7 日之前注册，但没有一个目录。 如果您已经创建了一个目录，您的订阅已与该目录相关联。

没有使用 Azure 的广告成本。 目录是一个免费的资源。 没有其他的 Azure 活动目录特优层单独许可并提供其他功能，例如公司品牌和自助服务密码重置。

若要更改目录的显示名称，单击目录的门户，单击**配置**。 在本主题后面所述，可以添加新的目录，或删除不再需要的目录。 您的订购与一个不同的目录，请单击**设置**扩展在左边的导航，和底部的**订阅**页**编辑目录**。 您还可以创建自定义的域使用已注册的 DNS 名称，而不默认值 *。 onmicrosoft.com 域，可能更为可取与 SharePoint Online 等服务。

## 如何管理目录数据

作为一个或多个 Microsoft 云服务预订的管理员，您可以使用 Azure 管理门户、 Microsoft Intune 帐户门户网站或 Office 365 管理中心来管理您的组织的目录数据。 您还可以下载并运行[Microsoft Azure 活动目录模块用于 Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) cmdlet 来帮助您管理在 Azure AD 存储的数据。

从上述这些门户网站 （或 cmdlet），您可以︰

- 创建和管理用户和组帐户
- 管理订阅您的组织相关的云服务
- 设置内部与目录服务的集成

Azure 管理门户、 Office 365 管理中心、 Microsoft Intune 帐户门户和 Azure AD cmdlet 所有读取和写入程序与您单位的目录，如下图中所示的 Azure 广告的单一共享实例。 以这种方式，门户网站 （或 cmdlet） 是作为一个前端界面纳入和/或修改您的目录数据。

![][2]

这些帐户门户和用于管理用户和组相关联的 Azure AD PowerShell cmdlet 基于 Azure 的广告平台。

如果对组织数据使用的任何门户网站 （或 cmdlet） 同时登录其中一个服务的上下文中进行了更改，更改也会显示其他门户网站中的下次您登录在该服务的环境下，由于此数据共享跨您订阅 Microsoft 云服务。
例如，如果 Office 365 管理中心用于禁止用户登录，该操作会阻止用户登录到您的组织当前订阅的任何其他服务。 如果您要提起 Microsoft Intune 帐户门户环境下该用户的帐户，您将看到用户被阻止。

## 如何添加和管理多个目录？

您可以在 Azure 管理门户添加 Azure 的广告目录。 选择左侧的**Active Directory**扩展并单击**添加**。

您可以管理每个目录作为完全独立的资源︰ 每个目录都是对等、 全功能型和逻辑上独立于其他目录管理;目录之间没有任何父-子关系。 此目录之间的独立性包括资源独立性，管理的独立性和同步独立性。

- **资源的独立性**。 如果您创建或删除一个目录中的资源，它具有外部用户，如下所述的部分异常的另一个目录中的任何资源上没有任何影响。 如果在一个目录使用一个自定义的域 'contoso.com'，它不能用于任何其他目录。
- **管理的独立性**。  如果目录 Contoso 非管理用户，然后创建一个测试目录 Test:
    - ◦The 目录同步工具，可以使用单个 AD 林同步数据。
    - 目录 Contoso 的 ◦The 管理员有没有直接的管理权限目录 Test，除非 Test 的管理员专门授予他们这些权限。 Contoso 的管理员可以控制访问目录 Test 凭借其所控制的用户帐户创建 Test。

    如果您更改 （添加或删除） 将一个目录中的用户管理员角色，更改不会影响任何管理员角色的用户可能必须在另一个目录中。


- **同步的独立性**。 您可以配置每个 Azure 广告分别以获取数据的单个实例同步︰
    - 目录同步工具，可以使用单个 AD 林同步数据
    - Azure Active Directory 连接器为前沿标识管理器，可以将数据与一个或多个内部林和/或非 AD 数据源同步。

此外请注意，与其他 Azure 的资源不同，您的目录不是 Azure 订阅的子资源。 因此取消或允许 Azure 订阅过期，如果您仍然可以访问目录数据如 Office 365 管理中心使用 Azure AD PowerShell，Azure 图形 API 或其他接口。 您可以将另一个订阅与目录相关联。

## 如何删除 Azure 的广告目录？
全局管理员可以从门户网站删除 Azure 的广告目录。 当删除一个目录时，也将删除该目录中包含的所有资源;因此，您应该确保您不需要该目录之前将其删除。

> [AZURE.NOTE]
> 如果用户使用工作或学校的帐户登录，用户必须不试图删除他或她的主目录。 例如，如果用户登录为 joe@contoso.onmicrosoft.com，该用户不能删除具有 contoso.onmicrosoft.com 作为其默认域的目录。

### 要删除 Azure 的广告目录，必须满足的条件

Azure 广告需要满足某些条件时要删除的目录。 这将减少用户或应用程序，例如用户能够登录到 Office 365 或访问 Azure 中的资源，删除一个目录将带来负面影响的风险。 例如，如果无意中删除订阅目录，成为用户不能访问该订阅 Azure 的资源。

检查以下条件︰

- 在目录中的唯一用户是全局管理员将删除该目录。 可以删除该目录之前，必须删除任何其他用户。 如果用户从内部同步，那么同步将需要被关闭，并必须使用为 Windows PowerShell 管理门户或 Azure 模块云目录中删除用户。 没有任何要求，若要删除组或联系人，如从 Office 365 管理中心添加联系人。
- 可以在目录中的任何应用程序。 可以删除该目录之前，必须删除任何应用程序。
- 可以为 Microsoft Azure，Office 365，如任何 Microsoft Online Services 没有订阅或 Azure AD 津贴与目录关联。 例如，如果在 Azure 中为您创建一个默认的目录，如果 Azure 订购仍然依赖于此目录的身份验证不能删除此目录。 同样，不能删除目录，如果另一用户都有与其关联的订阅。 您的订购与一个不同的目录，登录到 Azure 管理门户，在左侧导航窗格中单击**设置**。 然后在**订阅**页的底部，单击**编辑目录**。 有关 Azure 的订阅的详细信息，请参阅[如何 Azure 订阅是 Azure 的广告与相关联](active-directory-how-subscriptions-associated-directory.md)。

    > [AZURE.NOTE]
    > 如果用户使用工作或学校的帐户登录，用户必须不试图删除他或她的主目录。 例如，如果用户登录为 joe@contoso.onmicrosoft.com，该用户不能删除具有 contoso.onmicrosoft.com 作为其默认域的目录。

- 没有多因素身份验证提供程序可以链接到目录中。


## 其他资源

- [Azure 广告论坛](https://social.msdn.microsoft.com/Forums/home?forum=WindowsAzureAD)
- [Azure 的多因素身份验证论坛](https://social.msdn.microsoft.com/Forums/home?forum=windowsazureactiveauthentication)
- [Stackoverflow](http://stackoverflow.com/questions/tagged/azure)
- [作为一个组织注册 azure](sign-up-organization.md)
- [管理 Azure 使用 Windows PowerShell 的广告](https://msdn.microsoft.com/library/azure/jj151815.aspx)
- [在 Azure 的广告分配管理员角色](active-directory-assign-admin-roles.md)

<!--Image references-->
[1]: ./media/active-directory-administer/aad_portals.png
[2]: ./media/active-directory-administer/azure_tenants.png

测试

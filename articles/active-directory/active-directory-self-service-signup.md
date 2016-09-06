---
ms.openlocfilehash: 99db270ba46ec28d4d2545281b91ab625b37b0a0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="自助服务注册 Azure 是什么？"
    description="Azure，概述的自助服务注册如何管理注册过程和操作方法。"
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="stevenpo"
    editor="LisaToft"/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/14/2015" 
    ms.author="stevenpo"/>


# 自助服务注册 Azure 是什么？

本主题说明 （有时称为病毒注册） 自助注册过程以及如何接管一个 DNS 域名。  

## 为什么使用自助服务的注册？

- 获取客户对他们想要更快的服务。
- 创建基于电子邮件的 （病毒） 提供服务。
- 创建基于电子邮件的注册流程快速允许用户创建标识使用它们容易记住工作电子邮件别名。
- 非托管的 Azure 承租人可以扩展并成为以后管理租户和可重复使用的其他服务。

## 术语和定义

+ **自助服务注册**︰ 这是为云服务注册的用户和已为其在 Azure 活动目录中自动创建一个标识基于他们的电子邮件域的方法。
+ **非托管的 Azure 租户**︰ 这是在其中创建该身份信息的目录。 非托管的租户是没有全局管理员都有一个目录。
+ **电子邮件验证用户**︰ 这在 Azure AD 是一种类型的用户帐户。 具有自动创建后为自助服务的优惠注册被称为电子邮件验证用户身份的用户。 电子邮件验证用户已使用 creationmethod 标记的目录的常规成员 = EmailVerified。

## 客户体验

### 用户体验

例如，假设的用户的电子邮件是 Dan@BellowsCollege.com 收到通过电子邮件的敏感文件。 这些文件已被保护起来 Azure 权限管理 (Azure RMS)。 但陶建明的组织，风箱大学 Azure RMS 不注册了或已部署活动目录 RMS。 在这种情况下，Dan 可以注册 RMS 对于个人免费订阅才能读取受保护的文件。

如果陶建明是第一个从 BellowsCollege.com，那么在 Azure 广告将为 BellowsCollege.com 创建非托管的租户提供，该自助服务注册的电子邮件地址的用户。 如果 BellowsCollege.com 域中的其他用户注册此产品或类似的自助服务，他们还将在同一个非托管租户在 Azure 中创建的电子邮件验证用户帐户。

### 管理体验

管理员负责非托管的 Azure 租户的 DNS 域名可接管或合并后证明所有权的租户。 以下部分将介绍更多细节，管理经验，但以下是摘要︰

- 当接管非托管的 Azure 租户时，只会变得非托管客户端的全局管理员。 这有时被称为内部接管。
- 当合并非托管的 Azure 租户，将非托管客户端的 DNS 域名添加到您的托管 Azure 租户和创建一个映射的用户的资源因此用户可以继续访问而不会中断服务。 这有时称为外部接管。

### 获取创建的内容在 Microsoft Azure 目录？

#### 租户

- Azure 租户为域创建时，每个域一个租户。
- Azure 租户目录有无全局管理员。

#### 用户

- 为每个注册用户，在 Azure 的租户中创建的用户对象。
- 每个用户对象标记为病毒。
- 每个用户被授予访问该服务他们注册过的用户。

### 如何说我拥有域一个自助服务的 Azure 租户的？

可以声明一个自助服务的 Azure 租户执行域验证。 域验证证明您拥有创建 DNS 记录的域。

有两种方法实现 Azure 的租户 DNS 接管︰

- 内部接管 （管理员发现非托管的 Azure 租户，并想要变成一个托管的租户）
- 外部接管 （管理员尝试将新域添加到其托管的 Azure 租户）

您可能会感兴趣验证您拥有域，因为采取通过非托管的租户之后用户执行自助服务注册，或者您可能将新域添加到现有托管租户。 例如，您有一个名为 contoso.com 域和您想要添加新的域命名为 contoso.co.uk 或 contoso.uk。

## 什么是域接管？  

本部分介绍如何验证您拥有一个域

### 什么是域验证以及为什么使用？

为了对租户执行操作，Azure 广告要求您验证 DNS 域的所有权。  域的验证可以声称租户，并提升到托管的租户或合并到现有的托管租户租户自助服务自助服务组织。

## 域有效性规则的示例

有两种方法实现一个租户 DNS 接管︰

+ 内部的接管 （例如，管理员发现自助服务、 非托管的租户，并想要转换为托管的租户）
+ 外部接管 （例如，管理员尝试将新域添加到托管租户）

### 内部接管-升级自助服务、 非托管的租户要管理的租户

执行内部接管时，租户获取转换成从非托管的租户管理租户。 您需要完成您创建 DNS 区域中的 MX 记录或 TXT 记录的 DNS 域名称验证。 该操作︰

+ 验证您拥有域
+ 使组织管理
+ 使您组织的全局管理

假设 IT 管理员从风箱大学发现自助服务，用户从学校注册了。 如已注册的 DNS 所有者名称 BellowsCollege.com，IT 管理员可以验证在 Azure 中的 DNS 名称的所有权，并再接管非托管的租户。 租户则成为托管的租户，而且 IT 管理员分配全局管理员角色 BellowsCollege.com 目录。

### 外部接管-合并到现有的管理租户的自助服务的租户

在外部接管，您已经有一个托管的租户和所需的所有用户和组从非托管的租户，若要加入该托管的租户，而不是自己的两个单独的承租人。

作为托管组织的一名管理员，添加到域中，并且该域碰巧有与之相关联的非托管的租户。

例如，假设您是 IT 管理员，您已经为 Contoso.com，为您的组织注册一个域名托管的租户。 您会发现，您的组织中的用户有执行自助服务号向上服务通过使用电子邮件域名称 user@contoso.co.uk，这是您的组织拥有的另一个域名。 这些用户当前在 contoso.co.uk 为非托管租户拥有帐户。

不想管理两个单独的承租人，让 contoso.co.uk 的非托管的组织合并为 contoso.com 您现有的 IT 管理租户。

外部接管遵循相同的 DNS 验证进程内部的接管。  差别是︰ 用户和服务被重新映射到 IT 管理租户。

#### 执行外部接管的影响是什么？

有外部接管，以便用户能够继续访问而不会中断服务创建的用户对资源的映射。 许多应用程序，对于个人、 包括 RMS 处理用户对资源的映射，并且用户可以继续访问这些服务，而不会更改。 如果应用程序不能有效地处理用户对资源的映射，外部接管可能显式阻止以防止用户不好的体验。

#### 组织接管支持服务

目前下列服务支持接管︰

- RMS


很快将支持以下服务接管︰

- PowerBI

以下不这样做，需要更多的管理操作在外部接管后迁移用户数据。

- SharePoint/OneDrive


## 如何执行 DNS 域的名称接管

您有关于如何执行域验证 （和进行接管，如果您愿意的话） 的几个选项︰

1.  Azure 的管理门户

    接管时触发执行域添加。  如果承租人已存在的域，您必须执行外部接管的选项。

    登录到 Azure 门户使用您的凭据。  导航到您的现有租户，然后再**添加域**。

2.  365 office

    可以使用在 Office 365[管理域](https://support.office.com/article/Navigate-to-the-Office-365-Manage-domains-page-026af1f2-0e6d-4f2d-9b33-fd147420fac2/)页面上的选项若要使用您的域和 DNS 记录。 请参阅[验证您在 Office 365 中的域](https://support.office.com/article/Verify-your-domain-in-Office-365-6383f56d-3d09-4dcb-9b41-b5f5a5efd611/)。

3.  Windows PowerShell

    以下步骤需要执行使用 Windows PowerShell 的验证。

    步骤    |   若要使用的 Cmdlet
    ------- | -------------
    创建凭据 | 获取凭据
    连接到 Azure 的广告 | 连接 MsolService
    获取域的列表   | 获得 MsolDomain
    创建一个难题  | 获得 MsolDomainVerificationDns
    创建 DNS 记录   | 您的 DNS 服务器上执行此操作
    验证所面临的挑战    | 确认 MsolEmailVerifiedDomain

例如︰

1. 连接到使用自助服务响应时所用的凭据的 Azure AD︰ 导入模块 MSOnline $msolcred = 获取凭据连接 msolservice-凭据 $msolcred

2. 获取列表的域︰

    获得 MsolDomain

3. 然后运行 Get MsolDomainVerificationDns 用于创建一项挑战︰

    获得 MsolDomainVerificationDns – 域名*your_domain_name* – 模式 DnsTxtRecord

    例如︰

    获得 MsolDomainVerificationDns – 域名 contoso.com – 模式 DnsTxtRecord

4. 复制此命令返回的值 （挑战）。

    例如︰

    MS = 32DD01B82C05D27151EA9AE93C5890787F0E65D9

5. 在公用 DNS 命名空间，创建一个 DNS txt 记录包含您在上一步中复制的值。

    此记录的名称是父域的名称，因此如果您通过从 Windows 服务器的 DNS 角色创建该资源记录，保留记录名称为空，这只粘贴值在文本框中

6. 运行确认 MsolDomain cmdlet 以验证所面临的挑战︰

    确认 MsolEmailVerifiedDomain 的域名*your_domain_name*

    例如︰

    确认 MsolEmailVerifiedDomain-域名 contoso.com

一项成功的挑战将返回到没有错误提示。

## 如何控制自助服务设置？

管理员目前拥有两个自助服务的控件。 他们可以控制︰

- 无论用户可以加入组织通过电子邮件。
- 无论用户可以许可应用程序和服务本身。


### 如何控制这些功能？

管理员可以配置这些功能使用这些 Azure AD cmdlet 集 MsolCompanySettings 参数︰

+ **AllowEmailVerifiedUsers**控制用户是否可以创建或加入非托管的租户。 如果将该参数设置为 $false，没有电子邮件验证用户可以加入组织。
+ **AllowAdHocSubscriptions**控制向上执行自助服务登录的用户的能力。 如果将该参数设置为 $false，任何用户都可以不执行自助服务注册。


### 控件的工作方式在一起？

可以配合使用这两个参数来定义更精确地控制自助服务号组成。 例如，以下命令将允许用户执行自助服务号，但只，如果这些用户已经有一个帐户在 Azure AD （换句话说，用户需要创建一个电子邮件验证帐户无法执行自助服务号向上）︰

    Set-MsolCompanySettings -AllowEmailVerifiedUsers $false -AllowAdHocSubscriptions $true

下面的流程图说明了所有这些参数的不同组合，并为租户和自助服务号产生的条件。

![][1]

更多的信息和如何使用这些参数的示例，请参阅[设置 MsolCompanySettings](https://msdn.microsoft.com/library/azure/dn194127.aspx)。

## 请参见

-  [如何安装和配置 Azure PowerShell](../powershell-install-configure/)

-  [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx)

-  [Azure Cmdlet 引用](https://msdn.microsoft.com/library/azure/jj554330.aspx)

-  [一组 MsolCompanySettings](https://msdn.microsoft.com/library/azure/dn194127.aspx)

<!--Image references-->
[1]: ./media/active-directory-self-service-signup/SelfServiceSignUpControls.png

测试

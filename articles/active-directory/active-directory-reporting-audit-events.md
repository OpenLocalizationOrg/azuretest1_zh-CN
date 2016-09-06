---
ms.openlocfilehash: 89024b6e00bf955663d72a8640978ba5ba0d3070
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="Azure 的 Active Directory 审核报告事件"
   description="审核事件可用于查看和下载从 Azure 活动目录"
   services="active-directory"
   documentationCenter=""
   authors="kenhoff"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="06/18/2015"
   ms.author="kenhoff"/>

# Azure 的 Active Directory 审核报告事件

Azure 活动目录审核报告可帮助客户确定他们 Azure Active Directory 中出现的特权的操作。 特权的操作包括提升或做的更改 （例如，角色创建密码重置），更改策略配置 （例如密码策略） 或更改目录配置 （例如，更改域联盟设置）。 报告提供了事件名称，参与者执行操作，受更改的日期和时间 （UTC) 的目标资源的审核记录。 [查看您访问和使用情况的报告](active-directory-view-access-usage-reports.md)中所述，客户将能够通过[Azure 管理门户](https://manage.windowsazure.com/)，其 Azure 活动目录检索审核事件的列表。

## 审计报告保留

在 Azure 广告审查报告事件保留 180 天。 关于保留报表的详细信息，请参阅[Azure 活动目录报告保留策略](active-directory-reporting-retention.md)。

客户感兴趣较长保留期内存储的审核事件，对于报告 API 可以用于定期将审核事件拉入一个单独的数据存储区。 有关详细信息，请参阅[入门报告 API](active-directory-reporting-api-getting-started.md) 。

## 属性包含与每个审核事件

| 属性  | 说明                               |
| ------    | ------                                |
| 日期和时间 | 审核事件发生的时间与日期            |
| Actor     | 用户或服务执行操作的主体       |
| 操作    | 已执行的操作                     |
| Target    | 在执行该操作的服务主体的用户    |

## 审核报告事件列表

<!--- audit event descriptions should be in the past tense --->

| 事件                | 事件描述                                                                             |
| ------------------------------    | -------                                                                                   |
| 添加用户              | 将某个用户添加到目录中。                                                                        |
| 删除用户               | 从目录中删除用户。                                                                        |
| 设置许可证属性        | 在目录中的用户设置许可属性。                                                           |
| 重置用户密码           | 重置目录中的用户的密码。                                                               |
| 更改用户密码          | 更改该目录中的用户的密码。                                                             |
| 更改用户许可证           | 更改分配给该目录中的用户许可证。                                                          |
| 更新用户               | 更新用户目录中。                                                                      |
| 添加到角色的角色成员       | 将某个用户添加到目录角色。                                                                     |
| 从角色中删除角色成员      | 从目录角色中删除用户。                                                                 |
| 设置公司联系人信息   | 设置联系人公司级别的首选项。 这包括有关 Microsoft Online Services 的市场营销，以及技术通知的电子邮件地址。           |
| 公司向中添加合作伙伴        | 添加到目录的一个伙伴。                                                                     |
| 从公司中删除合作伙伴       | 从目录中删除一个合作伙伴。                                                                     |
| 添加服务主体         | 添加到目录服务主体。                                                                   |
| 删除服务主体      | 从目录中删除服务主体。                                                               |
| 添加服务主体凭据 | 为服务主体的添加的凭据。                                                                 |
| 删除服务主体凭据  | 从主体服务的已删除的凭据。                                                                 |
| 将域添加到公司         | 将域添加到目录。                                                                      |
| 从公司中删除域        | 从目录中删除域。                                                                      |
| 更新域             | 更新域目录上。                                                                        |
| 设置域身份验证     | 更改公司的默认域设置                                                                |
| 设置域的联盟 | 更新域的联合身份验证设置。                                                                 |
| 验证域             | 已验证域的目录上。                                                                       |
| 确认电子邮件已验证的域      | 已验证在使用电子邮件验证目录域。                                                          |
| 添加委派条目          | 委派条目添加到目录中。                                                                    |
| 设置委派条目          | 更新目录中的委派条目。                                                                   |
| 删除委派条目       | 从目录中删除委派条目。                                                                |
| 公司集 DirSyncEnabled 标志    | 设置为 Azure AD 同步启用目录的属性。                                                          |
| 设置密码策略           | 设置用户密码的长度和字符限制。                                                          |
| 集公司信息       | 更新公司级别信息。 [获得 MsolCompanyInformation](https://msdn.microsoft.com/library/azure/dn194126.aspx) PowerShell cmdlet 的更多详细信息，请参阅。   |
| 设置强制更改用户密码    | 设置强制用户更改其密码登录上的属性。                                                    |

<!---

List of events that still need descriptions:

Restore Application
Set String Auth Policy
Promote tenant to partner

--->

### 更新用户审核事件中包括的用户属性

"更新用户"审核事件包括有关哪些用户属性已更新的其他信息。 对于每个属性，请将包含的以前值和新值。

| 属性                 | 说明                                                                           |
| --------------------------------- | ---------                                                                         |
| AccountEnabled            | 允许用户登录。                                                               |
| AssignedLicense           | 已分配给用户的所有许可证。                                                     |
| AssignedPlan              | 从而从许可证分配给用户的服务计划。                                               |
| LicenseAssignmentDetail       | 许可证分配给用户的详细信息。 例如，如果涉及基于组的授权，这包括授予许可证组。     |
| 移动                | 用户的移动电话。                                                                  |
| OtherMail             | 用户的备用电子邮件地址。                                                               |
| OtherMobile               | 用户可选的移动电话。                                                                |
| StrongAuthenticationMethod        | 由用户配置的多因素身份验证，如语音呼叫、 SMS 或验证代码从移动应用程序的验证方法的列表。   |
| StrongAuthenticationRequirement   | 如果实施了多因素身份验证，启用或禁用该用户的。                                       |
| StrongAuthenticationUserDetails   | 用户的电话号码，备用电话号码和电子邮件地址，用于多因素身份验证和密码重置验证。         |
| TelephoneNumber           | 用户的电话号码。                                                                  |

审计记录是很多的法规遵从性要求的必需的控件。 对于使用 Azure 活动目录审核报表以满足他们的法规遵从性要求的客户，建议客户提交一份本帮助主题与客户的导出的审计报告，以帮助解释报告的详细信息的副本。 如果审计人员想了解 Azure 目前满足法规遵从性要求，直接到微软 Azure 信任中心的[法规遵从性页](http://azure.microsoft.com/support/trust-center/compliance/)审核员。
 

测试

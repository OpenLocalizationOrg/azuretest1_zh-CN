---
ms.openlocfilehash: 519c10699006179ef3a5a21db734b6435d5c7035
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="Azure AD 报告︰ 快速入门"
   description="Azure AD 报告︰ 快速入门"
   services="active-directory"
   documentationCenter=""
   authors="curtand"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="06/30/2015"
   ms.author="curtand;kenhoff"/>

# Azure AD 报告入门

## 它是什么

Azure 的 Active Directory 包括安全性、 活动和审计报告的目录。 下面是包含的报告的列表︰

### 安全报告

- 从未知的来源的签名宏
- 在多次失败后签名宏
- 在多个地域的签名宏
- 从 IP 地址与可疑活动签名宏
- 在活动中的不规则的符号
- 从可能是感染病毒的设备号单元
- 具有异常的用户登录活动

### 活动报告

- 应用程序使用情况︰ 摘要
- 应用程序使用情况︰ 详细
- 应用程序的仪表板
- 考虑资源调配错误
- 单个用户设备
- 单个用户活动
- 组活动报告
- 密码重置注册活动报告
- 密码重置活动

### 审核报告

- 目录审核报告

> [AZURE.TIP] 有关 Azure AD 报告上的多个文档，检查出[查看您访问和使用情况的报告](active-directory-view-access-usage-reports.md)。



## 它的工作原理


### 报告管线

报告管线由三个主要步骤组成。 每次用户登录或进行身份验证时，将发生以下情况︰

- 首先，对用户进行验证 （无论成功或不成功），并将结果存储在 Azure Active Directory 服务数据库。
- 定期处理单元的所有新符号。 我们的安全和反常活动算法在这种情况下，搜索所有新符号项中是否有可疑的活动。
- 处理之后，报告编写、 缓存，并在 Azure 管理门户中提供服务。

### 报告生成时间

由于大量的身份验证和签名 Azure 的广告平台处理单元，单元处理的最新符号是，平均而言，一小时以前。 在极少数情况下，可能需要 8 小时可以处理的最新签名宏。

通过检查每个报表顶部的帮助文本，您可以找到最新处理的登录。

![顶部的每个报表的帮助文本](./media/active-directory-reporting-getting-started/reportingWatermark.PNG)

> [AZURE.TIP] 有关 Azure AD 报告上的多个文档，检查出[查看您访问和使用情况的报告](active-directory-view-access-usage-reports.md)。



## 入门教程


### 登录到 Azure 的管理门户

首先，您需要登录到[Azure 管理门户](https://manage.windowsazure.com)为全局或符合性管理员。 您还必须是 Azure 订阅服务管理员或共同管理员，或使用 Azure 广告"访问"Azure 的订阅。

### 导航到报表

要查看报告，请导航到报告选项卡在您的目录的顶部。

如果这是您第一次查看报表时，您将需要同意一个对话框之前可以查看报告。 这是为了确保它是可接受的您查看此数据，可以考虑在一些国家和地区的私人信息的组织中的管理员。

![对话框中](./media/active-directory-reporting-getting-started/dialogBox.png)

### 浏览每个报表

导航到每个报表来查看所收集的数据和处理单元的符号。 您可以找到[所有的报表列表在此处](active-directory-reporting-what-it-is.md)。

![所有报表](./media/active-directory-reporting-getting-started/reportsMain.png)

### 下载为 CSV 报告

每个报告可以作为 CSV （逗号分隔值） 文件下载。 您可以使用这些文件在 Excel、 PowerBI 或第三方程序进一步分析数据的分析。

要下载以 CSV 的任何报表，请导航到报表和底部单击"下载"。

![下载按钮](./media/active-directory-reporting-getting-started/downloadButton.png)

> [AZURE.TIP] 有关 Azure AD 报告上的多个文档，检查出[查看您访问和使用情况的报告](active-directory-view-access-usage-reports.md)。





## 下一步行动

### 在活动中自定义警报的异常符号

导航到您的目录的"配置"选项卡。

滚动到"通知"部分。

启用或禁用电子邮件通知的 Anomalous 标记单元部分。

![通知部分](./media/active-directory-reporting-getting-started/notificationsSection.png)

### 将与 Azure AD 报告 API 集成

请参阅[报告 API 入门](active-directory-reporting-api-getting-started.md)。

### 对用户进行多因素身份验证

在报表中选择一个用户。

单击"启用 MFA"按钮在屏幕的底部。

![屏幕底部的多因素身份验证按钮](./media/active-directory-reporting-getting-started/mfaButton.png)

> [AZURE.TIP] 有关 Azure AD 报告上的多个文档，检查出[查看您访问和使用情况的报告](active-directory-view-access-usage-reports.md)。




## 了解更多信息


### 审核事件

了解目录中[Azure 活动目录报告审核事件](active-directory-reporting-audit-events.md)进行审核哪些事件有关。

### API 集成

请参阅[报告 API 入门](active-directory-reporting-api-getting-started.md)和[API 参考文档](https://msdn.microsoft.com/library/azure/mt126081.aspx)。

### 与我们联系

电子邮件[aadreportinghelp@microsoft.com](mailto:aadreportinghelp@microsoft.com)反馈、 帮助下，或如果您有任何问题 ！

> [AZURE.TIP] 有关 Azure AD 报告上的多个文档，检查出[查看您访问和使用情况的报告](active-directory-view-access-usage-reports.md)。

测试

---
ms.openlocfilehash: 6f54570f56a95b01bfa0d8dedaa8e6154dabc70a
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="Azure 的 Active Directory 报告通知"
    description="如何使用报告可疑迹象的通知在 Azure Active Directory 单元。"
    services="active-directory"
    documentationCenter=""
    authors="SSalahAhmed"
    manager="gchander"
    editor="LisaToft"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/17/2015"
    ms.author="saah;kenhoff"/>

# Azure 的 Active Directory 报告通知

## 哪种报告生成电子邮件通知

到目前为止，仅在活动报告触发中的不规则号发送电子邮件通知。

## "不规则登录"是什么？

不规则号单元是指那些由我们的机器学习算法，结合位置和设备中的一个反常号"根本不可能旅行"条件的基础。 这可能表明黑客想要使用此帐户登录。 

## 接收电子邮件通知用户？

电子邮件被发送到所有已分配活动目录特优许可证的全局管理员。 为了确保交付，我们将其发送给管理员备选电子邮件地址以及。 管理员应在其安全发件人列表中包括 aad-alerts-noreply@mail.windowsazure.com，使他们不会错过此电子邮件。

## 频率是发送这些电子邮件？

如果在活动中的 10 个新不规则号发生在过去的 30 天内，或由于最后一个电子邮件已发送，小于，发送电子邮件。

## 如何访问电子邮件中提到的报告？

当单击该链接时，您将被重定向到 Azure 管理门户中的报表页。 若要访问该报表，必须同时是︰

- 管理员或 co-管理 Azure 订购的
- 全局管理员在目录中，并分配一个活动目录特优许可证。 有关详细信息，请参见 Azure Active Directory 版本。

## 可以关闭这些电子邮件？

是的关闭相关的反常号单元内 Azure 管理门户，单击**配置**，然后选择下**通知**部分**被禁用**的通知。

## 接下来是什么
- 想了解可以使用哪些安全、 审核及活动报表？ 签出[Azure AD 安全、 审核及活动报告](active-directory-view-access-usage-reports.md)
- [要开始使用 Azure 活动目录津贴](active-directory-get-started-premium.md)
- [添加公司品牌为您登录并访问面板页](active-directory-add-company-branding.md)

测试

---
ms.openlocfilehash: 06a0e026d1a329d219e832743eb23a75d70deabb
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="Azure 的 Active Directory 报告搜索"
    description="如何搜索 Azure 活动目录的安全，活动和审核报告"
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
    ms.date="07/07/2015"
    ms.author="kenhoff"/>

# Azure 的 Active Directory 报告搜索

Azure 的 Active Directory 提供了目录管理员能够跨多个报告搜索用户安全性、 活动和审核事件。

若要查找搜索面板，请定位到**Azure 管理门户网站-> 您 Azure Active Directory-> 报告。** 顶部的报表列表中找不到面板。

若要搜索特定用户的活动或审核事件，在从和到字段中选择日期范围、 中键入用户的 UPN 或显示名称，并单击复选标记按钮。

## 报告包含在搜索中

并非所有报表尚未都包含在搜索结果中。 下表指示包含哪些报表。

|   报表                                              |   包含            |
|   ------                                              |   --------            |
|   从未知的来源的签名宏                       |   否                  |
|   在多次失败后签名宏                    |   否                  |
|   在多个地域的签名宏                  |   否                  |
|   从 IP 地址与可疑活动签名宏 |   否                  |
|   从可能是感染病毒的设备号单元             |   否                  |
|   在活动中的不规则的符号                          |   否                  |
|   具有异常的用户登录活动               |   否                  |
|   具有泄漏的凭据的用户                       |   否                  |
|   审计报告                                        |   是                 |
|   密码重置活动                             |   是                 |
|   密码重置注册的活动                |   是                 |
|   自助服务组活动                        |   是                 |
|   应用程序使用情况                                   |   否                  |
|   资源调配活动的帐户                       |   是                 |
|   密码翻转状态                            |   否                  |
|   考虑资源调配错误                         |   否                  |
|   RMS 使用情况                                           |   否                  |
|   最活跃的 RMS 用户                               |   否                  |
|   RMS 设备用法                                    |   否                  |

## 了解更多信息

 - [Azure 的 Active Directory 报告](active-directory-view-access-usage-reports.md)
 - [Azure Active Directory 报告审核事件](active-directory-reporting-audit-events.md)

测试

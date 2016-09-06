---
ms.openlocfilehash: a14a3be24ea1976418cec0be65ac1464bd66dcf0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

<properties 
    pageTitle="疑难解答动态组的成员身份 |Microsoft Azure" 
    description="在 Azure AD 中的动态组的成员身份提示列出疑难解答主题。" 
    services="active-directory" 
    documentationCenter="" 
    authors="femila" 
    manager="swadhwa" 
    editor="Curtis"
    tags="azure-classic-portal"/>

<tags 
    ms.service="active-directory" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/13/2015" 
    ms.author="femila"/>


# 疑难解答动态组的成员身份

| 症状                                                                        | 操作                                                                                                                                                                                                                                                                                                                                                                                                                        |
|--------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 我对一组配置规则，但不到成员资格获取更新的组     | 验证委派的组管理设置被设置为是在目录配置选项卡中启用。 请注意，您只能看到此设置是否具有分配给它一个 Azure 活动目录特优许可证的帐户登录。  验证规则的用户属性的值 — 是否满足该规则的用户？                                                                               |
| 我配置规则，但现在删除规则的现有成员      | 这预期行为 — 启用或更改规则时，将删除现有的组的成员。 结果集的计算规则的用户作为成员添加到组。                                                                                                                                                                                                                                      |
| 我看不到成员身份变化时添加或更改规则，为什么不立即？ | 专用的成员资格评估是定期执行异步后台进程。 在您组织中的用户的数量和大小的规则创建的组玩了多长时间的一个因素。 通常有少量用户的租户会在短短几分钟内看到组成员身份的更改。 承租人与大量用户可能需要 30 分钟或更长时间来填充。 |

下面是一些将 Azure Active Directory 提供一些附加信息的主题 

* [使用 Azure Active Directory 组管理对资源的访问](active-directory-manage-groups.md)

* [什么是 Azure 活动目录？](active-directory-whatis.md)

* [与 Azure Active Directory 集成您的内部标识](active-directory-aadconnect.md)



测试

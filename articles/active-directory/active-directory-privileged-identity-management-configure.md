---
ms.openlocfilehash: 04fa93f8b40bda45275a18db97869309f2410ef8
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="Azure AD 特权身份管理"
    description="解释什么 Azure AD 特权身份管理以及如何对其进行配置的主题。"
    services="active-directory"
    documentationCenter=""
    authors="IHenkel"
    manager="stevepo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/31/2015"
    ms.author="inhenk"/>

# Azure AD 特权身份管理

Azure AD 特权身份管理允许您管理、 控制和监视特权的身份和他们对资源的访问，在 Azure 的广告，和如 Office 365 或 Microsoft Intune 其他 Microsoft 在线服务。  

若要使用户能够执行特权操作，组织通常需要使其用户永久特权访问在 Azure 的广告，或为 Azure 或 Office 365 提供资源，很多或其他 SaaS 应用程序。 对于许多客户来说，这是为他们云托管的资源不断增加安全风险，因为它们无法充分监视执行具有管理员特权的用户。 此外，有特权访问一个破坏的用户帐户可能影响其整体的云安全。 Azure AD 特权身份管理可以帮助解决这一风险。  

在预览中显示的 azure AD 特权身份管理可以︰  

- 发现哪些用户将 Azure AD 管理员
- 启用点播、"刚好及时"管理对资源的访问目录
- 管理员分配中获得有关管理员的访问历史记录和更改的报告
- 获取有关访问特权角色的提醒

在此预览，Azure AD 特权身份管理可以管理内置的 Azure Active Directory 组织角色︰  

- 全局管理员
- 记帐管理员
- 服务管理员  
- 用户管理员
- 管理员密码

## 只是在时间管理员访问权限

从历史上看，可以为用户分配通过 Azure 管理门户或 Windows PowerShell 管理员角色。 因此，该用户将成为**永久的管理员**，他 / 她指定的角色中，始终处于活动状态。 此预览增加**临时管理**，这是一个需要以完成激活过程分配的角色的用户的支持。  激活过程为用户分配到角色中更改 Azure 广告从沉寂到活动。

## 为您的目录启用特权身份管理

您可以启动访问[Microsoft Azure 门户](https://portal.azure.com/)使用 Azure AD 特权身份管理。 现在，Azure AD 特权身份管理才会显示在 Microsoft Azure 门户。 您必须是全局管理员才能启用特权 Azure 广告标识管理目录。

![][1]

在初始化此扩展后将自动成为第一个**安全管理员**的目录。 安全管理员可以访问此扩展其他管理员的管理访问。  
在初始化期间，Azure AD 特权身份管理的平铺视图将被添加到 Azure 预览门户网站 startboard。

## 特权的身份管理仪表板

Azure AD 特权标识管理器提供了一个仪表板，这将使您如 importatnt 信息︰

- 分配给每个特权的角色的用户数  
- 临时和永久的管理员的数量
- 管理员的访问历史记录

![][2]

## 特权的角色管理

利用 Azure AD 特权身份管理解决方案，您可以通过添加或删除的永久或临时管理员为每个角色管理管理员。

![][3]

## 配置角色激活设置

使用角色激活设置您可以配置临时角色激活属性包括︰

- 角色激活期限内
- 角色激活通知
- 用户角色激活过程中提供所需信息  

![][4]

## 角色激活  

为了激活角色，临时管理员需要请求时间限制"激活"的角色。 可以在 Azure AD 特权身份管理中使用**激活我的角色**选项请求激活。

想要激活角色管理需要初始化在 Azure 预览门户的 Azure AD 特权身份管理。

任何类型的管理员可以使用 Azure AD 特权身份管理来激活他或她的角色。

角色激活是时间限制。 在角色激活设置中，您可以配置的激活，以及管理需求提供以激活该角色所需的信息的长度。

![][5]

## 角色激活历史记录

您还可以使用 Azure AD 特权身份管理跟踪特权的角色分配和角色激活历史记录中的更改。 这可以使用审核日志选项︰

![][6]

## 接下来是什么

[Microsoft Azure 博客](http://azure.microsoft.com/blog/)
[基于角色的访问控制](../role-based-access-control-configure.md)

<!--Image references-->
[1]: ./media/active-directory-privileged-identity-management-configure/Search_PIM.png
[2]: ./media/active-directory-privileged-identity-management-configure/PIM_Dash.png
[3]: ./media/active-directory-privileged-identity-management-configure/PIM_AddRemove.png
[4]: ./media/active-directory-privileged-identity-management-configure/PIM_RoleActivationSettings.png
[5]: ./media/active-directory-privileged-identity-management-configure/PIM_RequestActivation.png
[6]: ./media/active-directory-privileged-identity-management-configure/PIM_ActivationHistory.png

测试

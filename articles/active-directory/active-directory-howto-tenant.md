---
ms.openlocfilehash: fb6b6a50e0f6c1992fd90eb2b472b61caebb7244
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="如何获取 Azure AD 租户 |Microsoft Azure"
    description="Azure Active Directory 租户获得注册和构建应用程序的方式。"
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="terrylan"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="07/02/2015"
    ms.author="dastrock"/>

# 如何获取 Azure Active Directory 租户

在 Azure 活动目录 (AD Azure)[租户](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant)是一个组织的代表。  它是 Azure AD 服务组织接收，并拥有当 Microsoft 云服务，例如 Azure，Microsoft Intune 或 Office 365 的其签署的专用的实例。  每个 AD Azure 租户是截然不同且相互独立的其他 Azure AD 承租人。  

租户可容纳公司中的用户和他们的他们的密码、 用户配置文件数据、 权限等有关的信息。  它还可以包含组、 应用程序和其他属于组织和其安全性的信息。

允许 Azure AD 用户登录到您的应用程序，您必须注册您的应用程序在您自己的组织。  在 Azure AD 租户发布应用程序是**完全免费**的。  事实上，大多数开发人员将创建多个承租人和试验，开发，应用程序转移和测试目的。  企业注册和使用您的应用程序可以选择购买许可证，如果他们想要利用高级的目录功能。

因此，如何转有关获取 Azure AD 租户？  该进程可能很少的不同如果您︰

- [具有现有 Office 365 订阅](#use-an-existing-office-365-subscription)
- [具有现有 Azure 订阅 Microsoft 帐户关联](#use-an-msa-azure-subscription)
- [具有现有 Azure 订阅与组织的帐户](#use-an-organizational-azure-subscription)
- [没有任何以上和想要从头开始](#start-from-scratch)

## 使用现有的 Office 365 订阅
如果有现有 Office 365 订阅但没有 Azure 的订阅 （并且不能登录到[Azure 管理门户](https://manage.windowsazure.com)信息），请按照[这些说明](https://technet.microsoft.com/library/dn832618.aspx)访问 Azure AD 租户。

## 使用 MSA Azure 订阅
如果您曾经注册 Azure 的订阅使用单独的 Microsoft 帐户，您已经有一个租户 ！  在[Azure 管理门户](https://manage.windowsazure.com)中，您应该发现租户，名为"默认租户"列在"所有项目"和"活动目录。  您可以自由地使用本组织根据-但您可能希望创建组织的管理员帐户。

为此，请执行以下步骤。  或者，您可能想要创建一个新的租户，该租户遵循相似的过程中创建管理员。

1.  登录到[Azure 管理门户](https://manage.windowsazure.com)使用单独的帐户
2.  导航到 （在左侧的导航栏中找到） 门户"活动目录"部分
3.  在可用的目录列表中选择"默认目录"项
4.  单击用户链接在页面的顶部。  您将看到在"Microsoft 客户"源于从列中的值列表中的单个用户
5.  单击页面底部的"添加用户"
6.  在添加用户表单中提供了以下详细信息︰
    - 类型的用户︰ 您的组织中的新用户
    - 用户名: （选择此管理员的用户名）
    - 第一个姓名/上次显示名称/命名: （选择适当的值）
    - 角色︰ 全局管理员
    - 备选电子邮件地址: （输入相应的值）
    - 可选︰ 启用多因素身份验证
    - 最后，单击绿色的"创建"按钮，以完成用户创建 （并显示该临时密码）。
7.  当您完成添加用户窗体中，并收到的新的管理用户的临时密码时，请务必记录此密码，因为您将需要使用该新用户登录方能更改密码。 您还可以直接对用户来说，使用其他的电子邮件中发送密码。
8.  若要更改该临时密码，登录到 https://login.microsoftonline.com 用这个新的用户帐户并更改密码请求时。


## 使用组织的 Azure 订阅
如果您曾经注册 Azure 的订阅与您组织的帐户，您已经有一个租户 ！  在[Azure 管理门户](https://manage.windowsazure.com)中，您应该发现承租人出"所有项目"和"活动目录。  您可以自由地使用本组织根据。  您可能还希望创建一个新的租户使用门户的左下角中的"新建"按钮。


## 从头开始
如果上述所有杂乱给您，请不要担心。  只需访问[https://account.windowsazure.com/organization](https://account.windowsazure.com/organization)注册 Azure 与新的组织。  完成此过程后，您必须与域名注册时选择了自己非常 Azure AD 租户。  在[Azure 管理门户](https://manage.windowsazure.com)中，您可以通过导航到"Active Directory"左手 nav.中找到您租户

作为注册 Azure 的过程的一部分，您将需要提供信用卡详细信息。  可以继续，不用担心-您将不会收取在 Azure 广告发布的应用程序或创建新的租户。

测试

---
ms.openlocfilehash: 4b357d2c86da960a3fdb919c5f41410f6d65a48e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="应用程序模型 v2.0 |Microsoft Azure"
    description="构建与 Microsoft 帐户和 Azure Active Directory 登录应用程序简介。"
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/12/2015"
    ms.author="dastrock"/>

# 应用程序模型 v2.0 预览︰ 在单个应用程序的登录 Microsoft 客户和 Azure AD 用户

> [AZURE.NOTE]
    此信息适用于 2.0 版应用程序模型的公共预览。  有关说明如何与普遍适用的 Azure AD 集成服务，请参考[Azure 活动目录开发指南 》](active-directory-developers-guide.md)。

在过去，应用程序开发人员希望支持 Microsoft 帐户和 Azure Active Directory 所需与两个单独的系统集成。 使用 2.0 版应用程序模型中，您可以现在签名使用这两种类型的帐户的用户。 一个简单的集成使您可以跨越数以百万计的个人和工作/学校帐户的用户群体。

您的应用程序也可以使用[设置 Office 365 REST Api 的](https://www.msdn.com/office/office365/howto/authenticate-Office-365-APIs-using-v2)使用两种类型的帐户。  目前，这些 Api 包含 Outlook 的邮件、 联系人和日历的 Api。  在不久的将来将会添加额外的服务。
<!-- TODO: customer reference article -->
<!-- Several apps have already begun to bridge the gap between consumer and enterprise accounts, including: [Boomerang](), [TripIt](), & [Uber](). -->

2.0 版应用程序模型是在预览。  在预览期间，我们都渴望听到您的反馈意见和获得的经验的新的应用程序模型与试验情况。  根据反馈意见，我们可能提出为了改善服务的重大更改。  不应该释放在此期间使用 2.0 版应用程序模型的生产应用程序。
<!-- TODO: Get approval on how it looks  -->

## 入门教程
有两种方法来获取您自己的应用程序最多和运行使用 2.0 版应用程序模型。  您可以选择发送协议消息直接使用[OAuth 2.0](active-directory-v2-protocols.md#oauth2-authorization-code-flow)或[打开 ID 连接](active-directory-v2-protocols.md#openid-connect-sign-in-flow)。  或者，您可以使用我们的库来为您完成的工作-选择您最喜爱的平台下面并开始。
<!-- TODO: Finalize this table  -->

[AZURE.INCLUDE [active-directory-v2-quickstart-table](../../includes/active-directory-v2-quickstart-table.md)]

## 新增功能
在此处检查通常以了解将来对 2.0 版应用程序模型的公共预览更改。  有关使用 @AzureAD 的任何更新，我们还将 tweet。

- 学习[类型的应用程序可以生成与应用程序模型 2.0 版](active-directory-v2-flows.md)。
- 对于开发人员熟悉 Azure Active Directory，您应该检查[更新我们的协议和 2.0 版应用程序模型中的差异](active-directory-v2-compare.md)。
- 当前[预览限制、 限制和约束](active-directory-v2-limitations.md)。

## 参考
这些链接将用于探索中深度的平台︰

- 获取有关堆栈溢出使用[azure 活动目录](http://stackoverflow.com/questions/tagged/azure-active-directory)的帮助或[adal](http://stackoverflow.com/questions/tagged/adal)标记。
- 预览使用[用户的语音](http://feedback.azure.com/forums/169401-azure-active-directory)上给我们您的想法-我们想要听 ！  使用短语"AppModelv2:"您的帖子，以便我们可以找到它的标题中。
- [应用程序模型 2.0 版协议引用](active-directory-v2-protocols.md)
- [应用程序模型 v2.0 令牌引用](active-directory-v2-tokens.md)
- [Office 365 REST API 参考](https://www.msdn.com/office/office365/howto/authenticate-Office-365-APIs-using-v2)
- [作用域和 v2 终结点中的同意](active-directory-v2-scopes.md)

<!-- TODO: These articles
- [ADAL Library Reference]()
- [v2 Endpoint FAQs](active-directory-v2-faq.md)
-->

测试

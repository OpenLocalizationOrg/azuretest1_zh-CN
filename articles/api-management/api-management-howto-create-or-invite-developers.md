---
ms.openlocfilehash: 01715f3a6d7fb2edca1a83b0944230786e5c1ee0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何管理 Azure API 管理中的用户帐户" 
    description="了解如何创建或邀请在 Azure API 管理用户" 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/16/2015" 
    ms.author="sdanie"/>

# 如何管理在 Azure API 管理用户帐户

在 API 管理中，开发人员使用 API 管理公开的 Api 的用户。 本指南介绍如何创建并邀请开发人员可以使用 Api 和产品可以提供给他们的 API 管理实例。

## <a name="create-developer"> </a>创建新的开发人员

若要创建新的开发人员，请单击**管理**API 管理服务 Azure 门户中。 这将您带到管理 API 发布门户。 如果尚未创建 API 管理服务实例，请[开始使用 Azure API 管理][]教程中参阅[创建 API 管理服务实例][]。

![出版商门户][api-management-management-console]

单击在左侧， **API 管理**菜单中的**开发人员**，然后单击**添加用户**。

![创建开发人员][api-management-create-developer]

输入新开发人员的**电子邮件**、**密码**和**名称**，并单击**保存**。

![创建开发人员][api-management-add-new-user]

默认情况下，新创建的开发人员帐户**活动**，并与**开发人员**组相关联。

![新开发人员][api-management-new-developer]

用于访问他们拥有订阅的 Api 的所有开发人员帐户处于不**活动**状态。 若要将新创建的开发人员与其他组相关联，请参阅[如何将组与开发人员相关联][]。

## <a name="invite-developer"> </a>邀请开发人员

要邀请开发人员，请单击在左侧， **API 管理**菜单中的**开发人员**，然后单击**邀请用户**。

![邀请开发者][api-management-invite-developer]

输入开发人员的姓名和电子邮件地址，然后单击**邀请**。

![邀请开发者][api-management-invite-developer-window]

将显示一条确认消息，但新邀请开发者之后，没有出现在列表中，直到接受邀请。 

![邀请确认][api-management-invite-developer-confirmation]

当开发人员已邀请时，给开发人员发送一封电子邮件。 这封电子邮件使用模板生成的和自定义。 有关详细信息，请参阅[配置电子邮件模板][]。

一旦接受邀请，则该帐户将被激活。

## <a name="block-developer"> </a> 停用或重新激活开发者帐户

默认情况下，新创建的或被邀请开发者帐户处于**活动状态**。 要停用某个开发人员帐户，请单击**块**。 要重新激活已阻止开发人员帐户，请单击**激活**。 被阻止的开发人员帐户可以访问开发人员门户或调用任何 Api。

![块开发人员][api-management-new-developer]

## <a name="next-steps"> </a>下一步行动

开发人员帐户创建后，可以将它与角色相关联，并订阅产品和 Api。 有关详细信息，请参阅[如何创建和使用组][]。


[api 管理管理控制台]: ./media/api-management-howto-create-or-invite-developers/api-management-management-console.png
[api 的管理-添加-新-用户]: ./media/api-management-howto-create-or-invite-developers/api-management-add-new-user.png
[api 的管理-创建的开发人员]: ./media/api-management-howto-create-or-invite-developers/api-management-create-developer.png
[api 管理-邀请开发者]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer.png
[api 管理-新开发]: ./media/api-management-howto-create-or-invite-developers/api-management-new-developer.png
[api 的管理-邀请-开发-窗口]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-window.png
[api 的管理-邀请-开发-确认]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-confirmation.png
[api 的管理-]: ./media/api-management-howto-create-or-invite-developers/api-management-.png



[创建新的开发人员]: #create-developer
[邀请开发人员]: #invite-developer
[停用或重新激活开发者帐户]: #block-developer
[下一步行动]: #next-steps
[如何创建和使用组]: api-management-howto-create-groups.md
[如何将组与开发人员相关联]: api-management-howto-create-groups.md#associate-group-developer

[Azure API 管理入门]: api-management-get-started.md
[创建 API 管理服务实例]: api-management-get-started.md#create-service-instance
[配置电子邮件模板]: api-management-howto-configure-notifications.md#email-templates
测试t

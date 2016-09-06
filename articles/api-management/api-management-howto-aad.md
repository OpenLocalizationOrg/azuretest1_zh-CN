---
ms.openlocfilehash: cf167e697e2374042d6f465c503241df3d0fac52
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何授权使用 Azure Active Directory Azure API 管理中的开发人员帐户" 
    description="了解如何授权用户使用 Azure Active Directory API 管理中。" 
    services="api-management" 
    documentationCenter="API Management" 
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

# 如何授权使用 Azure Active Directory Azure API 管理中的开发人员帐户


## 概述
本指南介绍了如何启用一个或多个 Azure 活动目录中的所有用户对开发人员门户的访问。 本指南还介绍了如何通过添加外部组包含的 Azure Active Directory 用户管理 Azure Active Directory 用户组。

>若要完成本指南中的步骤首先必须 Azure 的 Active Directory 中要创建的应用程序。

## 如何授权使用 Azure Active Directory 的开发人员帐户

若要开始，请单击 Azure 门户 API 管理服务中的**管理**。 这将您带到管理 API 发布门户。

![出版商门户][api-management-management-console]

>如果尚未创建 API 管理服务实例，请[开始使用 Azure API 管理][]教程中参阅[创建 API 管理服务实例][]。

从左侧的**API 管理**菜单中单击**安全**，然后单击**外部标识**。

![外部标识][api-management-security-external-identities]

单击**Azure 的活动目录**。 记下的**重定向 URL**和开关到 Azure 活动目录在 Azure 门户。

![外部标识][api-management-security-aad-new]

单击**添加**按钮创建新的 Active Directory 的 Azure 应用程序，然后选择**添加我的公司正在开发的应用程序**。

![添加新的 Azure Active Directory 应用程序][api-management-new-aad-application-menu]

为应用程序输入一个名称，选择**Web 应用程序和/或 Web API**，并单击下一步按钮。

![新的 Azure Active Directory 应用程序][api-management-new-aad-application-1]

对于**登录 URL**， **Azure Active Directory**部分出版商门户中的**外部标识**选项卡的复制**重定向 URL**并删除 URL 末尾**的 aad**后缀。 在此示例中，**登录 URL**是`https://aad03.portal.current.int-azure-api.net/signin`。 

对于**应用程序 ID 的 URL**，Azure Active Directory 中，输入默认域或自定义域，将一个唯一的字符串追加到它。 在此示例中的**https://contoso5api.onmicrosoft.com**的默认域使用**/api**指定的后缀。

![新 Azure Active Directory 应用程序属性][api-management-new-aad-application-2]

单击检查按钮以保存并创建新的应用程序，然后切换到**配置**选项卡来配置新的应用程序。

![新创建的 Azure Active Directory 应用程序][api-management-new-aad-app-created]

如果多个 Azure 活动目录要用于此应用程序，请单击**是****应用程序是多租户**的。 默认值为**否**。

![应用程序是多租户][api-management-aad-app-multi-tenant]

从出版商门户中的**外部标识**选项卡的**Azure Active Directory**部分复制**重定向的 URL**并将其粘贴到**回复 URL**文本框。 

![回复 URL][api-management-aad-reply-url]

滚动到底部的配置选项卡，选择**应用程序权限**下拉列表，并检查**读取目录数据**。

![应用程序权限][api-management-aad-app-permissions]

选中**代理权限**下拉列表，并检查**启用登录和阅读用户的配置文件**。

![委派的权限][api-management-aad-delegated-permissions]

>有关应用程序和委派的权限的详细信息，请参阅[访问图形 API][]。

将**客户机 Id**复制到剪贴板。

![客户机 Id][api-management-aad-app-client-id]

切换回发布服务器门户并粘贴来自 Active Directory 的 Azure 应用程序配置的**客户端 Id** 。

![客户机 Id][api-management-client-id]

切换到 Azure Active Directory 配置，单击**选择持续时间**下拉列表中的**密钥**部分中，指定一个时间间隔。 在此示例中使用**1 年**。

![Key][api-management-aad-key-before-save]

单击**保存**以保存配置并显示该密钥。 将密钥复制到剪贴板。

>记下此注册表项。 一旦您关闭 Azure Active Directory 配置窗口，不能再次显示键。

![Key][api-management-aad-key-after-save]

切换回发布服务器门户和**客户端密钥**文本框中粘贴键。

![客户端密码][api-management-client-secret]

**允许承租人**指定哪些目录有权访问的 API 管理服务实例的 Api。 指定要向其授予访问权限的 Azure Active Directory 实例的域。 可以使用换行符、 空格或逗号来分隔多个域。

![允许的承租人][api-management-client-allowed-tenants]

可**允许承租人**一节中指定多个域。 任何用户可以从不同的域中注册应用程序的原始域登录之前，不同域的全局管理员必须授予权限访问目录数据的应用程序。 若要授予权限，全局管理员必须登录到应用程序并单击**接受**。 下面的示例在`miaoaad.onmicrosoft.com`已添加到**允许承租人**和全局在这个域中的管理员登录第一次。

![权限][api-management-permissions-form]

>如果非全局管理员尝试之前授予由全局管理员的权限登录时，登录尝试失败并显示错误屏幕的错误。

一旦指定了所需的配置，单击**保存**。

![保存][api-management-client-allowed-tenants-save]

一旦保存更改，在指定的 Azure Active Directory 中的用户可以登录到开发人员门户中[登录到使用 Azure Active Directory 帐户开发人员门户中][]的步骤。

## 如何添加外部 Azure 活动目录组

启用后的 Azure Active Directory 中的用户访问权限，可以到 API 管理更易于管理的组中的开发人员与所需产品的关联添加 Azure Active Directory 组。

> 要配置外部 Azure Active Directory 组，Azure Active Directory 必须首先配置在标识选项卡上一节中的过程。 

从产品您想要授予访问权限的组的**可见性**选项卡中添加外部 Azure Active Directory 组。 单击**产品**，然后单击所需的产品的名称。

![配置产品][api-management-configure-product]

切换到**可见性**选项卡，然后单击**添加组从 Azure Active Directory**。

![添加组][api-management-add-groups]

从下拉列表中，选择**Azure 活动目录租户**，然后**组**要添加文本框中键入所需的组的名称。

![选择组][api-management-select-group]

此组的名称可以找到在**组**列表中将 Azure Active Directory，如下面的示例中所示。

![Azure 的 Active Directory 组列表][api-management-aad-groups-list]

单击**添加**以验证组的名称并添加组。 在此示例中**Contoso 5 开发人员**添加外部组。 

![添加组][api-management-aad-group-added]

单击**保存**以保存新的组选择。

Azure Azure Active Directory 组已从一个产品配置，则可用的 API 管理服务实例中的其他产品的**可见性**选项卡上检查。

若要查看和配置外部组的属性，添加后，单击**组**选项卡中的组的名称。

![管理组][api-management-groups]

从这里您可以编辑**名称**和组的**说明**。

![编辑组][api-management-edit-group]

在已配置的 Azure Active Directory 中的用户可以登录到的开发人员门户和查看和订阅它们具有可见性按照下一节中的说明的任何组。

## 如何登录到使用 Azure Active Directory 帐户开发人员门户中

要登录到使用 Azure Active Directory 帐户配置前面各节中的开发人员门户，打开使用**登录 URL**从活动目录的应用程序配置中，一个新的浏览器窗口，然后单击**Azure Active Directory**。

![开发人员门户][api-management-dev-portal-signin]

Azure 活动目录，在输入一个用户的凭据，然后单击**登录**。

![登录][api-management-aad-signin]

您可能会提示您注册窗体如果还需要任何其他信息。 完成注册表单并单击**注册**。

![注册 ][api-management-complete-registration]

现在，您的用户已登录到 API 管理服务实例的开发人员门户。

![完成注册][api-management-registration-complete]



[api 管理管理控制台]: ./media/api-management-howto-aad/api-management-management-console.png
[api 的管理-安全-外部的标识]: ./media/api-management-howto-aad/api-management-security-external-identities.png
[api 的管理-安全-aad 的新]: ./media/api-management-howto-aad/api-management-security-aad-new.png
[api-management-new-aad-application-menu]: ./media/api-management-howto-aad/api-management-new-aad-application-menu.png
[api-management-new-aad-application-1]: ./media/api-management-howto-aad/api-management-new-aad-application-1.png
[api-management-new-aad-application-2]: ./media/api-management-howto-aad/api-management-new-aad-application-2.png
[api-management-new-aad-app-created]: ./media/api-management-howto-aad/api-management-new-aad-app-created.png
[api 的管理-aad 的应用程序的权限]: ./media/api-management-howto-aad/api-management-aad-app-permissions.png
[api-management-aad-app-client-id]: ./media/api-management-howto-aad/api-management-aad-app-client-id.png
[api 管理客户端 id]: ./media/api-management-howto-aad/api-management-client-id.png
[api-management-aad-key-before-save]: ./media/api-management-howto-aad/api-management-aad-key-before-save.png
[api-management-aad-key-after-save]: ./media/api-management-howto-aad/api-management-aad-key-after-save.png
[api 管理客户端密码]: ./media/api-management-howto-aad/api-management-client-secret.png
[api 的管理的客户端-允许的租户]: ./media/api-management-howto-aad/api-management-client-allowed-tenants.png
[api-management-client-allowed-tenants-save]: ./media/api-management-howto-aad/api-management-client-allowed-tenants-save.png
[api 的管理-aad 的委派的权限]: ./media/api-management-howto-aad/api-management-aad-delegated-permissions.png
[api 的管理-开发-门户-登录]: ./media/api-management-howto-aad/api-management-dev-portal-signin.png
[api 管理 aad 登录]: ./media/api-management-howto-aad/api-management-aad-signin.png
[api 管理-完成的注册]: ./media/api-management-howto-aad/api-management-complete-registration.png
[api 的管理--完成注册]: ./media/api-management-howto-aad/api-management-registration-complete.png
[api-management-aad-app-multi-tenant]: ./media/api-management-howto-aad/api-management-aad-app-multi-tenant.png
[api 的管理-aad 的答复-url]: ./media/api-management-howto-aad/api-management-aad-reply-url.png
[api 管理权限表单]: ./media/api-management-howto-aad/api-management-permissions-form.png
[api 的管理-配置-产品]: ./media/api-management-howto-aad/api-management-configure-product.png
[api 的管理-添加的组]: ./media/api-management-howto-aad/api-management-add-groups.png
[api 管理选择组]: ./media/api-management-howto-aad/api-management-select-group.png
[api 的管理-aad 的组的列表]: ./media/api-management-howto-aad/api-management-aad-groups-list.png
[api 的管理-aad--添加组]: ./media/api-management-howto-aad/api-management-aad-group-added.png
[api 管理组]: ./media/api-management-howto-aad/api-management-groups.png
[api 管理编辑组]: ./media/api-management-howto-aad/api-management-edit-group.png

[如何将操作添加到 API]: api-management-howto-add-operations.md
[如何添加和发布产品]: api-management-howto-add-products.md
[监控和分析]: api-management-monitoring.md
[向产品中添加 Api]: api-management-howto-add-products.md#add-apis
[发布产品]: api-management-howto-add-products.md#publish-product
[Azure API 管理入门]: api-management-get-started.md
[开始使用高级 API 配置]: api-management-get-started-advanced.md
[API 管理策略参考]: api-management-policy-reference.md
[缓存策略]: api-management-policy-reference.md#caching-policies
[创建 API 管理服务实例]: api-management-get-started.md#create-service-instance

[http://oauth.net/2/]: http://oauth.net/2/
[WebApp-GraphAPI-最低]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[访问图形 API]: http://msdn.microsoft.com/library/azure/dn132599.aspx#BKMK_Graph

[先决条件]: #prerequisites
[OAuth 2.0 授权服务器配置 API 管理]: #step1
[API 配置为使用 OAuth 2.0 用户授权]: #step2
[在开发人员门户中测试 OAuth 2.0 用户授权]: #step3
[下一步行动]: #next-steps

[登录到使用 Azure Active Directory 帐户开发人员门户中]: #Log-in-to-the-Developer-portal-using-an-Azure-Active-Directory-account


测试

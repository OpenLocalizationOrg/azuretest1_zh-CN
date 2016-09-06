---
ms.openlocfilehash: acb73d33836af36a4d1a8578754109a4c8cb4a57
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何创建和使用组来管理 Azure API 管理中的开发人员帐户" 
    description="了解如何管理开发人员帐户使用 Azure API 管理组" 
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
    ms.date="07/22/2015" 
    ms.author="sdanie"/>

# 如何创建和使用组来管理 Azure API 管理中的开发人员帐户

在 API 管理组用于管理产品给开发人员的可见性。 产品第一次到组，可见，然后在这些组中的开发人员可以查看并订阅与组相关联的产品。 

API 管理有以下不可变的系统组。

-   **管理员**-Azure 预订管理员是此组的成员。 管理员管理 API 管理服务实例，创建 Api、 操作和开发人员所使用的产品。
-   **开发人员**的经过身份验证的开发人员门户用户属于此组。 开发人员可以使用您的 Api 构建应用程序的客户。 开发人员对开发人员门户进行访问并生成调用操作的 API 的应用程序。
-   **客人**的未经身份验证的开发人员门户的用户，如潜在客户访问开发人员门户的 API 管理实例属于此组。 他们可以授予某些只读访问权限，如查看 Api，但不是调用它们的能力。

除了这些系统组，管理员可以创建自定义组或[利用外部组中关联的 Azure Active Directory 承租人][]。 可以与系统组让开发人员可见性和访问 API 产品同时使用自定义和外部组。 例如，可以为特定的伙伴组织与相关的开发人员创建一个自定义组，允许它们访问 Api 从产品包含相关的 Api。 用户可以是多个组的成员。

本指南介绍了如何添加新组并将它们关联产品和开发 API 管理实例的管理员。

## <a name="create-group"> </a>创建组

若要创建一个新组，请单击 Azure 门户 API 管理服务中的**管理**。 这将您带到管理 API 发布门户。

![出版商门户][api-management-management-console]

>如果尚未创建 API 管理服务实例，请[开始使用 Azure API 管理][]教程中参阅[创建 API 管理服务实例][]。

单击在左侧， **API 管理**菜单中的**组**，然后单击**添加组**。

![添加新组][api-management-add-group]

输入组和可选的描述，一个唯一的名称，然后单击**保存**。

![添加新组][api-management-add-group-window]

新组将显示在组选项卡。 若要编辑**名称**或组的**说明**，请单击列表中的组的名称。 要删除组，请单击**删除**。

![添加组][api-management-new-group]

现在，创建组后，它可以与产品和开发人员相关联。

## <a name="associate-group-product"> </a>与产品关联组

若要将一组与产品相关联，从**API 管理**菜单的左侧，单击**产品**，然后单击所需的产品的名称。

![设置可见性][api-management-add-group-to-product]

选择**可见性**选项卡上，添加和删除组，并查看产品的当前组。 若要添加或删除组，请选中或取消选中的复选框，所需的组，单击**保存**。

![设置可见性][api-management-add-group-to-product-visibility]

>[AZURE.NOTE] 若要添加 Azure Active Directory 组，请参阅[如何授权开发人员帐户使用 Azure 的 Active Directory 中 Azure API 管理](api-management-howto-aad.md)。
>
>若要配置产品的**可见性**选项卡中的组，请单击**管理组**。

与组相关联的产品后，该组中的开发人员可以查看和订阅该产品。

## <a name="associate-group-developer"> </a>将组与开发人员相关联

若要将组与开发人员相关联，从**API 管理**菜单的左侧，单击**用户**，然后选中复选框旁边的开发人员，您想要与组关联。

![将开发人员添加到组][api-management-add-group-to-developer]

一旦所需的开发人员进行检查，请单击**添加到组**下拉列表中所需的组。 通过使用**从组中删除**下拉列表，可以从组中删除开发人员。 

![开发人员][api-management-add-group-to-developer-saved]

一旦开发人员组之间添加关联，您可以在**用户**选项卡中查看它。

## <a name="next-steps"> </a>下一步行动

一旦开发人员添加到组中，可以查看并订阅与该组关联的产品。 有关详细信息，请参阅[如何创建并发布在 Azure API 管理产品][]，


[api 管理管理控制台]: ./media/api-management-howto-create-groups/api-management-management-console.png
[api 的管理-添加的组]: ./media/api-management-howto-create-groups/api-management-add-group.png
[api 的管理-添加的组的窗口]: ./media/api-management-howto-create-groups/api-management-add-group-window.png
[api 管理-新组]: ./media/api-management-howto-create-groups/api-management-new-group.png
[api-management-add-group-to-product]: ./media/api-management-howto-create-groups/api-management-add-group-to-product.png
[api-management-add-group-to-product-visibility]: ./media/api-management-howto-create-groups/api-management-add-group-to-product-visibility.png
[api-management-add-group-to-developer]: ./media/api-management-howto-create-groups/api-management-add-group-to-developer.png
[api-management-add-group-to-developer-saved]: ./media/api-management-howto-create-groups/api-management-add-group-to-developer-saved.png

[api 的管理-]: ./media/api-management-howto-create-groups/api-management-.png

[创建组]: #create-group
[与产品关联组]: #associate-group-product
[将组与开发人员相关联]: #associate-group-developer
[下一步行动]: #next-steps

[如何创建和发布 Azure API 管理产品]: api-management-howto-add-products.md

[Azure API 管理入门]: api-management-get-started.md
[创建 API 管理服务实例]: api-management-get-started.md#create-service-instance
[利用外部组中关联的 Azure Active Directory 承租人]: api-management-howto-aad.md#how-to-add-an-external-azure-active-directory-group

测试

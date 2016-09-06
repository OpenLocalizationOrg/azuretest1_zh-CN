---
ms.openlocfilehash: 0f6a1ebea892d34405d0065c1cf6fa23e12cd42c
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何在 Azure API 管理中创建 Api、 操作和产品" 
    description="了解如何创建 API 管理 Api、 操作和产品。" 
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
    ms.date="08/11/2015" 
    ms.author="sdanie"/>

# 如何在 Azure API 管理中创建 Api、 操作和产品

在 Azure API 管理，Api 和它们的操作将添加到位置可以构建使用的 Api 的应用程序开发人员通过使用它们的产品。 本节中的指南演示了如何创建 API、 将操作添加到其中，然后将 API 与产品相关联并发布它让开发者使用。

## <a name="create-apis"> </a>如何创建 Api

在 API 管理 API 表示一组可以通过客户端应用程序调用的操作。 在 API 管理门户中创建新的 Api。

本指南介绍了如何创建和配置 API 管理中的一个新的 API。

-   [如何创建 Api][]

## <a name="add-operations"> </a>如何将操作添加到 API

可以使用 API 管理 API 之前，必须添加操作。 本指南介绍了如何添加和配置 API 管理中的不同类型的 api 操作。

-   [如何将操作添加到 API][]

API 和其操作还可以导入中 WADL 或 Swagger 格式的一个步骤。

-   [如何导入操作的 API 定义][]

## <a name="add-product"> </a>如何创建和发布产品

在 API 管理产品包含一个或多个 Api，以及使用配额和使用条款。 一旦产品发布，则开发人员可以订阅产品和开始使用该产品的 Api。 这些主题提供如何创建产品、 添加一个 API，并将其发布为开发人员提供的指南。

-   [如何添加和发布产品][]
-   [如何创建和配置高级的产品设置][]

[创建产品]: #create-product
[向产品中添加 Api]: #add-apis
[向产品中添加描述性信息]: #add-description
[发布产品]: #publish-product
[使产品开发人员对可见]: #make-visible
[查看订阅服务器产品]: #view-subscribers
[下一步行动]: #next-steps

[api 的管理-]: ./media/

[如何创建 Api]: api-management-howto-create-apis.md
[如何将操作添加到 API]: api-management-howto-add-operations.md
[如何添加和发布产品]: api-management-howto-add-products.md
[监控和分析]: ../api-management-monitoring.md
[如何导入操作的 API 定义]: api-management-howto-import-api.md
[如何创建和配置高级的产品设置]: api-management-howto-product-with-rules.md 
测试t

---
ms.openlocfilehash: 8bc371feb1cf4cd352e7b927c3657d4b84146ec9
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="添加缓存以提高性能在 Azure API 管理"
    description="了解如何提高延迟、 带宽占用量和 API 管理服务调用的 web 服务加载。"
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
    ms.topic="get-started-article" 
    ms.date="08/05/2015"
    ms.author="sdanie"/>

# 添加缓存以提高性能在 Azure API 管理

可以为响应缓存配置 API 管理中的操作。 响应缓存可以显著减少 API 延迟、 带宽占用量和 web 服务负载并不经常更改的数据。

本指南介绍了如何添加响应缓存，您的 API 和配置示例回响 Api 操作的策略。 然后可以从开发人员门户来验证缓存操作中调用操作。


## 先决条件

在执行此步骤之前本指南，必须使用 API 和产品配置 API 管理服务实例。 如果尚未创建 API 管理服务实例，请参阅[创建 API 管理服务实例][] [Azure API 管理入门][]教程中。

## <a name="configure-caching"> </a>配置缓存的操作

在此步骤中，您可以回顾回响 Api 的示例**获得资源 （缓存的）**操作的缓存设置。

>[AZURE.NOTE] 每个 API 管理服务实例都预先配置了与 Echo API，可用于试验和了解 API 管理。 有关详细信息，请参阅[开始使用 Azure API 管理][]。

若要开始，请单击 Azure 门户 API 管理服务中的**管理**。 这将您带到管理 API 发布门户。

![出版商门户][api-management-management-console]

从**API 管理**菜单的左侧，单击**Api**然后单击**回响 API**。

![回音式 API][api-management-echo-api]

选择**操作**选项卡，然后单击**操作**列表中**获取资源，（缓存的）**操作。

![回音式 API 操作][api-management-echo-api-operations]

选择**缓存**选项卡以查看此操作的缓存设置。

![高速缓存选项卡][api-management-caching-tab]

要启用缓存的操作，请检查**启用**复选框。 在此示例中启用缓存。

每个操作响应键基于**查询字符串参数的变化**和**变化的标头**字段中的值。 如果您希望缓存多个基于查询字符串参数或头的响应，可以在这两个字段中进行配置。

**持续时间**指定缓存的响应的过期间隔。 在此示例中的间隔是**3600**秒，它相当于一个小时。

在此示例中，使用缓存的配置，**获得资源 （缓存的）**操作的第一个请求将返回响应，从后端服务。 将缓存此响应，关键字指定的标头和查询字符串参数。 随后调用该操作，具有匹配的参数，将缓存的响应的缓存持续时间间隔已过期之前返回。

## <a name="caching-policies"> </a>查看缓存策略

在此步骤中，您将查看回响 Api 的示例**获得资源 （缓存的）**操作的缓存设置。

缓存设置配置为**缓存**选项卡上的操作，操作会添加缓存策略。 这些策略都可以查看和编辑策略编辑器中。

从**API 管理**菜单的左侧，单击**策略**，然后选择**回声 API / 获得资源 （缓存的）**从**操作**下拉列表。

![策略的作用域操作][api-management-operation-dropdown]

这在策略编辑器中显示此操作的策略。

![API 管理策略编辑器][api-management-policy-editor]

此操作的策略定义包括的策略定义的缓存配置，使用上一步中的**缓存**选项卡进行了考察。

    <policies>
        <inbound>
            <base />
            <cache-lookup vary-by-developer="false" vary-by-developer-groups="false">
                <vary-by-header>Accept</vary-by-header>
                <vary-by-header>Accept-Charset</vary-by-header>
            </cache-lookup>
            <rewrite-uri template="/resource" />
        </inbound>
        <outbound>
            <base />
            <cache-store caching-mode="cache-on" duration="3600" />
        </outbound>
    </policies>

>对缓存策略在策略编辑器中所做的更改将反映在**缓存**选项卡的操作，反之亦然。

## <a name="test-operation"> </a>调用的操作和测试缓存

若要查看缓存的作用，我们可以从开发人员门户中调用操作。 顶部右菜单中单击**开发人员门户**。

![开发人员门户][api-management-developer-portal-menu]

顶部的菜单中单击**Api**并选择**回响 API**。

![回音式 API][api-management-apis-echo-api]

>如果您有一个 API 配置或可见到您的帐户，然后单击 Api 使您直接进入该 API 的操作。

选择**（缓存） 的获取资源**的操作，然后单击**打开控制台**。

![打开的控制台][api-management-open-console]

控制台允许您调用操作直接从开发人员门户。

![控制台][api-management-console]

请为**参数 1** ，**参数 2**的默认值。

从**订阅项**下拉列表中选择所需的密钥。 如果您的帐户已被选中的只有一个订阅。

**请求标头**的文本框中输入**sampleheader:value1** 。

单击**HTTP Get** ，记下的响应标头。

**请求标头**的文本框中输入**sampleheader:value2** ，然后单击**HTTP Get**。

请注意， **sampleheader**的值仍然为**value1**响应中。 尝试一些不同的值，请注意，返回第一个调用的缓存的响应。

在**参数 2**字段中，输入**25**并单击**HTTP Get**。

请注意， **sampleheader**在响应中的值现在**值 2**。 操作结果的关键字查询字符串，因为未返回以前缓存的响应。

## <a name="next-steps"> </a>下一步行动

-   查看[高级 API 配置入门][]教程中的其他主题。
-   缓存策略的详细信息，请参阅[API 管理策略引用][]中的[缓存策略][]。

[api 管理管理控制台]: ./media/api-management-howto-cache/api-management-management-console.png
[api 管理回响 api]: ./media/api-management-howto-cache/api-management-echo-api.png
[api 的管理-echo 的 api 的操作]: ./media/api-management-howto-cache/api-management-echo-api-operations.png
[api 的管理-缓存的选项卡]: ./media/api-management-howto-cache/api-management-caching-tab.png
[api 管理的操作下拉列表]: ./media/api-management-howto-cache/api-management-operation-dropdown.png
[api 管理策略编辑器]: ./media/api-management-howto-cache/api-management-policy-editor.png
[api 的管理-开发-门户-菜单]: ./media/api-management-howto-cache/api-management-developer-portal-menu.png
[api 的管理的 api-回响的 api]: ./media/api-management-howto-cache/api-management-apis-echo-api.png
[api 管理打开控制台]: ./media/api-management-howto-cache/api-management-open-console.png
[api 管理控制台]: ./media/api-management-howto-cache/api-management-console.png


[如何将操作添加到 API]: api-management-howto-add-operations.md
[如何添加和发布产品]: api-management-howto-add-products.md
[监控和分析]: api-management-monitoring.md
[向产品中添加 Api]: api-management-howto-add-products.md#add-apis
[发布产品]: api-management-howto-add-products.md#publish-product
[Azure API 管理入门]: api-management-get-started.md
[开始使用高级 API 配置]: api-management-get-started-advanced.md

[API 管理策略参考]: https://msdn.microsoft.com/library/azure/dn894081.aspx
[缓存策略]: https://msdn.microsoft.com/library/azure/dn894086.aspx

[创建 API 管理服务实例]: api-management-get-started.md#create-service-instance

[配置缓存的操作]: #configure-caching
[查看缓存策略]: #caching-policies
[调用的操作和测试缓存]: #test-operation
[下一步行动]: #next-steps

测试

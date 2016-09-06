---
ms.openlocfilehash: 9085ca93e746fc3d57f291f5e3ab4cdb599e98ea
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何在 Azure API 管理中创建 Api" 
    description="了解如何创建和配置在 Azure API 管理 Api。" 
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

# 如何在 Azure API 管理中创建 Api

在 API 管理 API 表示一组可以通过客户端应用程序调用的操作。 在出版商门户中，创建新的 Api，然后添加所需的操作。 一旦添加操作，API 将添加到产品，可以发布。 一旦发布 API，它可供订阅和由开发人员使用。

本指南显示进程中的第一步︰ 如何创建和配置 API 管理中的一个新的 API。 添加操作和发布产品的详细信息，请参阅[如何添加 api 操作][]以及[如何创建和发布产品][]。

## <a name="create-new-api"> </a>创建一个新的 API

创建和配置发布服务器门户中的 Api。 若要访问发布者门户，单击**管理**Azure API 管理服务门户中。

![出版商门户][api-management-management-console]

>如果尚未创建 API 管理服务实例，请[开始使用 Azure API 管理][]教程中参阅[创建 API 管理服务实例][]。

单击在左侧， **API 管理**菜单中的**Api** ，然后单击**添加 API**。

![创建 API][api-management-create-api]

使用**添加新的 API**窗口配置新的 API。

![添加新的 API][api-management-add-new-api]

以下三个字段用于配置新的 API。

-   **Web API 标题**提供 API 的唯一的、 描述性名称。 它显示在开发人员和管理门户。
-   **Web 服务 URL**引用实现 API 的 HTTP 服务。 API 管理转发请求到此地址。
-   **Web API URL 后缀**将附加到 API 管理服务的基 URL。 基 URL 是常见的以 API 管理服务实例为宿主的所有 Api。 API 管理由其后缀区分 Api，因此后缀必须是唯一的每个 API 为给定的发布服务器。
-   **Web API URL 方案**确定哪些协议可用于访问 API。 默认情况下指定 HTTPs。

一旦配置了三个值，则单击**保存**。 一旦创建新的 API，该 API 的摘要页显示在管理门户。

![API 摘要][api-management-api-summary]

## <a name="configure-api-settings"> </a>配置 API 设置

**设置**选项卡可用于验证和编辑的配置 API。 **Web API 标题**、 **Web 服务 URL**和**Web API URL 后缀**最初设置 API 创建可修改时这里。 **说明**提供的可选说明和**Web API URL 方案**确定哪些协议可用于访问 API。

![API 设置][api-management-api-settings]

若要为 web 服务实现 API 配置**代理服务器身份验证**，请选择**安全**选项卡。 **凭据**下拉列表可用于配置**基本身份验证**或**使用相互证书**身份验证。 若要使用基本身份验证，只需输入所需的凭据。 使用相互证书身份验证的信息，请参阅[如何安全使用相互证书身份验证在 Azure API 管理的后端服务][]。

**安全**选项卡还可以用于配置使用 OAuth 2.0**用户授权**。 有关详细信息，请参阅[如何授权开发人员帐户使用 OAuth 2.0 在 Azure API 管理][]。

![基本身份验证设置][api-management-api-settings-credentials]

单击**保存**以保存对 API 设置进行任何更改。

## <a name="next-steps"> </a>下一步行动

一旦 API 创建和配置的设置，接下来的步骤是将操作添加到 API，将 API 添加到一个产品，并发布它，这样就可供开发人员。 有关详细信息，请参阅以下两条参考线。

-   [如何将操作添加到 API][]
-   [如何创建和发布产品][]





[api 的管理-创建的 api]: ./media/api-management-howto-create-apis/api-management-create-api.png
[api 管理管理控制台]: ./media/api-management-howto-create-apis/api-management-management-console.png
[api 的管理-添加的新的 api]: ./media/api-management-howto-create-apis/api-management-add-new-api.png
[api 管理 api 设置]: ./media/api-management-howto-create-apis/api-management-api-settings.png
[api 的管理的 api-设置-凭据]: ./media/api-management-howto-create-apis/api-management-api-settings-credentials.png
[api 管理 api 摘要]: ./media/api-management-howto-create-apis/api-management-api-summary.png
[api 管理 echo 操作]: ./media/api-management-howto-create-apis/api-management-echo-operations.png

[什么是 API？]: #what-is-api
[创建一个新的 API]: #create-new-api
[配置 API 设置]: #configure-api-settings
[配置 API 操作]: #configure-api-operations
[下一步行动]: #next-steps

[如何将操作添加到 API]: api-management-howto-add-operations.md
[如何创建和发布产品]: api-management-howto-add-products.md

[Azure API 管理入门]: api-management-get-started.md
[创建 API 管理服务实例]: api-management-get-started.md#create-service-instance
[如何确保后端服务在 Azure API 管理使用相互证书身份验证的安全]: api-management-howto-mutual-certificates.md
[如何授权开发人员帐户在 Azure API 管理使用 OAuth 2.0]: api-management-howto-oauth2.md
测试t

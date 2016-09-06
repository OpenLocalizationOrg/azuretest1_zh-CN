---
ms.openlocfilehash: c06fba718a05cb2d026272d97be25ea2e0b9b8fc
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何确保后端服务在 Azure API 管理使用相互证书身份验证的安全" 
    description="了解如何安全使用相互证书身份验证 Azure API 管理中的后端服务。" 
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
    ms.date="06/18/2015" 
    ms.author="sdanie"/>

# 如何确保后端服务在 Azure API 管理使用相互证书身份验证的安全

API 管理让您能够安全地访问到后端服务的 API 使用相互证书。 本指南介绍了如何管理证书在 API 发布门户中，以及如何配置使用证书来访问它的后端服务的 API。

有关管理证书使用 API 管理 REST API 的信息，请参阅[Azure API 管理 REST API 证书实体][]。

## <a name="prerequisites"> </a>先决条件

本指南介绍了如何配置您的 API 管理服务实例使用相互证书身份验证来访问后端服务 API。 本主题中的步骤操作之前，应已配置为相互证书身份验证，您的后端服务和有权证书和密码 API 管理发布门户中上传的证书。

## <a name="step1"> </a>上载的客户端证书

若要开始，请单击 Azure 门户 API 管理服务中的**管理**。 这将您带到管理 API 发布门户。

![API 发布门户][api-management-management-console]

>如果尚未创建 API 管理服务实例，请[开始使用 Azure API 管理][]教程中参阅[创建 API 管理服务实例][]。

从**API 管理**菜单的左侧，单击**安全**，然后单击**客户端证书**。

![客户端证书][api-management-security-client-certificates]

若要上载一个新证书，请单击**上载证书**。

![将上载证书][api-management-upload-certificate]

浏览到您的证书，然后输入该证书的密码。

>证书必须是以**.pfx**格式。 允许使用自签名的证书。

![将上载证书][api-management-upload-certificate-form]

请单击**上载**上载证书。

>这一次验证的证书密码。 如果它不正确显示一条错误消息。

![上载的证书][api-management-certificate-uploaded]

一旦上载证书，它将出现在**客户端证书**选项卡上。 如果您有多个证书，请记下的该主题或指纹，后四个字符，用来根据下[一个 API，用于使用相互证书进行代理身份验证配置][]节中的介绍配置要使用的证书，API 时选择的证书。

## <a name="step1a"> </a>删除客户端证书

若要删除证书，请单击所需的证书旁边的**删除**。

![删除证书][api-management-certificate-delete]

单击**是，删除它**以确认。

![确认删除][api-management-confirm-delete]

如果证书是由 API 的使用中，将显示警告屏幕。 要删除的证书必须首先从配置为使用它的任何 Api 删除证书。

![确认删除][api-management-confirm-delete-policy]

## <a name="step2"> </a>API 配置为使用相互证书进行代理身份验证

单击左侧**API 管理**菜单上的**Api**单击所需的 API 中，名称，单击**安全**选项卡。

![API 的安全][api-management-api-security]

从**凭据**下拉列表中选择**使用相互证书**。

![使用相互证书][api-management-mutual-certificates]

从**客户端证书**下拉列表中选择所需的证书。 如果有多个证书，您可以查看的主题或指纹上一节中所述的最后四个字符来确定正确的证书。

![选择证书][api-management-select-certificate]

单击**保存**以将配置更改保存到该 API。

>此更改会立即生效，并对该 API 的操作的调用将使用证书进行身份验证后端服务器上。

![保存 API 更改][api-management-save-api]

>指定证书时进行代理身份验证后端服务的 API，它成为策略的一部分，该 API，并可以在策略编辑器中查看。

![证书策略][api-management-certificate-policy]

## 下一步行动

有关详细信息，请参见下面的视频。

> [AZURE.VIDEO last-mile-security]

[api 管理管理控制台]: ./media/api-management-howto-mutual-certificates/api-management-management-console.png
[api 的管理的安全的客户端-证书]: ./media/api-management-howto-mutual-certificates/api-management-security-client-certificates.png
[api 管理上载证书]: ./media/api-management-howto-mutual-certificates/api-management-upload-certificate.png
[api 管理-上载的证书的格式]: ./media/api-management-howto-mutual-certificates/api-management-upload-certificate-form.png
[api 的管理-证书上载]: ./media/api-management-howto-mutual-certificates/api-management-certificate-uploaded.png
[api 管理 api 安全]: ./media/api-management-howto-mutual-certificates/api-management-api-security.png
[api 管理相互证书]: ./media/api-management-howto-mutual-certificates/api-management-mutual-certificates.png
[api 管理选择证书]: ./media/api-management-howto-mutual-certificates/api-management-select-certificate.png
[api 管理存储 api]: ./media/api-management-howto-mutual-certificates/api-management-save-api.png
[api 管理证书策略]: ./media/api-management-howto-mutual-certificates/api-management-certificate-policy.png
[api 管理证书删除]: ./media/api-management-howto-mutual-certificates/api-management-certificate-delete.png
[api 的管理-确认-删除]: ./media/api-management-howto-mutual-certificates/api-management-confirm-delete.png
[api 的管理-确认-删除-策略]: ./media/api-management-howto-mutual-certificates/api-management-confirm-delete-policy.png



[如何将操作添加到 API]: api-management-howto-add-operations.md
[如何添加和发布产品]: api-management-howto-add-products.md
[监控和分析]: ../api-management-monitoring.md
[向产品中添加 Api]: api-management-howto-add-products.md#add-apis
[发布产品]: api-management-howto-add-products.md#publish-product
[Azure API 管理入门]: api-management-get-started.md
[开始使用高级 API 配置]: api-management-get-started-advanced.md
[API 管理策略参考]: api-management-policy-reference.md
[缓存策略]: api-management-policy-reference.md#caching-policies
[创建 API 管理服务实例]: api-management-get-started.md#create-service-instance

[Azure 的 API 管理 REST API 证书实体]: http://msdn.microsoft.com/library/azure/dn783483.aspx
[WebApp-GraphAPI-最低]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet

[先决条件]: #prerequisites
[上载的客户端证书]: #step1
[删除客户端证书]: #step1a
[API 配置为使用相互证书进行代理身份验证]: #step2
[通过开发人员门户中调用操作测试配置]: #step3
[下一步行动]: #next-steps



 
测试

---
ms.openlocfilehash: 88e4c983b01c176b030a2a00addf7b6203a93ba0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="Azure API 管理策略参考" 
    description="了解有关可用于配置 API 管理策略。" 
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

# Azure API 管理策略参考

本部分提供索引中[API 管理策略参考][]的策略。 有关添加和配置策略的信息，请参阅[API 管理中的策略][]。

策略表达式可作为特性值或文本值放到任何 API 管理策略，除非该策略指定，否则。 某些策略，如[控制流][]和[将变量设置][]策略基于策略表达式。 有关详细信息，请参阅[高级策略][]和[策略表达式][]

## 策略引用索引

-   [访问限制策略][]
    -   [检查 HTTP 标头][]的强制执行，是否存在和/或 HTTP 标头的值。
    -   通过限制调用和/或带宽消耗率高峰[限制调用速率][]-防止 API 的使用情况。
    -   [限制调用方 IPs][]的筛选器 （允许/拒绝） 调用从特定 IP 地址和/或地址范围。
    -   [设置使用配额][]-允许您强制续订或生存期调用卷和/或带宽配额。
    -   [验证 JWT][]的实施存在和 JWT 从指定的 HTTP 标头或指定的查询参数提取的有效性。
-   [高级的策略][]
    -   [控制流][]-有条件地适用策略语句根据布尔[表达式][]的计算结果。
    -   [设置变量][]-保持以便日后访问命名的[上下文][]变量中的值。
-   [身份验证策略][]
    -   [使用基本身份验证][]的身份验证与后端服务使用基本身份验证。
    -   [具有客户端证书进行身份验证][]的与后端服务使用客户端证书进行身份验证。
-   [缓存策略][] 
    -   [从缓存中获取][]-执行缓存查找并返回可用时有效的缓存的响应。
    -   [到高速缓存存储区][]-根据指定的缓存控件配置的缓存响应。
-   [跨域策略][] 
    -   [允许跨域调用][]-利用 API 访问 Adobe Flash 和 Microsoft Silverlight 的基于浏览器的客户端。
    -   [CORS][] -添加跨来源的资源共享 (CORS) 给操作或允许跨域调用从基于浏览器的客户端 API 的支持。
    -   [JSONP][] -添加带给操作填充 (JSONP) 支持的 JSON 或 API 允许跨域调用从 JavaScript 基于浏览器的客户端。
-   [转换策略][] 
    -   [将 JSON 转换到 XML][]的转换为请求或响应正文从 JSON 为 XML。
    -   [将 XML 转换为 JSON][]的转换为请求或响应正文从 XML 到 JSON。
    -   [查找和替换字符串正文中的][]查找请求或响应的子字符串并将它替换为不同的子字符串。
    -   [掩码中的内容的 Url][] -重写 （掩码） 在响应正文中和链接位置标头中，使它们指向通过代理服务器对等链接。
    -   [将后端服务设置][]-更改传入请求的后端服务。
    -   [设置正文][]— 设置邮件正文为传入和传出的请求。
    -   [设置 HTTP 标头][]-将一个值分配给现有响应和/或请求标头，或添加新的响应和/或请求标头。
    -   [设置查询字符串参数][]的添加，删除或替换值，请求查询字符串参数。
    -   [重写 URL][] -将从其公用格式请求 URL 转换为所需的 web 服务的表单中。

## 下一步行动

策略表达式的详细信息，请参见下面的视频。

> [AZURE.VIDEO policy-expressions-in-azure-api-management]

[访问限制策略]: https://msdn.microsoft.com/library/azure/dn894078.aspx
[检查 HTTP 标头]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#CheckHTTPHeader
[限制通话率]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#LimitCallRate
[限制调用方 IPs]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#RestrictCallerIPs
[设置使用配额]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#SetUsageQuota
[验证 JWT]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#ValidateJWT

[高级的策略]: https://msdn.microsoft.com/library/azure/dn894085.aspx
[控制流]: https://msdn.microsoft.com/library/azure/dn894085.aspx#choose
[设置变量]: https://msdn.microsoft.com/library/azure/dn894085.aspx#set_variable
[表达式]: https://msdn.microsoft.com/library/azure/dn910913.aspx
[上下文]: https://msdn.microsoft.com/library/azure/ea160028-fc04-4782-aa26-4b8329df3448#ContextVariables

[身份验证策略]: https://msdn.microsoft.com/library/azure/dn894079.aspx
[使用基本身份验证]: https://msdn.microsoft.com/library/azure/061702a7-3a78-472b-a54a-f3b1e332490d#Basic
[使用客户端证书进行身份验证]: https://msdn.microsoft.com/library/azure/061702a7-3a78-472b-a54a-f3b1e332490d#ClientCertificate
[缓存策略]: https://msdn.microsoft.com/library/azure/dn894086.aspx
[从缓存中获取]: https://msdn.microsoft.com/library/azure/8147199c-24d8-439f-b2a9-da28a70a890c#GetFromCache
[存储到缓存]: https://msdn.microsoft.com/library/azure/8147199c-24d8-439f-b2a9-da28a70a890c#StoreToCache

[跨域策略]: https://msdn.microsoft.com/library/azure/dn894084.aspx
[允许跨域调用]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#AllowCrossDomainCalls
[CORS]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#CORS
[JSONP]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#JSONP

[转换策略]: https://msdn.microsoft.com/library/azure/dn894083.aspx
[将 JSON 转换为 XML]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#ConvertJSONtoXML
[将 XML 转换为 JSON]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#ConvertXMLtoJSON
[查找和替换在正文中的字符串]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#Findandreplacestringinbody
[掩码中的内容的 Url]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#MaskURLSContent
[一组后端服务]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetBackendService
[设置正文]: https://msdn.microsoft.com/library/azure/dn894083.aspx#SetBody
[设置 HTTP 标头]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetHTTPheader
[设置查询字符串参数]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetQueryStringParameter
[重写的 URL]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#RewriteURL



[在 API 管理策略]: api-management-howto-policies.md
[API 管理策略参考]: https://msdn.microsoft.com/library/azure/dn894081.aspx

[策略表达式]: https://msdn.microsoft.com/library/azure/dn910913.aspx

 
测试

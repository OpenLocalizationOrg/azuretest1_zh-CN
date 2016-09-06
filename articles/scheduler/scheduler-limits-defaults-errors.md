---
ms.openlocfilehash: 68cc3868d1fdd63c21f28d40607dcc46c2270c53
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
 pageTitle="计划程序限制、 默认值和错误代码"
 description=""
 services="scheduler"
 documentationCenter=".NET"
 authors="krisragh"
 manager="dwrede"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="07/28/2015"
 ms.author="krisragh"/>

# 计划程序限制、 默认值和错误代码

## 计划配额、 限制、 默认设置和限制

[AZURE.INCLUDE [scheduler-limits-table](../../includes/scheduler-limits-table.md)]

## X-ms-请求 id 标头

根据计划服务的所有请求都返回名为**x ms 请求 id**响应标头。 此标头包含唯一地标识请求的不透明度值。

如果您已验证该请求正确制定以一致的方式失败的请求，可能会使用此值来该错误报告给 Microsoft。 在报表中，包括价值 ofx-ms-请求的 id，该请求时的大致时间、 订阅、 云服务、 作业集合和/或作业，并尝试执行请求的操作的类型的标识符。

## 计划程序状态和错误代码

除了标准的 HTTP 状态代码，Azure 计划 REST API 返回扩展的错误代码和错误消息。 扩展的代码不会取代标准的 HTTP 状态代码，但提供附加的可操作信息，可以使用标准的 HTTP 状态代码结合。

例如，HTTP 404 错误可以发生出于很多原因，以便扩展邮件中有更多的信息可以帮助解决问题。 返回的 REST API 标准 HTTP 代码的详细信息，请参阅[服务管理状态和错误代码](https://msdn.microsoft.com/library/windowsazure/ee460801.aspx)。 服务管理 API 的 REST API 操作返回标准的 HTTP 状态代码，如[HTTP/1.1 状态代码定义](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)中定义。 下表描述了服务可能返回的常见错误。

|错误代码|HTTP 状态代码|用户消息|
|----|----|----|
|MissingOrIncorrectVersionHeader|错误的请求 (400)|版本控制标题未指定或未正确指定。|
|InvalidXmlRequest|错误的请求 (400)|请求正文的 XML 无效或未正确指定。|
|MissingOrInvalidRequiredQueryParameter|错误的请求 (400)|所需的查询参数为此请求未指定或未正确指定。|
|InvalidHttpVerb|错误的请求 (400)|指定的 HTTP 谓词服务器无法识别，或者此资源的无效。|
|AuthenticationFailed|禁止 (403)|服务器无法对请求进行身份验证。 验证证书有效，以及与此订阅关联。|
|ResourceNotFound|找不到 (404)|不存在指定的资源。|
|InternalError|内部服务器错误 (500)|服务器遇到一个内部错误。 请重试请求。|
|OperationTimedOut|内部服务器错误 (500)|不能在允许的时间内完成该操作。|
|ServerBusy|服务不可用 (503)|当前不可用来接收请求时服务器 （或内部组件）。 请重试您的请求。|
|SubscriptionDisabled|禁止 (403)|订阅是处于禁用状态。|
|BadRequest|错误的请求 (400)|参数不正确。|
|ConflictError|冲突 (409)|发生了冲突，以防止操作无法完成。|
|TemporaryRedirect|临时重定向 (307)|请求的对象不可用。 从响应中的位置字段可以获得临时 URI 的对象的新位置。 在新的 URI 可以重复原始请求。|

API 操作也可能返回其他错误信息由管理服务。 此附加的错误信息返回响应正文中。 错误响应的正文中遵循的基本格式如下所示。

        <?xml version="1.0" encoding="utf-8"?>  
        <Error>  
            <Code>string-code</Code>  
            <Message>detailed-error-message</Message>  
        </Error>  

## 请参见

 [计划程序的概念、 术语和实体层次结构](scheduler-concepts-terms.md)

 [开始管理门户中使用计划程序](scheduler-get-started-portal.md)

 [计划和 Azure 的计划程序中的计费](scheduler-plans-billing.md)

 [如何构建复杂的调度和高级的定期使用 Azure 的调度程序](scheduler-advanced-complexity.md)

 [计划程序 REST API 参考](https://msdn.microsoft.com/library/dn528946)

 [计划程序的高可用性和可靠性](scheduler-high-availability-reliability.md)

 [计划程序的出站身份验证](scheduler-outbound-authentication.md)

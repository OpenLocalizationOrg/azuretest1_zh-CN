---
ms.openlocfilehash: c97bad1b779025c6c78f966faffbd3e0790d7e80
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何创建媒体处理器 |Microsoft Azure" 
    description="了解如何创建媒体处理器组件进行编码、 转换格式、 加密或解密的 Azure 媒体服务的媒体内容。" 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/11/2015" 
    ms.author="juliako"/>


#如何︰ 获取媒体处理器实例


> [AZURE.SELECTOR]
- [.NET](media-services-get-media-processor.md)
- [REST](media-services-rest-get-media-processor.md)

##概述

媒体服务，媒体处理器是处理特定的处理任务，如编码，组件中设置的格式转换，加密或解密的媒体内容。 创建一个任务进行编码、 加密，或将媒体内容的格式转换时，通常需要创建媒体处理器。

下表提供的名称和说明的每个可用的媒体处理器。

媒体处理器名称|说明|详细信息
---|---|---
Azure Media 编码器|允许您运行使用 Azure Media 编码器的编码任务。|[Azure Media 编码器](media-services-encode-asset.md#azure_media_encoder)
媒体编码标准|允许您运行使用媒体编码标准的编码任务。|[Azure Media 编码器](media-services-encode-asset.md#media_encoder_standard)
媒体编码器高级工作流|允许您运行编码使用媒体编码高级工作流的任务。|[媒体编码器高级工作流](media-services-encode-asset.md#media_encoder_premium_wokrflow)
Azure 媒体索引器| 使您可以将媒体文件和内容可搜索，以及生成闭合字幕轨道和关键字。|[媒体文件使用 Azure 媒体索引器的索引](media-services-index-content.md)。
Azure 媒体 Hyperlapse （预览）|使您能够消除视频稳定化与视频中的"碰撞"。 此外允许您在可耗用剪辑加快您的内容。|        [Azure 媒体 Hyperlapse](http://azure.microsoft.com/blog/?p=286281&preview=1&_ppp=61e1a0b3db)</a>
存储解密| 可用于解密加密使用的加密存储的媒体资产。|N/A
Windows Azure 媒体包装程序|允许您将.mp4 媒体资产转换为平滑流式处理的格式。 此外，还允许您从平滑流式处理到苹果 HTTP 实时流 (HLS) 格式转换媒体资产。|[任务预设 Azure 媒体包装程序字符串](http://msdn.microsoft.com/library/hh973635.aspx)
Windows Azure 媒体加密器|可以加密使用 PlayReady 保护的媒体资产。|[任务预设 Azure 媒体包装程序字符串](http://msdn.microsoft.com/library/hh973610.aspx)

##获得 MediaProcessor

>[AZURE.NOTE] 使用介质服务 REST API 时, 应该注意下列事项︰
>
>在访问中介质服务的实体，您必须设置特定的标头字段和值 HTTP 请求中。 有关详细信息，请参阅[设置介质服务 REST API 的开发](media-services-rest-how-to-use.md)。

>已成功连接到 https://media.windows.net 之后, 您将收到指定另一个媒体服务 URI 的 301 重定向。 必须使[连接到介质服务使用 REST API，](media-services-rest-connect_programmatically.md)所述新的 URI 对的后续调用。 



下面的其余部分调用演示如何按名称 （在此例中， **Azure Media 编码器**） 中获取媒体处理器实例。 

    
请求︰

    GET https://media.windows.net/api/MediaProcessors()?$filter=Name%20eq%20'Azure%20Media%20Encoder' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-e769-477b-2233-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423635565&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=6zwXEn7YJzVJbVCNpqDUjBLuE5iUwsdJbWvJNvpY3%2b8%3d
    x-ms-version: 2.11
    Host: media.windows.net
    
响应︰
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 273
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 8a291764-4ed7-405d-aa6e-d3ebabb0b3f6
    request-id: dceeb559-48b5-48e1-81d3-d324b6203d51
    x-ms-request-id: dceeb559-48b5-48e1-81d3-d324b6203d51
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Wed, 11 Feb 2015 00:19:56 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#MediaProcessors","value":[{"Id":"nb:mpid:UUID:1b1da727-93ae-4e46-a8a1-268828765609","Description":"Azure Media Encoder","Name":"Azure Media Encoder","Sku":"","Vendor":"Microsoft","Version":"4.4"}]}


##下一步行动
您已经知道了如何获取媒体处理器实例，转到[如何编码资产][]主题将向您展示如何使用 Azure Media 编码器编码资产的。

[如何对资产进行编码]: media-services-rest-encode-asset.md
[任务预设 Azure 媒体编码字符串]: http://msdn.microsoft.com/library/jj129582.aspx
[如何︰ 以编程方式连接到媒体服务]: ../media-services-rest-connect_programmatically/ 

测试

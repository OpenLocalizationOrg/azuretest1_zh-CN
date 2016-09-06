---
ms.openlocfilehash: a365c04db4193aec83e19bbfb4ad31fb918c547b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="CDN 缓存策略在媒体服务扩展" 
    description="本主题概述 CDM 的缓存策略在媒体服务扩展。" 
    services="cdn" 
    documentationCenter=".NET" 
    authors="juliako" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="cdn" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/01/2015" 
    ms.author="juliako"/>

#CDN 缓存策略在媒体服务扩展

Azure 媒体服务提供基于 HTTP 的自适应流式处理和渐进式下载。 基于 HTTP 的流是高度可扩展的代理和 CDN 层以及客户端缓存中的缓存的好处。 流终结点提供常规流功能以及配置为缓存的 HTTP 标头。 流终结点设置 HTTP 缓存控制︰ 最大年龄和过期标头。 从[W3.org](http://www.w3.org/Protocols/rfc2616/rfc2616-sec13.html)可以缓存的 HTTP 标头的详细信息。

##默认缓存标头

默认流终结点应用 3 天点播流数据 （实际媒体片段/块） 和 manifest(playlist) 的缓存标头。 实时流，流终结点应用 3 天缓存标头数据 （实际媒体片段/块） 和 2 秒缓存的 manifest(playlist) 请求的标头。 当实时程序变为点播 （活动存档） 的点播流缓存标头应用。

##Azure CDN 集成

Azure 媒体服务的流终结点提供[集成的 CDN](http://azure.microsoft.com/updates/azure-media-services-now-fully-integrated-with-azure-cdn/) 。 高速缓存控制标头应用相同的方式对 CDN 的流终结点启用流终结点。 Azure CDN 使用流式传输终结点配置缓存值来定义内部缓存对象的生存时间，也使用此值来设置高速缓存标头的传递。 当使用 CDN 启用流终结点不建议设置较小的缓存值。 设置较小的值会降低性能，并减少 CDN 的优点。 不允许将缓存标头设置为小于 600 秒的 CDN 启用流终结点。

##使用 Azure 媒体服务配置缓存标头

您可以使用 Azure 管理门户或 Azure 媒体服务 Api 来配置缓存标头值。

1. 配置缓存标头使用管理门户请参考[如何管理流式处理的终结点](../media-services-manage-origins.md)配置流式传输终结点一节。
2. Azure 媒体服务 REST API， [StreamingEndpoint](https://msdn.microsoft.com/library/azure/dn783468.aspx#StreamingEndpointCacheControl)。
3. Azure 媒体服务.NET SDK， [StreamingEndpointCacheControl 属性](http://go.microsoft.com/fwlink/?LinkId=615302)。

##缓存配置优先顺序

1. Azure 的介质服务配置缓存值将重写默认值。
2. 如果无需手动配置，将应用默认值。
3. 由默认 2 秒缓存标头适用于实时流式 manifest(playlist) 无论 Azure 媒体或 Azure 存储配置并不能重写此值。
 
测试

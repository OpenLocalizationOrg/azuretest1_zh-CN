---
ms.openlocfilehash: 0e902fd551691275227bd9791c335bac0ebd4a0d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何使用 CDN |Microsoft Azure" 
    description="了解如何使用 Azure 内容传递网络 (CDN) 提供高带宽内容缓存 blob 和静态内容。" 
    services="cdn" 
    documentationCenter=".net" 
    authors="zhangmanling" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="cdn" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="09/01/2015" 
    ms.author="mazha"/>


# Azure 使用 CDN

Azure 内容传递网络 (CDN) 是基本的构造块，在 Azure 上缩放任何 HTTP 应用程序。 它提供了 Azure 客户全球解决方案缓存并提供接近最终用户的内容。 因此，而不是每一次碰到来源，用户请求获得智能地路由到最佳执行 CDN 边缘 POP。 这样就大大提高了性能和用户体验。 CDN 节点位置的当前列表，请参阅 [Azure CDN 节点位置]。

使用 CDN 缓存 Azure 数据的优点包括︰

-   更高的性能和更好的用户体验有很的内容源，并使用应用程序需要多个互联网行程加载内容的最终用户
-   分布式的规模大，更好地处理瞬时高负载，说，开头的事件，如产品上市
-   通过将用户请求和服务内容从 Pop，更少的通信量发送到原点的全局边缘因此减轻原点。

CDN 的现有客户现在可以在 [Azure 管理门户] 使用 Azure CDN。 CDN 是附加功能向您的订购，有单独的 [付费计划]。

##步骤 1︰ 创建在 Azure CDN 原点

CDN 来源是来自哪个 CDN 提取内容并缓存在边缘 Pop 的位置。 集成的 Azure 来源包括 Azure 应用程序、 云服务、 存储和媒体服务。 

## 步骤 2︰ 创建指向您 Azure 原点的 CDN 终结点

一旦您原始的设置，它将在原点列出当您[创建新的 CDN 终结点](cdn-create-new-endpoint.md)。  

> [AZURE.NOTE] 创建终结点的配置将立即不可用;可能需要多达 60 分钟的登记通过 CDN 网络传播。 通过 CDN 可用内容之前，请立即使用 CDN 的域名用户可能会收到状态代码 400 （错误请求）。

## 步骤 3︰ 设置 CDN 配置 

您可以启用多种功能为您 CDN 端点，例如高速缓存策略和缓存查询字符串等。  

## 第 4 步︰ 访问 CDN 的内容

若要访问在 CDN 缓存的内容，使用 CDN URL 提供的门户。 例如，为 blob 缓存的地址将与以下内容类似︰ http://<*CDNNamespace*\>.vo.msecnd.net/ <*myPublicContainer*\>/<*BlobName*\>



## 请参见

[Azure 内容传递网络 (CDN) 的概述](cdn-overview.md)
 

测试

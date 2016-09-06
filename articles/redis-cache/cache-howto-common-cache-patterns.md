---
ms.openlocfilehash: a76b99c860d0b28908e104ce45a970115f4d2186
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="常见的缓存模式使用 Azure Redis 高速缓存" 
   description="了解在哪里以及为什么要使用 Azure Redis 高速缓存" 
   services="redis-cache" 
   documentationCenter="" 
   authors="Rick-Anderson" 
   manager="wpickett" 
   editor=""/>

<tags
   ms.service="cache"
   ms.devlang="all"
   ms.topic="article"
   ms.tgt_pltfrm="cache-redis"
   ms.workload="tbd" 
   ms.date="08/11/2015"
   ms.author="riande"/>

# 常见的缓存模式使用 Azure Redis 高速缓存

本页面列出了为使用缓存带来的最常见的好处。

## 优化数据访问和缓存

使用缓存可以大大加快数据访问通过从数据存储区中提取。 缓存提供高吞吐量和低延迟。 通过从缓存中提取热的数据，不仅能加快您的应用程序，但可以减少数据访问负载并提高其响应速度的其他查询。 在高速缓存中存储的信息有助于节省资源并提高了可扩展性，为应用程序增加的需求。 它有效地可以从缓存获取数据时，您的应用程序将会对突发负载的响应速度快得多。 

## 分布式的会话状态
虽然它被视为一种最佳做法是避免使用会话状态，某些应用程序可能会遇到性能/降低的复杂性优势的使用数据，完全是其他应用程序时需要会话状态的会话。  内存会话状态提供程序中的默认设置不允许扩大规模 （运行该 web 站点的多个实例）。 SQL Server ASP.NET 会话状态提供程序将允许多个网站使用会话状态，但它会招致高滞后时间成本与内存中提供。 在 Redis 会话状态高速缓存提供程序是非常容易配置和设置一个低延迟替代方案。 如果您的应用程序使用仅有限的数量的会话状态，可用于大多数缓存的缓存数据和少量的会话状态。

## 未发生故障停机 （回退高速缓存）
 通过将数据存储在高速缓存中，应用程序可能能够在系统故障如网络延迟、 Web 服务问题和硬件故障。 通常是更好的办法将其缓存的数据到您的 web 服务或数据库恢复，比用于完全失败，您的应用程序。

## 下一步行动
若要了解有关使用 Azure Redis 高速缓存的详细信息︰
 
- [Redis Azure 缓存文档](http://azure.microsoft.com/documentation/services/cache/)︰ 此页面提供许多好链接使用 Redis Azure 缓存。
- [Azure Redis 缓存在 15 分钟内用 MVC 影片 app](http://azure.microsoft.com/blog/2014/06/05/mvc-movie-app-with-azure-redis-cache-in-15-minutes/)︰ 博客文章提供了使用 Azure Redis 缓存 ASP.NET MVC 应用程序中的快速入门。
- [如何使用 ASP.NET 会话状态与 Azure 网站](../app-service-web/web-sites-dotnet-session-state-caching.md)︰ 本主题说明如何使用会话状态的 Azure Redis 缓存服务。





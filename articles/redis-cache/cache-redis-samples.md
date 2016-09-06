---
ms.openlocfilehash: 12723c4a46f10b22dcdb70a5ef687ed973f6f139
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="Azure Redis 缓存示例" 
    description="了解如何使用 Azure Redis 高速缓存" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="08/25/2015" 
    ms.author="sdanie"/>

# Azure Redis 缓存示例 

本主题提供的 Azure Redis 缓存示例，其中包含方案，如连接到高速缓存、 读取和写入数据和从缓存中，并使用 ASP.NET Redis 高速缓存提供程序的列表。 某些示例是可下载的项目以及某些提供了分步指导，包括代码段但未链接到一个可下载的项目。

## 大家好世界示例

本节中的示例显示连接到 Azure Redis 缓存实例读取和将数据写入到缓存使用多种语言的基础知识和 Redis 客户端。

[Hello world](https://github.com/rustd/RedisSamples/tree/master/HelloWorld)示例演示如何执行各种使用[StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) .NET 客户端的缓存操作。

此示例演示如何︰

-   使用各种连接选项
-   读取和写入对象与使用同步和异步操作高速缓存
-   使用 Redis 的 MGET/MSET 命令来返回指定项的值
-   执行 Redis 事务性操作
-   使用 Redis 列表和排序的设置
-   使用 JsonConvert 的序列化程序的存储.NET 对象
-   使用 Redis 集实现标记

有关详细信息，在 github，看到的[StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis)文档，并使用更多的情况下请参阅[StackExchange.Redis.Tests](https://github.com/StackExchange/StackExchange.Redis/tree/master/StackExchange.Redis.Tests)单元测试。

[如何使用 Azure Python Redis 缓存](cache-python-get-started.md)演示如何开始使用 Azure Redis 高速缓存使用 Python 和[redis 年度](https://github.com/andymccurdy/redis-py)客户端。

[PHP 示例](https://msdn.microsoft.com/library/azure/dn690470.aspx#PHPExample)演示了如何开始使用 PHP 和[predis](https://github.com/nrk/predis)客户端使用 Azure Redis 高速缓存。

[使用缓存中的.NET 对象](https://msdn.microsoft.com/library/azure/dn690521.aspx#Objects)显示.NET 对象序列化，因此可以写到并读取它们从 Azure Redis 缓存实例的一种方法。 

## 使用 Redis 缓存 ASP.NET 那么 SignalR 作为底板扩张

[使用 Redis 缓存 ASP.NET 那么 SignalR 的底板扩张](https://github.com/rustd/RedisSamples/tree/master/RedisAsSignalRBackplane)示例演示如何为那么 SignalR 底板使用 Azure Redis 缓存。 背板的详细信息，请参阅[那么 SignalR Scaleout 与 Redis](http://www.asp.net/signalr/overview/performance/scaleout-with-redis)。

## Redis 高速缓存客户查询示例

此示例演示如何访问缓存数据和访问持久性存储中数据之间的比较性能。 此示例包含两个项目。

-   [演示如何 Redis 缓存可以提高性能的缓存数据](https://github.com/rustd/RedisSamples/tree/master/RedisCacheCustomerQuerySample)
-   [数据库和缓存演示的种子](https://github.com/rustd/RedisSamples/tree/master/SeedCacheForCustomerQuerySample)

## ASP.NET 会话状态和输出缓存

[使用 Azure Redis 缓存来存储 ASP.NET 用户和 OutputCache](https://github.com/rustd/RedisSamples/tree/master/SessionState_OutputCaching)示例演示了如何使用 Azure Redis 缓存来存储 ASP.NET 会话，并使用 Redis 的用户和 OutputCache 提供的输出缓存。

## 管理 Azure 的 Redis 与 MAML 的高速缓存

[管理 Azure Redis 缓存使用 Azure 管理库](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML)的示例演示如何使用 Azure 管理库管理-(创建 / 更新 / 删除) 高速缓存。 

## 自定义监视示例

[访问 Redis 缓存监控数据](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring)的示例演示如何为 Azure Redis 缓存在 Azure 预览门户之外访问监视数据。

## 使用 PHP 和 Redis 编写一个 Twitter 式克隆

[Retwis](https://github.com/SyntaxC4-MSFT/retwis)示例是 Redis Hello World。 它是最小的 Twitter 式社交网络克隆使用 Redis，并使用[Predis](https://github.com/nrk/predis)客户端 PHP 编写。 源代码设计非常简单，并在同一时间以显示不同 Redis 数据结构。

## 带宽监视器

[带宽监视器](https://github.com/JonCole/SampleCode/tree/master/BandWidthMonitor)示例允许您监视客户端上使用的带宽。 若要测量带宽、 缓存客户端计算机上运行该示例，打电话到缓存中，并观察报告的带宽监视器示例的带宽。

---
ms.openlocfilehash: d13ab577c100ef50d3d720b29453e9801a7fa8ed
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="如何配置 Redis Azure 的高速缓存"
   description="了解 Redis Azure 的高速缓存的默认 Redis 配置，并了解如何配置 Redis Azure 的高速缓存实例"
   services="redis-cache"
   documentationCenter="na"
   authors="steved0x"
   manager="dwrede"
   editor="tysonn" />
<tags 
   ms.service="cache"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="cache-redis"
   ms.workload="tbd"
   ms.date="09/03/2015"
   ms.author="sdanie" />

# 如何配置 Redis Azure 的高速缓存

本主题介绍如何查看和更新您的 Azure Redis 缓存实例的配置，并涵盖 Azure Redis 缓存实例的默认 Redis 服务器配置。

## 配置 Redis 缓存设置

缓存可以在[Azure 预览门户](https://portal.azure.com)使用**浏览**刀片式服务器访问。

![Azure Redis 缓存浏览刀片式服务器](./media/cache-configure/IC796920.png)

单击**Redis 缓存**以查看您的缓存。

![Azure Redis 缓存浏览缓存列表](./media/cache-configure/IC796921.png)

选择所需的高速缓存，以查看该缓存的属性。

![Redis 高速缓存的所有设置](./media/cache-configure/IC808312.png)

单击以查看和配置您的缓存的**设置**或**所有设置**。

![Redis 高速缓存设置](./media/cache-configure/IC808313.png)

## 属性

单击**属性**以查看有关您的高速缓存，包括缓存端点和端口的信息。

![Redis 缓存属性](./media/cache-configure/IC808314.png)

## 访问键

单击此处，查看或重新生成缓存的访问键**访问键**。 以及主机名和端口**属性**刀片式服务器从客户端连接到您的缓存时使用这些密钥。

![Redis 高速缓存访问键](./media/cache-configure/IC808315.png)

## 访问端口

默认情况下，非 SSL 访问已禁用新的缓存。 要启用非 SSL 端口，单击刀片式服务器**访问端口**，然后单击**否**。

![Redis 高速缓存访问端口](./media/cache-configure/IC808316.png)

## 定价层

单击要查看或更改您的缓存的定价层**定价层**。 扩展的详细信息，请参阅[如何缩放 Azure Redis 高速缓存](cache-how-to-scale.md)。

![Redis 定价层缓存](./media/cache-configure/pricing-tier.png)

## Diagnostics

单击要配置的存储帐户，用来存储缓存诊断**诊断**。

![Redis 高速缓存诊断](./media/cache-configure/IC808317.png)

有关详细信息，请参阅[如何监视 Redis Azure 的高速缓存](cache-how-to-monitor.md)。

## Maxmemory 策略和 maxmemory 保留

单击要将高速缓存的内存策略配置的**Maxmemory 策略**。 **Maxmemory 策略**设置可配置为缓存逐出策略和**maxmemory 保留**配置为非缓存过程保留的内存。

![Redis Maxmemory 的缓存策略](./media/cache-configure/IC808318.png)

**Maxmemory 策略**允许您从以下逐出策略选择。

-   易失性 lru-这是默认设置。
-   allkeys lru
-   易失性随机
-   allkeys-随机
-   易失性 ttl
-   noeviction

有关 maxmemory 策略的详细信息，请参阅[逐出策略](http://redis.io/topics/lru-cache#eviction-policies)。

**Maxmemory 保留**设置配置保留非缓存的操作，例如复制故障切换期间的 MB 的内存量。 它也用于当您有大量碎片率。 设置此值可以有更加一致的 Redis 服务器体验，当负载变化。 此值应该设置更高的工作负载，这是写入粗。 对于这类操作而保留的内存时，缓存的数据存储不可用。

>[AZURE.IMPORTANT] **Maxmemory 保留**设置功能仅适用于标准的缓存。

## （高级设置） 的密钥空间通知

单击**高级的设置**来配置 Redis 密钥空间的通知。 密钥空间通知允许客户端在特定事件发生时接收通知。

![Redis 高级设置缓存](./media/cache-configure/IC808319.png)

>[AZURE.IMPORTANT] 密钥空间通知和**通知密钥空间事件**设置属性仅适用于标准的缓存。

有关详细信息，请参阅[Redis 密钥空间的通知](http://redis.io/topics/notifications)。 有关代码示例，请参见[Hello world](https://github.com/rustd/RedisSamples/tree/master/HelloWorld)示例中的[KeySpaceNotifications.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/KeySpaceNotifications.cs)文件。

## 用户和标记

![Redis 高速缓存用户标记和标记](./media/cache-configure/IC808320.png)

**用户**部分中预览门户可帮助组织满足其访问管理简单而精确的基于角色的访问控制 (RBAC) 提供支持。 有关详细信息，请参阅[在 Azure 预览门户的基于角色的访问控制](http://go.microsoft.com/fwlink/?LinkId=512803)。

**标记**部分可帮助您组织您的资源。 有关详细信息，请参阅[使用标签来组织您的 Azure 资源](../resource-group-using-tags.md)。

## 默认值 Redis 服务器配置

新 Azure Redis 缓存实例都配置了以下默认 Redis 配置值。

>[AZURE.NOTE] 此部分中的设置不能更改使用`StackExchange.Redis.IServer.ConfigSet`方法。 如果使用本节中的命令调用此方法，将引发类似于以下异常︰  
>
>`StackExchange.Redis.RedisServerException: ERR unknown command 'CONFIG'`
>  
>通过预览门户可以配置的例如**最大内存策略**，可以配置的任何值。

|设置|默认值|说明|
|---|---|---|
|数据库|16|默认数据库 DB 0，您可以选择使用连接的每个连接基础上另一个。其中 dbid 是介于 0 到 15 GetDataBase(dbid)。|
|maxclients|取决于定价层<sup>1</sup>|这是最大允许同时连接的客户端数。 一旦达到限制 Redis 将关闭所有新建连接，发送错误达到客户端的最大数字。|
|maxmemory 策略|易失性 lru|Maxmemory 策略是为 Redis 将选择什么去达到 maxmemory （高速缓存提供了您在创建时选择缓存的大小） 的设置。 使用 Azure Redis 高速缓存的默认设置是易失的 lru，与使用 LRU 算法设置过期删除键。 可以预览门户中配置该设置。 有关详细信息，请参阅[Maxmemory 策略并保留 maxmemory](#maxmemory-policy-and-maxmemory-reserved)。|
|maxmemory 示例|3|LRU 和最小的 TTL 算法不是精确的算法，但逼近算法 （为了节省内存），以便您可以同时选择要检查的样本大小。 默认实例的 Redis 将检查这三个键并选取最近使用过少的一个。|
|lua 时间限制|5000|最大执行时间，以毫秒为单位的 Lua 脚本。 如果达到了最长执行时间 Redis 将记录的最大允许时间，并将开始回复对象添加到查询的错误之后，脚本是仍在执行。|
|lua 事件限制|500|这是脚本事件队列的最大大小。|
|客户端输出缓冲区限制 normalclient 输出缓冲区限制 pubsub|0 0 由 032 mb 8mb 60|客户端输出缓冲区限制可用于强制断开连接的客户端都不从读取数据速度不够快的服务器因某种原因 （的一个常见原因是 Pub/Sub 客户端不能使用快发布服务器可以生成这些消息）。 有关详细信息，请参阅[http://redis.io/topics/clients](http://redis.io/topics/clients)。|

<sup>1</sup>`maxclients`对于定价层每个 Azure Redis 缓存是不同的。

-   C0 250 MB 高速缓存 – 最多 256 个连接
-   C1 (1 GB) 高速缓存 – 最多 1000 个连接
-   C2 (2.5 GB) 高速缓存 – 最多 2000 个连接
-   C3 (6 GB) 缓存的最多 5000 个连接
-   C4 (13 GB) 高速缓存 – 最多 10000 个连接
-   C5 (26 GB) 高速缓存 – 最多 15000 个连接
-   C6 (53 GB) 高速缓存 – 最多 20000 个连接

## Redis 在 Redis Azure 的高速缓存中不支持的命令

>[AZURE.IMPORTANT] 因为 Azure Redis 缓存实例的配置和管理是使用预览门户以下命令被禁用。 如果您尝试调用这些您将收到一条错误消息类似于`"(error) ERR unknown command"`。
>
>-  BGREWRITEAOF
>-  BGSAVE
>-  配置
>-  调试
>-  迁移
>-  保存
>-  关机
>-  SLAVEOF

Redis 命令的详细信息，请参阅[http://redis.io/commands](http://redis.io/commands)。

## Redis 控制台

使用标准的高速缓存提供的**Redis 控制台**，您的 Azure Redis 高速缓存实例，可以安全地发出命令。 若要访问 Redis 控制台，单击**控制台**，从**Redis 缓存**刀片式服务器。

![Redis 控制台](./media/cache-configure/redis-console-menu.png)

>[AZURE.IMPORTANT] 在 Redis 控制台功能仅适用于标准的缓存。

要发出针对缓存实例的命令时，只需键入所需命令到控制台。

![Redis 控制台](./media/cache-configure/redis-console.png)

Azure Redis 缓存被禁用的 Redis 命令的列表，请参阅上文[Redis Redis Azure 的高速缓存中不支持的命令](#redis-commands-not-supported-in-azure-redis-cache)。 Redis 命令的详细信息，请参阅[http://redis.io/commands](http://redis.io/commands)。 

## 下一步行动
-   使用 Redis 命令的详细信息，请参阅[如何运行 Redis 命令？](cache-faq.md#how-can-i-run-redis-commands)。
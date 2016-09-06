---
ms.openlocfilehash: 0e8a0cba6353c68a4f2909997e30f21898da4d88
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="如何使用 Java 的 Azure Redis 高速缓存 |Microsoft Azure"
    description="学习如何使用 Java 的 Azure Redis 缓存使用"
    services="redis-cache"
    documentationCenter=""
    authors="steved0x"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="cache"
    ms.devlang="java"
    ms.topic="hero-article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="08/25/2015"
    ms.author="sdanie"/>

# 如何使用 Java 的 Azure Redis 高速缓存

Azure 的 Redis 高速缓存可以访问专用 Redis 高速缓存，由 Microsoft 管理。 高速缓存是从 Microsoft Azure 中的任何应用程序访问。

本主题演示您如何开始使用 Azure Redis 高速缓存使用 Java。


## 先决条件

[Jedis](https://github.com/xetorthio/jedis) -Redis 的 Java 客户端

本教程使用 Jedis，但您可以使用[http://redis.io/clients](http://redis.io/clients)中列出的任何 Java 客户端。


## 在 Azure 上创建一个 Redis 高速缓存

在[Azure 预览门户](http://go.microsoft.com/fwlink/?LinkId=398536)，单击**新建****数据 + 存储**，并选择**Redis 高速缓存**。

  ![][1]

输入 DNS 主机名。 窗体中，将`<name>.redis.cache.windows.net`。 单击**创建**。

  ![][2]


一旦创建缓存时，单击它以查看缓存设置的预览门户。 单击该链接下**键**并复制的主键。 您需要该请求进行身份验证。

  ![][4]


## 启用非 SSL 端点


在**端口**下单击链接，单击**否**为"允许访问只能通过 SSL"。 这使缓存的非 SSL 端口。 目前，Jedis 客户端不支持 SSL。

  ![][3]


## 向缓存中添加内容和检索

    package com.mycompany.app;
    import redis.clients.jedis.Jedis;
    import redis.clients.jedis.JedisShardInfo;

    /* Make sure you turn on non-SSL port in Azure Redis using the Configuration section in the preview portal */
    public class App
    {
      public static void main( String[] args )
      {
        /* In this line, replace <name> with your cache name: */
        JedisShardInfo shardInfo = new JedisShardInfo("<name>.redis.cache.windows.net", 6379);
        shardInfo.setPassword("<key>"); /* Use your access key. */
        Jedis jedis = new Jedis(shardInfo);
        jedis.set("foo", "bar");
        String value = jedis.get("foo");
      }
    }


## 下一步行动

- [启用缓存诊断程序](https://msdn.microsoft.com/library/azure/dn763945.aspx#EnableDiagnostics)使您可以[监视](https://msdn.microsoft.com/library/azure/dn763945.aspx)您的缓存的运行状况。
- 阅读官方的[Redis 文档](http://redis.io/documentation)。


<!--Image references-->
[1]: ./media/cache-java-get-started/cache01.png
[2]: ./media/cache-java-get-started/cache02.png
[3]: ./media/cache-java-get-started/cache03.png
[4]: ./media/cache-java-get-started/cache04.png

---
ms.openlocfilehash: 594003cb797359afbb85863d8f8bc60b06b20c72
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="如何通过 Node.js 使用 Azure Redis 高速缓存 |Microsoft Azure"
    description="学习如何使用 Azure Redis 缓存使用 Node.js 和 node_redis。"
    services="redis-cache"
    documentationCenter=""
    authors="steved0x"
    manager="dwrede"
    editor="v-lincan"/>

<tags
    ms.service="cache"
    ms.devlang="nodejs"
    ms.topic="hero-article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="08/25/2015"
    ms.author="sdanie"/>

# 如何通过 Node.js 使用 Azure Redis 高速缓存

Azure Redis 缓存到一个安全的、 专用 Redis 高速缓存，由 Microsoft 使您可以访问。 高速缓存是从 Microsoft Azure 中的任何应用程序访问。

本主题演示您如何开始使用 Azure Redis 高速缓存使用 Node.js。 与 Node.js 使用 Azure Redis 缓存的另一个示例，请参见[生成 Node.js 聊天应用程序与 Socket.IO Azure 网站上][]。


## 先决条件

安装[node_redis](https://github.com/mranney/node_redis):

    npm install redis

本教程使用[node_redis](https://github.com/mranney/node_redis)，但您可以使用[http://redis.io/clients](http://redis.io/clients)中列出的任何 Node.js 客户端。

## 在 Azure 上创建一个 Redis 高速缓存

在[Azure 预览门户](http://go.microsoft.com/fwlink/?LinkId=398536)，单击**新建****数据 + 存储**，并选择**Redis 高速缓存**。

  ![][1]

输入 DNS 主机名。 窗体中，将`<name>.redis.cache.windows.net`。 单击**创建**。

  ![][2]


一旦创建缓存时，单击它以查看缓存设置的预览门户。 单击该链接下**键**并复制的主键。 您需要该请求进行身份验证。

  ![][4]


## 启用非 SSL 端点


在**端口**下单击链接，单击**否**为"允许访问只能通过 SSL"。 这使缓存的非 SSL 端口。 目前，node_redis 客户端不支持 SSL。

  ![][3]


## 向缓存中添加内容和检索

    var redis = require("redis");

    // Add your cache name and access key.
    var client = redis.createClient(6379,'<name>.redis.cache.windows.net', {auth_pass: '<key>' });

    client.set("foo", "bar", function(err, reply) {
        console.log(reply);
    });

    client.get("foo",  function(err, reply) {
        console.log(reply);
    });


输出︰

    OK
    bar


## 下一步行动

- [启用缓存诊断程序](cache-how-to-monitor.md#enable-cache-diagnostics)使您可以[监视](cache-how-to-monitor.md)您的缓存的运行状况。
- 阅读官方的[Redis 文档](http://redis.io/documentation)。


<!--Image references-->
[1]: ./media/cache-nodejs-get-started/cache01.png
[2]: ./media/cache-nodejs-get-started/cache02.png
[3]: ./media/cache-nodejs-get-started/cache03.png
[4]: ./media/cache-nodejs-get-started/cache04.png

[生成与 Azure 网站 Socket.IO Node.js 聊天应用程序]: ../app-service-web/web-sites-nodejs-chat-app-socketio.md

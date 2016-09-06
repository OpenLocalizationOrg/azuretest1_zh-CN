---
ms.openlocfilehash: 3ac385695dd412442c0d1ead29122d0b642e9124
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="如何使用 Python 的 Azure Redis 高速缓存 |Microsoft Azure"
    description="学习如何使用 Python 的 Azure Redis 缓存使用"
    services="redis-cache"
    documentationCenter=""
    authors="steved0x"
    manager="dwrede"
    editor="v-lincan"/>

<tags
    ms.service="cache"
    ms.devlang="python"
    ms.topic="hero-article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="08/25/2015"
    ms.author="sdanie"/>

# 如何使用 Python Redis Azure 的高速缓存

本主题演示您如何开始使用 Azure Redis 高速缓存使用 Python。


## 先决条件

安装[redis 的上一年度](https://github.com/andymccurdy/redis-py)。


## 在 Azure 上创建一个 Redis 高速缓存

在[Azure 预览门户](http://go.microsoft.com/fwlink/?LinkId=398536)，单击**新建****数据 + 存储**，并选择**Redis 高速缓存**。

  ![][1]

输入 DNS 主机名。 窗体中，将`<name>.redis.cache.windows.net`。 单击**创建**。

  ![][2]

一旦创建缓存时，单击它以查看缓存设置的预览门户。 您将需要︰

- **主机名。** 创建高速缓存时，您可以输入该名称。
- **端口。** 单击以查看端口**端口**下的链接。 使用的 SSL 端口。
- **访问键。** 单击该链接下**键**并复制的主键。

## 向缓存中添加内容和检索

    >>> import redis
    >>> r = redis.StrictRedis(host='<name>.redis.cache.windows.net',
          port=6380, db=0, password='<key>', ssl=True)
    >>> r.set('foo', 'bar')
    True
    >>> r.get('foo')
    b'bar'

更换*&lt;名称&gt;*与缓存名称和*&lt;键&gt;*与您的访问密钥。


<!--Image references-->
[1]: ./media/cache-python-get-started/cache01.png
[2]: ./media/cache-python-get-started/cache02.png

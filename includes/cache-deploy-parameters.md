---
ms.openlocfilehash: 124951974d472000f5420fd9c4446c83aff3f3c4
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

### redisCacheName

Azure Redis 缓存创建的名称。

    "redisCacheName": {
      "type": "string"
    }

### redisCacheSKU

新的 Azure Redis 缓存定价层。

    "redisCacheSKU": {
      "type": "string",
      "allowedValues": [ "Basic", "Standard" ],
      "defaultValue": "Basic"
    }

模板定义允许的 （基本或标准），此参数的值，并指派默认值 （基本），如果未不指定任何值。 Basic 提供单个节点上可用的多个大小为 53 GB。
标准提供了两个节点的主副本，可与多个大小提供了 53 GB 和 99.9%的 SLA。

### redisCacheFamily

Sku 系列。

    "redisCacheFamily": {
      "type": "string",
      "defaultValue": "C"
    }

### redisCacheCapacity

新的 Redis Azure 的高速缓存实例的大小。 

    "redisCacheCapacity": {
      "type": "int",
      "allowedValues": [ 0, 1, 2, 3, 4, 5, 6 ],
      "defaultValue": 0
    }

模板定义允许 （0、 1、 2、 3、 4、 5 或 6），此参数的值，并指派默认值 (0)，如果未不指定任何值。 这些数字对应于以下的高速缓存大小︰ 0 = 250 MB、 1 = 1 GB，2 = 2.5 GB，3 = 6 GB，4 = 13 GB，5 = 26 GB，6 = 53 GB

### redisCacheVersion

Redis 服务器的新的高速缓存版本。

    "redisCacheVersion": {
      "type": "string",
      "defaultValue": "2.8"
    }

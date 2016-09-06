---
ms.openlocfilehash: 1f6ec860c33890ea0d5bdc3bf952df093bd8065e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
数据工厂是一种多租户服务到位，以确保客户订阅得到保护，免受对方工作负载具有下面的默认限制。 许多限制可以轻松地为您的订阅的最大限制由引发联系支持人员。 

**资源** | **默认限制** | **最大限制**
-------- | ------------- | -------------
管道内的数据工厂 | 100 | 2500
数据集内的数据工厂 | 500 | 5000
每个数据集的并行切片 | 10 | 10
每个管道对象的对象的字节<sup>1</sup> | 200 KB | 2000 KB
每个数据集和 linkedservice 对象的对象的字节<sup>1</sup> | 30 KB | 2000 KB
每个对象的字段 | 100 | [与支持部门联系](http://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
每个字段名称或标识符的字节数 | 2 KB | [与支持部门联系](http://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
每个字段的字节数 | 30 KB | [与支持部门联系](http://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
HDInsight 按需群集核心内订阅<sup>2</sup> | 48 | [与支持部门联系](http://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
重试次数对管道活动运行 | 1000 | MaxInt （32 位）

<sup>1</sup>管道、 数据集和链接的服务对象表示指定工作负荷的逻辑分组。 限制这些对象可以移动并使用 Azure 数据工厂服务处理的数据量无关。 数据工厂旨在扩展以处理 petabytes 的数据。

<sup>2</sup>点播 HDInsight 内核分配从订阅包含数据工厂。 因此，上述限制是数据工厂实施按需 HDInsight 核的核限制和区别与 Azure 订阅相关的核限制。


**资源** | **默认下限** | **最小限制**
-------- | ------------------- | -------------
计划间隔 | 15 分钟 | 5 分钟
重试尝试之间的间隔 | 1 秒 | 1 秒
重试超时值 | 1 秒 | 1 秒


### Web 服务呼叫限制

Azure 的资源管理器 API 调用的限制。 可以在[Azure 资源管理器 API 限制](azure-subscription-service-limits/#resource-group-limits)的速率使 API 调用。 



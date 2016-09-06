---
ms.openlocfilehash: c03a90e53740894e019a3e5156140787856f4d8d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="分区结构服务"
   description="描述如何进行分区服务结构服务"
   services="service-fabric"
   documentationCenter=".net"
   authors="appi101"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/26/2015"
   ms.author="aprameyr"/>

# 分区服务结构服务
服务结构容易构建可伸缩的有状态服务支持的服务状态的分区和具有运行的总体状态的一个子集的每个分区。 每个分区变为由[高度可用](service-fabric-availability-services.md)的单位。 分区的副本分布在群集中的节点和平衡。

> [AZURE.NOTE] 无状态服务还可以进行分区时这种情况下很少不必要对于大多数服务结构服务。  

有三个不同的分区方案。

## 单一实例分区方案
这用来指定该服务不需要分区。

## 指定的分区方案
这用来指定一组固定的服务的每个分区的名称。 可以通过其名称来查找各个分区。

## 范围分区方案
这用于指定整数范围 （由较低和高键标识） 和分区 (n) 数。 它将创建 n 分区，每个负责非重叠的子范围。 示例︰ 指定范围的分区方案 （与三个副本服务） 与低键为 0，99 高键和 4 的计数将创建 4 分区如下所示。

![范围分区](./media/service-fabric-concepts-partitioning/range-partitioning.png)

一般情况下是创建数据集内的唯一键的哈希值。 键的一些常见示例是汽车识别码 (VIN)、 雇员 ID 或一个唯一的字符串。 使用该唯一键然后将创建长的哈希代码，取模键范围，将用作您的密钥。 您可以指定允许的键范围的上下限。

此外，应当估计高足以处理最坏情况的资源 （如内存限制或网络带宽） 加载，但没有这么多该分区非常稀疏的分区数。

### 选择哈希算法
哈希的一个重要部分选择哈希算法。 一个重要的考虑因素是目标是进行分组 （位置敏感散列），接近类似键还是活动应该广泛地分布在所有分区 （分发散列）。

常规的哈希代码的算法选择的很好资源是[哈希函数的维基百科页面](http://en.wikipedia.org/wiki/Hash_function)。

> [AZURE.NOTE] 不能更改分区的数目或类型的用于服务后已创建的分区方案。

## 下一步行动

服务结构的概念信息，请参阅以下资源︰

- [服务结构服务的可用性](service-fabric-availability-services.md)

- [服务结构服务的可扩展性](service-fabric-concepts-scalability.md)
 

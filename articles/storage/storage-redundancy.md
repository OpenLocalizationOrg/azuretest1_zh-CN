---
ms.openlocfilehash: ad211b500237daee2e0a73b6d94f077bbc1bc4df
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

<properties 
  pageTitle="Azure 存储复制 |Microsoft Azure" 
  description="Microsoft Azure 存储帐户中的数据将被复制的持续性和高可用性。 复制选项包括本地冗余存储 (LRS)、 区域冗余存储 (ZRS)、 地理冗余存储 (GRS) 和读取访问地理冗余存储 (RA GRS)。" 
  services="storage" 
  documentationCenter="" 
  authors="tamram" 
  manager="adinah" 
  editor=""/>

<tags 
  ms.service="storage" 
  ms.workload="storage" 
  ms.tgt_pltfrm="na" 
  ms.devlang="na" 
  ms.topic="article" 
  ms.date="09/01/2015" 
  ms.author="tamram"/>

# Azure 存储复制

始终复制您的 Microsoft Azure 存储帐户中的数据以确保持续性和高可用性，会议[Azure 存储 SLA](http://azure.microsoft.com/support/legal/sla/)即使面对暂时的硬件故障。

创建存储帐户时，必须选择下面的复制选项之一︰  

- [本地冗余存储 (LRS)](#locally-redundant-storage)
- [区域冗余存储 (ZRS)](#zone-redundant-storage)
- [Geo 冗余存储 (GRS)](#geo-redundant-storage)
- [读取访问地理冗余存储 (RA GRS)](#read-access-geo-redundant-storage)

下表提供简要概述了 LRS、 ZRS、 GRS 和 RA GRS 的之间的差别而后续各节介绍每种类型的复制的详细信息。


|复制策略|LRS|ZRS|GRS|RA GRS
|--------------------|---|---|---|------
|在多个设备之间复制数据。|否|是|是|是|
|可以读取数据，从次级位置和主要的位置。|否|否|否|是
|在不同的节点上维护数据的副本数。|3|3|6|6


## 本地冗余存储

本地冗余存储 (LRS) 复制您在其中创建存储帐户的区域中的数据。 要最大化的耐用性，每个请求对您的存储帐户中的数据被复制三次。 这些三个副本每个驻留在单独的故障域和升级域。 容错域 (FD) 是一组表示出现故障的物理单位，可以被认为是属于同一个物理机架上的节点的节点。 升级域 (UD) 是一组节点的服务升级 （推广） 的过程中一起升级。 三个副本散布在 UDs 和 FDs 以确保数据可用，即使硬件故障影响单个机架期间推出升级后的节点。 请求成功返回仅后都已写入所有三个副本。

虽然地理冗余存储 (GRS) 建议对于大多数应用程序，也可能在某些情况下需要本地冗余存储︰  

- LRS GRS，比便宜，还提供了更高的吞吐量。 如果您的应用程序存储的数据，可以轻松地重新构建，您可以选择为 LRS。

- 某些应用程序被限制为仅在由于数据治理要求单个区域中的数据复制。

- 如果您的应用程序有自己地区复制策略，然后它可能需要 GRS。


## 区域冗余存储

区域冗余存储 (ZRS) 跨两到三个设施，在一个区域内或跨两个地区，提供较高的耐久性比 LRS 复制数据。 如果您的存储帐户具有 ZRS 启用，您的数据是持久即使发生故障时设施之一。


>[AZURE.NOTE]  ZRS 目前仅为块 blob。 请注意，一旦创建您的存储帐户和所选的冗余区域复制，您不能将它用于任何其他类型的复制，反之亦然。


## Geo 冗余存储

Geo 冗余存储 (GRS) 将数据复制到数百英里的主区域的辅助区域。 如果您的存储帐户具有 GRS 启用，则甚至在完成区域停电或灾难的主要区域是不可恢复的持久数据。

对于 GRS 启用使用存储帐户，到主要地区，其中三次复制它首先提交更新。 然后更新被复制到次区域，其中还将复制该三次，在不同的故障域和升级域。


> [AZURE.NOTE] 使用 GRS，写入数据的请求是向第二区域异步复制。 请务必注意 GRS 的选择不会影响对主区域发出的请求的延迟。 但是，由于异步复制是指一段延迟，在地区性灾难很可能，尚未复制到辅助区域的更改可能会丢失，如果数据无法恢复从主要区域。

创建存储帐户时，您选择的帐户的主要地区。 辅助区域由基于的主要区域，并且无法更改。 下表显示了主要和次要地区配对。

|主要            |第二
| ---------------   |----------------
|美国中北部   |美国中南部
|美国中南部   |美国中北部
|东亚的美国            |美国西部
|美国西部            |东亚的美国
|美国东部 2          |美国中部
|美国中部         |美国东部 2
|北欧       |西欧
|西欧        |北欧
|东南亚    |东亚
|东亚          |东南亚
|东亚中国         |北美中国
|北美中国        |东亚中国
|日本东         |日本西部
|日本西部         |日本东
|巴西南部       |美国中南部
|澳大利亚东部     |澳大利亚东南部
|澳大利亚东南部|澳大利亚东部  


## 读取访问地理冗余存储

读取访问地理冗余存储 (RA GRS) 通过提供对在辅助位置，除了在 GRS 所提供的两个区域之间复制数据的只读访问权限最大化可用性对于您的存储帐户。 主区域中的数据不可用，则您的应用程序可以从次区域读取数据。

辅助区域数据启用只读访问权限时，您的数据是辅助端点，除了您的存储帐户的主终结点上可用。 次要终点类似于主的终结点，但追加后缀`–secondary`在帐户名。 例如，如果您主要终点为 Blob 服务是`myaccount.blob.core.windows.net`，则辅助终结点为`myaccount-secondary.blob.core.windows.net`。 您的存储帐户的访问键都相同的两个主要和次要的端点。

## 下一步行动

- [Azure 存储可扩展性和性能目标](storage-scalability-targets.md)
- [Microsoft Azure 存储冗余选项和读取访问地理冗余存储 ](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx)  
- [RA GRS 的 Microsoft Azure 存储仿真器 3.1 ](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/08/microsoft-azure-storage-emulator-3-1-with-ra-grs.aspx)
- [Azure 存储 SOSP 纸张](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)  

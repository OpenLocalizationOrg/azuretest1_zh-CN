---
ms.openlocfilehash: d91c83db86d87adcdd45e7162e780d14900596b1
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="Azure 存储可扩展性和性能目标 |Microsoft Azure"
   description="了解为 Azure 存储，包括产能、 请求速率和这两个标准和最优存储帐户的入站和出站带宽的可扩展性和性能目标。 了解在 Azure 存储服务的每一个分区的性能目标。"
   services="storage"
   documentationCenter="na"
   authors="tamram"
   manager="na"
   editor="na" />
<tags 
   ms.service="storage"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="storage"
   ms.date="08/07/2015"
   ms.author="tamram" />

# Azure 存储可扩展性和性能目标

本主题介绍 Microsoft Azure 存储的可扩展性和性能的主题。 其他 Azure 限制的摘要，请参阅[Azure 订阅和服务限制、 配额和限制](../azure-subscription-service-limits.md)。

>[AZURE.NOTE] 所有存储帐户运行在新的平面网络拓扑，并且支持下述，无论创建时的可扩展性和性能目标。 在 Azure 存储平面网络体系结构和可伸缩性的详细信息，请参阅[Microsoft Azure 存储︰ 高度可用云存储服务强一致性与](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)。

<!-- -->

>[AZURE.IMPORTANT] 此处列出的可扩展性和性能目标是高端的目标，但能够实现。 在所有情况下，请求速率和带宽，通过您的存储帐户取决于存储的对象，利用访问模式的大小和应用程序的执行的负载类型。 一定要测试您的服务，以确定其性能是否满足您的需求。 如果可能，请避免突然高峰通信量的速率并确保通信良好分布式跨分区。

>当您的应用程序到达的分区可以处理工作负载限制时，将开始 Azure 存储 500 （操作超时） 响应返回 503 （服务器忙） 的错误代码或错误代码。 在这种情况下，应用程序应使用的重试次数的指数退避算法策略。 指数退避算法允许分区以减小，并缓出该分区到流量的峰值负载。

如果您的应用程序的需要超出单个存储帐户的可伸缩性目标，可以生成应用程序使用多个存储帐户，并跨这些存储帐户分区数据对象。 在批发价上的信息，请参阅[存储定价详细信息](http://azure.microsoft.com/pricing/details/storage/)。

## 标准的存储帐户的可伸缩性目标

[AZURE.INCLUDE [azure-storage-limits](../../includes/azure-storage-limits.md)]

## 高级存储帐户的可伸缩性目标

[AZURE.INCLUDE [azure-storage-limits-premium-storage](../../includes/azure-storage-limits-premium-storage.md)]

## 存储限制-Azure 的资源管理器

[AZURE.INCLUDE [azure-storage-limits-azure-resource-manager](../../includes/azure-storage-limits-azure-resource-manager.md)]

## 在 Azure 存储中的分区

包含的数据可以存储在 Azure 存储 （blob、 邮件、 实体和文件） 中的每个对象所属的分区中，并由分区键。 该分区确定如何 Azure 存储负载均衡 blob、 消息、 实体和文件到服务器，以满足这些对象的通信需求。 分区键存储帐户内是唯一的并用于定位 blob、 消息或实体。

在[标准的存储帐户的可伸缩性目标](#scalability-targets-for-standard-storage-accounts)上面所示的表列出每个服务的单个分区的性能目标。

分区存储服务的每个影响负载平衡和可伸缩性在以下方面︰

- **Blob**︰ 为 blob 的分区键是容器名称 + blob 名称。 这意味着每个 blob 具有它自己的分区。 Blob 可以因此分布于多个服务器以扩张对它们的访问。 虽然 blob 可以在 blob 容器中进行逻辑分组，有这种分组从任何分区暗示。

- **文件**︰ 一个文件的分区键是帐户名称 + 文件共享的名称。 这意味着一个文件共享中的所有文件也都是在一个分区中。

- **消息**︰ 消息的分区键是队列名称，因此在队列中的所有消息分为多个分区和由一个服务器提供服务。 可能由不同的服务器，但是很多队列的存储帐户可能具有平衡的负载处理不同的队列。

- **实体**︰ 分区键的实体是表名称 + 分区的项，其中的分区键是属性的值需要用户定义**PartitionKey**的实体。  

    分区键值相同的所有实体被分组到同一分区和存储在相同的分区服务器上。 这是重要的一点在设计您的应用程序。 您的应用程序应该平衡实体分布到多个分区，用对一个分区中的实体进行分组的数据访问优点的可伸缩性优势。 

    对一组表中的实体进行分组到单个分区一个主要优点是可以执行跨实体在同一分区中，原子的批处理操作，因为在单个服务器上存在一个分区。 因此如果您想要执行的批处理操作，请考虑将使用相同的分区键的实体。

    另一方面，但属于不同的分区在同一表中的图元可以是负载平衡跨不同的服务器，这样就可以有更大可伸缩性的大表。

## 请参见

- [定价详细信息的存储](http://azure.microsoft.com/pricing/details/storage/)
- [Azure 订阅和服务限制、 配额和约束](../azure-subscription-service-limits.md)
- [高级存储︰ Azure 的虚拟机的工作负载的高性能存储](storage-premium-storage-preview-portal/)
- [Azure 存储复制](storage-redundancy.md)
- [Microsoft Azure 存储性能和可扩展性的核对清单](storage-performance-checklist.md)
- [Microsoft Azure 存储︰ 高可用性云存储服务具有强一致性](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)



 
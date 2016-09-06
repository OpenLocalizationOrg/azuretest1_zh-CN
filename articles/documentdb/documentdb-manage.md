---
ms.openlocfilehash: 07115a5035be335885b6bc46875a2dd46a0d4b27
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="管理容量，DocumentDB |Microsoft Azure" 
    description="了解如何可以扩展以满足您的应用程序的容量需求的 DocumentDB。" 
    services="documentdb" 
    authors="mimig1" 
    manager="jhubbard" 
    editor="cgronlun" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/27/2015" 
    ms.author="mimig"/>

# 管理 DocumentDB 容量需求
DocumentDB 是一个完全托管的、 可扩展的文档面向 NoSQL 数据库服务。  使用 DocumentDB，您不必出租的虚拟机、 软件部署、 监视数据库或担心灾难恢复。 DocumentDB 是拥有并由 Microsoft 工程师提供世界类可用性、 性能和数据保护连续监控。  

您可以开始使用 DocumentDB 通过[Azure 预览门户](https://portal.azure.com/)[创建数据库帐户](documentdb-create-account.md)。 DocumentDB 将提供单位的固态驱动器 (SSD) 备份存储和吞吐量。 通过创建数据库集合数据库帐户内的调配这些单位。 每个集合包含 10 GB 的数据库存储和保留的吞吐量。 如果您的应用程序的吞吐量要求更改，您动态地更改此设置的每个集合的[性能级别](documentdb-performance-levels.md)。  

当您的应用程序超过一个或多个集合的性能级别时，则请求将被控制在每个集合的基础上。 这意味着，虽然其他人可能会阻止某些应用程序请求可能会成功。

## 数据库帐户和管理资源
作为 Azure 的订阅者，您可以设置一个或多个 DocumentDB 数据库帐户。 每个数据库帐户附带的管理资源，包括数据库、 用户和权限配额。 这些资源进行[限制和配额](documentdb-limits.md)。 如果您需要管理的其他资源，请[与支持人员联系](documentdb-increase-limits.md)。   

## 具有无限制的文档存储数据库
一个 DocumentDB 数据库可以包含几乎不限制的数量的文档存储分区的集合。 集合构成事务域内包含的文档。 DocumentDB 数据库是弹性的大小----从几 GB 到潜在数万亿字节的 SSD 备份文档存储和资源调配的吞吐量。 与传统的 RDBMS 数据库，DocumentDB 中的数据库不限于一台计算机。   

使用 DocumentDB，您可以根据需要扩展您的应用程序，您可以创建多个集合或数据库。 通过数万亿字节的文档存储与每个包含数百个或数千个集合创建非常大的 DocumentDB 数据库，在 Microsoft 内部的各种第一方应用程序在使用者比例使用 DocumentDB。 您可以扩大或通过添加或删除收藏集来满足应用程序的规模收缩数据库。 

## 数据库集合
DocumentDB 中的每个数据库都可以包含一个或多个集合。 集合作为文档存储和处理的高可用的数据分区。 具有异构架构的文档可以存储每个集合。 DocumentDB 的自动索引和查询功能，可以轻松地过滤和检索文档。 集合提供文档存储和查询执行的范围。 集合也是它所包含的所有文档的事务领域。 集合分配根据其指定的性能级别的吞吐量。  这可以通过 Azure 预览门户或 Sdk 动态调整。 

您可以创建任意数量的集合来满足您的应用程序的存储和吞吐量比例要求。 通过[Azure 预览门户](https://portal.azure.com/)或通过任何一种[DocumentDB 的 Sdk](https://msdn.microsoft.com/library/azure/dn781482.aspx)，可以创建集合。   

>[AZURE.NOTE] 每个集合支持的文档数据达 10 gb 存储。 
 
## 申请单位以及数据库操作
DocumentDB 允许一组丰富的数据库操作，包括对 Udf、 存储的过程和触发器 — 所有操作在数据库集合中的文档的关系和分层查询。 与每一种操作相关联的处理成本的差异取决于 CPU、 I/O 和内存完成该操作所需。 而不是考虑和管理硬件资源，您可以将请求单位 (RU) 的视为单个度量值执行各种数据库操作和应用程序的请求提供服务所需的资源。

申请单位提供和分配根据设置为集合的性能级别。 每个集合被指定在创建时的性能级别。 这一性能级别确定每秒请求单位集合的处理能力。 性能级别可以调整整个生命周期中的集合来适应不断变化的处理需求和访问您的应用程序的模式。 有关详细信息，请参阅[DocumentDB 的性能级别](documentdb-performance-levels.md)。 

>[AZURE.IMPORTANT] 集合是收费的实体。 成本取决于分配给该集合的性能级别。 

申请单位消耗量计为每秒的速率。 对于应用程序超出集合资源调配的请求单元速率，将限制到该集合的请求，直到率低于保留级别。 如果您的应用程序要求更高的吞吐量，可调整现有集合的性能级别或应用程序请求传播到新的集合。

申请单位是标准化的请求处理成本的措施。 单个请求单位代表包含 10 个独特的属性值的单个 1 KB JSON 文档读取 （通过自助链接） 所需的处理能力。 申请单位费用假定一致性级别设置为"会话"的默认值自动设置的所有文档索引。 插入、 替换或删除同一文档的请求将消耗更多处理服务，从而更多的申请单位。 每个服务的响应包括测量使用请求的请求单位自定义页眉 （x-ms-请求的费用）。 此标头也是可以通过[Sdk](https://msdn.microsoft.com/library/azure/dn781482.aspx)访问的。 在.NET SDK 中，RequestCharge 是 ResourceResponse 对象的一个属性。

>[AZURE.NOTE] 1 申请单位为 1 KB 文档的基线对应于简单获取自我文档的链接。 

有几个因素影响消耗对 DocumentDB 数据库帐户的操作的请求单元。 这些因素包括︰

- 文档的大小。 随着文档大小的增加单位消耗来读取或写入的数据也会增加。
- 属性计数。 假定默认索引的所有属性，使用编写文档的单位将作为属性计数的增加而增加。
- 数据的一致性。 当使用强或包围的失效数据一致性级别，需要消耗额外单位读取文档。
- 索引的属性。 在每个集合的索引策略确定哪些属性进行索引，默认情况。 您可以减少您请求单位消耗量限制索引的属性的数目。 
- 文档索引。 默认情况下，每个文档将自动设置索引，将消耗少的申请单位，如果您选择不进行索引的文档部分。

查询、 存储的过程和触发器使用请求单位根据正在执行的操作的复杂性。 在开发应用程序时，检查请求的费用标头，以更好地了解每个操作消耗请求单位容量的方式。  

## 选择一致性级别和吞吐量
默认一致性级别的选择有一定影响的吞吐量和滞后时间。 以编程方式并通过 Azure 预览门户，可以设置默认的一致性级别。 您还可以重写基于每个请求的一致性级别。 默认情况下，一致性级别为该会话具有单调性的读/写功能，读您写的保证。 会话的一致性非常适合以用户为中心的应用程序，并提供一致性和性能权衡的完美结合。    

更改您的一致性级别在 Azure 预览门户网站上的说明，请参阅[如何管理 DocumentDB 帐户](documentdb-manage-account.md#consistency)。 或者，一致性级别的详细信息，请参阅[使用一致性级别](documentdb-consistency-levels.md)。

## 设置的文档存储和索引开销
创建每个集合都提供 10 gb SSD 备份文档存储空间。 文档存储 10GB 包括文档及相关索引的存储。 默认情况下，DocumentDB 集合配置为自动索引的所有文档而无需显式要求任何辅助索引或架构。 根据应用程序使用 DocumentDB，2-20%之间为典型的索引开销。 所使用的 DocumentDB 的索引技术可以确保，无论属性的值，索引开销不超过 80%以上的具有默认设置的文档的大小。 

默认情况下所有文档会自动建立都索引的 DocumentDB。 但是，如果您想要微调索引开销，您可以选择删除某些文档正在被编制索引时插入或替换文档中的所述[DocumentDB 索引策略](documentdb-indexing-policies.md)。 您可以配置一个 DocumentDB 集合来排除在索引集合中的所有文档。 您还可以配置 DocumentDB 集合进行有选择地索引仅某些属性或路径替换通配符的您的 JSON 文档，[配置集合的索引策略](documentdb-indexing-policies.md#configuring-the-indexing-policy-of-a-collection)中所述。 排除属性或文档还可以提高写入吞吐量 – 这意味着您将占用更少的申请单位。   
 
## 下一步行动
有关监视性能级别在 Azure 预览门户网站上的说明，请参阅[显示器 DocumentDB 帐户](documentdb-monitor-accounts.md)。

选择集合的性能级别的详细信息，请参阅[DocumentDB 中的性能级别](documentdb-performance-levels)。
 
测试

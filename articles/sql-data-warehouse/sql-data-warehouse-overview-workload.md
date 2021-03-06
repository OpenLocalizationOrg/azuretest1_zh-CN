---
ms.openlocfilehash: efb9120373f6600d067a6bb39bac144691ec5c90
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="数据仓库工作负载"
   description="SQL 数据仓库的弹性，可以扩大、 缩小，或暂停使用数据仓库单元 (DWUs) 的滑尺的计算能力。 本文介绍了数据仓库标准和它们与 DWUs。 "
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="05/23/2015"
   ms.author="barbkess;JRJ@BigBangData.co.uk"/>

# 数据仓库工作负载
数据仓库工作负载是指所有针对数据仓库发生的操作。 数据仓库工作负荷包括数据加载到数据仓库、 进行分析和报告在数据仓库、 数据仓库中，在数据管理和数据仓库从数据库导出数据的整个过程。 深度和广度的这些组件通常是与数据仓库的完善程度相称。


## 对数据仓库的新内容？
数据仓库是从一个或多个数据源将加载和用于执行商业智能任务，如报告和数据分析的数据集合。 

数据仓库的特点是扫描更大数量的行，大范围的数据，可能会返回相对较大的结果进行分析和报告有关的查询。 数据仓库也表征相对数据量较大与较小的事务级别插入/更新/删除。 

- 当数据存储以优化查询需要扫描大量行或大范围的数据的一种，数据仓库的性能最佳。 在数据存储和搜索的列，而不是按行时，这种类型的扫描效果最佳。 

>[AZURE.NOTE] 内存中 columnstore 索引，使用列存储，通过传统的二进制树提供达 5 倍压缩收益是其 10 倍的提高查询性能的报告和分析查询。 我们认为 columnstore 索引作为存储和扫描数据仓库中的较大数据的标准。

- 数据仓库具有不同优化用于联机事务处理 (OLTP) 系统的要求。 OLTP 系统中有多个插入、 更新和删除操作。 这些操作查找到表中的特定行。 查找表中按行方式存储数据时的效果最好。 数据可以进行排序和快速搜索与除数和战胜称为二进制树或 b 树搜索的方法。


## 数据加载
加载数据是数据仓库工作负荷的重要部分。 企业通常都有跟踪更改客户时生成业务事务整天忙 OLTP 系统。 定期，通常在维护窗口期间的晚上，交易记录将移动或复制到数据仓库。 数据仓库数据后，分析师可以进行分析并做出业务决策的数据。

- 传统上，加载的过程称为 ETL 的提取、 转换和加载。 数据通常需要进行转换，使其与数据仓库中的其他数据保持一致。 以前，企业用专用的 ETL 服务器来执行转换。 现在，用这种快速大规模并行处理可以首先将数据加载到 SQL 数据仓库并执行转换，然后。 此过程称为提取、 加载和变形 (ELT)，并正在成为一个新的标准的数据仓库工作负载。

> [AZURE 的注释]与 SQL Server CTP2，您现在可以执行在分析实时 OLTP 表上。 这不能取代需要数据仓库来存储和分析数据，但它确实提供了一种实时执行分析。
 
### 报告和分析查询
报告和分析查询通常可分为小、 中和大基于一系列标准，但通常基于时间。 在大多数数据仓库，没有快速运行而不是长时间运行的查询混合的负载。 在每种情况下，是很重要的以确定此组合并确定其频率 （每小时、 每天、 月末、 季末，等等）。 务必要了解混合的查询工作负载，结合并发，导致适当的容量规划为数据仓库。

- 容量规划可以是一个复杂的任务，为混合的查询工作负荷，尤其是当您需要将容量添加到数据仓库长的提前期。 SQL 数据仓库中移除容量规划的紧迫性由于可以增长和收缩的计算容量，在任何时间和因为存储和计算能力独立调整大小。

### 数据管理
管理数据很重要，尤其是当您知道您可能会在不久的将来用完磁盘空间。 数据仓库通常将分成有意义的变化范围，为分区表中存储的数据。 所有基于 SQL Server 的产品让您移出表的分区。 切换允许此分区将旧数据移动到较便宜的存储和联机存储空间上保持可用的最新数据。

- Columnstore 的索引支持分区的表。 Columnstore 索引的分区的表使用的数据管理和存档。 用于存储表逐行，分区查询性能有更大的角色。  
 
- PolyBase 扮演重要的角色，在管理数据。 使用 PolyBase，您可以选择将旧数据存档到 Hadoop 或 Azure blob 存储。  这提供了许多选项，因为数据仍处于联机状态。  可能要花费更长的时间来检索数据来自 Hadoop，但检索时间的代价可能超过了存储成本。 
 
### 导出数据
使数据可用于报告和分析的一种方法是将数据从发送数据仓库到服务器专用于运行报告和分析。 这些服务器称为数据集市。 例如，可以预先处理报告数据，然后将其从数据仓库导出到多台服务器在世界各地，以使它广泛可用于客户和分析家。

- 用于生成报告，每个晚上您无法填充只读的报告服务器的每日数据快照。 这样更高的带宽为客户同时降低上数据仓库的计算资源需求。 从安全角度讲，数据集市可以减少到数据仓库具有访问权限的用户的数量。
- 分析，可以建立在数据仓库分析多维数据集和对数据仓库中，运行分析或预先处理数据并将其导出到分析服务器进行进一步分析。 

## 下一步行动
要开始开发数据仓库，请参阅[开发概述][]。

## 书籍
[大型数据仓库](https://www.manning.com/books/big-data-warehousing)通过 Karthik Ramachandran，Istvan Szededi，Richard L.Saltzer （Manning 发布）。 [第 1 章](https://manning-content.s3.amazonaws.com/download/e/3d94acd-9512-46c8-b0b0-8c9c3c6a303b/BDW_MEAP_ch1.pdf)

<!--Image references-->

<!--Article references-->
[开发概述]: sql-data-warehouse-overview-development.md

<!--MSDN references-->

<!--Other web references-->


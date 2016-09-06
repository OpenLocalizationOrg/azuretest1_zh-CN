---
ms.openlocfilehash: 40347c6695dbd7a1a6f4764667e9404025108597
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="SQL 数据仓库中的业务连续性规划 |Microsoft Azure"
   description="SQL 数据仓库中的业务连续性的概述。 "
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sahaj08"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/25/2015"
   ms.author="sahajs"/>


# 规划业务连续性 SQL 数据仓库中

本文讲解计划业务连续性和灾难恢复与 SQL 数据仓库的基础知识。 数据仓库是面向企业的信息的中央存储库，它们发挥关键作用，对于分析和商业智能的日常操作中所有级别的组织。 因此有必要数据仓库是可靠的并允许可恢复性和连续操作。 特别是，这篇文章将介绍 SQL 数据仓库如何使您可以从用户错误和使用时间点恢复和地区恢复的灾难恢复。

## 概念

业务连续性和灾难恢复计划需要为以下优化︰

**所需的恢复时间目标 (RTO):**数据库完全恢复中断事件即大胡子可用性之后在失败之前的最大可接受时间。

**所需的恢复点目标 (RPO):**当数据库从中断事件即大丢失的数据恢复在故障期间丢失的更新的最大可接受的时间窗口。

**估计恢复时间 (ERT):**数据库的恢复请求后才能完全正常工作估计的工期。

## 业务连续性方案

**从基础架构故障中恢复︰**这种情况下是指恢复基础结构问题，如磁盘故障等。客户想要确保具有容错能力的业务连续性和高可用的基础结构。

**从用户错误中恢复︰**这种情况下是指从数据损坏或删除非故意或偶然中恢复。 如果用户无意中顺便提一句或者修改或删除数据，客户希望通过数据库还原到更早的点，时间可确保业务连续性。

**的灾难 (DR) 恢复︰**这种情况下指的是从灾难中恢复。 其中中断事件如自然灾害或地区性停电使数据库变得不可用的情况下，客户希望在其他地区继续业务运营恢复数据库。


## 业务连续性功能

让我们看一看 SQL 数据仓库如何增强了数据库的可靠性和可恢复性和连续操作，在上述情况下允许。 


### 数据冗余

由于 SQL 数据仓库分开计算和存储，您的所有数据直接都写入地理冗余 Azure 存储 (RA GRS)。 Geo 冗余存储将数据复制到数百英里的主区域的辅助区域。 在主要和次要的地区，您的数据被复制每个三次跨不同故障域和升级域。 这样可以确保您的数据持久即便完成区域停电或灾难呈现某个区域不可用。 若要了解有关地理冗余存储的读取访问权限的详细信息，请阅读[Azure 存储冗余选项][]。

### 数据库恢复

数据库恢复旨在及时将数据库还原到更早的点。 Azure SQL 数据仓库服务保护与自动存储快照的所有数据库每隔 8 小时，并保留它们的 7 天内为您提供一组离散的还原点。 这些备份都存储在 RA GRS Azure 存储，因此地理冗余，默认情况。 自动备份和还原功能附带任何附加费用，并提供了一种零成本、 零管理的方法，以防止意外损坏或删除的数据库。 若要了解有关数据库还原的详细信息，请参阅[从用户错误中恢复][]。

### 地区恢复

地区恢复旨在在它不可，因为破坏性事件的情况下可以将数据库恢复。 您可以联系支持，从地理冗余在 Azure 的任何区域中创建新的数据库备份还原数据库。 因为备份是在地理冗余可用于恢复数据库，即使数据库是由于停电而无法访问。 Geo 还原功能不附带任何附加费用。


## 下一步行动
若要了解详细信息的其他 SQL 数据库版本的业务连续性功能，请阅读[SQL 数据库业务连续性概述][]。

<!--Image references-->

<!--Article references-->
[业务连续性概览]: ../sql-database/sql-database-business-continuity.md
[完成恢复的数据库]: ../sql-database/sql-database-recovered-finalize.md
[Azure 存储冗余选项]: storage-redundancy/#read-access-geo-redundant-storage-ra-grs.md
[SQL 数据库业务连续性概述]: ../sql-database/sql-database-business-continuity.md
[从用户错误中恢复]: sql-data-warehouse-business-continuity-recover-from-user-error.md

<!--MSDN references-->
[创建数据库的恢复请求]: http://msdn.microsoft.com/library/azure/dn509571.aspx
[数据库操作状态]: http://msdn.microsoft.com/library/azure/dn720371.aspx
[获取可还原已删除的数据库]: http://msdn.microsoft.com/library/azure/dn509574.aspx
[列表可恢复删除数据库]: http://msdn.microsoft.com/library/azure/dn509562.aspx

<!--Other Web references-->



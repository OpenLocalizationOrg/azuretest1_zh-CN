---
ms.openlocfilehash: 715aa594ed217cae8d6e33675bb6359c3bd72f4a
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="迁移到 SQL 数据仓库解决方案 |Microsoft Azure"
   description="迁移到 Azure SQL 数据仓库平台使您的解决方案的指南。"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/22/2015"
   ms.author="JRJ@BigBangData.co.uk;barbkess"/>

# 将您的解决方案迁移到 SQL 数据仓库

SQL 数据仓库是一个分布式的数据库系统，弹性可以进行扩展以满足您的需要。 不维护性能和规模在 SQL Server 中的所有功能都实现 SQL 数据仓库内。 以下迁移主题触摸上迁移到 SQL 数据仓库解决方案的关键因素。 设计数据仓库中的进行比例引入了不同的设计模式，因此传统的方法并不总是最好的。 您可能会发现要使您的解决方案，可以确保您充分利用提供的 SQL 数据仓库的分布式平台。

还有一点需要记住，SQL 数据仓库是基于 Microsoft Azure 中的平台。 因此您迁移过程中的可能也包括将数据转移至云。 数据传输是本身就一个话题，应该仔细考虑;特别是随着量的增加。 数据传输也不应混淆与数据加载的再次一个离散的话题。

## 迁移指南
着手迁移之前请确保您已阅读这些文章，请确保您理解一些基本概念和产品差异。

- [将迁移您的架构][]
- [迁移数据][]
- [将代码迁移][]
 
## 下一步行动
其他的开发技巧，请参阅[开发概述][]。

您还可以查看[事务处理 SQL 参考][]更多的信息。

最后，检查出[加载概述][]，讨论了各种数据加载选项，以及提供逐步指导。

<!--Image references-->

<!--Article references-->
[将迁移您的架构]: sql-data-warehouse-migrate-schema.md
[迁移数据]: sql-data-warehouse-migrate-data.md
[将代码迁移]: sql-data-warehouse-migrate-code.md

[开发概述]: sql-data-warehouse-overview-develop.md
[正在加载概述]: sql-data-warehouse-overview-load.md
[事务处理 SQL 参考]: sql-data-warehouse-overview-migrate.md

<!--MSDN references-->


<!--Other Web references-->


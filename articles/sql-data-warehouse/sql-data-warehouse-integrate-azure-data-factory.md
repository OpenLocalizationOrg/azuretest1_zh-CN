---
ms.openlocfilehash: 0ba059e1c0e36de34d428ff18dff7496eb51c845
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="与 SQL 数据仓库使用 Azure 数据工厂 |Microsoft Azure"
   description="使用 Azure SQL 数据仓库开发解决方案 Azure 数据工厂 (ADF) 的提示。"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/22/2015"
   ms.author="lodipalm"/>

# 与 SQL 数据仓库使用 Azure 数据工厂

Azure 数据工厂提供了协调数据传输和存储过程的 SQL 数据仓库执行完全托管的方法。  这将允许您更方便地设置和计划复杂提取转换和加载 (ETL) 过程与 SQL 数据仓库。 Azure 数据工厂的详细概述，请参阅[Azure 数据工厂文档][]。

## 数据移动 

Azure 数据工厂实现内部来源和不同的 Azure 服务之间的数据移动。  与 Azure 数据工厂集成整体，当前支持的数据移动到和从下列位置︰

+ Azure blob 存储
+ SQL azure 数据库
+ SQL Server 内部
+ SQL Server 在 IaaS 上

有关如何设置数据复制活动请参阅[复制数据与 Azure 数据工厂][]。

> [AZURE.NOTE] 这一次 Azure 数据工厂不能用于数据传输到 SQL 数据仓库中的非空列。 

## 存储的过程
 以相同的方式，它可以用来安排数据传输，Azure 数据工厂还可以用来协调执行的存储过程。  这允许更复杂的管线，以创建并扩展了 Azure 数据工厂能够利用 SQL 数据仓库的计算能力。

## 下一步行动
集成的概述，请参阅[SQL 数据仓库集成概述][]。
开发的更多提示，请参阅[SQL 数据仓库开发概述][]。

<!--Image references-->

<!--Article references-->

[复制数据与 Azure 数据工厂]: ./data-factory-copy-activity/
[SQL 数据仓库开发概述]:  ./sql-data-warehouse-overview-develop/
[SQL 数据仓库集成概述]:  ./sql-data-warehouse-overview-integration/

<!--MSDN references-->

<!--Other Web references-->
[Azure 数据工厂文档]: https://azure.microsoft.com/documentation/services/data-factory/
[复制数据与 Azure 数据工厂]:https://azure.microsoft.com/en-us/documentation/articles/data-factory-data-movement-activities/

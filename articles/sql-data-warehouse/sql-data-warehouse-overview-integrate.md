---
ms.openlocfilehash: 563ba5a8af9a2f42964a7364fe2ac7c296fad56c
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="建立与 SQL 数据仓库集成的解决方案 |Microsoft Azure"
   description="工具和使用与 SQL 数据仓库集成的解决方案的合作伙伴。 "
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

#与 SQL 数据仓库集成服务
其核心功能，除了 SQL 数据仓库使用户能够利用许多 Azure 中的其他服务在其旁边。  具体而言，我们目前已经深结合以下的步骤︰

+ 双电源
+ Azure 数据工厂
+ Azure 的机器学习
+ Azure 流分析

此外，我们正在努力为更多深度合作伙伴提供大量的其他服务整个 Azure 的生态系统。

##双电源
电源双向集成，用户可以很大程度利用 SQL 数据仓库与动态报表的计算能力和可视化的双电源。 与电源 BI corrently 的集成包括︰ 

+ **直接连接**︰ 更高级与对照 SQL 数据仓库逻辑下推连接。  这允许更快地分析更大的比例。
+ **打开电源 BI**: 电源 BI 中打开按钮将实例信息传递给电源 BI，从而更加无缝连接。 

请参阅[集成与双电源](../sql-data-warehouse-integrate-power-bi.md)或[电源 BI 文档](http://blogs.msdn.com/b/powerbi/archive/2015/06/24/exploring-azure-sql-data-warehouse-with-power-bi.aspx)以了解更多信息。

##Azure 数据工厂
Azure 数据工厂为用户提供了创建复杂提取转换加载管道的托管的平台。  Azure 数据工厂与 SQL 数据仓库集成包括以下内容︰

+ **数据移动**︰ 通过大量的内部部署版本和 azure 服务传输日程数据。
+ **存储过程**︰ 协调 SQL 数据仓库上的存储过程的执行。

请参阅[使用 Azure 数据工厂集成](../sql-data-warehouse-integrate-azure-data-factory.md)或[Azure 数据工厂文档](https://azure.microsoft.com/documentation/services/data-factory/)以了解更多信息。

##Azure 的机器学习
Azure 的机器学习是一个完全托管的分析服务，它允许用户创建复杂模型，利用预测工具一大批。  SQL 数据仓库支持作为源和目标的这些模型具有以下功能︰

+ **读取数据︰**在对 SQL 数据仓库使用 T SQL 的扩展驱动器模型。 
+ **将数据写入︰**任何模型的更改提交到 SQL 数据仓库。

请参阅[使用 Azure 机器学习的集成](../sql-data-warehouse-integrate-azure-machine-learning.md)或[Azure 机器学习文档](https://azure.microsoft.com/services/machine-learning/)以了解更多信息。

##Azure 流分析
Azure 流分析是处理和使用从 Azure 事件中心生成的事件数据的复杂的、 完全托管的基础结构。  与 SQL 数据仓库集成允许进行流式处理能够有效地处理和旁边启用更深层次、 更高级的分析的关系数据存储的数据。  

+ **作业输出︰**将输出从流分析作业发送到 SQL 数据仓库直接。

请参阅[使用 Azure 流分析的集成](../sql-data-warehouse-integrate-azure-stream-analytics.md)或[Azure 流分析文档](https://azure.microsoft.com/documentation/services/stream-analytics/)以了解更多信息。

<!--Image references-->

<!--Article references-->
[开发概述]: sql-data-warehouse-overview-develop/

[Azure 数据工厂]: sql-data-warehouse-integrate-azure-data-factory.md
[Azure 的机器学习]: sql-data-warehouse-integrate-azure-machine-learning.md
[Azure 流分析]: sql-data-warehouse-integrate-azure-stream-analytics.md
[双电源]: sql-data-warehouse-integrate-power-bi.md
[合作伙伴]: sql-data-warehouse-integrate-solution-partners.md

<!--MSDN references-->

<!--Other Web references-->

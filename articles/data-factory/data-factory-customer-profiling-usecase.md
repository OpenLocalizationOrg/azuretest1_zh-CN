---
ms.openlocfilehash: f8df4d2903a0a9734cf007ddafbaa66cf9e8a4dd
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="使用案例 — 客户分析" 
    description="了解如何使用 Azure 数据工厂创建数据驱动的工作流 （管道） 来分析游戏的客户。" 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/26/2015" 
    ms.author="spelluru"/>

# 使用案例 — 客户分析

Azure 数据工厂是一种用于实现了 Cortana 分析一套解决方案加速器的许多服务。  Cortana 分析有关的详细信息，请访问[Cortana 分析套件](http://www.microsoft.com/cortanaanalytics)。 在本文中，我们描述了一个简单的使用案例来帮助您开始了解如何 Azure 数据工厂可以解决常见的分析问题。

您只需要访问和试验这一简单的使用情形是[Azure 的订阅](https://azure.microsoft.com/pricing/free-trial/)。  您可以部署[示例](data-factory-samples.md)文章中介绍的步骤实现这一使用情形示例。

## 方案

Contoso 是一家游戏公司创建多个平台的游戏︰ 游戏控制台、 手持式设备和个人电脑 (Pc)。 当用户玩这些游戏，大量日志数据生成跟踪使用模式、 游戏样式和用户的首选项。  再加上人口统计学，区域，以及产品数据，Contoso 还可以指导他们如何增强每个用户的体验有关的分析和目标升级并在游戏中购买。 

Contoso 的目标是确定向上销售和交叉销售机会根据游戏历史记录用户的配置文件，并制定引人注目的新功能来推动业务增长，并为客户提供更好的体验。 为此用例中，我们使用游戏公司为例来希望其用户的行为，对于优化业务，但这些原则适用于任何想要让其客户解决其货物和服务并增强客户体验的企业。

## 面临的挑战

有许多游戏公司试图实现这种类型的用例时所面临的挑战。 首先，从多个数据源，必须 ingested 不同的大小和形状的数据内部部署和在云中捕获产品数据、 历史的用户行为数据和用户数据，如用户在多个平台上玩的游戏。 第二，游戏使用模式必须合理而准确地计算。 第三，游戏公司需要衡量跟踪通过其方法的有效性整体向上销售和交叉销售配置文件-到-中-游戏-采购成功并对其未来的市场营销活动进行调整。

## 解决方案概述

这简单的用例可以用作如何使用 Azure 数据工厂来摄取、 准备、 转换、 分析和发布数据的一个示例。

![端到端工作流](./media/data-factory-customer-profiling-usecase/EndToEndWorkflow.png)上面的图显示了如何数据管道后，显示在 Azure 门户 UI 已部署。

1.  **PartitionGameLogsPipeline**从 blob 存储中读取原始游戏事件并创建分区的年、 月和日。
2.  **EnrichGameLogsPipeline**加入分区与地区代码引用数据的游戏事件和丰富数据，通过将 IP 地址映射到相应的地理位置。
3.  **AnalyzeMarketingCampaignPipeline**管道利用更丰富的数据，以创建最终的输出包含市场活动的市场营销效果的广告数据对其进行处理。

在此示例中使用的大小写，Azure 数据工厂用于协调活动，将输入的数据复制、 转换和处理的数据 （配置单元和猪的转换） 的 HDInsight 活动和输出到 SQL Azure 数据库的最终数据。  此外可以直观地显示数据管道网络、 管理它们，并监视其状态从用户界面。

## 优点

通过优化其用户配置文件分析并使其与业务目标，游戏公司是能够快速收集使用模式，并分析其对所有其不同的游戏产品的市场营销活动的有效性。





测试

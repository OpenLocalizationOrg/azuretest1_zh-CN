---
ms.openlocfilehash: 2c13f030d5201846a476f5e19d2cef91817d38eb
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="实时事件流分析与处理 |Microsoft Azure" 
    description="了解一套 Azure 服务可以启用实时事件处理和分析中进行交互操作。" 
    services="stream-analytics,event-hubs,storage,sql-database" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="paulettm" 
    editor=""/>

<tags 
    ms.service="stream-analytics" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/19/2015" 
    ms.author="jeffstok"/>

# 参考体系结构︰ 实时事件处理与 Microsoft Azure 流分析

实时事件 Azure 流分析与处理的参考体系结构用于为部署实时平台即服务 (PaaS) 流处理解决方案与 Microsoft Azure 提供一个通用的蓝图。

## 摘要

传统上，分析解决方案基于的功能，如 ETL （提取、 转换、 加载） 和数据仓库、 数据存储在分析之前的位置。 不断变化的需求，包括迅速到达更多的数据，都将此现有模型推送到限制。 分析移动存储在流中的数据的能力是一个解决方案，并且虽然不是一项新功能，使用方法已不广泛采用跨所有行业。 

Microsoft Azure 提供广泛的分析技术，能支持多种不同的解决方案应用场景和需求目录。 选择要部署的端到端解决方案的 Azure 服务会提供丰富的产品很难。 本文旨在描述的功能和互操作的事件流解决方案提供支持的各种 Azure 服务。 此外，它还说明了在某些情形下的客户可以受益于这种类型的方法。

## 内容

- 执行摘要
- 实时分析简介
- 在 Azure 中的实时数据的价值主张
- 常见的情况实时分析
- 体系结构和组件
    - 数据源
    - 数据集成层
    - 实时分析层
    - 数据存储层
    - 演示文稿 / 消耗量图层
- 结论

**作者︰**Charles Feddersen，解决方案架构师的卓越，微软公司的数据见解中心

**发布︰**2015 年 1 月

**修订版︰** 1.0

**下载︰**[实时事件处理与 Microsoft Azure 流分析](http://download.microsoft.com/download/6/2/3/623924DE-B083-4561-9624-C1AB62B5F82B/real-time-event-processing-with-microsoft-azure-stream-analytics.pdf)


## 获取帮助
进一步的帮助，请尝试我们的[Azure 流分析论坛](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## 下一步行动

- [Azure 流分析简介](stream-analytics-introduction.md)
- [开始使用 Azure 流分析](stream-analytics-get-started.md)
- [小数位数 Azure 流分析作业](stream-analytics-scale-jobs.md)
- [Azure 流分析查询语言参考](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure 流分析管理 REST API 参考](https://msdn.microsoft.com/library/azure/dn835031.aspx)

 
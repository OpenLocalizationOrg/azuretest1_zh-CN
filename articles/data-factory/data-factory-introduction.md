---
ms.openlocfilehash: b7963b7082bd76958c08be599fb483c3e8291e38
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="Azure 数据工厂简介" 
    description="了解如何使用 Azure 数据工厂服务撰写数据处理、 数据存储和数据移动服务创建生成受信任的信息的管道。" 
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
    ms.topic="get-started-article" 
    ms.date="08/05/2015" 
    ms.author="spelluru"/>

# Azure 数据工厂服务简介

## 概述
数据工厂是协调和自动化移动和转换数据的基于云的数据集成服务。 就像运行设备获取原材料，并将其转换为成品的制造工厂，数据工厂协调，收集原始数据并将其转换为随时可用的信息的现有服务。 

数据工厂工作跨内部和云数据源和 SaaS 可以摄取、 准备、 转换、 分析和发布数据。  数据工厂用于组合到托管的数据流动管线转换数据类似[Azure HDInsight (Hadoop)](http://azure.microsoft.com/documentation/services/hdinsight/)和[Azure 批](http://azure.microsoft.com/documentation/services/batch/)服务为大数据计算的需要，通过使用[Azure 机器学习](http://azure.microsoft.com/documentation/services/machine-learning/)实施分析解决方案的服务。  不再局限于只是表格式监视视图，并使用数据工厂丰富可视化快速显示沿袭和数据管道之间的依赖关系。 监视所有数据流动管线从单个的统一视图，可以方便地查明问题和设置监控警报。

![概述](./media/data-factory-introduction/data-factory-overview.png)

**图 1。** 从许多不同的内部部署数据源收集数据、 摄取并做准备、 组织和分析它与变换，一系列然后将消耗的现成的数据发布。

您可以使用数据工厂，只要您需要收集数据的不同形状和大小，转换，并将其发布来提取深见解--所有在一个可靠的计划。 数据工厂用于创建高可用的数据流量管道，很多情况下不同行业及其分析管道的需求。  在线零售商使用它来生成基于客户浏览行为的个性化的[产品的建议](data-factory-product-reco-usecase.md)。 游戏工作室使用它来了解[其营销的有效性](data-factory-customer-profiling-usecase.md)的市场活动。 直接从我们的客户了解如何以及为什么他们使用数据工厂通过查看[客户案例研究](data-factory-customer-case-studies.md)。 

## 主要概念

Azure 数据工厂有几个关键实体，它们一起定义输入和输出数据，处理事件，并计划和执行所需的数据流所需的资源。

![主要概念](./media/data-factory-introduction/key-concepts.png)

**图 2。** 数据集、 活动、 管道和链接服务之间的关系


### 活动
活动定义要对数据执行的操作。 每个活动采用零个或多个[数据集](data-factory-create-datasets.md)作为输入，并生成输出为一个或多个数据集。 活动是 Azure 数据工厂中业务流程的一个单位。 例如，您可以使用[复制活动](data-factory-data-movement-activities.md)协调的数据从一个数据集复制到另一个。 同样，您可以使用[配置单元活动](data-factory-data-transformation-activities.md)运行配置单元查询 Azure HDInsight 群集转换或分析数据上。 Azure 数据工厂提供了大量的数据转换、 分析和数据移动活动。 

### 管道
[管线](data-factory-create-pipelines.md)是活动的逻辑分组。 他们习惯于到单元组合在一起执行任务的组活动。 例如，一系列几个转换活动可能需要清理日志文件数据。  此序列可以具有复杂的计划，并必须统一协调和自动化的相关性。 所有这些活动可分为单条管道名为"CleanLogFiles"。  "CleanLogFiles"可以再部署、 安排，或删除作为一个单元而不是独立地管理每个单独的活动。

### 数据集
[数据集](data-factory-create-datasets.md)称为引用/指针到您想要用作输入或输出的活动数据。 数据集识别中不同的数据存储，包括表、 文件、 文件夹和文档的数据结构。

### 链接的服务
链接的服务定义为连接到外部资源的数据工厂所需的信息。  链接的服务用于数据工厂中的两个目的︰

- 表示数据存储区，其中包括但不是限于内部部署 SQL Server，Oracle 数据库、 文件共享或 Azure Blob 存储帐户。   如以上所述，数据集表示中连接到数据工厂通过链接服务的数据存储结构。
- 表示计算资源所能承载的活动的执行。  例如，"HDInsightHive 活动"将在 HDInsight Hadoop 群集上执行。

与数据集、 活动、 管道和链接的服务的四个简单的概念，您就可以开始 ！  您可以从零开始[创建您的第一个渠道](data-factory-build-your-first-pipeline.md)，或通过我们[的工厂数据样本](data-factory-samples.md)的文章中的说明部署准备好的示例。 

## 发送反馈
我们真诚欢迎您的反馈意见，在这篇文章。 请花几分钟的时间来提交您的反馈意见通过[电子邮件](mailto:adfdocfeedback@microsoft.com?subject=data-factory-introduction.md)。  





测试

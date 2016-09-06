---
ms.openlocfilehash: 6260bd3fe4131d2a6394561f455d1a61b8499f4e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="数据转换活动 |Microsoft Azure" 
    description="了解如何使用 Azure 数据工厂服务转换和分析数据。" 
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

# 转换和分析使用 Azure 数据工厂

## 概述
在 Azure 数据工厂转换活动转换和处理原始数据到预测和见解。 如 Azure HDInsight 群集或 Azure 批处理计算环境中执行的转换活动。 Azure 数据工厂支持以下转换活动可添加到[管线](data-factory-create-pipelines.md)或者单独或链与另一个活动。


转换活动 |  计算环境 
----------------------- | --------------------
[配置单元](data-factory-hive-activity.md) | HDInsight [Hadoop] 
[小猪](data-factory-pig-activity.md) | HDInsight [Hadoop]  
[MapReduce](data-factory-map-reduce.md) | HDInsight [Hadoop]  
[Hadoop 流](https://msdn.microsoft.com/library/mt185698.aspx) | HDInsight [Hadoop]
[机器学习批处理执行](data-factory-azure-ml-batch-execution-activity.md) | Azure 的 VM 
[存储的过程](data-factory-stored-proc-activity.md) | SQL azure | 
[最低](data-factory-use-custom-activities.md) | HDInsight [Hadoop] 或 Azure 的批处理    

您需要创建链接的服务的计算环境，并将链接的服务定义转换活动时。 有两种类型的数据工厂支持的计算环境。 

1. **按需**︰ 在这种情况下，通过数据工厂完全托管计算环境。 它会自动创建由数据工厂服务，则在提交给流程数据和当作业完成时删除作业之前。 用户可以配置和控制作业执行、 群集管理和引导操作按需计算环境的精确设置。 
2. **将您自己**︰ 在这种情况下，您可以注册自己的计算环境 （例如 HDInsight 群集） 作为链接服务数据工厂中。 由您管理计算环境和数据工厂服务使用它来执行活动。 

请参阅[计算链接服务](data-factory-compute-linked-services.md)文章，以了解有关计算数据工厂支持的链接的服务。 

## 发送反馈
我们真诚欢迎您的反馈意见，在这篇文章。 请花几分钟的时间来提交您的反馈意见通过[电子邮件](mailto:adfdocfeedback@microsoft.com?subject=data-factory-data-transformation-activities.md)。 测试

---
ms.openlocfilehash: cda2760790fa41c94f56983bd5fbfb3354fc3fc1
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="计划一个机器学习高级分析功能环境 |Microsoft Azure" 
    description="通过考虑关键问题规划高级分析功能环境。" 
    services="machine-learning" 
    solutions="" 
    documentationCenter="" 
    authors="msolhab"
    manager="paulettm" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/22/2015" 
    ms.author="mohabib;bradsev" /> 


# 规划 Azure 机器学习高级的分析环境

哪个方案匹配分析问题时准备设置环境以执行高级分析功能使用 Azure 机器学习？  所需的资源方面所做的选择基于类型、 大小和源数据和此数据的目标位置。 本文讨论了这些问题，可帮助您识别您的方案。

一旦确定了相关方案，高级的分析过程和技术 （调整） 工作流中提供的[学习路径︰ 高级 Azure 中的分析解决方案生成](machine-learning-data-science-how-to-create-machine-learning-service.md)。
步骤您通过一系列的任务，从获取数据集通过创建和发布的模型作为 Azure 的 web 服务应用程序能够使用。

本主题还列举的一些资源和工具所使用此高级分析过程。

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## 回答这些问题
回答这些问题，以确定您正在使用之前创建高级的分析环境哪个方案。

1. **您的数据位于何处？** （此位置称为***数据源***。）例如︰
    - 有在 HTTP 地址公开可用的数据。
    - 数据驻留在本地/网络文件位置。
    - 数据是在 SQL Server 数据库中。
    - 数据存储在 Azure 存储容器
2. **如何设置数据的格式？** 例如︰
    - 以逗号分隔或制表符分隔文件，解压缩。
    - 以逗号分隔或制表符分隔文件，压缩。
    - 压缩或未压缩的 Azure blob。
    - SQL Server 表。
3. **您的数据是多少？**
    - 小︰ 小于 2 GB
    - 媒体︰ 大于 2 GB 且不少于 10 GB
    - 大︰ 大于 10 GB
    - 非常大︰ 数百 Gb
4. **如何都熟悉您的数据？**
    - 您是否需要考察数据，从而发现其架构、 变量分布，缺少值，etcetera？ 
    - 数据是否需要预处理或清洗之前转换的表格表示？ 
5. **您是否计划 （或您能） 移动到 Azure 存储的所有数据？**
    - 的打算将整个数据集复制到云环境以进行处理。
    - 不，只有一个子集的数据将复制到 Azure。
6. **什么是您首选的 Azure 云目标？** 例如︰
    - 将数据移动到 Azure 存储 blob。
    - 在装 Azure 虚拟硬盘存储数据。
    - 将数据加载到 SQL Server 数据库 Azure 虚拟机上。
    - 将数据映射到 Azure HDInsight 配置单元表。

## 什么是您的方案？
回答完上一节中的问题，现在可以确定哪种方案最适合您的情况。 示例方案详列[在 Azure 机器学习中的高级分析功能的方案](../machine-learning-data-science-plan-sample-scenarios.md)。

## 在 Azure 中的高级分析功能资源
这取决于您的方案中，您可能需要一些下面的工具和资源。

1.  Azure 的工具︰ 
    *   [Azure PowerShell SDK](../install-configure-powershell.md)， 
    *   [Azure 存储资源管理器](http://azurestorageexplorer.codeplex.com/)
    *   [AzCopy](../storage-use-azcopy.md)
2.  运行 SQL Server 的 azure 虚拟机
3.  Azure HDInsight (Hadoop)
4.  Azure 虚拟网络上-prem 到 Azure 的文件共享
5.  计划的数据迁移的 azure 数据工厂






 
测试

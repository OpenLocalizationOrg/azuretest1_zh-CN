---
ms.openlocfilehash: 9e41c1bfff379504944323cce59039ce38114eac
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="在 HDInsight 中运行 Hadoop 样本 |Microsoft Azure"
    description="开始使用 Azure HDInsight 服务提供的示例。 使用 PowerShell 脚本数据群集上运行 MapReduce 程序。"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="paulettm"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/09/2015"
    ms.author="jgao"/>




#在 HDInsight 中运行 Hadoop 示例

[AZURE.INCLUDE [samples-selector](../../includes/hdinsight-run-samples-selector.md)]

样本一套可用于帮助您获得对 Hadoop 群集使用 Azure HDInsight 启动运行 MapReduce 作业。 这些示例是可在每个创建的 HDInsight 管理群集。 运行这些示例将使您熟悉使用 Azure PowerShell cmdlet Hadoop 群集上运行作业。

MapReduce 的程序也可以运行以编程方式从应用程序通过使用 HDInsight Microsoft.NET API。 有关的作业提交使用 HDInsight Api 的详细信息，请参阅[HDInsight 中提交 Hadoop 作业] [hdinsight 提交作业]。

很多其它文档存在于 Hadoop 相关技术，如基于 Java 的 MapReduce 编程和流式处理，以及在 Windows PowerShell 使用的 cmdlet 的文档 web 脚本。 有关这些资源的详细信息，请参阅[简介 Azure HDInsight][hdinsight 简介]的最后**HDInsight 的资源**部分。

**这些样本是什么**

<p>这些示例旨在使您快速快速上如何部署 Hadoop 作业，并为您提供可扩展的测试平台上使用的概念和服务所使用的脚本编写过程。 他们为您提供的常见任务，如创建和导入数据集的各种大小、 运行及撰写作业按顺序，以及检查作业的结果的示例。 您使用的数据集可以改变大小，以便您可以看到各种大小的数据集上作业性能的影响。</p>


**系统必备组件**︰

- **Azure 订阅**。 请参阅[获取 Azure 免费试用版](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)。
- **HDInsight 群集**。 可以在其中创建此类簇的各种方式的说明，请参阅[设置 HDInsight 群集](hdinsight-provision-clusters.md)。
- **带有 Azure PowerShell 的工作站**。 请参阅[安装和使用 Azure PowerShell](http://azure.microsoft.com/documentation/videos/install-and-use-azure-powershell/)。



## 这些示例 ##

HDInsight 配有以下示例︰

- [**Pi 估算器 Hadoop 示例**][hdinsight 示例-pi 评估程序]︰ 演示如何通过使用统计的 HDInsight 运行 MapReduce 程序 (准蒙特卡罗) 方法来估计 pi 的值。
- [**运行 Hadoop 群集上的 MapReduce 单词计数示例**][hdinsight-示例 wordcount]︰ 说明如何使用 HDInsight 群集运行 MapReduce 程序在文本文件中的单词发生进行计数。
- [**10 GB Graysort Hadoop 示例**][hdinsight 示例 10 gb graysort]︰ 说明如何使用 HDInsight 10 GB 文件上运行通用 GraySort。 有三个要运行的作业︰ Teragen 以生成数据，Terasort 的数据进行排序和确认数据是否已正确排序的 Teravalidate。
- [**C# 流在 Hadoop wordcount MapReduce 示例**][hdinsight-示例-csharp 流]︰ 说明如何使用 C# 来编写一个使用 Hadoop 流接口的 MapReduce 程序。


## 如何运行示例 ##

可以通过使用 Azure PowerShell 运行这些示例。 对于每个前面的示例提供了有关如何执行此操作的说明。

##下一步行动 ##

从这篇文章，文章每个示例中的，您学习了如何运行使用 Azure PowerShell HDInsight 群集中包含的示例。 关于使用的 HDInsight 的小猪，配置单元，然后 MapReduce 的教程，请参阅下列主题︰

* [开始使用 Hadoop 使用 HDInsight 来分析移动手机使用中的配置单元][hdinsight--入门]
* [使用 Hadoop 在 HDInsight 上使用的小猪][使用猪的 hdinsight]
* [使用 Hadoop HDInsight 在蜂巢][配置单元使用 hdinsight-]
* [将 HDInsight 中的 Hadoop 作业提交] [hdinsight 提交作业]
* [Azure HDInsight SDK 文档][hdinsight sdk 文档]
* [在 HDInsight 调试 Hadoop︰ 错误消息] [hdinsight 错误]


[hdinsight 错误]: hdinsight-debug-jobs.md

[hdinsight sdk 文档]: https://msdn.microsoft.com/library/azure/dn479185.aspx

[hdinsight 提交作业]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-简介]: hdinsight-hadoop-introduction.md


[powershell 安装配置]: ../install-configure-powershell.md

[hdinsight--入门]: ../hdinsight-get-started.md

[hdinsight 示例]: hdinsight-run-samples.md
[hdinsight 示例 10 gb graysort]: hdinsight-sample-10gb-graysort.md
[hdinsight-示例-csharp 流]: hdinsight-sample-csharp-streaming.md
[hdinsight 示例-pi 评估程序]: hdinsight-sample-pi-estimator.md
[字数--统计样本 hdinsight]: hdinsight-sample-wordcount.md

[配置-单元使用 hdinsight]: hdinsight-use-hive.md
[使用猪的 hdinsight]: hdinsight-use-pig.md

测试

---
ms.openlocfilehash: 5db7d0546b6a5b49251a9d649dd84163eb6e3627
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="Apache HDInsight 中的触发事件的概述 |Microsoft Azure" 
    description="Apache HDInsight 中的触发和在您的应用程序在 HDInsight 上使用触发的方案简介。" 
    services="hdinsight" 
    documentationCenter="" 
    authors="nitinme" 
    manager="paulettm" 
    editor="cgronlun"
    tags="azure-portal"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/19/2015" 
    ms.author="nitinme"/>

# 概述︰ 在 Azure HDInsight 上的 Apache 触发 
 
<a href="http://spark.apache.org/" target="_blank">Apache 触发</a>支持内存中的并行处理的开放源码框架处理，以提高大数据分析应用程序的性能。 触发处理引擎构建速度，易用性和完善的分析功能。 触发的内存中计算能力使其适合用于迭代算法在机器学习和关系图的计算中。 触发也是 Azure Blob 存储 (WASB) 与兼容的所以您在 Azure 存储的现有数据可以轻松地处理通过触发。

当您配置触发群集在 HDInsight 中的时，触发安装并配置与调配 Azure 计算资源。 只需大约 10 分钟时间准备在 HDInsight 中的触发群集。 要处理的数据存储在 Azure Blob 存储中。 请参阅[使用 Azure Blob 存储与 HDInsight][hdinsight 存储]。

![在 Azure HDInsight 上的 Apache 触发](./media/hdinsight-apache-spark-overview/SparkArchitecture.png  "Apache Spark on Azure HDInsight")


**要开始使用 Azure HDInsight 上的 Apache 触发？** 请参阅[快速入门︰ 调配上 HDInsight 的一个触发群集和运行示例应用程序使用 Jupyter 和 Zeppelin](hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql.md)。



观看概述视频描述在 Azure HDInsight 上的 Apache 触发。

> [AZURE.VIDEO announcing-apache-spark-on-azure-hdinsight]

## 为什么在 Azure HDInsight 上使用触发？ 

Azure HDInsight 提供了一个完全托管的触发服务。 在 HDInsight 上使用触发的好处包括︰

| 功能                              | 说明       |
|-------------------------------------|-------------------|
| 方便的资源调配            | 在几分钟内使用 Azure 管理门户，Azure PowerShell 或 HDInsight.NET SDK，可以设置 HDInsight 上的一个新触发群集。 请参阅[设置触发群集中 HDInsight](hdinsight-apache-spark-provision-clusters.md) |
| 易用性                     | 触发 HDInsight 群集中的包括 Zeppelin 和 Jupyter 笔记本预配置。 您可以使用这些交互式数据处理和可视化效果。 您可以启动群集操控板直接针对触发群集工作的这些笔记本。|
| REST Api                       | 在 HDInsight 中的触发包括触发作业服务器，以便用户能够远程提交和监控正在运行的作业的 REST API 服务器。 |
| 并行查询              | 在 HDInsight 中的触发支持并发查询。 这样，从一个用户或多个查询，从各种用户和应用程序共享相同的群集资源的多个查询。 |
| 在 SSDs 缓存                 | 您可以在内存中或在 SSDs 连接到群集节点中的缓存数据。 缓存在内存中提供的最佳查询性能，但可能是昂贵的;在 SSDs 缓存提供选项非常适合于提高查询性能，而无需创建群集的内存中容纳整个数据集所需的大小。|
| 与 Azure 服务集成 | 在 HDInsight 上的触发到 Azure 事件集线器带有连接器。 客户可以构建流的应用程序使用事件集线器，除了[Kafka](http://kafka.apache.org/)，已可作为触发的一部分。 |
| 与 BI 工具的集成       | HDInsight 的触发流行等[电源 BI](http://www.powerbi.com/)和[Tableau](http://www.tableau.com/products/desktop)用于数据分析的 BI 工具提供接口。|
| 预加载的 Anaconda 库        | 触发 HDInsight 上的群集都附带有预装 Anaconda 库。 [Anaconda](http://docs.continuum.io/anaconda/)会接近 200 库提供了用于机器学习、 数据分析、 可视化等。|
| 可扩展性                     | 虽然在创建过程中，您可以指定群集中的节点数，您可以扩大或缩小群集以匹配工作负载。 所有 HDInsight 群集都允许您更改群集中的节点数。 此外，触发群集可以删除而不会丢失的数据因为所有数据都存储在 Azure Blob 存储。 |
| 24/7 支持                    | 在 HDInsight 上的触发提供了企业级 24/7 支持，99.9%正常运行时间 SLA。|



## 在 HDInsight 上触发的使用情形有哪些？

Apache HDInsight 中的触发使以下关键方案。

### 交互式数据分析和 BI

[看一看教程](hdinsight-apache-spark-use-bi-tools.md)

Apache HDInsight 中的触发在 Azure Blob 存储数据。 业务专家和关键决策者可以分析和生成报表，该数据并使用 Microsoft 电源 BI 构建交互式报告分析的数据。 分析师可以在 Azure 存储中开始从非结构化/半结构化数据、 定义使用笔记本的数据架构，然后生成使用 Microsoft 电源 BI 数据模型。 在 HDInsight 中的触发还支持大量第三方 BI 工具如 Tableau、 Qlikview 和 SAP Lumira 使其对数据分析师、 企业专家和关键决策者的理想平台。

### 迭代的机器学习

[看一看教程](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

Apache 触发配有[MLlib](http://spark.apache.org/mllib/)，机器学习基于触发生成的库中。 除此之外，在 HDInsight 上的触发还包括 Anaconda，Python 分发了大量的机器学习的包。 再加上对 Jupyter 笔记本电脑的内置支持，您必须创建机器学习应用程序的顶级的环境。  

### 流式处理和实时数据分析

[看一看教程](hdinsight-apache-spark-csharp-apache-zeppelin-eventhub-streaming.md)

实时数据分析用于从数据了解到减少处理数据，如落，构建真正的流式传输解决方案的时间的方案。 在 HDInsight 中的触发提供有力的支持，构建实时分析解决方案。 虽然触发已连接器可以摄取像 Kafka，Flume、 Twitter，ZeroMQ 或 TCP 套接字的多个来源的数据，触发 HDInsight 中的增加对接收数据的 Azure 事件集线器的一流支持。 事件集线器是 Azure 上最广泛使用的队列服务。 具有优秀的支持事件集线器的 HDInsight 在一个理想的平台，构建实时分析管道的激励使。

##<a name="next-steps"></a>作为触发群集的一部分包括哪些组件？

在 HDInsight 中的触发包括下列组件默认情况下的群集上可用。

- [1.3.1 触发](https://spark.apache.org/docs/1.3.1/)。 带有触发核、 触发 SQL、 流式 Api、 GraphX 和 MLlib 的触发。
- [Anaconda](http://docs.continuum.io/anaconda/)
- [触发作业服务器](https://github.com/spark-jobserver/spark-jobserver)
- [Zeppelin 笔记本](https://zeppelin.incubator.apache.org)
- [Jupyter 笔记本](https://jupyter.org)

在 HDInsight 中的触发还提供了[ODBC 驱动程序](http://go.microsoft.com/fwlink/?LinkId=616229)连接到 HDInsight 中的触发群集从 Microsoft 电源 BI 和 Tableau BI 工具。

##<a name="see-also"></a>请参见

* [在 HDInsight 与 Zeppelin 笔记本进行交互式数据分析的快速入门︰ 使用触发](hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql.md)
* [HDInsight 在 Apache 触发群集配置](hdinsight-apache-spark-provision-clusters.md)
* [执行与 BI 工具一起使用在 HDInsight 中的触发交互式数据分析](hdinsight-apache-spark-use-bi-tools.md)
* [用 HDInsight 生成计算机教育应用程序触发](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [在 HDInsight 中使用触发，构建实时流的应用程序](hdinsight-apache-spark-csharp-apache-zeppelin-eventhub-streaming.md)
* [管理在 Azure HDInsight Apache 触发群集的资源](hdinsight-apache-spark-resource-manager.md)
* [将作业提交到 Azure HDInsight 上的 Apache 触发群集远程](hdinsight-apache-spark-job-server.md)


[hdinsight 存储]: ../hdinsight-use-blob-storage/

测试

---
ms.openlocfilehash: 2ac0730db06a5b13273fd3605d570ab1fbc7bc91
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="使用资源管理器将资源分配到 Apache 触发群集中 HDInsight |Microsoft Azure" 
    description="了解如何使用触发群集在 HDInsight 上更好的性能资源管理器。" 
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
    ms.date="07/31/2015" 
    ms.author="nitinme"/>


# 管理在 Azure HDInsight Apache 触发群集的资源

资源管理器使您能够管理资源，如内核和群集上运行每个应用程序所使用的 RAM 的触发群集仪表板组件。

## <a name="launchrm"></a>如何启动资源管理器？

1. 从[Azure 预览门户网站](https://ms.portal.azure.com/)，startboard，从单击触发群集的拼贴 （如果您将它固定到 startboard）。 您还可以向下**浏览所有**群集导航 > **HDInsight 群集**。 
 
2. 从触发群集刀片式服务器，单击**仪表板**。 出现提示时，输入触发群集管理员凭据。

    ![启动资源管理器](./media/hdinsight-apache-spark-resource-manager/HDI.Cluster.Launch.Dashboard.png "Start Resource Manager")   

##<a name="scenariosrm"></a>如何解决这些问题，使用资源管理器？

以下是一些您可能会遇到与触发群集，并说明如何解决那些使用资源管理器的常见方案。

### 我在 HDInsight 上的触发群集速度很慢

Apache 触发群集在 HDInsight 中的面向多租户，因此资源拆分为多个组件 （笔记本、 作业服务器等） 不同。 这使您可以同时使用所有触发组件，而不必担心任何组件不能得到资源来运行，但由于资源分散存储，每个组件的速度会变慢。  这可以调整根据您的需要。 


### 我只能用触发群集使用 Jupyter 笔记本。 如何分配给它的所有资源？

1. 从**触发控制板**，请单击**触发用户界面**选项卡，以了解内核，您可以分配给应用程序的最大内存的最大数量。

    ![资源分配](./media/hdinsight-apache-spark-resource-manager/HDI.Spark.UI.Resource.png "Find resources allocated to a Spark cluster")

    将由上述屏幕截图，您可以分配的最大核心是的 7 （总在其中使用 1 8 核） 和最大值，您可以分配的内存是 9 GB （总 12 GB RAM，其中 2 GB 必须设置保留供系统使用和正在由其他应用程序使用的 1 GB）。

    此外应考虑任何正在运行的应用程序。 您可以查看**触发用户界面**选项卡上运行的应用程序。

    ![运行应用程序](./media/hdinsight-apache-spark-resource-manager/HDI.Spark.UI.Running.Apps.png "Applications running on the cluster")

    
2. 从 HDInsight 触发面板，单击**资源管理器**选项卡并指定**默认应用程序核心计数**和**默认执行器每个工作节点的内存**值。 其他属性设置为 0。

    ![资源分配](./media/hdinsight-apache-spark-resource-manager/HDI.Spark.UI.Allocate.Resources.png "Allocate resources to your applications")

### 我没有与触发群集一起使用 BI 工具。 如何重新采取资源？ 

指定为 0 的储蓄服务器核心数和储蓄服务器执行器内存。 没有核心或分配的内存，储蓄服务器将进入**等待**状态。

![资源分配](./media/hdinsight-apache-spark-resource-manager/HDI.Spark.UI.No.Thrift.png "No resources to the thrift server")

##<a name="seealso"></a>请参见

* [概述︰ 在 Azure HDInsight 上的 Apache 触发](hdinsight-apache-spark-overview.md)
* [设置触发 HDInsight 群集上](hdinsight-apache-spark-provision-clusters.md)
* [执行与 BI 工具一起使用在 HDInsight 中的触发交互式数据分析](hdinsight-apache-spark-use-bi-tools.md)
* [用 HDInsight 生成计算机教育应用程序触发](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [在 HDInsight 中使用触发，构建实时流的应用程序](hdinsight-apache-spark-csharp-apache-zeppelin-eventhub-streaming.md)


[hdinsight 版本]: ../hdinsight-component-versioning/
[hdinsight 上载数据]: ../hdinsight-upload-data/
[hdinsight 存储]: ../hdinsight-use-blob-storage/


[azure 的购买选项]: http://azure.microsoft.com/pricing/purchase-options/
[azure 的成员提供]: http://azure.microsoft.com/pricing/member-offers/
[azure 释放试验]: http://azure.microsoft.com/pricing/free-trial/
[azure 管理门户]: https://manage.windowsazure.com/
[azure 创建 storageaccount]: ../storage-create-storage-account/ 

测试

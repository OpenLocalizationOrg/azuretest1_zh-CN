---
ms.openlocfilehash: cb479654b61023d16d16ee93dfcf132a820e6896
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="使用 HDInsight 中的 Apache 触发 Azure 事件集线器来处理流式数据 |Microsoft Azure" 
    description="分步说明如何将数据发送到 Azure 事件集线器传输，然后接收这些事件触发使用 Zeppelin 笔记本" 
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


# 从 Azure 事件集线器与在 HDInsight 上的 Apache 触发触发流︰ 处理事件

触发流扩展触发 API 构建可扩展、 容错的高吞吐量的流处理应用程序的核心。 数据可以从很多来源被 ingested。 在这篇文章中我们将使用事件集线器来摄取数据。 事件集线器是每秒钟的事件，可以摄入量数以百万计的具有高可扩展性的接收系统。 

在本教程中，您将学习如何创建一个 Azure 事件集线器，如何到事件中心在 C# 中使用一个控制台应用程序接收消息并在并行使用配置为在 HDInsight 的 Apache 触发 Zeppelin 笔记本中检索它们。

> [AZURE.NOTE] 若要执行本文中的说明进行操作，必须使用 Azure 门户的两个版本。 若要创建事件中心将使用[Azure 的门户](https://manage.windowsazure.com)。 若要使用 HDInsight 触发群集，您将使用[Azure 预览门户](https://ms.portal.azure.com/)。  

**先决条件：**

您必须具有以下各项︰

- Azure 的订阅。 请参阅[获取 Azure 免费试用版](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)。
- 一个 Apache 触发的群集。 有关说明，请参阅[在 Azure HDInsight 的设置 Apache 触发群集](hdinsight-apache-spark-provision-clusters.md)。
- [Azure 事件中心](service-bus-event-hubs-csharp-ephcs-getstarted.md)。
- 具有 Microsoft Visual Studio 2013年的工作站。 有关说明，请参阅[安装 Visual Studio](https://msdn.microsoft.com/library/e2h7fzkw.aspx)。

##<a name="createeventhub"></a>创建 Azure 事件中心

1. 从[Azure 的门户网站](https://manage.windowsazure.com)，选择**新建** > **服务总线** > **事件中心** > **自定义创建**。

2. 在**添加新的事件中心**屏幕上，输入**事件中心的名称**，选择要创建中，并创建新的命名空间或选择一个现有的**地区**。 单击**箭头**以继续。

    ![向导第 1 页](./media/hdinsight-apache-spark-csharp-apache-zeppelin-eventhub-streaming/HDI.Spark.Streaming.Create.Event.Hub.png "Create an Azure Event Hub")

    > [AZURE.NOTE] 作为减少反应时间和成本的 HDInsight 您 Apache 触发的群集，则应选择相同的**位置**。

3. 在**配置事件中心**屏幕上，输入**分区计数**和**邮件保留**的值，然后单击复选标记。 对于此示例，使用 10 分区计数和 1 的消息保留。 请注意分区计数，因为您以后将需要此值。

    ![向导的第 2 页](./media/hdinsight-apache-spark-csharp-apache-zeppelin-eventhub-streaming/HDI.Spark.Streaming.Create.Event.Hub2.png "Specify partition size and retention days for Event Hub")

4. 单击创建事件中心，单击**配置**，然后创建两个事件中心的访问策略。

    <table>
    <tr><th>名称</th><th>权限</th></tr>
    <tr><td>mysendpolicy</td><td>发送</td></tr>
    <tr><td>myreceivepolicy</td><td>侦听</td></tr>
    </table>

    您创建的权限后，选择页面底部的**保存**图标。 这将创建将用于发送 (**mysendpolicy**) 和侦听此事件中心 (**myreceivepolicy**) 的共享的访问策略。

    ![策略](./media/hdinsight-apache-spark-csharp-apache-zeppelin-eventhub-streaming/HDI.Spark.Streaming.Event.Hub.Policies.png "Create Event Hub policies")

    
5. 在同一页上，记下生成的两个策略的策略密钥。 保存这些键，因为它们将稍后使用。

    ![策略键](./media/hdinsight-apache-spark-csharp-apache-zeppelin-eventhub-streaming/HDI.Spark.Streaming.Event.Hub.Policy.Keys.png "Save policy keys")

6. 在**仪表板**页面上，从底部来检索和保存连接字符串，以事件中心使用两种策略中单击**连接信息**。

    ![策略键](./media/hdinsight-apache-spark-csharp-apache-zeppelin-eventhub-streaming/HDI.Spark.Streaming.Event.Hub.Policy.Connection.Strings.png "Save policy connection strings")

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-csharp](../../includes/service-bus-event-hubs-get-started-send-csharp.md)]

##<a name="receivezeppelin"></a>在触发 HDInsight 使用 Zeppelin 上接收消息

在本节中，您将创建[Zeppelin](https://zeppelin.incubator.apache.org)笔记本到 HDInsight 中触发群集事件集线器接收消息。

1. 从[Azure 预览门户网站](https://ms.portal.azure.com/)，startboard，从单击触发群集的拼贴 （如果您将它固定到 startboard）。 您还可以向下**浏览所有**群集导航 > **HDInsight 群集**。   

2. 启动 Zeppelin 笔记本。 从触发群集刀片式服务器，单击**快速链接**，然后单击**群集仪表板**刀片式服务器，从的**Zeppelin 笔记本**。 出现提示时，输入群集管理员凭据。 按照打开启动笔记本的页上的说明。

2. 创建新的笔记本。 从头窗格中，单击**笔记本**，从下拉列表中，单击**创建新便笺**。

    ![创建新的 Zeppelin 笔记本](./media/hdinsight-apache-spark-csharp-apache-zeppelin-eventhub-streaming/HDI.Spark.CreateNewNote.png "Create a new Zeppelin notebook")

    在同一页上，在**笔记本**标题下，应看到新的笔记本开始**注意 XXXXXXXXX**的名称。 单击新建笔记本。

3. 在 web 页上新笔记本，请单击该标题，和如果您想更改的笔记本名称。 按 ENTER 键保存更改后的名称。 此外，还要确保笔记本头中的右上角显示**已连接**状态。

    ![Zeppelin 笔记本状态](./media/hdinsight-apache-spark-csharp-apache-zeppelin-eventhub-streaming/HDI.Spark.NewNote.Connected.png "Zeppelin notebook status")

4. 默认情况下，新的笔记本中创建的空白段落，粘贴下面的代码段和替换占位符使用事件中心配置。 在本段中，从事件中心接收到该流，并注册到一个临时表，称为**mytemptable**的流。 在下一部分中，我们将开始发送方应用程序。 然后可以直接从表中读取数据。

        import org.apache.spark.streaming.{Seconds, StreamingContext}
        import org.apache.spark.streaming.eventhubs.EventHubsUtils
        import sqlContext.implicits._
        
        val ehParams = Map[String, String](
          "eventhubs.policyname" -> "<name of policy with listen permissions>",
          "eventhubs.policykey" -> "<key of policy with listen permissions>",
          "eventhubs.namespace" -> "<service bus namespace>",
          "eventhubs.name" -> "<event hub in the service bus namespace>",
          "eventhubs.partition.count" -> "1",
          "eventhubs.consumergroup" -> "$default",
          "eventhubs.checkpoint.dir" -> "/<check point directory in your storage account>",
          "eventhubs.checkpoint.interval" -> "10"
        )
        
        val ssc =  new StreamingContext(sc, Seconds(10))
        val stream = EventHubsUtils.createUnionStream(ssc, ehParams)
        
        case class Message(msg: String)
        stream.map(msg=>Message(new String(msg))).foreachRDD(rdd=>rdd.toDF().registerTempTable("mytemptable"))

        stream.print
        ssc.start


##<a name="runapps"></a>运行应用程序

1. 从运行段与段的 Zeppelin 笔记本。 在右上角，请按**SHIFT + enter 键**或**播放**按钮。

    段落的右角上的状态应从已准备好，待执行、 对已完成的运行进度。 输出将显示在同一段落底部。 屏幕抓图如下所示︰

    ![输出的代码段](./media/hdinsight-apache-spark-csharp-apache-zeppelin-eventhub-streaming/HDI.Spark.Streaming.Event.Hub.Zeppelin.Code.Output.png "Output of the snipet")

2. 运行该**发件人**项目并按**enter 键**在控制台窗口中开始将消息发送到该事件的中心。

3. 从 Zeppelin 笔记本，在一个新的段落，输入以下片段阅读中触发接收邮件。

        %sql 
        select * from mytemptable limit 10

    下面的屏幕抓图显示了**mytemptable**在接收到的消息。

    ![在 Zeppelin 收到邮件](./media/hdinsight-apache-spark-csharp-apache-zeppelin-eventhub-streaming/HDI.Spark.Streaming.Event.Hub.Zeppelin.Output.png "Receive messages in Zeppelin notebook")

4. 重新启动触发 SQL 解释器退出应用程序。 单击**解释**选项卡的顶部，并触发解释程序，单击**重新启动**。

    ![重新启动 Zeppelin intepreter](./media/hdinsight-apache-spark-csharp-apache-zeppelin-eventhub-streaming/HDI.Spark.Zeppelin.Restart.Interpreter.png "Restart the Zeppelin intepreter")

##<a name="sparkstreamingha"></a>运行流应用程序提供高可用性

使用 Zeppelin 接收流数据到 HDInsight 上的触发群集是良好的做法到原型应用程序。 但是，运行流应用程序，在生产设置中具有高可用性和恢复能力，需要执行下列操作︰

1. 通过依赖关系的 jar 复制到与群集相关的存储帐户。
2. 生成流的应用程序。
3. RDP 到群集并转移到群集中的 headnode 应用程序 jar 的副本。
3. RDP 到群集和群集节点上运行的应用程序。

有关如何执行这些步骤和流式应用程序示例说明可从 GitHub 在[https://github.com/hdinsight/hdinsight-spark-examples](https://github.com/hdinsight/hdinsight-spark-examples)。 


##<a name="seealso"></a>请参见


* [概述︰ 在 Azure HDInsight 上的 Apache 触发](hdinsight-apache-spark-overview.md)
* [快速入门︰ 设置 Apache 触发 HDInsight 和使用触发 SQL 的运行交互式查询](hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql.md)
* [用 HDInsight 生成计算机教育应用程序触发](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [执行与 BI 工具一起使用在 HDInsight 中的触发交互式数据分析](hdinsight-apache-spark-use-bi-tools.md)
* [管理在 Azure HDInsight Apache 触发群集的资源](hdinsight-apache-spark-resource-manager.md)


[hdinsight 版本]: ../hdinsight-component-versioning/
[hdinsight 上载数据]: ../hdinsight-upload-data/
[hdinsight 存储]: ../hdinsight-use-blob-storage/

[azure 的购买选项]: http://azure.microsoft.com/pricing/purchase-options/
[azure 的成员提供]: http://azure.microsoft.com/pricing/member-offers/
[azure 释放试验]: http://azure.microsoft.com/pricing/free-trial/
[azure 管理门户]: https://manage.windowsazure.com/
[azure 创建 storageaccount]: ../storage-create-storage-account/ 

测试

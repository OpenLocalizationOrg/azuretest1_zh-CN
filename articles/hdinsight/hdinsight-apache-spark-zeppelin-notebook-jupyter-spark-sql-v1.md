---
ms.openlocfilehash: d905c1f002dd8596a0c6d7ac54dcebd9d91a12cd
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="调配上 HDInsight 的一个触发群集和交互式分析使用触发 SQL Zeppelin 与 Jupyter |Azure" 
    description="分步说明如何快速设置 Apache 触发群集在 HDInsight 中，然后使用从 Zeppelin 和 Jupyter 笔记本触发 SQL 运行交互式查询。" 
    services="hdinsight" 
    documentationCenter="" 
    authors="nitinme" 
    manager="paulettm" 
    editor="cgronlun"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/07/2015" 
    ms.author="nitinme"/>


# 快速入门︰ 设置 Apache 触发 HDInsight 和使用触发 SQL 的运行交互式查询

[AZURE.INCLUDE [hdinsight-azure-portal](../../includes/hdinsight-azure-portal.md)]

* [设置在 HDInsight 上的 Apache 触发并运行交互式使用触发 SQL 的查询](hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql.md)

了解如何设置 Apache 触发群集中 HDInsight 使用快速创建选项，然后使用基于 web 的[Zeppelin](https://zeppelin.incubator.apache.org)和[Jupyter](https://jupyter.org)笔记本触发群集上运行触发 SQL 交互式查询。


   ![在 HDInsight 中使用 Apache 触发开始](./media/hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql-v1/HDI.GetStartedFlow.Spark.png  "Get started using Apache Spark in HDInsight tutorial. Steps illustrated: create a storage account; provision a cluster; run Spark SQL statements")

**先决条件：**

在开始本教程之前，您必须先 Azure 的订阅。 请参阅[获取 Azure 免费试用版](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)。

##<a name="storage"></a>创建一个 Azure 存储帐户

当您配置一个在 HDInsight 的 HDInsight 群集时，您指定的 Azure 存储帐户。 特定的 Blob 存储容器从该帐户被指定为默认的文件系统。 默认情况下，HDInsight 群集配置中为您指定的存储帐户相同的数据中心。 有关详细信息，请参阅[使用 Azure Blob 存储与 HDInsight][hdinsight 存储]。


**若要创建一个 Azure 存储帐户**

1. 登录到[Azure 门户][azure 管理门户]。
2. 单击左下角的**新建**，然后输入的值，如图所示。

    ![Azure 的门户位置可用于快速创建新的存储帐户设置](./media/hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql-v1/HDI.StorageAccount.QuickCreate.png "Azure portal where you can use Quick Create to set up a new storage account")

>[AZURE.NOTE]  请确保您在支持的群集中的某个位置中创建存储帐户。

从列表中选择新的存储帐户，然后单击页面底部的**管理访问键**。 记下要**访问主键**(或**辅助快捷键**— 任一项工作)。  在本教程后面部分，将会需要它。 有关详细信息，请参阅[如何创建存储帐户][azure 创建 storageaccount] 。
    
##<a name="provision"></a>条款 HDInsight 触发群集

在本节中，您将配置 HDInsight 3.2 版本的群集，它基于触发版本 1.3.1。 其服务级别协议和 HDInsight 版本有关的信息，请参阅[HDInsight 组件版本控制](hdinsight-component-versioning.md)。

>[AZURE.NOTE] 这篇文章中的步骤创建在 HDInsight Apache 触发群集使用基本配置设置。 有关其他群集配置设置 （如使用配置单元的额外存储空间，Azure 的虚拟网络或 metastore） 的信息，请参阅[设置 HDInsight 群集使用自定义选项](hdinsight-apache-spark-provision-clusters.md)。


**若要设置触发群集** 

1. 登录到[Azure 门户][azure 管理门户]。 

2. 单击左下角的**新建**，然后输入的值，如图所示。

    ![在 HDInsight 中创建一个触发群集](./media/hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql-v1/HDI.QuickCreateCluster.png "Create a Spark cluster in HDInsight")


##<a name="zeppelin"></a>运行交互式触发 SQL 查询使用 Zeppelin 笔记本

配置群集后，您可以使用基于 web 的 Zeppelin 笔记本对触发 HDInsight 群集运行触发 SQL 交互式查询。 在本部分中，我们将使用示例数据文件 (hvac.csv)，可用默认情况下，在群集上运行一些交互式的触发 SQL 查询。

>[AZURE.NOTE] 按照以下说明创建笔记本也是默认情况下，群集上的。 已启动 Zeppelin 后，您会发现此笔记本**Zeppelin HVAC 教程**的名称。

1. 启动 Zeppelin 笔记本。 选择您新创建的触发群集在 Azure 的门户网站，并从门户任务条形图的底部，单击**Zeppelin 笔记本**。 出现提示时，输入群集管理员凭据。 按照打开启动笔记本的页上的说明。

2. 创建新的笔记本。 从头窗格中，单击**笔记本**，然后单击**创建新便笺**。

    ![创建新的 Zeppelin 笔记本](./media/hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql-v1/HDI.Spark.CreateNewNote.png "Create a new Zeppelin notebook")

    在同一页上，在**笔记本**标题下，应看到新的笔记本开始**注意 XXXXXXXXX**的名称。 单击新建笔记本。

3. 在 web 页上新笔记本，请单击该标题，和如果您想更改的笔记本名称。 按 ENTER 键保存更改后的名称。 此外，还要确保笔记本头中的右上角显示**已连接**状态。

    ![Zeppelin 笔记本状态](./media/hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql-v1/HDI.Spark.NewNote.Connected.png "Zeppelin notebook status")

4. 将示例数据加载到临时表中。 当您配置触发群集在 HDInsight 中的时，示例数据文件中， **hvac.csv**，复制到**\HdiSamples\SensorSampleData\hvac**的关联的存储帐户。

    默认情况下，新的笔记本中创建的空白段落，粘贴下面的代码段。

        // Create an RDD using the default Spark context, sc
        val hvacText = sc.textFile("wasb:///HdiSamples/SensorSampleData/hvac/HVAC.csv")
        
        // Define a schema
        case class Hvac(date: String, time: String, targettemp: Integer, actualtemp: Integer, buildingID: String)
        
        // Map the values in the .csv file to the schema
        val hvac = hvacText.map(s => s.split(",")).filter(s => s(0) != "Date").map(
            s => Hvac(s(0), 
                    s(1),
                    s(2).toInt,
                    s(3).toInt,
                    s(6)
            )
        ).toDF()
        
        // Register as a temporary table called "hvac"
        hvac.registerTempTable("hvac")
        
    按**SHIFT + enter 键**或单击**播放**按钮以运行该代码段的段。 段落的右角上的状态应从已准备好，待执行、 对已完成的运行进度。 输出显示在同一段落底部。 屏幕抓图如下所示︰

    ![从原始数据中创建临时表](./media/hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql-v1/HDI.Spark.Note.LoadDataIntoTable.png "Create a temporary table from raw data")

    您还可以提供对每个段落的标题。 从右下角，单击**设置**图标，然后单击**显示标题**。

5. 您现在可以在**hvac**表上运行触发 SQL 语句。 将以下查询粘贴在一个新的段落。 查询检索到的建筑物 ID 和目标和每个构建在给定日期的实际温度之间的差异。 按**SHIFT + ENTER**。

        %sql
        select buildingID, (targettemp - actualtemp) as temp_diff, date 
        from hvac
        where date = "6/1/13" 

    **%Sql**语句的开头告诉笔记本使用触发 SQL 解释器。 您可以查看从笔记本标头中的**解释器**选项卡上定义的译员。

    下面的屏幕快照显示的输出。

    ![运行使用笔记本的触发 SQL 语句](./media/hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql-v1/HDI.Spark.Note.SparkSQLQuery1.png "Run a Spark SQL statement using the notebook")

     单击显示选项 （突出显示的矩形） 为相同的输出的不同表示形式之间进行切换。 单击**设置**以在输出中的键和值选择何种 consitutes。 上面的屏幕捕获使用**buildingID**作为键， **temp_diff**的平均值作为值。

    
6. 您还可以运行触发 SQL 语句的查询中使用的变量。 下一步的代码段演示如何可能值与您要查询的查询中定义一个变量，**温度**。 第一次运行查询时，是为变量指定值自动填充下拉列表。

        %sql
        select buildingID, date, targettemp, (targettemp - actualtemp) as temp_diff
        from hvac
        where targettemp > "${Temp = 65,65|75|85}" 

    粘贴此代码段中一个新的段落，然后按**SHIFT + ENTER**。 下面的屏幕快照显示的输出。

    ![运行使用笔记本的触发 SQL 语句](./media/hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql-v1/HDI.Spark.Note.SparkSQLQuery2.png "Run a Spark SQL statement using the notebook")

    随后的查询，可以从下拉列表中选择一个新值，并再次运行查询。 单击**设置**以在输出中的键和值选择何种 consitutes。 上面的屏幕捕获使用**buildingID**作为键， **temp_diff**作为值和**targettemp**作为组的平均值。

7. 重新启动触发 SQL 解释器退出应用程序。 单击**解释**选项卡的顶部，并触发解释程序，单击**重新启动**。

    ![重新启动 Zeppelin intepreter](./media/hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql-v1/HDI.Spark.Zeppelin.Restart.Interpreter.png "Restart the Zeppelin intepreter")

##<a name="jupyter"></a>运行触发 SQL 查询使用 Jupyter 笔记本

在本节中，您可以使用 Jupyter 笔记本对触发群集运行触发 SQL 查询。

>[AZURE.NOTE] 按照以下说明创建笔记本也是默认情况下，群集上的。 推出了 Jupyter 之后，您会发现此笔记本， **HVACTutorial.ipynb**的名称。

1. 启动 Jupyter 笔记本。 选择触发群集在 Azure 的门户网站，并从门户任务条形图的底部，单击**Jupyter 笔记本**。 出现提示时，输入触发群集管理员凭据。
2. 创建新的笔记本。 单击**新建**，然后单击**Python2**。

    ![创建一个新的 Jupyter 笔记本](./media/hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql-v1/HDI.Spark.Note.Jupyter.CreateNotebook.png "Create a new Jupyter notebook")

3. 创建并打开名为 Untitled.pynb 的新笔记本。 单击顶部的笔记本名称并输入好记的名称。

    ![提供笔记本的名称](./media/hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql-v1/HDI.Spark.Note.Jupyter.Notebook.Name.png "Provide a name for the notebook")

4. 导入所需的模块和创建添姿加 SQL 上下文。 空单元格中粘贴下面的代码段，然后按**SHIFT + ENTER**。

        from pyspark import SparkContext
        from pyspark.sql import SQLContext
        from pyspark.sql.types import *

        # Create Spark and SQL contexts
        sc = SparkContext('spark://headnodehost:7077', 'pyspark')
        sqlContext = SQLContext(sc)

    每次您在 Jupyter 中运行作业，您的 web 浏览器窗口标题将显示笔记本标题以及**（忙碌）**状态。 您还将看到右上角中的**Python 2**文本旁边实心圆。 作业完成之后，这将变成空心圆。

     ![Jupyter 笔记本作业的状态](./media/hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql-v1/HDI.Spark.Jupyter.Job.Status.png "Status of a Jupyter notebook job")

4. 将示例数据加载到临时表中。 当您配置触发群集在 HDInsight 中的时，示例数据文件中， **hvac.csv**，复制到**\HdiSamples\SensorSampleData\hvac**的关联的存储帐户。

    空单元格中粘贴下面的代码段，请按**SHIFT + ENTER**。 此代码段将数据注册到一个名为**hvac**的临时表。


        # Load the data
        hvacText = sc.textFile("wasb:///HdiSamples/SensorSampleData/hvac/HVAC.csv")
        
        # Create the schema
        hvacSchema = StructType([StructField("date", StringType(), False),StructField("time", StringType(), False),StructField("targettemp", IntegerType(), False),StructField("actualtemp", IntegerType(), False),StructField("buildingID", StringType(), False)])
        
        # Parse the data in hvacText
        hvac = hvacText.map(lambda s: s.split(",")).filter(lambda s: s[0] != "Date").map(lambda s:(str(s[0]), str(s[1]), int(s[2]), int(s[3]), str(s[6]) ))
        
        # Create a data frame
        hvacdf = sqlContext.createDataFrame(hvac,hvacSchema)
        
        # Register the data fram as a table to run queries against
        hvacdf.registerAsTable("hvac")
        
        # Run queries against the table and display the data
        data = sqlContext.sql("select buildingID, (targettemp - actualtemp) as temp_diff, date from hvac where date = \"6/1/13\"")
        data.show()

5. 一旦作业成功完成后，将显示以下输出。

        buildingID temp_diff date  
        4          8         6/1/13
        3          2         6/1/13
        7          -10       6/1/13
        12         3         6/1/13
        7          9         6/1/13
        7          5         6/1/13
        3          11        6/1/13
        8          -7        6/1/13
        17         14        6/1/13
        16         -3        6/1/13
        8          -8        6/1/13
        1          -1        6/1/13
        12         11        6/1/13
        3          14        6/1/13
        6          -4        6/1/13
        1          4         6/1/13
        19         4         6/1/13
        19         12        6/1/13
        9          -9        6/1/13
        15         -10       6/1/13

6. 重新启动要退出该应用程序的内核。 从顶部的菜单栏中，单击**内核**，**重新启动**，请单击，然后单击**重新启动**再次提示。

    ![重新启动 Jupyter 内核](./media/hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql-v1/HDI.Spark.Jupyter.Restart.Kernel.png "Restart the Jupyter Kernel")


##<a name="seealso"></a>请参见


* [概述︰ 在 Azure HDInsight 上的 Apache 触发](hdinsight-apache-spark-overview.md)
* [设置触发 HDInsight 群集上](hdinsight-apache-spark-provision-clusters.md)
* [执行与 BI 工具一起使用在 HDInsight 中的触发交互式数据分析](hdinsight-apache-spark-use-bi-tools.md)
* [用 HDInsight 生成计算机教育应用程序触发](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [在 HDInsight 中使用触发，构建实时流的应用程序](hdinsight-apache-spark-csharp-apache-zeppelin-eventhub-streaming.md)
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

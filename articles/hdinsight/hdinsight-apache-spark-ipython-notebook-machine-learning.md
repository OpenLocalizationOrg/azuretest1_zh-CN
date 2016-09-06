---
ms.openlocfilehash: e95bc18418b20745f3a0c7e3dff0e6c741b6f253
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="使用 Apache 触发生成上 HDInsight 的机器学习应用程序 |Microsoft Azure" 
    description="如何使用 Apache 触发笔记本构建机器学习应用程序的分步指导" 
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


# 构建在 Azure HDInsight 上使用 Apache 触发的机器学习应用程序

了解如何构建机器学习在 HDInsight 中使用 Apache 触发群集的应用程序。 本文介绍如何使用群集可用的 Jupyter 笔记本以生成并测试我们的应用程序。 应用程序使用在默认情况下所有群集可用的示例 HVAC.csv 数据。

**先决条件：**

您必须具有以下各项︰

- Azure 的订阅。 请参阅[获取 Azure 免费试用版](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)。
- 一个 Apache 触发的群集。 有关说明，请参阅[在 Azure HDInsight 的设置 Apache 触发群集](hdinsight-apache-spark-provision-clusters.md)。 

##<a name="data"></a>显示数据

我们开始构建该应用程序之前，让我们了解数据和分析的数据，我们可以执行何种的结构。 

在本文中，我们使用在默认情况下，在**\HdiSamples\SensorSampleData\hvac**的所有 HDInsight 群集可用的示例**HVAC.csv**数据文件。 下载并打开该 CSV 文件获取的数据的快照。  

![取暖通风空调数据快照](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/HDI.Spark.ML.Show.Data.png "Snapshot of the HVAC data")

数据显示目标温度和已安装的 HVAC 系统的建筑物的实际温度。 让我们假定**系统**列表示系统 ID **SystemAge**列表示的 HVAC 系统已存在于该建筑物的年数。

我们使用此数据来预测是否建筑物会炎热或 colder 基于目标温度，给出系统 ID 和系统年龄上。

##<a name="app"></a>编写使用触发 MLlib 的机器学习应用程序

1. 从[Azure 预览门户网站](https://ms.portal.azure.com/)，startboard，从单击触发群集的拼贴 （如果您将它固定到 startboard）。 您还可以向下**浏览所有**群集导航 > **HDInsight 群集**。 
 
2. 启动[Jupyter](https://jupyter.org)笔记本。 从触发群集刀片式服务器，单击**快速链接**，然后从**群集仪表板**刀片式服务器，请单击**Jupyter 笔记本**。 出现提示时，输入触发群集管理员凭据。

2. 创建新的笔记本。 单击**新建**，然后单击**Python 2**。

    ![创建一个新的 Jupyter 笔记本](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/HDI.Spark.Note.Jupyter.CreateNotebook.png "Create a new Jupyter notebook")

3. 创建并打开名为 Untitled.pynb 的新笔记本。 单击顶部的笔记本名称并输入好记的名称。

    ![提供笔记本的名称](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/HDI.Spark.Note.Jupyter.Notebook.Name.png "Provide a name for the notebook")

3. 开始构建您的机器学习的应用程序。 此应用程序中我们使用触发 ML 管道执行文档分类。 在管线中，我们将文档拆分为词、 将文字转换为数字的特征向量，和最后生成特征向量和标签使用的预测模型。

    若要开始生成应用程序，首先导入所需的模块，并将资源分配给应用程序。 新笔记本中的空单元格，在粘贴下面的代码段，然后按**SHIFT + ENTER**。


        from pyspark.ml import Pipeline
        from pyspark.ml.classification import LogisticRegression
        from pyspark.ml.feature import HashingTF, Tokenizer
        from pyspark.sql import Row, SQLContext
        
        import os
        import sys
        from pyspark import SparkConf
        from pyspark import SparkContext
        from pyspark.sql import SQLContext
        from pyspark.sql.types import *
        
        from pyspark.mllib.classification import LogisticRegressionWithSGD
        from pyspark.mllib.regression import LabeledPoint
        from numpy import array
        
        # Assign resources to the application
        conf = SparkConf()
        conf.setMaster('spark://headnodehost:7077')
        conf.setAppName('pysparkregression')
        conf.set("spark.cores.max", "4")
        conf.set("spark.executor.memory", "4g")
        
        sc = SparkContext(conf=conf)
        sqlContext = SQLContext(sc)

    每次您在 Jupyter 中运行作业，您的 web 浏览器窗口标题将显示笔记本标题以及**（忙碌）**状态。 您还将看到右上角中的**Python 2**文本旁边实心圆。 作业完成之后，这将变成空心圆。

     ![Jupyter 笔记本作业的状态](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/HDI.Spark.Jupyter.Job.Status.png "Status of a Jupyter notebook job")
 
4. 您现在必须加载数据 (hvac.csv)，分析它，并使用它来训练模型。 为此，您可以定义检查建筑物的实际温度是否大于目标温度的函数。 如果实际温度更高，建筑物热，是由值**1.0**。 如果实际温度较低，建筑是冷，值**0.0**表示。 

    在一个空单元格中粘贴下面的代码段，然后按**SHIFT + ENTER**。

        
        # List the structure of data for better understanding. Becuase the data will be
        # loaded as an array, this structure makes it easy to understand what each element
        # in the array corresponds to

        # 0 Date
        # 1 Time
        # 2 TargetTemp
        # 3 ActualTemp
        # 4 System
        # 5 SystemAge
        # 6 BuildingID

        LabeledDocument = Row("BuildingID", "SystemInfo", "label")

        # Define a function that parses the raw CSV file and returns an object of type LabeledDocument
        
        def parseDocument(line):
            values = [str(x) for x in line.split(',')]
            if (values[3] > values[2]):
                hot = 1.0
            else:
                hot = 0.0        
    
            textValue = str(values[4]) + " " + str(values[5])
    
            return LabeledDocument((values[6]), textValue, hot)

        # Load the raw HVAC.csv file, parse it using the function
        data = sc.textFile("wasb:///HdiSamples/SensorSampleData/hvac/HVAC.csv")

        documents = data.filter(lambda s: "Date" not in s).map(parseDocument)
        training = documents.toDF()


5. 配置触发机器学习管道组成的三个阶段︰ 标记器、 hashingTF 和 lr。 有关什么是管道及其工作原理请参阅详细信息<a href="http://spark.apache.org/docs/latest/ml-guide.html#how-it-works" target="_blank">触发机器学习管道</a>。

    在一个空单元格中粘贴下面的代码段，然后按**SHIFT + ENTER**。

        tokenizer = Tokenizer(inputCol="SystemInfo", outputCol="words")
        hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
        lr = LogisticRegression(maxIter=10, regParam=0.01)
        pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])

6. 适合于培训文档的管道。 在一个空单元格中粘贴下面的代码段，然后按**SHIFT + ENTER**。

        model = pipeline.fit(training)

7. 验证检查点培训文档您与应用程序的进度。 在一个空单元格中粘贴下面的代码段，然后按**SHIFT + ENTER**。

        training.show()

    这应类似于下面的输出︰

        BuildingID SystemInfo label
        4          13 20      0.0  
        17         3 20       0.0  
        18         17 20      1.0  
        15         2 23       0.0  
        3          16 9       1.0  
        4          13 28      0.0  
        2          12 24      0.0  
        16         20 26      1.0  
        9          16 9       1.0  
        12         6 5        0.0  
        15         10 17      1.0  
        7          2 11       0.0  
        15         14 2       1.0  
        6          3 2        0.0  
        20         19 22      0.0  
        8          19 11      0.0  
        6          15 7       0.0  
        13         12 5       0.0  
        4          8 22       0.0  
        7          17 5       0.0

    回过头来验证对该原始 CSV 文件的输出。 例如，该 CSV 文件的第一行具有此数据︰

    ![取暖通风空调数据快照](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/HDI.Spark.ML.Show.Data.First.Row.png "Snapshot of the HVAC data")

    请注意如何实际温度小于目标温度建议建筑很冷。 因此在培训输出中，**标签**中的第一行的值为**0.0**，这意味着建筑物不热。

8.  准备一个数据集来运行对培训的模型。 这样做，我们将通过系统 ID 和系统 （称为**SystemInfo**培训输出中） 时代，而模型可以预测是否具有该系统 ID 和系统时代的建筑是炎热 （由 1.0） 或冷色 （由 0.0 表示）。

    在一个空单元格中粘贴下面的代码段，然后按**SHIFT + ENTER**。
        
        # SystemInfo here is a combination of system ID followed by system age
        Document = Row("id", "SystemInfo")
        test = sc.parallelize([(1L, "20 25"),
                      (2L, "4 15"),
                      (3L, "16 9"),
                      (4L, "9 22"),
                      (5L, "17 10"),
                      (6L, "7 22")]) \
            .map(lambda x: Document(*x)).toDF() 

9. 最后，对测试数据进行预测。 在一个空单元格中粘贴下面的代码段，然后按**SHIFT + ENTER**。

        # Make predictions on test documents and print columns of interest
        prediction = model.transform(test)
        selected = prediction.select("SystemInfo", "prediction", "probability")
        for row in selected.collect():
            print row

10. 您应该看到类似于下面的输出︰

        Row(SystemInfo=u'20 25', prediction=1.0, probability=DenseVector([0.4999, 0.5001]))
        Row(SystemInfo=u'4 15', prediction=0.0, probability=DenseVector([0.5016, 0.4984]))
        Row(SystemInfo=u'16 9', prediction=1.0, probability=DenseVector([0.4785, 0.5215]))
        Row(SystemInfo=u'9 22', prediction=1.0, probability=DenseVector([0.4549, 0.5451]))
        Row(SystemInfo=u'17 10', prediction=1.0, probability=DenseVector([0.4925, 0.5075]))
        Row(SystemInfo=u'7 22', prediction=0.0, probability=DenseVector([0.5015, 0.4985]))

    从预测中的第一行，您可以看到对于 HVAC 系统 ID 20 和系统期限为 25 年，建筑物将会热 (**预测 = 1.0**)。 DenseVector (0.49999) 的第一个值对应预测 0.0，1.0 预测与对应的第二个值 (0.5001)。 在输出中，即使第二个值只是略高，模型显示**预测 = 1.0**。

11. 现在，您可以通过重新启动内核退出笔记本。 从顶部的菜单栏中，单击**内核**，**重新启动**，请单击，然后单击**重新启动**再次提示。

    ![重新启动 Jupyter 内核](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/HDI.Spark.Jupyter.Restart.Kernel.png "Restart the Jupyter Kernel")
           

##<a name="anaconda"></a>使用 Anaconda scikit-为机器学习了解库

在 HDInsight 上的 Apache 触发群集包括 Anaconda 库。 这还包括**scikit-了解**的机器学习的库。 库还包含可用于生成示例应用程序直接从 Jupyter 笔记本的各种数据集。 有关示例使用 scikit-了解库信息，请参阅[http://scikit-learn.org/stable/auto_examples/index.html](http://scikit-learn.org/stable/auto_examples/index.html)。

##<a name="seealso"></a>请参见

* [概述︰ 在 Azure HDInsight 上的 Apache 触发](hdinsight-apache-spark-overview.md)
* [设置触发 HDInsight 群集上](hdinsight-apache-spark-provision-clusters.md)
* [执行与 BI 工具一起使用在 HDInsight 中的触发交互式数据分析](hdinsight-apache-spark-use-bi-tools.md)
* [在 HDInsight 中使用触发，构建实时流的应用程序](hdinsight-apache-spark-csharp-apache-zeppelin-eventhub-streaming.md)
* [管理在 Azure HDInsight Apache 触发群集的资源](hdinsight-apache-spark-resource-manager.md)


[hdinsight 版本]: ../hdinsight-component-versioning/

[hdinsight 上载数据]: ../hdinsight-upload-data/

[hdinsight 存储]: ../hdinsight-use-blob-storage/


[网络日志的 hdinsight 示例]: ../hdinsight-hive-analyze-website-log/
[hdinsight 传感器数据示例]: ../hdinsight-hive-analyze-sensor-data/

[azure 的购买选项]: http://azure.microsoft.com/pricing/purchase-options/
[azure 的成员提供]: http://azure.microsoft.com/pricing/member-offers/
[azure 释放试验]: http://azure.microsoft.com/pricing/free-trial/
[azure 管理门户]: https://manage.windowsazure.com/
[azure 创建 storageaccount]: ../storage-create-storage-account/

测试

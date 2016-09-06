---
ms.openlocfilehash: 7f1bf39dbf011f98e2da578f5623bd0402d8862c
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="Apache 激励作业服务器上 HDInsight |Microsoft Azure" 
    description="了解如何使用触发作业服务器远程提交并管理一个触发群集上的作业。" 
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
    ms.date="07/10/2015" 
    ms.author="nitinme"/>


# 在 Azure HDInsight 触发作业服务器群集

在 Azure HDInight 上的 Apache 触发群集包触发作业服务器群集部署的一部分。 触发作业服务器提供了 REST Api 来创建触发上下文、 提交触发应用程序上下文、 检查作业状态、 kill 上下文等。本文提供有关如何使用 Curl 触发群集使用的作业服务器上执行一些常见任务的一些示例。

>[AZURE.NOTE] 有关触发作业服务器的完整文档，请参阅[https://github.com/spark-jobserver/spark-jobserver](https://github.com/spark-jobserver/spark-jobserver)。 

## <a name="uploadjar"></a>将 jar 上载到触发群集

    curl.exe -k -u "<hdinsight user>:<user password>" --data-binary @<location of jar on the computer> https://<cluster name>.azurehdinsight.net/sparkjobserver/jars/<application name>

示例：
    
    curl.exe -k -u "myuser:myPass@word1" --data-binary @C:\mylocation\eventhubs-examples\target\spark-streaming-eventhubs-example-0.1.0-jar-with-dependencies.jar https://mysparkcluster.azurehdinsight.net/sparkjobserver/jars/streamingjar


##<a name="createcontext"></a>在作业服务器中创建新的持久上下文

    curl.exe -k -u "<hdinsight user>:<user password>" -d "" "https://<cluster name>.azurehdinsight.net/sparkjobserver/contexts/<context name>?num-cpu-cores=<value>&memory-per-node=<value>"

示例：

    curl.exe -k -u "myuser:myPass@word1" -d "" "https://mysparkcluster.azurehdinsight.net/sparkjobserver/contexts/mystreaming?num-cpu-cores=4&memory-per-node=1024m"


##<a name="submitapp"></a>提交到群集应用程序

    curl.exe -k -u "<hdinsight user>:<user password>" -d @<input file name> "https://<cluster name>.azurehdinsight.net/sparkjobserver/jobs?appName=<app name>&classPath=<class path>&context=<context>"

示例：

    curl.exe -k -u "myuser:myPass@word1" -d @mypostdata.txt "https://mysparkcluster.azurehdinsight.net/sparkjobserver/jobs?appName=streamingjar&classPath=org.apache.spark.streaming.eventhubs.example.EventCountJobServer&context=mystreaming"

其中 mypostdata.txt 定义您的应用程序。


##<a name="submitapp"></a>删除作业

    curl.exe -X DELETE -k -u "<hdinsight user>:<user password>" "https://<cluster name>.azurehdinsight.net/sparkjobserver/contexts/<context>"

示例：

    curl.exe -X DELETE -k -u "myuser:myPass@word1" "https://mysparkcluster.azurehdinsight.net/sparkjobserver/contexts/mystreaming"


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

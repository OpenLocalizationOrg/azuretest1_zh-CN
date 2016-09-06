---
ms.openlocfilehash: 81528d9c9ac9279cb04d1efef2efd237e796f95e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="调试和分析堆转储的 Hadoop 服务 |Microsoft Azure"
    description="自动收集堆转储的 Hadoop 服务并将为调试和分析的 Azure Blob 存储帐户内。"
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


# 收集堆转储调试和分析 Hadoop 服务的 Blob 存储在

[AZURE.INCLUDE [heapdump-selector](../../includes/hdinsight-selector-heap-dump.md)]

堆转储可以自动为 Hadoop 服务收集并放在 Azure Blob 存储帐户下 HDInsightHeapDumps 的用户 /。 服务与堆转储文件包含应用程序的内存的快照。 这在创建转储的时间包括变量的值。

单个群集服务必须启用的堆转储的不同服务的集合。 此功能的默认设置是关闭群集。 因此它们被保存后集合已启用建议监视 Blob 存储帐户，则可能很大，这些堆转储。

> [AZURE.NOTE] 这篇文章中的信息仅适用于基于 Windows 的 HDInsight。 基于 linux * 的 HDInsight 的信息，请参阅[启用基于 Linux 的 HDInsight 上的 Hadoop 服务的堆转储](hdinsight-hadoop-collect-debug-heap-dump-linux.md)

## <a name="whichServices"></a>堆转储的合格服务

可以启用请求的堆转储的服务有︰

*  **hcatalog** -tempelton
*  **配置单元**-hiveserver2、 metastore、 derbyserver
*  **mapreduce** -jobhistoryserver
*  **yarn** -resourcemanager、 nodemanager、 timelineserver
*  **hdfs** -datanode、 secondarynamenode、 namenode

## <a name="configuration"></a>启用堆转储的配置元素

若要打开堆转储的服务，您需要该服务，由**service_name**指定的节中设置相应的配置元素。

    "javaargs.<service_name>.XX:+HeapDumpOnOutOfMemoryError" = "-XX:+HeapDumpOnOutOfMemoryError",
    "javaargs.<service_name>.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\Dumps\<service_name>_%date:~4,2%%date:~7,2%%date:~10,2%%time:~0,2%%time:~3,2%%time:~6,2%.hprof"

**Service_name**的值可以是任何上面列出的服务︰ tempelton，hiveserver2，metastore，derbyserver，jobhistoryserver，resourcemanager，nodemanager，timelineserver，datanode，secondarynamenode，或 namenode。

## <a name="powershell"></a>如何启用使用 Azure PowerShell 的堆转储

例如，若要通过使用 jobhistoryserver Azure PowerShell 打开堆转储，您可以执行以下︰

    $MapRedConfigValues = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightMapReduceConfiguration'

    $MapRedConfigValues.Configuration = @{ "javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError"="-XX:+HeapDumpOnOutOfMemoryError" ; "javaargs.jobhistoryserver.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof" }

## <a name="sdk"></a>如何启用使用 Azure HDInsight.NET SDK 的堆转储

例如，若要通过使用 jobhistoryserver Azure HDInsight.NET SDK 打开堆转储，您可以执行以下︰

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError", "-XX:+HeapDumpOnOutOfMemoryError"));

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:HeapDumpPath", "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof"));

测试

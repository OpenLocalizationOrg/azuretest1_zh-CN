---
ms.openlocfilehash: 184a5274bc725044e9ac3251615e870758d3782f
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="C# 流 wordcount Hadoop 示例 |Microsoft Azure"
    description="如何编写 MapReduce 程序在 C# 中使用 Hadoop 流界面，以及如何在使用 PowerShell cmdlet 的 HDInsight 上运行它们。"
    editor="cgronlun"
    manager="paulettm"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="07/09/2015"
    ms.author="jgao"/>

# C# 流中 Hadoop 在 HDInsight 上的单词计数 MapReduce 示例

Hadoop 为 MapReduce，使您能够编写地图和减少非 Java 语言的函数提供的流式 API。 本教程展示如何使用 Hadoop 流界面中 C# 编写 MapReduce 程序以及如何使用 Azure PowerShell cmdlet Azure HDInsight 中运行的程序。

> [AZURE.NOTE] 在本教程中的步骤仅适用于基于 Windows HDInsight 群集。 流的基于 Linux 的 HDInsight 群集的示例，请参见[开发 Python HDInsight 流程序](hdinsight-hadoop-streaming-python.md)。

在示例中，映射器和变径的输入读取[stdin][stdin 的 stdout stderr] （线） 的可执行文件，并发出到[stdout][stdin 的 stdout stderr]输出。 该程序计算所有文本中的单词。

**Mappers**用于指定可执行文件时，每个映射任务启动作为一个单独的进程可执行文件，映射程序初始化时。 映射任务运行时，它将其输入转换成线，并对进程的[stdin][stdin 的 stdout stderr]源行。

在此期间，映射程序收集进程的标准输出从面向行的输出。 它将每行转换为键/值对，收集作为映射器的输出。 默认情况下，直到第一个制表符的行的前缀是关键，，（不包括制表符） 该行的其余部分值。 如果在行中有任何制表符，整行被视为作为键，并且值为 null。

对于**变径**指定可执行文件时，每个变径任务启动作为一个单独的进程可执行文件，变径初始化时。 变径任务运行时，它将转换行，为其输入的键/值对和它对进程的[stdin][stdin 的 stdout stderr]源行。

在此期间，变径收集进程的[标准输出][stdin 的 stdout stderr]从面向行的输出。 它将每行转换为键/值对，其中作为变径输出收集。 默认情况下，直到第一个制表符的行的前缀是关键，，（不包括制表符） 该行的其余部分值。

关于 Hadoop 流界面的详细信息，请参阅[Hadoop 流][hadoop 流]。

**在本教程中，您将学习如何︰**

* 使用 Azure PowerShell 运行 C# 流程序在 HDInsight 文件中包含的数据进行分析。
* 编写使用 Hadoop 流接口的 C# 代码。


**系统必备组件**︰

在开始之前，您必须具有以下︰

- **Azure 订阅**。 请参阅[获取 Azure 免费试用版](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)。
- **HDInsight 群集**。 可以在其中创建此类簇的各种方式的说明，请参阅[设置 HDInsight 群集](hdinsight-provision-clusters.md)。
- **带有 Azure PowerShell 的工作站**。 请参阅[安装和使用 Azure PowerShell](http://azure.microsoft.com/documentation/videos/install-and-use-azure-powershell/)。



## <a id="run-sample"></a>使用 Azure PowerShell 运行示例

**若要运行 MapReduce 作业**

1.  打开**Azure PowerShell**。 若要打开 Azure PowerShell 控制台窗口的说明，请参阅[安装和配置 Azure PowerShell][powershell 安装配置]。

3. 设置这两个变量中的以下命令，然后运行它们︰

        $subscriptionName = "<SubscriptionName>"   # Azure subscription name
        $clusterName = "<ClusterName>"             # HDInsight cluster name


2. 运行以下命令，以定义 MapReduce 作业︰

        # Create a MapReduce job definition for the streaming job.
        $streamingWC = New-AzureHDInsightStreamingMapReduceJobDefinition -Files "/example/apps/wc.exe", "/example/apps/cat.exe" -InputPath "/example/data/gutenberg/davinci.txt" -OutputPath "/example/data/StreamingOutput/wc.txt" -Mapper "cat.exe" -Reducer "wc.exe"

    参数指定的映射器和变径函数和输入的文件和输出文件。

5. 运行以下命令来运行 MapReduce 作业，等待作业完成，然后打印标准错误消息︰

        # Run the C# Streaming MapReduce job.
        # Wait for the job to complete.
        # Print output and standard error file of the MapReduce job
        Select-AzureSubscription $subscriptionName
        $streamingWC | Start-AzureHDInsightJob -Cluster $clustername | Wait-AzureHDInsightJob -WaitTimeoutInSeconds 3600 | Get-AzureHDInsightJobOutput -Cluster $clustername -StandardError

6. 运行以下命令以显示的字数统计结果︰

        $subscriptionName = "<SubscriptionName>"
        $storageAccountName = "<StorageAccountName>"
        $containerName = "<ContainerName>"

        # Select the current subscription
        Select-AzureSubscription $subscriptionName

        # Blob storage container and account name
      $storageAccountKey = 获取 AzureStorageKey StorageAccountName $storageAccountName |%{ $_.主要} $storageContext = New AzureStorageContext-StorageAccountName $storageAccountName-StorageAccountKey $storageAccountKey

        # Retrieve the output
        Get-AzureStorageBlobContent -Container $containerName -Blob "example/data/StreamingOutput/wc.txt/part-00000" -Context $storageContext -Force

        # The number of words in the text is:
        cat ./example/data/StreamingOutput/wc.txt/part-00000

    请注意，MapReduce 作业的输出文件是不可变的。 所以如果您重新运行此示例，您需要更改输出文件的名称。


## <a id="java-code"></a>Hadoop 流 C# 代码


MapReduce 程序使用 cat.exe 应用程序映射接口作为流文本到控制台和 wc.exe 应用程序作为减少接口的传输文档中的单词计数。 映射器和变径从标准输入流 (stdin) 中读取字符，行的行，并将写入到标准输出流 (stdout)。



    // The source code for the cat.exe (Mapper).

    using System;
    using System.IO;

    namespace cat
    {
        class cat
        {
            static void Main(string[] args)
            {
                if (args.Length > 0)
                {
                    Console.SetIn(new StreamReader(args[0]));
                }

                string line;
                while ((line = Console.ReadLine()) != null)
                {
                    Console.WriteLine(line);
                }
            }
        }
    }



在 cat.cs 文件中的映射代码使用[StreamReader][streamreader]对象读取到控制台，然后将流写入标准输出流具有静态[Console.Writeline][控制台 writeline]方法传入流的字符。


    // The source code for wc.exe (Reducer) is:

    using System;
    using System.IO;
    using System.Linq;

    namespace wc
    {
        class wc
        {
            static void Main(string[] args)
            {
                string line;
                var count = 0;

                if (args.Length > 0){
                    Console.SetIn(new StreamReader(args[0]));
                }

                while ((line = Console.ReadLine()) != null) {
                    count += line.Count(cr => (cr == ' ' || cr == '\n'));
                }
                Console.WriteLine(count);
            }
        }
    }


Wc.cs 文件中的变径代码使用[StreamReader][streamreader]对象都已输出 cat.exe 映射程序的标准输入流中读取字符。 与[Console.Writeline][控制台 writeline]方法读取字符，它的方法是计算空格和行尾字符结尾的每个单词计算单词。 然后写入总写入标准输出流与[Console.Writeline][控制台 writeline]方法。


## <a id="summary"></a>摘要

在本教程中，您看到了如何使用 Hadoop 流部署中 HDInsight 的 MapReduce 作业。

## <a id="next-steps"></a>下一步行动


教程，运行其他示例，并提供运行猪、 配置单元和 MapReduce 作业与 Azure PowerShell Azure HDInsight 中的说明，请参阅下列文章︰

* [开始使用 Azure HDInsight][hdinsight--入门]
* [示例︰ Pi 评估程序][hdinsight 示例-pi 评估程序]
* [示例︰ 字数][字数统计样本-hdinsight-]
* [示例︰ 10 GB GraySort][hdinsight 示例 10 gb graysort]
* [使用与 HDInsight 的小猪][使用猪的 hdinsight]
* [使用 HDInsight 蜂巢][配置单元使用 hdinsight-]
* [Azure HDInsight SDK 文档][hdinsight sdk 文档]

[hdinsight sdk 文档]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[hadoop 流]: http://wiki.apache.org/hadoop/HadoopStreaming
[streamreader]: http://msdn.microsoft.com/library/system.io.streamreader.aspx
[控制台 writeline]: http://msdn.microsoft.com/library/system.console.writeline
[标准输入的 stdout stderr]: http://msdn.microsoft.com/library/3x292kth(v=vs.110).aspx

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

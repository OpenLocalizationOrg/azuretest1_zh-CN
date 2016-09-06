---
ms.openlocfilehash: a0563f43b80d76db61e2065f1c8711ab03fedca3
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="使用 Hadoop MapReduce 和 PowerShell |Microsoft Azure"
   description="了解如何使用 PowerShell 远程运行 MapReduce 作业使用 Hadoop HDInsight。"
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="paulettm"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="07/06/2015"
   ms.author="larryfr"/>

#运行配置单元查询使用 Hadoop 使用 PowerShell 的 HDInsight

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

本文档提供使用 Azure PowerShell Hadoop HDInsight 群集上运行 MapReduce 作业的示例。

##<a id="prereq"></a>先决条件

若要完成本文中的步骤操作，您需要︰

- **Azure HDInsight (在 HDInsight 上的 Hadoop) 群集 （基于 Windows 或基于 Linux 的）**

- **带有 Azure PowerShell 的工作站**。 请参见[安装和配置 Azure PowerShell](../powershell-install-configure.md)

##<a id="powershell"></a>运行 MapReduce 作业使用 Azure PowerShell

Azure PowerShell 提供*cmdlet* ，您可以在 HDInsight 上远程运行 MapReduce 作业。 在内部，这是通过使用 HDInsight 群集上运行对[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (以前称为 Templeton) 的其余部分调用来实现的。

在远程 HDInsight 群集中运行 MapReduce 作业时，将使用以下 cmdlet。

* **添加 AzureAccount**: Azure 订阅验证 Azure PowerShell

* **新 AzureHDInsightMapReduceJobDefinition**︰ 使用指定的 MapReduce 信息创建新的*作业定义*

* **开始-AzureHDInsightJob**︰ 将作业定义发送到 HDInsight，启动该作业，并返回一个*作业*对象，可用于检查作业的状态

* **等待-AzureHDInsightJob**︰ 使用作业对象来检查作业的状态。 它一直等到作业完成或超时等待。

* **获得 AzureHDInsightJobOutput**︰ 用于检索作业的输出

以下步骤演示如何使用这些 cmdlet HDInsight 群集中运行作业。

1. 使用编辑器，将以下代码保存为**mapreducejob.ps1**。 您必须使用 HDInsight 群集的名称替换**群集名称**。

        #Login to your Azure subscription
        # Is there an active Azure subscription?
        $sub = Get-AzureSubscription -ErrorAction SilentlyContinue
        if(-not($sub))
        {
            Add-AzureAccount
        }

        #Specify the cluster name
        $clusterName = "CLUSTERNAME"

        #Define the MapReduce job
        #NOTE: If using an HDInsight 2.0 cluster, use hadoop-examples.jar instead.
        # -JarFile = the JAR containing the MapReduce application
        # -ClassName = the class of the application
        # -Arguments = The input file, and the output directory
        $wordCountJobDefinition = New-AzureHDInsightMapReduceJobDefinition -JarFile "wasb:///example/jars/hadoop-mapreduce-examples.jar" `
                                  -ClassName "wordcount" `
                                  -Arguments "wasb:///example/data/gutenberg/davinci.txt", "wasb:///example/data/WordCountOutput"

        #Submit the job to the cluster
        Write-Host "Start the MapReduce job..." -ForegroundColor Green
        $wordCountJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $wordCountJobDefinition

        #Wait for the job to complete
        Write-Host "Wait for the job to complete..." -ForegroundColor Green
        Wait-AzureHDInsightJob -Job $wordCountJob -WaitTimeoutInSeconds 3600

        # Print the output
        Write-Host "Display the standard output..." -ForegroundColor Green
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $wordCountJob.JobId -StandardOutput

2. 打开一个新的**Azure PowerShell**命令提示符。 将目录更改到的位置的**mapreducejob.ps1**文件，然后使用下面的命令来运行此脚本︰

        .\mapreducejob.ps1

3. 当作业完成时，您会收到与下面类似的输出︰

        Cluster         : CLUSTERNAME
        ExitCode        : 0
        Name            : wordcount
        PercentComplete : map 100% reduce 100%
        Query           :
        State           : Completed
        StatusDirectory : f1ed2028-afe8-402f-a24b-13cc17858097
        SubmissionTime  : 12/5/2014 8:34:09 PM
        JobId           : job_1415949758166_0071

    该输出指示作业已成功完成。

    > [AZURE.NOTE] 如果**退出代码**为 0 以外的值，请参阅[疑难解答](#troubleshooting)。

##<a id="results"></a>查看作业输出

MapReduce 作业存储到 Azure Blob 存储操作的结果在**wasb: / 示例/数据/WordCountOutput**作为作业参数指定的路径。 Azure Blob 存储被访问通过 Azure PowerShell，但您必须知道存储帐户名称、 密钥和 HDInsight 群集用于直接访问这些文件的容器。

幸运的是，您可以通过使用下面的 Azure PowerShell cmdlet 来获取此信息︰

* **获得 AzureHDInsightCluster**︰ 返回 HDInsight 群集，包括所有与之关联的存储帐户的信息。 将始终与群集相关的默认存储帐户。
* **新 AzureStorageContext**︰ 给定的存储帐户名称和检索使用**Get AzureHDInsightCluster**键，返回可用于访问存储帐户的上下文对象。
* **获得 AzureStorageBlob**︰ 给定的上下文对象和容器名称，返回列表中的 blob 容器内。
* **获得 AzureStorageBlobContent**︰ 给定的上下文对象，文件路径和名称、 容器名称 （从**Get AzureHDinsightCluster**），下载文件从 Azure Blob 存储。

下面的示例检索存储的信息，然后将下载的输出**wasb: / 示例/数据/WordCountOutput/**。 **群集名称**替换 HDInsight 群集的名称。

        #Login to your Azure subscription
        # Is there an active Azure subscription?
        $sub = Get-AzureSubscription -ErrorAction SilentlyContinue
        if(-not($sub))
        {
            Add-AzureAccount
        }

        #Specify the cluster name
        $clusterName = "CLUSTERNAME"

        #Retrieve the cluster information
        $clusterInfo = Get-AzureHDInsightCluster -ClusterName $clusterName

        #Get the storage account information
        $storageAccountName = $clusterInfo.DefaultStorageAccount.StorageAccountName
        $storageAccountKey = $clusterInfo.DefaultStorageAccount.StorageAccountKey
        $storageContainer = $clusterInfo.DefaultStorageAccount.StorageContainerName

        #Create the context object
        $context = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

        #Download the files from wasb:///example/data/WordCountOutput
        #Use the -blob switch to filter only blobs contained in example/data/WordCountOutput
        Get-AzureStorageBlob -Container $storageContainer -Blob example/data/WordCountOutput/* -Context $context | Get-AzureStorageBlobContent -Context $context

> [AZURE.NOTE] 本示例将运行的脚本的目录中存储的**示例/数据/WordCountOutput**文件夹下载的文件。

MapReduce 作业的输出存储在名称*一部分的 r-###*的文件中。 看到的单词和盘点作业所产生的文本编辑器中打开**示例/数据/WordCountOutput/部件-r-00000**文件。

> [AZURE.NOTE] MapReduce 作业的输出文件是不可变的。 所以如果您重新运行此示例，您需要更改输出文件的名称。

##<a id="troubleshooting"></a>疑难解答

在作业完成时，则不返回任何信息，如果在处理过程中可能时出错。 若要查看此作业的错误信息，请将以下命令添加到**mapreducejob.ps1**文件的末尾，保存它，然后再次运行。

    # Print the output of the WordCount job.
    Write-Host "Display the standard output ..." -ForegroundColor Green
    Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $wordCountJob.JobId -StandardError

此方法返回的信息写入到 STDERR 在服务器上运行该作业时，它可能会帮助您确定该作业失败的原因。

##<a id="summary"></a>摘要

如您所见，Azure PowerShell 提供了一种简便方法在 HDInsight 群集上运行 MapReduce 作业，监视作业状态，并检索输出。

##<a id="nextsteps"></a>下一步行动

在 HDInsight 中的 MapReduce 作业有关的一般信息︰

* [在 HDInsight Hadoop 使用 MapReduce](hdinsight-use-mapreduce.md)

有关其他方法的信息可以使用 Hadoop HDInsight 上︰

* [使用 Hadoop HDInsight 上配置单元](hdinsight-use-hive.md)

* [使用 Hadoop HDInsight 上的小猪](hdinsight-use-pig.md)

测试

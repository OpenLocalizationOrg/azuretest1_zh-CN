---
ms.openlocfilehash: 52ab6149e27b043622a77820733695a730769602
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="Hadoop 配置单元使用 PowerShell 中 HDInsight |Microsoft Azure"
   description="使用 PowerShell Hadoop HDInsight 上运行配置单元查询。"
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

#运行配置单元查询使用 PowerShell

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

本文档提供使用 Azure PowerShell 以运行 Hadoop HDInsight 群集上配置单元查询的示例。

> [AZURE.NOTE] 本文档不提供的示例中使用的 HiveQL 语句执行的操作的详细的说明。 在此示例中使用 HiveQL 上的信息，请参阅[使用配置单元与 Hadoop HDInsight 上](hdinsight-use-hive.md)。


##<a id="prereq"></a>先决条件

若要完成本文中的步骤操作，您需要。

- **Azure HDInsight (在 HDInsight 上的 Hadoop) 群集 （基于 Windows 或基于 Linux 的）**
- **带有 Azure PowerShell 的工作站**。 请参阅[安装和使用 Azure PowerShell](http://azure.microsoft.com/documentation/videos/install-and-use-azure-powershell/)。

##<a id="powershell"></a>运行配置单元查询使用 Azure PowerShell

Azure PowerShell 提供*cmdlet* ，您可以在 HDInsight 上远程运行配置单元查询。 在内部，这是通过使用 HDInsight 群集上运行对[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (以前称为 Templeton) 的其余部分调用来实现的。

运行在一个远程 HDInsight 群集配置单元查询时，将使用以下 cmdlet:

* **添加 AzureAccount**: Azure 订阅验证 Azure PowerShell

* **新 AzureHDInsightHiveJobDefinition**︰ 使用指定的 HiveQL 语句创建新的*作业定义*

* **开始-AzureHDInsightJob**︰ 将作业定义发送到 HDInsight，启动该作业，并返回一个*作业*对象，可用于检查作业的状态

* **等待-AzureHDInsightJob**︰ 使用作业对象来检查作业的状态。 它将一直等待直到作业完成或超时等待。

* **获得 AzureHDInsightJobOutput**︰ 用于检索作业的输出

* **调用配置单元**︰ 用于运行 HiveQL 语句。 这将阻止该查询完成，然后返回结果

* **使用 AzureHDInsightCluster**︰ 设置当前群集**配置单元调用的**命令的使用

下面的步骤演示了如何使用这些 cmdlet HDInsight 群集中运行作业︰

1. 使用编辑器，将以下代码保存为**hivejob.ps1**。 您必须使用 HDInsight 群集的名称替换**群集名称**。

        #Login to your Azure subscription
        # Is there an active Azure subscription?
        $sub = Get-AzureSubscription -ErrorAction SilentlyContinue
        if(-not($sub))
        {
            Add-AzureAccount
        }

        #Specify the cluster name
        $clusterName = "CLUSTERNAME"

        #HiveQL
        $queryString = "DROP TABLE log4jLogs;" +
                       "CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION 'wasb:///example/data/';" +
                       "SELECT t4 AS sev, COUNT(*) AS cnt FROM log4jLogs WHERE t4 = '[ERROR]' GROUP BY t4;"

        #Create an HDInsight Hive job definition
        $hiveJobDefinition = New-AzureHDInsightHiveJobDefinition -Query $queryString

        #Submit the job to the cluster
        Write-Host "Start the Hive job..." -ForegroundColor Green
        $hiveJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $hiveJobDefinition

        #Wait for the Hive job to complete
        Write-Host "Wait for the job to complete..." -ForegroundColor Green
        Wait-AzureHDInsightJob -Job $hiveJob -WaitTimeoutInSeconds 3600

        # Print the output
        Write-Host "Display the standard output..." -ForegroundColor Green
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $hiveJob.JobId -StandardOutput

2. 打开一个新的**Azure PowerShell**命令提示符。 将目录更改到的位置的**hivejob.ps1**文件，然后使用下面的命令来运行此脚本︰

        .\hivejob.ps1

7. 当作业完成时，它应该返回信息与以下类似︰

        Display the standard output...
        [ERROR] 3

4. 如上文所述，可以使用**调用配置单元**以运行查询并等待响应。 使用以下命令，并将**群集名称**替换您群集的名称︰

        Use-AzureHDInsightCluster CLUSTERNAME
        Invoke-Hive -Query @"
        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]';
        SELECT * FROM errorLogs;
        "@

    输出将如下所示︰

        2012-02-03  18:35:34    SampleClass0    [ERROR] incorrect   id
        2012-02-03  18:55:54    SampleClass1    [ERROR] incorrect   id
        2012-02-03  19:25:27    SampleClass4    [ERROR] incorrect   id

    > [AZURE.NOTE] 对于较长的 HiveQL 查询，可以使用 Azure PowerShell**这里字符串**cmdlet 的 HiveQL 脚本文件。 下面的代码段演示如何使用**调用配置单元**cmdlet 运行 HiveQL 脚本文件。 HiveQL 脚本文件必须上载到 wasb: / /。
    >
    > `Invoke-Hive -File "wasb://<ContainerName>@<StorageAccountName>/<Path>/query.hql"`
    >
    > 有关**此处字符串**的详细信息，请参阅<a href="http://technet.microsoft.com/library/ee692792.aspx" target="_blank">使用 Windows PowerShell 这里的字符串</a>。

##<a id="troubleshooting"></a>疑难解答

在作业完成时，则不返回任何信息，如果在处理过程中可能时出错。 若要查看此作业的错误信息，将以下内容添加到**hivejob.ps1**文件的末尾，保存它，然后重新运行它。

    # Print the output of the Hive job.
    Write-Host "Display the standard output ..." -ForegroundColor Green
    Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $hiveJob.JobId -StandardError

此方法返回时，写入到 STDERR 在服务器上运行此作业的信息，它可能会帮助您确定该作业失败的原因。

##<a id="summary"></a>摘要

如您所见，Azure PowerShell 轻松地运行在 HDInsight 群集配置单元查询，监视作业状态，并检索输出。

##<a id="nextsteps"></a>下一步行动

HDInsight 中的配置单元有关的一般信息︰

* [使用 Hadoop HDInsight 上配置单元](hdinsight-use-hive.md)

有关其他方法的信息可以使用 Hadoop HDInsight 上︰

* [使用 Hadoop HDInsight 上的小猪](hdinsight-use-pig.md)

* [在 HDInsight 上的 Hadoop 使用 MapReduce](hdinsight-use-mapreduce.md)

测试

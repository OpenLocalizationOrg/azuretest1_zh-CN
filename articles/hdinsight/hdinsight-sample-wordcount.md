---
ms.openlocfilehash: 7bae39282a373e3d248844bb046f05e311688007
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="Hadoop MapReduce 单词计数示例在 HDInsight |Microsoft Azure"
    description="在 HDInsight 中 Hadoop 群集上运行 MapReduce 单词计数示例。 在 Java 中，编写的程序统计文本文件中的单词匹配项。"
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
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/09/2015"
    ms.author="jgao"/>

#运行 Hadoop 群集在 HDInsight 上用 Java 编写的 MapReduce 单词计数示例

本教程展示了如何在 HDInsight 中 Hadoop 群集上运行 MapReduce 单词计数示例。 在 Java 中是编写程序。 它在文本文件中的单词发生进行计数，然后输出一个新的文本文件，其中包含每个单词配对其频率的匹配项。 分析在此示例中的文件是项目堡电子书版笔记本的列奥纳多达芬奇。

> [AZURE.NOTE] 本文档中的步骤要求 Windows 客户端。 Linux、 OS X 或 Unix 客户端中的单词计数示例使用基于 Linux 的 HDInsight 群集的步骤，请参阅[使用 MapReduce 与 Hadoop HDInsight 使用 SSH 上](hdinsight-hadoop-use-mapreduce-ssh.md)或[使用 MapReduce 与 Hadoop 使用卷曲的 HDInsight 上](hdinsight-hadoop-use-mapreduce-curl.md)。

**您将了解︰**

* 如何使用 Azure PowerShell HDInsight 群集上运行 MapReduce 程序。
* 如何编写 Java MapReduce 程序。


**系统必备组件**︰

- **Azure 订阅**。 请参阅[获取 Azure 免费试用版](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)。

- **HDInsight 群集**。 有关可以在其中创建此类簇的各种方式的说明，请参阅[开始使用 Azure HDInsight][hdinsight 获取启动]或[规定 HDInsight 群集](hdinsight-provision-clusters.md)。

- **带有 Azure PowerShell 的工作站**。 请参阅[安装和使用 Azure PowerShell](http://azure.microsoft.com/documentation/videos/install-and-use-azure-powershell/)。



## <a id="run-sample"></a>通过使用 Azure PowerShell 运行示例</h2>

**若要提交 MapReduce 作业**

1.  打开**Azure PowerShell**控制台。 有关说明，请参阅[安装和配置 Azure PowerShell][powershell 安装配置]。

3. 设置这两个变量中的以下命令，然后运行它们︰

        $subscriptionName = "<SubscriptionName>"   # Azure subscription name
        $clusterName = "<ClusterName>"             # HDInsight cluster name

5. 运行以下命令以创建 MapReduce 作业定义︰

        # Define the MapReduce job
        $wordCountJobDefinition = New-AzureHDInsightMapReduceJobDefinition -JarFile "wasb:///example/jars/hadoop-mapreduce-examples.jar" -ClassName "wordcount" -Arguments "wasb:///example/data/gutenberg/davinci.txt", "wasb:///example/data/WordCountOutput"

    > [AZURE.NOTE] *hadoop examples.jar*附带了 HDInsight 版本 2.1 群集。 文件被重命名为的*hadoop mapreduce.jar* HDInsight 版本 3.0 群集。

    Hadoop 的 mapreduce examples.jar 文件附带了 HDInsight 群集。 有两种争论 MapReduce 作业。 第一个是源代码文件的名称，第二是输出文件路径。 源代码文件附带了 HDInsight 群集中，并将在运行时创建的输出文件路径。

6. 运行以下命令，以提交 MapReduce 作业︰

        # Submit the job
        Select-AzureSubscription $subscriptionName
        $wordCountJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $wordCountJobDefinition | Wait-AzureHDInsightJob -WaitTimeoutInSeconds 3600  

    除了 MapReduce 作业定义，您还可以提供想要运行 MapReduce 作业的 HDInsight 群集名称。

8. 运行下面的命令以检查运行 MapReduce 作业的错误︰

        # Get the job output
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $wordCountJob.JobId -StandardError

**若要检索 MapReduce 作业的结果**

1. 打开**Azure PowerShell**控制台。
2. 在后面的命令设置的三个变量，然后运行它们︰

        $subscriptionName = "<SubscriptionName>"       # Azure subscription name
        $storageAccountName = "<StorageAccountName>"   # Azure storage account name
        $containerName = "<ContainerName>"             # Blob storage container name

    Azure 存储帐户是先前在本教程中创建。 使用存储帐户托管用作默认 HDInsight 群集文件系统的 blob。 Azure Blob 存储容器名称通常共享 HDInsight 群集中，相同的名称，除非您指定一个不同的名称，当您配置群集。

3. 运行以下命令以创建 Azure 存储上下文对象︰

        # Select the current subscription
        Select-AzureSubscription $subscriptionName

        # Create the storage account context object
        $storageAccountKey = Get-AzureStorageKey $storageAccountName | %{ $_.Primary }
        $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey  

    **选择 AzureSubscription**用于设置当前订阅，如果您有多个订阅，并且默认订阅不是您想要使用的一个。

4. 运行以下命令来从该 blob 的 MapReduce 作业输出下载到工作站︰

        # Download the job output to the workstation
        Get-AzureStorageBlobContent -Container $ContainerName -Blob example/data/WordCountOutput/part-r-00000 -Context $storageContext -Force

    */Example/data/WordCountOutput*文件夹是输出文件夹指定当您运行 MapReduce 作业。 *部分-r-00000*是 MapReduce 作业输出的默认文件名。 该文件将下载到本地文件夹中相同的文件夹结构。 例如，在下面的屏幕快照，当前文件夹是 C 的根文件夹。 该文件将下载到*C:\example\data\WordCountOutput*文件夹中。

5. 运行以下命令来打印 MapReduce 作业输出文件︰

        cat ./example/data/WordCountOutput/part-r-00000 | findstr "there"


    MapReduce 作业产生名为*部件-r-00000*，其中包含单词和计数的文件。 该脚本使用**findstr**命令列出所有包含*"那里"*字。

字数统计脚本的输出应显示在命令窗口中︰

![单词频率产生 PowerShell Hadoop MapReduce 单词计数示例在 HDInsight 中。][image-hdi-sample-wordcount-output]

请注意，MapReduce 作业的输出文件是不可变的。 所以如果您重新运行此示例，您需要更改输出文件的名称。

## <a id="java-code"></a>WordCount MapReduce 程序的 Java 代码</h2>



    package org.apache.hadoop.examples;
    import java.io.IOException;
    import java.util.StringTokenizer;
    import org.apache.hadoop.conf.Configuration;
    import org.apache.hadoop.fs.Path;
    import org.apache.hadoop.io.IntWritable;
    import org.apache.hadoop.io.Text;
    import org.apache.hadoop.mapreduce.Job;
    import org.apache.hadoop.mapreduce.Mapper;
    import org.apache.hadoop.mapreduce.Reducer;
    import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
    import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
    import org.apache.hadoop.util.GenericOptionsParser;

    public class WordCount {

    public static class TokenizerMapper
       extends Mapper<Object, Text, Text, IntWritable>{

    private final static IntWritable one = new IntWritable(1);
    private Text word = new Text();

    public void map(Object key, Text value, Context context
                    ) throws IOException, InterruptedException {
      StringTokenizer itr = new StringTokenizer(value.toString());
      while (itr.hasMoreTokens()) {
        word.set(itr.nextToken());
        context.write(word, one);
        }
      }
    }

    public static class IntSumReducer
       extends Reducer<Text,IntWritable,Text,IntWritable> {
    private IntWritable result = new IntWritable();

    public void reduce(Text key, Iterable<IntWritable> values,
                       Context context
                       ) throws IOException, InterruptedException {
      int sum = 0;
      for (IntWritable val : values) {
        sum += val.get();
      }
      result.set(sum);
      context.write(key, result);
      }
    }

    public static void main(String[] args) throws Exception {
    Configuration conf = new Configuration();
    String[] otherArgs = new GenericOptionsParser(conf, args).getRemainingArgs();
    if (otherArgs.length != 2) {
      System.err.println("Usage: wordcount <in> <out>");
      System.exit(2);
        }
    Job job = new Job(conf, "word count");
    job.setJarByClass(WordCount.class);
    job.setMapperClass(TokenizerMapper.class);
    job.setCombinerClass(IntSumReducer.class);
    job.setReducerClass(IntSumReducer.class);
    job.setOutputKeyClass(Text.class);
    job.setOutputValueClass(IntWritable.class);
    FileInputFormat.addInputPath(job, new Path(otherArgs[0]));
    FileOutputFormat.setOutputPath(job, new Path(otherArgs[1]));
    System.exit(job.waitForCompletion(true) ? 0 : 1);
    }
    }



在本教程中，您已经了解如何运行 MapReduce 程序使用 Azure PowerShell HDInsight 的文本文件中的单词发生进行计数。

## <a id="next-steps"></a>下一步行动</h2>

运行其他示例，并提供有关使用 Azure PowerShell Azure HDInsight 上使用猪、 配置单元和 MapReduce 作业指导的教程，请参阅︰

* [开始使用 Azure HDInsight][hdinsight--入门]
* [示例︰ 10 GB GraySort][hdinsight 示例 10 gb graysort]
* [示例︰ Pi 评估程序][hdinsight 示例-pi 评估程序]
* [示例︰ C# Steaming][hdinsight-示例-cs 流]
* [使用与 HDInsight 的小猪][使用猪的 hdinsight]
* [使用 HDInsight 蜂巢] [配置单元使用 hdinsight-]
* [Azure HDInsight SDK 文档][hdinsight sdk 文档]

[hdinsight sdk 文档]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[hdinsight 示例 10 gb graysort]: hdinsight-sample-10gb-graysort.md
[hdinsight 示例-pi 评估程序]: hdinsight-sample-pi-estimator.md
[hdinsight-示例-cs 流]: hdinsight-sample-csharp-streaming.md


[配置-单元使用 hdinsight]: hdinsight-use-hive.md
[使用猪的 hdinsight]: hdinsight-use-pig.md

[hdinsight--入门]: ../hdinsight-get-started.md

[powershell 安装配置]: ../install-configure-powershell.md

[图像的 hdi--字数统计的输出示例]: ./media/hdinsight-sample-wordcount/HDI.Sample.WordCount.Output.png

测试

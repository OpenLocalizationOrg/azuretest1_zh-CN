---
ms.openlocfilehash: 7475a007eca3dce9f329fbe057c9fde8b7e6d27e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

<properties
    pageTitle="10 GB GraySort Hadoop MapReduce 示例 |Microsoft Azure"
    description="了解如何运行 Hadoop 使用 Azure PowerShell 的 HDInsight 与常规用途非常大量的数据，通常 100 TB 最小，为 GraySort。"
    editor="cgronlun"
    manager="paulettm"
    tags="azure-portal"
    services="hdinsight"
    documentationCenter=""
    authors="mumian"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/09/2015"
    ms.author="jgao"/>

# HDInsight 中的 10 GB GraySort Hadoop MapReduce 示例

本主题的示例演示如何通过使用 Azure PowerShell Azure HDInsight 上运行通用的 GraySort Hadoop MapReduce 程序。 GraySort 是基准排序的度量标准是实现排序非常大量的数据，通常 100 TB 最低时的排序速率 （TB/分钟）。

> [AZURE.NOTE] 本文档中的步骤要求基于 Windows HDInsight 群集。 有关使用基于 Linux 的群集中运行此和其他示例的信息，请参阅[运行中 HDInsight 的 Hadoop 示例](hdinsight-hadoop-run-samples-linux.md)

此示例使用适度的 10 GB 的数据，以便它可以较快运行。 它使用 MapReduce 应用程序由 Owen O'Malley 和 Arun Murthy 率为 0.578 TB/最小值 (以分钟为单位 173 100 TB) 2009 年赢得年度通用 ("daytona") terabyte 排序准则。 这和其他排序基准的详细信息，请参阅[Sortbenchmark](http://sortbenchmark.org/)站点。

此示例使用三套 MapReduce 程序︰

1. **TeraGen**是一个 MapReduce 程序，您可以使用生成的数据进行排序的行。
2. **TeraSort**对输入的数据进行取样，并使用 MapReduce 的数据排序到总的订单。 TeraSort 是标准的 MapReduce 的功能，但使用定义每个减少为主要范围的 N-1 取样键的排序的列表的自定义分区。 特别是，所有此类的键示例 [i-1] < = 键 < 发送示例 [i] 以减少 i。 这保证的输出降低 i 均小于输出的减少 i + 1。
3. **TeraValidate**是一种 MapReduce 程序验证输出全局进行排序。 它将在输出目录中，创建一个映射每个文件和每个映射可确保每个键都小于或等于上一。 映射函数还生成记录的第一个和最后一个键的每个文件，并将归约函数可确保文件我的第一个键大于文件 i-1 最后一个键。 任何问题报告为减少输出顺序错乱的键。

输入和输出格式，由所有三个应用程序，读取和写入文本文件的格式。 输出的减少已复制设置为 1，而不是默认值 3，因为准则比赛不需要的输出数据被复制到多个节点。


**您将了解︰**
* 如何使用 Azure PowerShell Azure HDInsight 上运行一系列的 MapReduce 程序。
* 用 Java 编写的 MapReduce 程序如下所示。


**先决条件：**

- **Azure 订阅**。 请参阅[获取 Azure 免费试用版](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)。
- **HDInsight 群集**。 可以在其中创建此类簇的各种方式的说明，请参阅[设置 HDInsight 群集](hdinsight-provision-clusters.md)。
- **带有 Azure PowerShell 的工作站**。 请参阅[安装和使用 Azure PowerShell](http://azure.microsoft.com/documentation/videos/install-and-use-azure-powershell/)。



##使用 Azure PowerShell 运行示例

三个任务所需的示例中，每个对应于简介中所述的 MapReduce 程序之一︰

1. 生成运行的**TeraGen** MapReduce 作业排序的数据。
2. 通过运行**TeraSort** MapReduce 作业排序数据。
3. 确认数据已正确排序通过运行**TeraValidate** MapReduce 作业。


**若要运行 TeraGen 程序**

1. 打开 Azure PowerShell。 有关说明打开 Azure PowerShell 控制台窗口中，请参阅[安装和配置 Azure PowerShell] [powershell 安装配置]。
2. 设置这两个变量中的以下命令，然后运行它们︰

        # Provide the Azure subscription name and the HDInsight cluster name
        $subscriptionName = "myAzureSubscriptionName"
        $clusterName = "myClusterName"

4. 运行以下命令以创建 MapReduce 作业定义︰

        # Create a MapReduce job definition for the TeraGen MapReduce program
        $teragen = New-AzureHDInsightMapReduceJobDefinition -JarFile "/example/jars/hadoop-mapreduce-examples.jar" -ClassName "teragen" -Arguments "-Dmapred.map.tasks=50", "100000000", "/example/data/10GB-sort-input"

    *"-Dmapred.map.tasks=50"*参数指定将创建 50 映射来执行作业。 *100000000*参数指定要生成数据的量。 最后一个参数， */example/data/10GB-sort-input*，指定输出目录到其结果保存 （其中包含以下排序阶段的输入）。

5. 运行下列命令以提交作业，作业要完成，等待，然后打印标准错误︰

        # Run the TeraGen MapReduce job
        # Wait for the job to finish
        # Print output and standard error file of the MapReduce job
        Select-AzureSubscription $subscriptionName
        $teragen | Start-AzureHDInsightJob -Cluster $clustername | Wait-AzureHDInsightJob -WaitTimeoutInSeconds 3600 | Get-AzureHDInsightJobOutput -Cluster $clustername -StandardError


**若要运行 TeraSort 程序**

1. 打开 Azure PowerShell。
2. 设置这两个变量中的以下命令，然后运行它们︰

        # Provide the Azure subscription name and the HDInsight cluster name
        $subscriptionName = "myAzureSubscriptionName"
        $clusterName = "myClusterName"

3. 运行以下命令，以定义 MapReduce 作业︰

        # Create a MapReduce job definition for the TeraSort MapReduce program
        $terasort = New-AzureHDInsightMapReduceJobDefinition -JarFile "/example/jars/hadoop-mapreduce-examples.jar" -ClassName "terasort" -Arguments "-Dmapred.map.tasks=50", "-Dmapred.reduce.tasks=25", "/example/data/10GB-sort-input", "/example/data/10GB-sort-output"

    *"-Dmapred.map.tasks=50"*参数指定将创建 50 映射来执行作业。 *100000000*参数指定要生成数据的量。 最后两个参数指定输入和输出目录。

4. 运行以下命令，以提交作业，等待作业完成，并打印标准错误︰

        # Run the TeraSort MapReduce job
        # Wait for the job to finish
        # Print output and standard error file of the MapReduce job
        Select-AzureSubscription $subscriptionName
        $terasort | Start-AzureHDInsightJob -Cluster $clustername | Wait-AzureHDInsightJob -WaitTimeoutInSeconds 3600 | Get-AzureHDInsightJobOutput -Cluster $clustername -StandardError


**若要运行 TeraValidate 程序**

1. 打开 Azure PowerShell。
2. 设置这两个变量中的以下命令，然后运行它们︰

        # Provide the Azure subscription name and the HDInsight cluster name
        $subscriptionName = "myAzureSubscriptionName"
        $clusterName = "myClusterName"

3. 运行以下命令，以定义 MapReduce 作业︰

        #   Create a MapReduce job definition for the TeraValidate MapReduce program
        $teravalidate = New-AzureHDInsightMapReduceJobDefinition -JarFile "/example/jars/hadoop-mapreduce-examples.jar" -ClassName "teravalidate" -Arguments "-Dmapred.map.tasks=50", "-Dmapred.reduce.tasks=25", "/example/data/10GB-sort-output", "/example/data/10GB-sort-validate"

    *"-Dmapred.map.tasks=50"*参数指定将创建 50 映射来执行作业。 *"-Dmapred.reduce.tasks=25"*参数指定 25 减少任务将创建要执行的作业。 最后两个参数指定输入和输出目录。  


4. 运行下列命令以提交 MapReduce 作业、 等待作业完成后，和打印标准错误︰

        # Run the TeraSort MapReduce job
        # Wait for the job to finish
        # Print output and standard error file of the MapReduce job
        Select-AzureSubscription $subscriptionName
        $teravalidate | Start-AzureHDInsightJob -Cluster $clustername | Wait-AzureHDInsightJob -WaitTimeoutInSeconds 3600 | Get-AzureHDInsightJobOutput -Cluster $clustername -StandardError


##TeraSort MapReduce 程序的 Java 代码

在本部分中的检查显示 TeraSort MapReduce 程序的代码。


    /**
     * Licensed to the Apache Software Foundation (ASF) under one
     * or more contributor license agreements.  See the NOTICE file
     * distributed with this work for additional information
     * regarding copyright ownership.  The ASF licenses this file
     * to you under the Apache License, Version 2.0 (the
     * "License"); you may not use this file except in compliance
     * with the License.  You may obtain a copy of the License at
     *
     *     http://www.apache.org/licenses/LICENSE-2.0
     *
     * Unless required by applicable law or agreed to in writing, software
     * distributed under the License is distributed on an "AS IS" BASIS,
     * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
     * See the License for the specific language governing permissions and
     * limitations under the License.
     */

    package org.apache.hadoop.examples.terasort;

    import java.io.IOException;
    import java.io.PrintStream;
    import java.net.URI;
    import java.util.ArrayList;
    import java.util.List;

    import org.apache.commons.logging.Log;
    import org.apache.commons.logging.LogFactory;
    import org.apache.hadoop.conf.Configured;
    import org.apache.hadoop.filecache.DistributedCache;
    import org.apache.hadoop.fs.FileSystem;
    import org.apache.hadoop.fs.Path;
    import org.apache.hadoop.io.NullWritable;
    import org.apache.hadoop.io.SequenceFile;
    import org.apache.hadoop.io.Text;
    import org.apache.hadoop.mapred.FileOutputFormat;
    import org.apache.hadoop.mapred.JobClient;
    import org.apache.hadoop.mapred.JobConf;
    import org.apache.hadoop.mapred.Partitioner;
    import org.apache.hadoop.util.Tool;
    import org.apache.hadoop.util.ToolRunner;

    /**
     * Generates the sampled split points, launches the job,
     * and waits for it to finish.
     * <p>
     * To run the program:
     * <b>bin/hadoop jar hadoop-examples-*.jar terasort in-dir out-dir</b>
     */

    public class TeraSort extends Configured implements Tool {
      private static final Log LOG = LogFactory.getLog(TeraSort.class);

      /**
       * A partitioner that splits text keys into roughly equal
       * partitions in a global sorted order.
       */

      static class TotalOrderPartitioner implements Partitioner<Text,Text>{
        private TrieNode trie;
        private Text[] splitPoints;

        /**
         * A generic trie node
         */
        static abstract class TrieNode {
          private int level;
          TrieNode(int level) {
            this.level = level;
          }
          abstract int findPartition(Text key);
          abstract void print(PrintStream strm) throws IOException;
          int getLevel() {
            return level;
          }
        }

        /**
         * An inner trie node that contains 256 children based on the next
         * character.
         */
        static class InnerTrieNode extends TrieNode {
          private TrieNode[] child = new TrieNode[256];

          InnerTrieNode(int level) {
            super(level);
          }
          int findPartition(Text key) {
            int level = getLevel();
            if (key.getLength() <= level) {
              return child[0].findPartition(key);
            }
            return child[key.getBytes()[level]].findPartition(key);
          }
          void setChild(int idx, TrieNode child) {
            this.child[idx] = child;
          }
          void print(PrintStream strm) throws IOException {
            for(int ch=0; ch < 255; ++ch) {
              for(int i = 0; i < 2*getLevel(); ++i) {
                strm.print(' ');
              }
              strm.print(ch);
              strm.println(" ->");
              if (child[ch] != null) {
                child[ch].print(strm);
              }
            }
          }
        }

        /**
         * A leaf trie node that does string compares to figure out where the given
         * key belongs between lower..upper.
         */
        static class LeafTrieNode extends TrieNode {
          int lower;
          int upper;
          Text[] splitPoints;
          LeafTrieNode(int level, Text[] splitPoints, int lower, int upper) {
            super(level);
            this.splitPoints = splitPoints;
            this.lower = lower;
            this.upper = upper;
          }
          int findPartition(Text key) {
            for(int i=lower; i<upper; ++i) {
              if (splitPoints[i].compareTo(key) >= 0) {
                return i;
              }
            }
            return upper;
          }
          void print(PrintStream strm) throws IOException {
            for(int i = 0; i < 2*getLevel(); ++i) {
              strm.print(' ');
            }
            strm.print(lower);
            strm.print(", ");
            strm.println(upper);
          }
        }


        /**
         * Read the cut points from the given sequence file.
         * @param fs the file system
         * @param p the path to read
         * @param job the job config
         * @return the strings to split the partitions on
         * @throws IOException
         */
        private static Text[] readPartitions(FileSystem fs, Path p,
                                             JobConf job) throws IOException {
          SequenceFile.Reader reader = new SequenceFile.Reader(fs, p, job);
          List<Text> parts = new ArrayList<Text>();
          Text key = new Text();
          NullWritable value = NullWritable.get();
          while (reader.next(key, value)) {
            parts.add(key);
            key = new Text();
          }
          reader.close();
          return parts.toArray(new Text[parts.size()]);  
        }

        /**
         * Given a sorted set of cut points, build a trie that will find the correct
         * partition quickly.
         * @param splits the list of cut points
         * @param lower the lower bound of partitions 0..numPartitions-1
         * @param upper the upper bound of partitions 0..numPartitions-1
         * @param prefix the prefix that we have already checked against
         * @param maxDepth the maximum depth we will build a trie for
         * @return the trie node that will divide the splits correctly
         */
        private static TrieNode buildTrie(Text[] splits, int lower, int upper,
                                          Text prefix, int maxDepth) {
          int depth = prefix.getLength();
          if (depth >= maxDepth || lower == upper) {
            return new LeafTrieNode(depth, splits, lower, upper);
          }
          InnerTrieNode result = new InnerTrieNode(depth);
          Text trial = new Text(prefix);
          // append an extra byte on to the prefix
          trial.append(new byte[1], 0, 1);
          int currentBound = lower;
          for(int ch = 0; ch < 255; ++ch) {
            trial.getBytes()[depth] = (byte) (ch + 1);
            lower = currentBound;
            while (currentBound < upper) {
              if (splits[currentBound].compareTo(trial) >= 0) {
                break;
              }
              currentBound += 1;
            }
            trial.getBytes()[depth] = (byte) ch;
            result.child[ch] = buildTrie(splits, lower, currentBound, trial,
                                         maxDepth);
          }
          // pick up the rest
          trial.getBytes()[depth] = 127;
          result.child[255] = buildTrie(splits, currentBound, upper, trial,
                                        maxDepth);
          return result;
        }

        public void configure(JobConf job) {
          try {
            FileSystem fs = FileSystem.getLocal(job);
            Path partFile = new Path(TeraInputFormat.PARTITION_FILENAME);
            splitPoints = readPartitions(fs, partFile, job);
            trie = buildTrie(splitPoints, 0, splitPoints.length, new Text(), 2);
          } catch (IOException ie) {
            throw new IllegalArgumentException("can't read paritions file", ie);
          }
        }

        public TotalOrderPartitioner() {
        }

        public int getPartition(Text key, Text value, int numPartitions) {
          return trie.findPartition(key);
        }

      }

      public int run(String[] args) throws Exception {
        LOG.info("starting");
        JobConf job = (JobConf) getConf();
        Path inputDir = new Path(args[0]);
        inputDir = inputDir.makeQualified(inputDir.getFileSystem(job));
        Path partitionFile = new Path(inputDir, TeraInputFormat.PARTITION_FILENAME);
        URI partitionUri = new URI(partitionFile.toString() +
                                   "#" + TeraInputFormat.PARTITION_FILENAME);
        TeraInputFormat.setInputPaths(job, new Path(args[0]));
        FileOutputFormat.setOutputPath(job, new Path(args[1]));
        job.setJobName("TeraSort");
        job.setJarByClass(TeraSort.class);
        job.setOutputKeyClass(Text.class);
        job.setOutputValueClass(Text.class);
        job.setInputFormat(TeraInputFormat.class);
        job.setOutputFormat(TeraOutputFormat.class);
        job.setPartitionerClass(TotalOrderPartitioner.class);
        TeraInputFormat.writePartitionFile(job, partitionFile);
        DistributedCache.addCacheFile(partitionUri, job);
        DistributedCache.createSymlink(job);
        job.setInt("dfs.replication", 1);
        TeraOutputFormat.setFinalSync(job, true);
        JobClient.runJob(job);
        LOG.info("done");
        return 0;
      }

      /**
       * @param args
       */

      public static void main(String[] args) throws Exception {
        int res = ToolRunner.run(new JobConf(), new TeraSort(), args);
        System.exit(res);
      }

    }


##摘要

此示例演示了如何使用 Azure HDInsight，其中的数据输出为一个作业成为系列中的下一个作业的输入运行 MapReduce 作业一系列。

##下一步行动

教程，指导您完成运行其他示例和上使用 Azure HDInsight Azure PowerShell 与小猪、 配置单元和 MapReduce 作业提供的说明，请参阅下列主题︰

* [开始使用 Azure HDInsight][hdinsight--入门]
* [示例︰ Pi 评估程序][hdinsight 示例-pi 评估程序]
* [示例︰ 字数][字数统计样本-hdinsight-]
* [示例︰ C# 流][hdinsight-示例-csharp 流]
* [使用与 HDInsight 的小猪][使用猪的 hdinsight]
* [使用 HDInsight 蜂巢][配置单元使用 hdinsight-]
* [Azure HDInsight SDK 文档][hdinsight sdk 文档]

[hdinsight sdk 文档]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx


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

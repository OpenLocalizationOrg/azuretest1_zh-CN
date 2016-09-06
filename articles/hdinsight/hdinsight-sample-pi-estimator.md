---
ms.openlocfilehash: 60275160dc45c34e6b341fd03c7c02b9be980baf
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="Pi 估算器 Hadoop 示例在 HDInsight |Microsoft Azure"
    description="了解如何在 HDInsight 上运行 Hadoop MapReduce 示例 Pi 评估程序。"
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

# HDInsight 中的 pi 估算器 Hadoop 示例

本主题演示如何通过使用 Azure PowerShell 估计数学常数 pi 的值的 Azure HDInsight 在运行 Hadoop MapReduce 程序。 它还提供了用于估计用于检查 pi 的值 MapReduce 程序中使用的 Java 代码。

> [AZURE.NOTE] 本文档中的步骤要求基于 Windows HDInsight 群集。 有关使用基于 Linux 的群集中运行此和其他示例的信息，请参阅[运行中 HDInsight 的 Hadoop 示例](hdinsight-hadoop-run-samples-linux.md)

程序使用统计 (准蒙特卡罗) 方法来估计 pi 的值。 点随机放置在一个单元内方还范围内记录该正方形内圆，面积等于是概率圆 pi/4。 Pi 的值可以从 4R，其中 R 是在正方形内的点的总数在圆内的点数目的比率值估计。 使用点的样本更大，更好地估计是。

Pi 评估程序 Java 代码中包含映射器和变径函数是可用于检查下。 映射器程序生成指定的数量的随机放置在一个单位正方形内的点，然后计算在圆内的点的数目。 变径程序累积点由 mappers 计数并估计公式 4R，其中 R 是计在正方形内的点的总数在圆内的点数目的比率从 pi 的值。

为此示例提供的脚本提交 Hadoop jar 作业，并设置到运行值 16 图，其中每个需要计算 10 万个取样点的参数值。 可以更改这些参数值以提高 pi 的预计的值。 为了便于参考，pi 的前 10 的小数位数是 3.1415926535。

Hadoop 在 Azure 上部署应用程序所需的文件包含的.jar 文件是一个.zip 文件，并可供下载。 您可以与各种压缩实用程序将其解压缩，然后在方便的时候来浏览文件。

[运行 HDInsight 示例][hdinsight 样本]，以及指向有关如何运行它们列出了可用于帮助您在使用 HDInsight 运行 MapReduce 作业速度的其他示例。

**您将了解︰**

* 如何使用 Azure PowerShell Azure HDInsight 上运行 pi 估算器 MapReduce 程序。
* 用 Java 编写的 MapReduce 程序如下所示。

**系统必备组件**︰

- **Azure 订阅**。 请参阅[获取 Azure 免费试用版](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)。
- **HDInsight 群集**。 可以在其中创建此类簇的各种方式的说明，请参阅[设置 HDInsight 群集](hdinsight-provision-clusters.md)。
- **带有 Azure PowerShell 的工作站**。 请参阅[安装和使用 Azure PowerShell](http://azure.microsoft.com/documentation/videos/install-and-use-azure-powershell/)。



## <a id="run-sample"></a>使用 Azure PowerShell 运行示例

**若要提交 MapReduce 作业**s

1. 打开 Azure PowerShell。 有关说明如何使用 Azure PowerShell 控制台窗口中，请参阅[安装和配置 Azure PowerShell][powershell 安装配置]。
2. 设置这两个变量中的以下命令，然后运行它们︰

        $subscriptionName = "<SubscriptionName>"   # Azure subscription name
        $clusterName = "<ClusterName>"             # HDInsight cluster name

4. 运行以下命令以创建 MapReduce 定义︰

        $piEstimatorJobDefinition = New-AzureHDInsightMapReduceJobDefinition -JarFile "wasb:///example/jars/hadoop-mapreduce-examples.jar" -ClassName "pi" -Arguments "16", "10000000"


    第一个参数指示创建映射数量 （默认值为 16）。 第二个参数指示多少样本生成每个映射 （默认为 10 万美元）。 因此，该程序使用 10 * 10 万个 = 160 万随机点，以使其估计的 pi。 第三个参数指示的位置和用于在 HDInsight 3.0 和 3.1 群集上运行该示例的 jar 文件的名称。 （请参阅下面此文件的内容）。

5. 运行下列命令以提交 MapReduce 作业并等待作业完成︰

        # Run the pi estimator MapReduce job
        Select-AzureSubscription $subscriptionName
        $piJob = $piEstimatorJobDefinition | Start-AzureHDInsightJob -Cluster $clusterName

        # Wait for the job to finish  
        $piJob | Wait-AzureHDInsightJob -Subscription $subscriptionName -WaitTimeoutInSeconds 3600  

6. 运行下列命令以检索 MapReduce 作业的标准输出︰

        # Print output and standard error file of the MapReduce job
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $piJob.JobId -StandardOutput

    为了进行比较，pi 的前 10 的小数位数是 3.1415926535。


## <a id="java-code"></a>Pi 估算器 MapReduce 程序的 Java 代码



    /**
    * Licensed to the Apache Software Foundation (ASF) under one
    * or more contributor license agreements. See the NOTICE file
    * distributed with this work for additional information
    * regarding copyright ownership. The ASF licenses this file
    * to you under the Apache License, Version 2.0 (the
    * "License"); you may not use this file except in compliance
    * with the License. You may obtain a copy of the License at
    *
    * http://www.apache.org/licenses/LICENSE-2.0
    *
    * Unless required by applicable law or agreed to in writing, software
    * distributed under the License is distributed on an "AS IS" BASIS,
    * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or   implied.
    * See the License for the specific language governing permissions and
    * limitations under the License.
    */

    package org.apache.hadoop.examples;

    import java.io.IOException;
    import java.math.BigDecimal;
    import java.util.Iterator;

    import org.apache.hadoop.conf.Configured;
    import org.apache.hadoop.fs.FileSystem;
    import org.apache.hadoop.fs.Path;
    import org.apache.hadoop.io.BooleanWritable;
    import org.apache.hadoop.io.LongWritable;
    import org.apache.hadoop.io.SequenceFile;
    import org.apache.hadoop.io.Writable;
    import org.apache.hadoop.io.WritableComparable;
    import org.apache.hadoop.io.SequenceFile.CompressionType;
    import org.apache.hadoop.mapred.FileInputFormat;
    import org.apache.hadoop.mapred.FileOutputFormat;
    import org.apache.hadoop.mapred.JobClient;
    import org.apache.hadoop.mapred.JobConf;
    import org.apache.hadoop.mapred.MapReduceBase;
    import org.apache.hadoop.mapred.Mapper;
    import org.apache.hadoop.mapred.OutputCollector;
    import org.apache.hadoop.mapred.Reducer;
    import org.apache.hadoop.mapred.Reporter;
    import org.apache.hadoop.mapred.SequenceFileInputFormat;
    import org.apache.hadoop.mapred.SequenceFileOutputFormat;
    import org.apache.hadoop.util.Tool;
    import org.apache.hadoop.util.ToolRunner;


    //A Map-reduce program to estimate the value of Pi
    //using quasi-Monte Carlo method.
    //
    //Mapper:
    //Generate points in a unit square
    //and then count points inside/outside of the inscribed circle of the square.
    //
    //Reducer:
    //Accumulate points inside/outside results from the mappers.
    //Let numTotal = numInside + numOutside.
    //The fraction numInside/numTotal is a rational approximation of
    //the value (Area of the circle)/(Area of the square),
    //where the area of the inscribed circle is Pi/4
    //and the area of unit square is 1.
    //Then, Pi is estimated value to be 4(numInside/numTotal).
    //

    public class PiEstimator extends Configured implements Tool {
    //tmp directory for input/output
    static private final Path TMP_DIR = new Path(
    PiEstimator.class.getSimpleName() + "_TMP_3_141592654");

    //2-dimensional Halton sequence {H(i)},
    //where H(i) is a 2-dimensional point and i >= 1 is the index.
    //Halton sequence is used to generate sample points for Pi estimation.
    private static class HaltonSequence {
    // Bases
    static final int[] P = {2, 3};
    //Maximum number of digits allowed
    static final int[] K = {63, 40};

    private long index;
    private double[] x;
    private double[][] q;
    private int[][] d;

    //Initialize to H(startindex),
    //so the sequence begins with H(startindex+1).
    HaltonSequence(long startindex) {
    index = startindex;
    x = new double[K.length];
    q = new double[K.length][];
    d = new int[K.length][];
    for(int i = 0; i < K.length; i++) {
    q[i] = new double[K[i]];
    d[i] = new int[K[i]];
    }

    for(int i = 0; i < K.length; i++) {
    long k = index;
    x[i] = 0;

    for(int j = 0; j < K[i]; j++) {
    q[i][j] = (j == 0? 1.0: q[i][j-1])/P[i];
    d[i][j] = (int)(k % P[i]);
    k = (k - d[i][j])/P[i];
    x[i] += d[i][j] * q[i][j];
    }
    }
    }

    //Compute next point.
    //Assume the current point is H(index).
    //Compute H(index+1).
    //@return a 2-dimensional point with coordinates in [0,1)^2
    double[] nextPoint() {
    index++;
    for(int i = 0; i < K.length; i++) {
    for(int j = 0; j < K[i]; j++) {
    d[i][j]++;
    x[i] += q[i][j];
    if (d[i][j] < P[i]) {
    break;
    }
    d[i][j] = 0;
    x[i] -= (j == 0? 1.0: q[i][j-1]);
    }
    }
    return x;
    }
    }

    //Mapper class for Pi estimation.
    //Generate points in a unit square and then
    //count points inside/outside of the inscribed circle of the square.
    public static class PiMapper extends MapReduceBase
    implements Mapper<LongWritable, LongWritable, BooleanWritable, LongWritable> {

    //Map method.
    //@param offset samples starting from the (offset+1)th sample.
    //@param size the number of samples for this map
    //@param out output {ture->numInside, false->numOutside}
    //@param reporter
    public void map(LongWritable offset,
    LongWritable size,
    OutputCollector<BooleanWritable, LongWritable> out,
    Reporter reporter) throws IOException {

    final HaltonSequence haltonsequence = new HaltonSequence(offset.get());
    long numInside = 0L;
    long numOutside = 0L;

    for(long i = 0; i < size.get(); ) {
    //generate points in a unit square
    final double[] point = haltonsequence.nextPoint();

    //count points inside/outside of the inscribed circle of the square
    final double x = point[0] - 0.5;
    final double y = point[1] - 0.5;
    if (x*x + y*y > 0.25) {
    numOutside++;
    } else {
    numInside++;
    }

    //report status
    i++;
    if (i % 1000 == 0) {
    reporter.setStatus("Generated " + i + " samples.");
    }
    }

    //output map results
    out.collect(new BooleanWritable(true), new LongWritable(numInside));
    out.collect(new BooleanWritable(false), new LongWritable(numOutside));
    }
    }


    //Reducer class for Pi estimation.
    //Accumulate points inside/outside results from the mappers.
    public static class PiReducer extends MapReduceBase
    implements Reducer<BooleanWritable, LongWritable, WritableComparable<?>, Writable> {

    private long numInside = 0;
    private long numOutside = 0;
    private JobConf conf; //configuration for accessing the file system

    //Store job configuration.
    @Override
    public void configure(JobConf job) {
    conf = job;
    }


    // Accumulate number of points inside/outside results from the mappers.
    // @param isInside Is the points inside?
    // @param values An iterator to a list of point counts
    // @param output dummy, not used here.
    // @param reporter

    public void reduce(BooleanWritable isInside,
    Iterator<LongWritable> values,
    OutputCollector<WritableComparable<?>, Writable> output,
    Reporter reporter) throws IOException {
    if (isInside.get()) {
    for(; values.hasNext(); numInside += values.next().get());
    } else {
    for(; values.hasNext(); numOutside += values.next().get());
    }
    }

    //Reduce task done, write output to a file.
    @Override
    public void close() throws IOException {
    //write output to a file
    Path outDir = new Path(TMP_DIR, "out");
    Path outFile = new Path(outDir, "reduce-out");
    FileSystem fileSys = FileSystem.get(conf);
    SequenceFile.Writer writer = SequenceFile.createWriter(fileSys, conf,
    outFile, LongWritable.class, LongWritable.class,
    CompressionType.NONE);
    writer.append(new LongWritable(numInside), new LongWritable(numOutside));
    writer.close();
    }
    }

    //Run a map/reduce job for estimating Pi.
    //@return the estimated value of Pi.
    public static BigDecimal estimate(int numMaps, long numPoints, JobConf jobConf
    )
    throws IOException {
    //setup job conf
    jobConf.setJobName(PiEstimator.class.getSimpleName());

    jobConf.setInputFormat(SequenceFileInputFormat.class);

    jobConf.setOutputKeyClass(BooleanWritable.class);
    jobConf.setOutputValueClass(LongWritable.class);
    jobConf.setOutputFormat(SequenceFileOutputFormat.class);

    jobConf.setMapperClass(PiMapper.class);
    jobConf.setNumMapTasks(numMaps);

    jobConf.setReducerClass(PiReducer.class);
    jobConf.setNumReduceTasks(1);

    // turn off speculative execution, because DFS doesn't handle
    // multiple writers to the same file.
    jobConf.setSpeculativeExecution(false);

    //setup input/output directories
    final Path inDir = new Path(TMP_DIR, "in");
    final Path outDir = new Path(TMP_DIR, "out");
    FileInputFormat.setInputPaths(jobConf, inDir);
    FileOutputFormat.setOutputPath(jobConf, outDir);

    final FileSystem fs = FileSystem.get(jobConf);
    if (fs.exists(TMP_DIR)) {
     throw new IOException("Tmp directory " + fs.makeQualified(TMP_DIR)
     + " already exists. Please remove it first.");
     }
     if (!fs.mkdirs(inDir)) {
     throw new IOException("Cannot create input directory " + inDir);
     }

     //generate an input file for each map task
     try {
     for(int i=0; i < numMaps; ++i) {
     final Path file = new Path(inDir, "part"+i);
     final LongWritable offset = new LongWritable(i * numPoints);
     final LongWritable size = new LongWritable(numPoints);
     final SequenceFile.Writer writer = SequenceFile.createWriter(
     fs, jobConf, file,
     LongWritable.class, LongWritable.class, CompressionType.NONE);
     try {
     writer.append(offset, size);
     } finally {
     writer.close();
     }
     System.out.println("Wrote input for Map #"+i);
     }

     //start a map/reduce job
     System.out.println("Starting Job");
     final long startTime = System.currentTimeMillis();
     JobClient.runJob(jobConf);
     final double duration = (System.currentTimeMillis() - startTime)/1000.0;
     System.out.println("Job Finished in " + duration + " seconds");

     //read outputs
     Path inFile = new Path(outDir, "reduce-out");
     LongWritable numInside = new LongWritable();
     LongWritable numOutside = new LongWritable();
     SequenceFile.Reader reader = new SequenceFile.Reader(fs, inFile, jobConf);
     try {
     reader.next(numInside, numOutside);
     } finally {
     reader.close();
     }

     //compute estimated value
     return BigDecimal.valueOf(4).setScale(20)
     .multiply(BigDecimal.valueOf(numInside.get()))
     .divide(BigDecimal.valueOf(numMaps))
     .divide(BigDecimal.valueOf(numPoints));
     } finally {
     fs.delete(TMP_DIR, true);
     }
     }

    //Parse arguments and then runs a map/reduce job.
    //Print output in standard out.
    //@return a non-zero if there is an error. Otherwise, return 0.
     public int run(String[] args) throws Exception {
     if (args.length != 2) {
     System.err.println("Usage: "+getClass().getName()+" <nMaps> <nSamples>");
     ToolRunner.printGenericCommandUsage(System.err);
     return -1;
     }

     final int nMaps = Integer.parseInt(args[0]);
     final long nSamples = Long.parseLong(args[1]);

     System.out.println("Number of Maps = " + nMaps);
     System.out.println("Samples per Map = " + nSamples);

     final JobConf jobConf = new JobConf(getConf(), getClass());
     System.out.println("Estimated value of Pi is "
     + estimate(nMaps, nSamples, jobConf));
     return 0;
     }

     //main method for running it as a stand alone command.
     public static void main(String[] argv) throws Exception {
     System.exit(ToolRunner.run(null, new PiEstimator(), argv));
     }
     }

## <a id="summary"></a>摘要

在本教程中，您看到了如何在 HDInsight 上运行 MapReduce 作业以及如何使用 Monte Carlo 方法的要求，并生成可通过此服务的大型数据集。

下面是使用默认的 HDInsight 3.1 群集或 3.0 的群集上运行此示例的完整脚本︰

    ### Provide the Azure subscription name and the HDInsight cluster name
    $subscriptionName = "<SubscriptionName>"
    $clusterName = "<ClusterName>"  

    ###Select the Azure subscription to use
    Select-AzureSubscription $subscriptionName

    ### Create a MapReduce job definition
    $piEstimatorJobDefinition = New-AzureHDInsightMapReduceJobDefinition -ClassName "pi" –Arguments “32”, “1000000000” -JarFile "wasb:///example/jars/hadoop-mapreduce-examples.jar"

    ### Run the MapReduce job
    $piJob = $piEstimatorJobDefinition | Start-AzureHDInsightJob -Cluster $clusterName

    ### Wait for the job to finish  
    $piJob | Wait-AzureHDInsightJob -WaitTimeoutInSeconds 3600

    ### Print the standard error file of the MapReduce job
    Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $piJob.JobId -StandardOutput



## <a id="next-steps"></a>下一步行动

描述运行其他示例，并介绍了对使用 Azure HDInsight Azure PowerShell 与猪、 配置单元和 MapReduce 作业上的教程，请参阅下列主题︰

* [开始使用 Azure HDInsight][hdinsight--入门]
* [示例︰ 10 GB GraySort][hdinsight 示例 10 gb graysort]
* [示例︰ 字数统计][字数统计样本-hdinsight-]
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

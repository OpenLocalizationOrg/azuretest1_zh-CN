---
ms.openlocfilehash: b3b20d195bc6e1d84e68f130e4e23a5a61328239
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="Apache 风暴为开发基于 Java 的拓扑结构 |Microsoft Azure"
   description="了解如何通过创建一个简单的单词计数拓扑结构在 Java 中创建风暴拓扑。"
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="paulettm"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="07/08/2015"
   ms.author="larryfr"/>

#开发基于 Java 的基本的字数统计应用程序与 Apache 风暴和 HDInsight 在 Maven 的拓扑结构

了解使用 Maven Apache 风暴在 HDInsight 上创建一个基于 Java 的拓扑的基本过程。 您将演练创建一个基本的字数统计应用程序，使用 Maven 和 Java 的过程。 尽管使用 Eclipse 提供了说明，您还可以使用您选择的文本编辑器。

完成本文档中的步骤之后，将拥有基本的拓扑结构，您可以将它们部署到 HDInsight 上的 Apache 风暴。

##先决条件

* <a href="https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html" target="_blank">Java 开发人员工具箱 (JDK) 7 版</a>

* <a href="https://maven.apache.org/download.cgi" target="_blank">Maven</a>: Maven 是项目生成系统用于 Java 项目。

* 文本编辑器 （如记事本）， <a href="http://www.gnu.org/software/emacs/" target="_blank">Emacs<a>，<a href="http://www.sublimetext.com/" target="_blank">大文本</a>， <a href="https://atom.io/" target="_blank">Atom.io</a>， <a href="http://brackets.io/" target="_blank">Brackets.io</a>。 或者您可以使用集成的开发环境 (IDE) 如<a href="https://eclipse.org/" target="_blank">Eclipse</a> (Luna 版本或更高版本)。

    > [AZURE.NOTE] 您的编辑器或 IDE 可能必须使用 Maven 不解决本文档中的特定功能。 编辑环境中的功能的信息，请参阅您正在使用的产品的文档。

##配置环境变量

Java 和 JDK 安装时，可以设置以下环境变量。 但是，您应该检查它们存在，它们包含您系统的正确值。

* **JAVA_HOME** -应指向 Java 运行时环境 (JRE) 的安装目录。 例如，在 Unix 或 Linux 分发，它应具有值类似于`/usr/lib/jvm/java-7-oracle`。 在 Windows 中，它将具有类似于一个值 `c:\Program Files (x86)\Java\jre1.7`

* **路径**-应包含以下路径︰

    * **JAVA_HOME**（或等效的路径）

    * **JAVA_HOME\bin**（或等效的路径）

    * Maven 时的安装目录

##创建一个新的 Maven 项目

从命令行中，使用以下代码创建一个名为**字数统计**的新 Maven 项目︰

    mvn archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart -DgroupId=com.microsoft.example -DartifactId=WordCount -DinteractiveMode=false

这将创建一个名为在当前的位置，其中包含基本的 Maven 项目**字数统计**的新目录。

**字数统计**目录将包含以下各项︰

* **pom.xml**︰ 包含 Maven 项目设置。

* **src\main\java\com\microsoft\example**︰ 包含应用程序代码。

* **src\test\java\com\microsoft\example**︰ 包含应用程序的测试。 对于本示例，我们将不会创建测试。

###删除示例代码

因为我们要创建我们的应用程序，删除生成的测试和应用程序文件︰

*  **src\test\java\com\microsoft\example\AppTest.java**

*  **src\main\java\com\microsoft\example\App.java**

##添加依赖项

因为这是一个风暴拓扑结构，您必须添加风暴组件的依赖项。 打开**pom.xml**文件并添加下面的代码在**&lt;依赖项 >**部分中︰

    <dependency>
      <groupId>org.apache.storm</groupId>
      <artifactId>storm-core</artifactId>
      <version>0.9.2-incubating</version>
      <!-- keep storm out of the jar-with-dependencies -->
      <scope>provided</scope>
    </dependency>

在编译时，Maven 将使用此信息来查找 Maven 存储库中的**风暴核心**。 它首先查找本地计算机上存储库中。 如果文件不存在，它将从公用 Maven 存储库下载它们并将它们存储在本地存储库中。

> [AZURE.NOTE] 请注意`<scope>provided</scope>`的部分中，我们添加的行。 这会告诉 Maven，因为系统将提供从我们创建，任何 JAR 文件中排除**风暴核心**。 这使包创建要稍小一些，并且保证，他们将使用包括在 HDInsight 群集上风暴的**风暴核心**位。

##生成配置

Maven 插件允许您自定义的生成阶段的项目，如如何编译项目，或如何将其打包到一个 JAR 文件。 打开**pom.xml**文件并添加下面的代码直接上面`</project>`线。

    <build>
      <plugins>
      </plugins>
    </build>

本部分将用于添加插件和其他生成配置选项。

###添加插件

对于风暴拓扑<a href="http://mojo.codehaus.org/exec-maven-plugin/" target="_blank">执行 Maven 插件</a>非常有用，因为它允许您轻松地在您的开发环境中本地运行拓扑结构。 添加到以下`<plugins>`包括执行 Maven 插件**pom.xml**文件部分︰

    <plugin>
      <groupId>org.codehaus.mojo</groupId>
      <artifactId>exec-maven-plugin</artifactId>
      <executions>
        <execution>
        <goals>
          <goal>exec</goal>
        </goals>
        </execution>
      </executions>
      <configuration>
        <executable>java</executable>
        <includeProjectDependencies>true</includeProjectDependencies>
        <includePluginDependencies>false</includePluginDependencies>
        <classpathScope>compile</classpathScope>
        <mainClass>${storm.topology}</mainClass>
      </configuration>
    </plugin>

另一个有用的插件是<a href="http://maven.apache.org/plugins/maven-compiler-plugin/" target="_blank">Apache Maven 编译器插件</a>，该对话框用于更改编译选项。 我们需要这样的主要原因是要更改的源和目标应用程序使用 Maven 的 Java 版本。 我们希望 1.7 版。

添加在以下`<plugins>` **pom.xml**文件包括 Apache Maven 编译器插件并将源和目标版本设置为 1.7 节。

    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-compiler-plugin</artifactId>
      <configuration>
        <source>1.7</source>
        <target>1.7</target>
      </configuration>
    </plugin>

##创建拓扑

一个基于 Java 的风暴拓扑组成必须创作的三个组件 （或引用） 为依赖项。

* **Spouts**︰ 从外部数据源并发出到拓扑结构中的数据流中读取。

* **螺栓**︰ 执行处理流发射的 spouts 或其他细节，并发出一个或多个数据流。

* **拓扑结构**︰ 定义如何 spouts 和螺栓排列，并为拓扑提供入口点。

###创建作为管口

为了减少设置外部数据源的要求，下面的管口只是发出随机的句子。 它是提供与管口的修改的版本 (<a href="https://github.com/apache/storm/blob/master/examples/storm-starter/" target="_blank">风暴初学者示例</a>)。

> [AZURE.NOTE] 从外部数据源中读取管口的示例，请参见以下示例之一︰
>
> * <a href="https://github.com/apache/storm/blob/master/examples/storm-starter/src/jvm/storm/starter/spout/TwitterSampleSpout.java" target="_blank">TwitterSampleSpout</a>︰ 从 Twitter 中读取示例作为管口
>
> * <a href="https://github.com/apache/storm/tree/master/external/storm-kafka" target="_blank">冲击 Kafka</a>︰ 读取 Kafka 管口

管口，创建新的**src\main\java\com\microsoft\example**目录中名为**RandomSentenceSpout.java**的文件并使用以下内容︰

    /**
     * Licensed to the Apache Software Foundation (ASF) under one
     * or more contributor license agreements.  See the NOTICE file
     * distributed with this work for additional information
     * regarding copyright ownership.  The ASF licenses this file
     * to you under the Apache License, Version 2.0 (the
     * "License"); you may not use this file except in compliance
     * with the License.  You may obtain a copy of the License at
     *
     * http://www.apache.org/licenses/LICENSE-2.0
     *
     * Unless required by applicable law or agreed to in writing, software
     * distributed under the License is distributed on an "AS IS" BASIS,
     * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
     * See the License for the specific language governing permissions and
     * limitations under the License.
     */

     /**
      * Original is available at https://github.com/apache/storm/blob/master/examples/storm-starter/src/jvm/storm/starter/spout/RandomSentenceSpout.java
      */

    package com.microsoft.example;

    import backtype.storm.spout.SpoutOutputCollector;
    import backtype.storm.task.TopologyContext;
    import backtype.storm.topology.OutputFieldsDeclarer;
    import backtype.storm.topology.base.BaseRichSpout;
    import backtype.storm.tuple.Fields;
    import backtype.storm.tuple.Values;
    import backtype.storm.utils.Utils;

    import java.util.Map;
    import java.util.Random;

    //This spout randomly emits sentences
    public class RandomSentenceSpout extends BaseRichSpout {
      //Collector used to emit output
      SpoutOutputCollector _collector;
      //Used to generate a random number
      Random _rand;

      //Open is called when an instance of the class is created
      @Override
      public void open(Map conf, TopologyContext context, SpoutOutputCollector collector) {
      //Set the instance collector to the one passed in
        _collector = collector;
        //For randomness
        _rand = new Random();
      }

      //Emit data to the stream
      @Override
      public void nextTuple() {
      //Sleep for a bit
        Utils.sleep(100);
        //The sentences that will be randomly emitted
        String[] sentences = new String[]{ "the cow jumped over the moon", "an apple a day keeps the doctor away",
            "four score and seven years ago", "snow white and the seven dwarfs", "i am at two with nature" };
        //Randomly pick a sentence
        String sentence = sentences[_rand.nextInt(sentences.length)];
        //Emit the sentence
        _collector.emit(new Values(sentence));
      }

      //Ack is not implemented since this is a basic example
      @Override
      public void ack(Object id) {
      }

      //Fail is not implemented since this is a basic example
      @Override
      public void fail(Object id) {
      }

      //Declare the output fields. In this case, an sentence
      @Override
      public void declareOutputFields(OutputFieldsDeclarer declarer) {
        declarer.declare(new Fields("sentence"));
      }
    }

花点时间通读代码注释，以了解此管口的工作原理。

> [AZURE.NOTE] 此拓扑结构使用只有一个管口，虽然其他人可能有几种不同来源的数据送到拓扑结构中。

###创建螺栓

螺栓处理数据处理。 对于此拓扑中，我们有两个细节︰

* **SplitSentence**︰ 拆分成各个单词**RandomSentenceSpout**发出的句子。

* **字数统计**︰ 对每个单词出现的次数进行计数。

> [AZURE.NOTE] 螺栓实际上什么都可以做，例如，计算、 持久性，或与外部组件。

创建两个新的文件， **SplitSentence.java**和**WordCount.Java**在**src\main\java\com\microsoft\example**目录中。 使用以下内容作为内容的文件︰

**SplitSentence**

    package com.microsoft.example;

    import java.text.BreakIterator;

    import backtype.storm.topology.BasicOutputCollector;
    import backtype.storm.topology.OutputFieldsDeclarer;
    import backtype.storm.topology.base.BaseBasicBolt;
    import backtype.storm.tuple.Fields;
    import backtype.storm.tuple.Tuple;
    import backtype.storm.tuple.Values;

    //There are a variety of bolt types. In this case, we use BaseBasicBolt
    public class SplitSentence extends BaseBasicBolt {

      //Execute is called to process tuples
      @Override
      public void execute(Tuple tuple, BasicOutputCollector collector) {
        //Get the sentence content from the tuple
        String sentence = tuple.getString(0);
        //An iterator to get each word
        BreakIterator boundary=BreakIterator.getWordInstance();
        //Give the iterator the sentence
        boundary.setText(sentence);
        //Find the beginning first word
        int start=boundary.first();
        //Iterate over each word and emit it to the output stream
        for (int end=boundary.next(); end != BreakIterator.DONE; start=end, end=boundary.next()) {
          //get the word
          String word=sentence.substring(start,end);
          //If a word is whitespace characters, replace it with empty
          word=word.replaceAll("\\s+","");
          //if it's an actual word, emit it
          if (!word.equals("")) {
            collector.emit(new Values(word));
          }
        }
      }

      //Declare that emitted tuples will contain a word field
      @Override
      public void declareOutputFields(OutputFieldsDeclarer declarer) {
        declarer.declare(new Fields("word"));
      }
    }

**字数统计**

    package com.microsoft.example;

    import java.util.HashMap;
    import java.util.Map;

    import backtype.storm.topology.BasicOutputCollector;
    import backtype.storm.topology.OutputFieldsDeclarer;
    import backtype.storm.topology.base.BaseBasicBolt;
    import backtype.storm.tuple.Fields;
    import backtype.storm.tuple.Tuple;
    import backtype.storm.tuple.Values;

    //There are a variety of bolt types. In this case, we use BaseBasicBolt
    public class WordCount extends BaseBasicBolt {
      //For holding words and counts
        Map<String, Integer> counts = new HashMap<String, Integer>();

        //execute is called to process tuples
        @Override
        public void execute(Tuple tuple, BasicOutputCollector collector) {
          //Get the word contents from the tuple
          String word = tuple.getString(0);
          //Have we counted any already?
          Integer count = counts.get(word);
          if (count == null)
            count = 0;
          //Increment the count and store it
          count++;
          counts.put(word, count);
          //Emit the word and the current count
          collector.emit(new Values(word, count));
        }

        //Declare that we will emit a tuple containing two fields; word and count
        @Override
        public void declareOutputFields(OutputFieldsDeclarer declarer) {
          declarer.declare(new Fields("word", "count"));
        }
      }

花点时间通读代码注释，以了解每个螺栓的工作原理。

###创建拓扑

拓扑结构将 spouts 和螺栓在一起成一个关系图，它定义数据的组件之间的流动方式。 它还提供了创建群集中的组件的实例时，将使用风暴的并行性提示。

下面是此拓扑中的组件图的基本框图。

![显示的 spouts 和螺栓排列图表](./media/hdinsight-storm-develop-java-topology/wordcount-topology.png)

若要实现拓扑结构，创建新的**src\main\java\com\microsoft\example**目录中名为**WordCountTopology.java**的文件。 使用以下内容作为内容的文件︰

    package com.microsoft.example;

    import backtype.storm.Config;
    import backtype.storm.LocalCluster;
    import backtype.storm.StormSubmitter;
    import backtype.storm.topology.TopologyBuilder;
    import backtype.storm.tuple.Fields;

    import com.microsoft.example.RandomSentenceSpout;

    public class WordCountTopology {

      //Entry point for the topology
      public static void main(String[] args) throws Exception {
      //Used to build the topology
        TopologyBuilder builder = new TopologyBuilder();
        //Add the spout, with a name of 'spout'
        //and parallelism hint of 5 executors
        builder.setSpout("spout", new RandomSentenceSpout(), 5);
        //Add the SplitSentence bolt, with a name of 'split'
        //and parallelism hint of 8 executors
        //shufflegrouping subscribes to the spout, and equally distributes
        //tuples (sentences) across instances of the SplitSentence bolt
        builder.setBolt("split", new SplitSentence(), 8).shuffleGrouping("spout");
        //Add the counter, with a name of 'count'
        //and parallelism hint of 12 executors
        //fieldsgrouping subscribes to the split bolt, and
        //ensures that the same word is sent to the same instance (group by field 'word')
        builder.setBolt("count", new WordCount(), 12).fieldsGrouping("split", new Fields("word"));

        //new configuration
        Config conf = new Config();
        conf.setDebug(true);

        //If there are arguments, we are running on a cluster
        if (args != null && args.length > 0) {
          //parallelism hint to set the number of workers
          conf.setNumWorkers(3);
          //submit the topology
          StormSubmitter.submitTopology(args[0], conf, builder.createTopology());
        }
        //Otherwise, we are running locally
        else {
          //Cap the maximum number of executors that can be spawned
          //for a component to 3
          conf.setMaxTaskParallelism(3);
          //LocalCluster is used to run locally
          LocalCluster cluster = new LocalCluster();
          //submit the topology
          cluster.submitTopology("word-count", conf, builder.createTopology());
          //sleep
          Thread.sleep(10000);
          //shut down the cluster
          cluster.shutdown();
        }
      }
    }

花点时间通读代码注释，以了解如何定义并将提交给群集拓扑结构。

##测试本地拓扑

保存文件后，使用下面的命令来测试本地的拓扑。

    mvn compile exec:java -Dstorm.topology=com.microsoft.example.WordCountTopology

当它运行时，拓扑结构将显示启动信息。 然后它开始发出从管口和处理由螺栓的句子时显示与以下内容类似的行。

    15398 [Thread-16-split] INFO  backtype.storm.daemon.executor - Processing received message source: spout:10, stream: default, id: {}, [an apple a day keeps thedoctor away]]
    15398 [Thread-16-split] INFO  backtype.storm.daemon.task - Emitting: split default [an]
    15399 [Thread-10-count] INFO  backtype.storm.daemon.executor - Processing received message source: split:6, stream: default, id: {}, [an]
    15399 [Thread-16-split] INFO  backtype.storm.daemon.task - Emitting: split default [apple]
    15400 [Thread-8-count] INFO  backtype.storm.daemon.executor - Processing received message source: split:6, stream: default, id: {}, [apple]
    15400 [Thread-16-split] INFO  backtype.storm.daemon.task - Emitting: split default [a]
    15399 [Thread-10-count] INFO  backtype.storm.daemon.task - Emitting: count default [an, 53]
    15400 [Thread-12-count] INFO  backtype.storm.daemon.executor - Processing received message source: split:6, stream: default, id: {}, [a]
    15400 [Thread-16-split] INFO  backtype.storm.daemon.task - Emitting: split default [day]
    15400 [Thread-8-count] INFO  backtype.storm.daemon.task - Emitting: count default [apple, 53]
    15401 [Thread-10-count] INFO  backtype.storm.daemon.executor - Processing received message source: split:6, stream: default, id: {}, [day]
    15401 [Thread-16-split] INFO  backtype.storm.daemon.task - Emitting: split default [keeps]
    15401 [Thread-12-count] INFO  backtype.storm.daemon.task - Emitting: count default [a, 53]

从这个输出中，可以看到，出现了下列︰

1. 管口发出"一个苹果每天保持医生离开。"

2. 拆分螺栓开始发出句子中的个别单词。

3. 计数螺栓开始发出已发出的每个单词和它多少次。

通过查看计数螺栓所发出的数据，我们可以看到 '苹果' 已被发出的 53 倍。 计数将继续上升，只要拓扑运行因为随机一再发出相同的句子，并且永远不会重置计数。

##Trident

Trident 是由电流冲击的高级抽象。 它支持有状态的处理。 Trident 的主要优点是，它可以保证进入拓扑每封邮件进行一次性处理。 这是难以实现的保证的消息将进行至少一次处理一个原始 Java 拓扑结构中。 也有其他差异，例如，可以使用而不是创建螺栓的内置组件。 事实上，螺栓将完全替换较通用组件，例如筛选、 投影和函数。

三叉戟的应用程序可以通过使用 Maven 项目创建。 如本文前面部分介绍使用相同的基本步骤 — — 只需要代码不同。

关于 Trident 的详细信息，请参阅<a href="http://storm.apache.org/documentation/Trident-API-Overview.html" target="_blank">Trident API 概述</a>。

Trident 应用程序的示例，请参阅[使用 Twitter 趋势主题，与在 HDInsight 上的 Apache 风暴](hdinsight-storm-twitter-trending.md)。

##下一步行动

您学习了如何使用 Java 创建风暴拓扑。 现在学习如何︰

* [部署和管理在 HDInsight 上的 Apache 风暴拓扑](hdinsight-storm-deploy-monitor-topology.md)

* [在使用 Visual Studio 的 HDInsight 上的 Apache 风暴为开发 C# 拓扑](hdinsight-storm-develop-csharp-visual-studio-topology.md)

通过访问[示例拓扑上 HDInsight 的风暴](hdinsight-storm-example-topology.md)中，找到更多示例风暴拓扑。

测试

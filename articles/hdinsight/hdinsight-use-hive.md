---
ms.openlocfilehash: 0a28c548eb82a4045a5ca1aef844e10c254a992e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="了解什么是配置单元，以及如何使用 HiveQL |Microsoft Azure"
    description="了解有关 Apache 配置单元以及如何使用 Hadoop HDInsight 中。 选择如何运行您配置单元的作业，并使用 HiveQL 来分析一个 Apache log4j 文件示例。"
    keywords="hiveql，什么是配置单元"
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
    ms.date="08/28/2015"
    ms.author="larryfr"/>

# 用于配置单元和 HiveQL 与 HDInsight 在 Hadoop 分析 Apache log4j 文件示例

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]


在本教程中，将了解如何使用上 HDInsight，Apache 配置单元中 Hadoop 并选择运行配置单元工作的方式。 您还将学习有关 HiveQL 和如何分析一个 Apache log4j 文件示例。

##<a id="why"></a>什么是配置单元和为什么要使用它吗？
[Apache 配置单元](http://hive.apache.org/)是数据仓库系统的 Hadoop，通过使用 HiveQL （一种查询语言类似于 SQL） 实现数据汇总、 查询和数据分析。 以交互方式浏览您的数据，或创建可重用的批次处理作业，可配置单元。

配置单元，可以在很大程度上非结构化数据的项目结构。 定义的结构后，您可以使用配置单元查询 Java 或 MapReduce 不知情的情况下，数据。 **HiveQL**（配置单元查询语言） 允许您编写查询时将是类似于 T SQL 语句。

配置单元就能理解如何使用结构化和半结构化数据，例如位置字段由特定字符分隔的文本文件。 配置单元也支持复杂或不规则结构化数据的自定义**序列化/反序列化程序 (SerDe)** 。 有关详细信息，请参阅[如何使用自定义的 JSON SerDe 与 HDInsight](http://blogs.msdn.com/b/bigdatasupport/archive/2014/06/18/how-to-use-a-custom-json-serde-with-microsoft-azure-hdinsight.aspx)。

配置单元也可通过**用户定义函数 (UDF)**进行扩展。 UDF 可以在 HiveQL 中实现功能或逻辑，不很容易建立模型。 配置单元中使用 Udf 的示例，请参见以下资源︰

* [配置单元和猪的 HDInsight 中使用 Python](hdinsight-python.md)

* [使用 C# 配置单元和 HDInsight 中的小猪](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

* [如何将自定义的配置单元 UDF 添加到 HDInsight](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/14/how-to-add-custom-hive-udfs-to-hdinsight.aspx)

##<a id="data"></a>有关示例数据，Apache log4j 文件

此示例使用*log4j*示例文件，它们存储在**/example/data/sample.log**到 blob 存储容器中。 包含行字段的每个日志文件中的包含`[LOG LEVEL]`字段，以显示的类型和严重程度，例如︰

    2012-02-03 20:26:41 SampleClass3 [ERROR] verbose detail for id 1527353937

在前面的示例中，日志级别是错误。

> [AZURE.NOTE] 可以使用[Apache Log4j](http://en.wikipedia.org/wiki/Log4j)日志记录工具还生成 log4j 文件，然后将该文件上载到 blob 容器。 说明，请参阅[将数据上载到 HDInsight](hdinsight-upload-data.md) 。 有关如何与 HDInsight 一起使用 Azure Blob 存储的详细信息，请参阅[使用 Azure Blob 存储与 HDInsight](../hdinsight-use-blob-storage.md)。

示例数据存储在 Azure Blob 存储，HDInsight 将用作默认的文件系统中。 HDInsight 可以访问存储 blob 中使用的**wasb**前缀的文件。 例如，若要访问的 sample.log 文件，将使用以下语法︰

    wasb:///example/data/sample.log

因为 Azure Blob 存储是 HDInsight 的默认存储区，还可以通过从 HiveQL **/example/data/sample.log**来访问该文件。

> [AZURE.NOTE] 该语法， **wasb: / /**，用来访问 HDInsight 群集的默认存储容器中存储的文件。 如果您指定附加的存储帐户设置您的群集，并且想要访问这些帐户中存储的文件时，您可以通过指定容器名称和存储帐户的地址，例如， **wasb://mycontainer@mystorage.blob.core.windows.net/example/data/sample.log**访问的数据。

##<a id="job"></a>示例作业︰ 列投影到带分隔符的数据

下面的 HiveQL 语句将列投影带分隔符的数据存储在**wasb: / / 数据示例**目录︰

    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION 'wasb:///example/data/';
    SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

在前面的示例中，HiveQL 语句执行以下操作︰

* **除去表**︰ 删除的表和数据文件，如果表已存在。
* **创建外部表**︰ 在配置单元中创建一个新的**外部**表。 外部表仅将表定义存储在配置单元;将数据留在原来的位置，以原始格式。
* **行格式**︰ 告诉配置单元设置数据的格式。 在这种情况下，由空格分隔每个日志中的字段。
* **存储作为文本文件位置**︰ 告诉配置单元数据的存储 （示例中的数据目录），它将存储为文本。 数据可以在一个文件中或跨多个目录中的文件。
* **选择**︰ 选择其中列**t4**包含**[错误]**的值的所有行的计数。 这应返回**3** ，因为有三行包含此值。
* **像 '%.log' INPUT__FILE__NAME** -告诉配置单元，我们只应从以结尾的文件返回的数据。 日志。 这将搜索限制到 sample.log 文件中包含的数据，并防止它与我们所定义的架构不匹配的数据文件从其他示例返回数据。

> [AZURE.NOTE] 期望通过外部源，例如自动化的数据上载过程中，或通过 MapReduce 的另一个操作，更新基础数据并始终希望使用最新的数据的配置单元查询时，应使用外部表。
>
> 删除外部表 does**不**删除该数据，它只删除表定义。

创建外部表后, 以下语句用于创建的**内部**表。

    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs
    SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

这些语句执行以下操作︰

* **创建表如果不存在**︰ 创建一个表，如果尚不存在。 由于不使用**外部**关键字，这是内部的表，它存储在配置单元数据仓库和完全由配置单元。
* **存储 AS ORC**︰ 优化行纵栏式 (ORC) 格式存储的数据。 这是用于存储配置单元数据的高度优化和有效格式。
* **插入覆盖...选择**︰ 从包含**[错误]**，和**errorLogs**表中插入数据的**log4jLogs**表中选择行。

> [AZURE.NOTE] 与外部表，删除内部表还会删除基础数据。

##<a id="usetez"></a>Apache Tez 用于提高性能

[Apache Tez](http://tez.apache.org)是一个框架，允许数据密集型应用程序，如配置单元，按比例更高效地运行。 在最新的 HDInsight 版本，配置单元支持运行在 Tez 上。

目前，Tez 对于基于 Windows 的 HDInsight 群集默认情况下处于关闭状态，必须启用它。 利用 Tez，必须配置单元查询设置以下值︰

    set hive.execution.engine=tez;

通过将它放在您的查询的开头，可以在每个查询的基础上提交这。 您还可以设置此选项以通过设置配置值，当您创建群集时，默认情况下，在群集上守。 [资源调配 HDInsight 群集](hdinsight-provision-clusters.md)中，您可以找到更多详细信息。

Tez 是为基于 Linux 的 HDInsight 群集的默认值。

[在 Tez 设计文档配置单元](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez)包含大量的实现选择和优化配置的详细信息。


##<a id="run"></a>选择如何运行 HiveQL 作业

HDInsight 可以运行 HiveQL 作业使用不同的方法。 使用下表来确定哪种方法最适合您，再按照演练的链接。

| 如果需要，请**使用此选项**...                                                     | ...an**交互式**外壳程序 | ...**批处理**操作 | ..厎此**群集操作系统** | ...from 此**客户端操作系统** |
|:--------------------------------------------------------------------------------|:---------------------------:|:-----------------------:|:------------------------------------------|:-----------------------------------------|
| [SSH](hdinsight-hadoop-use-hive-ssh.md)                                         |              ✔              |            ✔            | Linux                                     | Linux 和 Unix、 Mac OS X 和 Windows        |
| [卷曲](hdinsight-hadoop-use-hive-curl.md)                                       |           &nbsp;            |            ✔            | Linux 或窗口                          | Linux 和 Unix、 Mac OS X 和 Windows        |
| [查询控制台](hdinsight-hadoop-use-hive-query-console.md)                     |           &nbsp;            |            ✔            | Windows                                   | 基于浏览器的                            |
| [Visual Studio 的 HDInsight 工具](hdinsight-hadoop-use-hive-visual-studio.md) |           &nbsp;            |            ✔            | Linux 或窗口                          | Windows                                  |
| [Windows PowerShell](hdinsight-hadoop-use-hive-powershell.md)                   |           &nbsp;            |            ✔            | Linux 或窗口                          | Windows                                  |
| [远程桌面](hdinsight-hadoop-use-hive-remote-desktop.md)                   |              ✔              |            ✔            | Windows                                   | Windows                                  |

## 使用内部部署 SQL Server Integration Services Azure HDInsight 上运行配置单元作业

您可以使用 SQL Server Integration Services (SSI) 才能运行配置单元作业。 SSIS Azure 功能包提供了在 HDInsight 使用配置单元作业的以下组件。


- [Azure HDInsight 配置单元任务][hivetask]
- [Azure 订阅连接管理器][调试日志不充分]


了解更多有关 Azure 功能包为 SSIS[这里][ssispack]。


##<a id="nextsteps"></a>下一步行动

现在，您已经了解配置单元是什么，以及如何使用 Hadoop 在 HDInsight 中，使用下面的链接继续探讨其他方法以使用 Azure HDInsight。


- [上载到 HDInsight 的数据][hdinsight 上载数据]
- [使用与 HDInsight 的小猪][使用猪的 hdinsight]
- [HDInsight 使用 MapReduce 作业][hdinsight-使用 mapreduce]

[检查]: ./media/hdinsight-use-hive/hdi.checkmark.png

[1]: ../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md

[hdinsight sdk 文档]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure 的购买选项]: http://azure.microsoft.com/pricing/purchase-options/
[azure 的成员提供]: http://azure.microsoft.com/pricing/member-offers/
[azure 释放试验]: http://azure.microsoft.com/pricing/free-trial/

[apache tez]: http://tez.apache.org
[apache 配置单元]: http://hive.apache.org/
[apache log4j]: http://en.wikipedia.org/wiki/Log4j
[配置单元上 tez wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[导入 excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/
[hivetask]: http://msdn.microsoft.com/en-US/library/mt146771(v=sql.120).aspx
[调试日志不充分]: http://msdn.microsoft.com/en-US/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/en-US/library/mt146770(v=sql.120).aspx

[使用猪的 hdinsight]: hdinsight-use-pig.md
[hdinsight-使用 oozie]: hdinsight-use-oozie.md
[hdinsight-分析-飞行的数据]: hdinsight-analyze-flight-delay-data.md
[hdinsight-使用 mapreduce]: hdinsight-use-mapreduce.md


[hdinsight 存储]: ../hdinsight-use-blob-storage.md

[hdinsight 规定]: hdinsight-provision-clusters.md
[hdinsight 提交作业]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight 上载数据]: hdinsight-upload-data.md
[hdinsight--入门]: ../hdinsight-get-started.md

[Powershell 安装配置]: ../install-configure-powershell.md
[这里的 powershell 字符串]: http://technet.microsoft.com/library/ee692792.aspx

[图像 hdi 配置单元 powershell]: ./media/hdinsight-use-hive/HDI.HIVE.PowerShell.png
[img 的 hdi 的配置单元的 powershell 的输出]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png
[图像 hdi 的配置单元体系结构]: ./media/hdinsight-use-hive/HDI.Hive.Architecture.png

测试

---
ms.openlocfilehash: 948b32186a05c12c336f8f4aa8fd8e3aec923279
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="Hadoop 群集版本的 HDInsight 中的新增功能是什么？ |Microsoft Azure"
    description="HDInsight 支持多个部署的 Hadoop 群集版本。 请参阅支持的 Hadoop 和 HortonWorks 数据平台 (HDP) 分发版本。"
    services="hdinsight"
    editor="cgronlun"
    manager="paulettm"
    authors="mumian"
    documentationCenter=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/09/2015"
    ms.author="jgao"/>


#由 HDInsight 提供的 Hadoop 群集版本中的新增功能是什么？

##HDInsight 版本和 Hadoop 组件
Azure HDInsight 支持多个 Hadoop 群集版本，可以部署在任何时候。 每个版本选项提供的特定版本的 Hortonworks 数据平台 (HDP) 分发和一套该通讯组中包含的组件。 下表中列出是 HDInsight 群集版本关联的组件版本。 请注意，使用 Azure HDInsight 的默认群集版本目前是 3.1，并从 11/7/2014 年起根据 HDP 2.1.7。


<table border="1">
<tr><th>组件</th><th>3.2 HDInsight 版</th><th>HDInsight 版本 3.1 （默认值）</th><th>HDInsight 3.0 版</th><th>HDInsight 版本 2.1</th></tr>
<tr><td>Hortonworks 数据平台</td><td>2.2</td><td>2.1.7</td><td>2.0</td><td>1.3</td></tr>
<tr><td>Apache Hadoop & YARN</td><td>2.6.0</td><td>2.4.0</td><td>2.2.0</td><td>1.2.0</td></tr>
<tr><td>Apache Tez</td><td>0.5.2</td><td>0.4.0</td><td></td><td></td></tr>
<tr><td>Apache 的小猪</td><td>0.14.0</td><td>0.12.1</td><td>0.12.0</td><td>0.11.0</td></tr>
<tr><td>Apache 配置单元和 HCatalog</td><td>0.14.0</td><td>0.13.1</td><td>0.12.0</td><td>0.11.0</td></tr>
<tr><td>Apazhe HBase </td><td>0.98.4</td><td>0.98.0</td><td></td><td></td></tr>
<tr><td>Apache Sqoop</td><td>1.4.5</td><td>1.4.4</td><td>1.4.4</td><td>1.4.3</td></tr>
<tr><td>Apache Oozie</td><td>4.1.0</td><td>4.0.0</td><td>4.0.0</td><td>3.3.2</td></tr>
<tr><td>Apache Zookeeper</td><td>3.4.6 节</td><td>3.4.5</td><td>3.4.5</td><td></td></tr>
<tr><td>Apache 的暴风雨</td><td>0.9.3</td><td>0.9.1</td><td></td><td></td></tr>
<tr><td>Apache Mahout</td><td>0.9.0</td><td>0.9.0</td><td></td><td></td></tr>
<tr><td>Apache 菲尼克斯</td><td>4.2.0</td><td>4.0.0.2.1.7.0-2162</td><td></td><td></td></tr>
<tr><td>Apache 触发</td><td>1.3.1</td><td></td><td></td><td></td></tr>
</table>


**获取组件的当前版本信息**

HDInsight 群集版本关联的组件版本可能会在将来更改更新到 HDInsight。 确定可用的组件，并验证群集正在使用哪个版本的一种方法是使用 Ambari REST API。 可以使用**GetComponentInformation**命令来检索服务组件有关的信息。 有关详细信息，请参阅[Ambari 文档][ambari 文档]。 若要获取此信息的另一个方法是使用远程桌面登录到群集中，检查的内容，"C:\apps\dist\"目录直接。


**发行说明**

HDInsight 的最新版本的其他的发行说明，请参阅[HDInsight 发行说明](hdinsight-release-notes.md)。

### 选择一个版本，当 HDInsight 群集资源调配

在创建群集通过 HDInsight Windows PowerShell cmdlet 或 HDInsight.NET SDK 时，可以使用"版本"参数选择 HDInsight Hadoop 群集版本。

如果您使用**快速创建**选项，则会 3.1 版的 HDInsight，创建默认的 Hadoop 群集。 如果您使用 Azure 门户中的**创建自定义**选项，可以从**HDInsight 版本****群集的详细信息**页上的下拉列表选择将部署的群集版本。

##突出显示功能
一些 HDInsight 平台的突出特征︰

- **触发**的 Apache 触发是开源的并行处理支持内存中处理大数据分析应用程序的性能提高的框架。 触发的内存中计算能力使其适合用于迭代算法在机器学习和关系图的计算中。

    此外可以使用触发执行传统的基于磁盘的数据处理。 触发避免写入磁盘中的中间阶段，从而提高了传统 MapReduce 框架。 此外，触发与程序兼容 Hadoop 分布式文件系统 (HDFS) 和 Azure Blob 存储以便可以轻松地通过触发处理现有的数据。

    此外可以使用脚本操作添加触发。  脚本操作将添加到 HDInsight 3.2 群集或触发 1.0.2 到 HDInsight 3.1 群集的任一触发 1.2.0。 有关详细信息，请参阅[安装和使用 HDInsight Hadoop 群集上触发](hdinsight-hadoop-spark-install.md)。


- **风暴**的风暴在 Azure HDInsight 现已可用，使部署实时分析中只需几次单击，并在几分钟内快速简便的方法。 在 Azure HDInsight 上的 Apache 风暴是提供能够可靠地处理数以百万计的事件分析平台对 Apache Hadoop 生态系统中的一个开放源代码项目。 现在 Hadoop 用户可获得的见解，如发生的事件，以及从过去的事件的见解。 Microsoft 将还提供内置 Visual Studio，集成开发人员与风暴可以轻松。 您现在可以开发、 部署和调试从 Visual Studio 内的风暴拓扑结构。

- **Linux （预览） 上的 HDInsight** -Azure HDInsight 提供的资源调配 (Ubuntu) Linux 虚拟机 (Vm) 运行的 Hadoop 群集的选项。 如果您熟悉 Linux 或 Unix、 迁移从一个现有的基于 Linux 的 Hadoop 解决方案，或与 Hadoop 生态系统组件构建 linux 需要方便地集成，您可以使用此选项。 可以通过使用 Azure 门户、 Azure CLI 或 HDInsight.NET SDK (仅限 Windows) 设置在 Linux 从客户端计算机运行 Windows 或 Linux HDInsight 群集。

- **其他 VM 大小**-HDInsight 群集现在网上有多个虚拟机的类型和大小。 HDInsight 群集现在可以利用 A2 到 A7 大小生成用于常规;D 系列节点采用固态驱动器 (SSDs) 和 60%更快的处理器;和 A8 和 A9 大小具有无限带宽的快速网络连接支持。 Apache HBase Azure HDInsight 客户可以受益于更大的内存配置的 D 系列以提高性能。 Apache 风暴对 Azure HDInsight 客户也可受益于额外的内存加载较大的引用数据集，以及更高的吞吐量更快的 Cpu 中。

- **扩展群集**-群集扩展使您可以更改正在运行的 HDInsight 群集中的节点数而无需删除或重新创建它。 目前，只有 Hadoop 查询和 Apache 风暴具有这一功能，但是 Apache HBase 是很快按照。

- **将操作脚本保存**-此群集自定义功能使用自定义脚本以任意的方式使 Hadoop 群集的修改。 使用此新功能，用户可以试验和部署到 Azure HDInsight 群集可从 Apache Hadoop 生态系统的项目。 此自定义功能是提供对所有类型的 HDInsight 群集，包括 Hadoop，HBase 和风暴。

- **HBase** -HBase 是一个低延迟 NoSQL 数据库，允许大数据的联机事务处理。 HBase 被作为托管群集集成到 Azure 的环境。 群集配置数据将直接存储在 Azure Blob 存储，它提供低延迟和更高的性价比选择在弹性。 这使客户能够构建处理大型数据集，若要生成传感器和遥测数据存储的终点，数以百万计的服务并分析此数据与 Hadoop 作业的交互式网站。

- **Apache 菲尼克斯**的 Apache 菲尼克斯是结构化查询语言 (SQL) 查询层对 HBase。 它支持 SQL 查询语言规范，其中包括次级索引支持的有限的子集。 它将被作为客户端嵌入 Java 数据库连接 (JDBC) 驱动程序对 HBase 数据面向低延迟查询传递。 Apache 菲尼克斯采用 SQL 查询、 将它编译为一系列 HBase 扫描和协处理器调用，并生成正则的 JDBC 结果集。 Apache 菲尼克斯是对 HBase 关系数据库图层。 它将被作为嵌入客户端的 JDBC 驱动程序对 HBase 数据面向低延迟查询传递。 Apache 菲尼克斯采用 SQL 查询、 将它编译成一系列的 HBase 扫描并协调这些扫描，以生成正则的 JDBC 结果集运行。

- **群集的仪表板**的新 web 应用程序部署到您的 HDInsight 群集。 使用该运行配置单元查询、 检查作业日志并浏览 Azure Blob 存储。 用来访问 web 应用程序的 URL 是 <*群集名称*>。 azurehdinsight.net。

- **Microsoft Avro 库**-此库实现 Microsoft.NET 环境的 Apache Avro 数据序列化系统。 Apache Avro 为序列化提供紧凑的二进制数据交换格式。 它使用 JavaScript 对象符号 (JSON) 来定义欠载而语言互操作性的语言无关架构。 在另一个可以读取序列化在一种语言中的数据。 目前支持 C、 c + +、 C#、 Java、 PHP、 Python 和拼音。 Apache Avro 的序列化格式广泛使用 Azure HDInsight，用来表示对 Hadoop MapReduce 作业内复杂的数据结构。

- **YARN** -更换 Hadoop 群集中处理数据的经典 Apache Hadoop MapReduce 框架的一种新的、 通用的分布式应用程序管理框架。 它有效地用作 Hadoop 的操作系统，并采用 Hadoop 从单一用途数据平台的多用途平台，让批处理交互式的在线和流处理批处理。 这一新管理框架可以提高可伸缩性和群集的利用率，根据产能保证公平，以及服务级别协议 (Sla) 等条件。

- **Tez (HDInsight 3.1 和上面只)** -创建跨在 Hadoop 的小规模和大规模工作负载简化的数据处理任务的通用且可自定义框架。 它提供了执行复杂向非循环图 (DAG) 的一次作业任务的能力，以便在 Apache Hadoop 生态系统，例如，Apache 配置单元和 Apache 猪的项目能够满足需求，人工交互的响应时间和极大吞吐量在 petabyte 规模较大。 请注意，配置单元 0.13 允许配置单元查询运行在 Tez，而不是在 MapReduce。

- **高可用性 (HA)**的第二头节点已添加到 Hadoop 群集部署的 HDInsight，以提高服务的可用性。 Hadoop 群集的标准实现通常有单一的头节点。 HDInsight 删除辅助头节点添加此单点故障。 切换到新的高可用性群集配置不会改变价格的群集中，除非客户提供有超大的头节点，而不是默认大尺寸节点的群集中。

- **配置单元性能**的配置单元查询响应时间 (最多 40 x) 和数据压缩的数量级的提高 （可达 80%） 使用**优化行纵栏**(ORC) 设置格式。

- **小猪，Sqoop，Oozie，Ambari** -HDInsight 群集版本 2.1 (HDP 1.3/Hadoop 1.2) 提供奇偶校验的组件版本升级为 HDInsight 群集版本 3.0 (HDP 2.0/Hadoop 2.2)。 请参阅版本表下面的具体情况。

- **Mahout** -此库的可伸缩的机器学习算法是预装上 HDInsight 3.1 （及以上） Hadoop 群集。 因此，您可以运行 Mahout 作业，而不需要任何附加的群集配置。

- **虚拟网络支持**-HDInsight 可以使用 Azure 虚拟网络用于群集以支持隔离的云资源或链接与您的数据中心中的云资源的混合方案。


## 受支持的版本
下表列出了 HDInsight 当前可用的版本、 相应的 Hortonworks 数据平台版本，并且使用它们和它们的发行日期。 如果已知，还提供了其支持过期和否决日期。 请注意以下事项︰

* 有两种头节点的高可用群集部署默认情况下 HDInsight 2.1 和上面。 它们不可用于 HDInsight 1.6 群集。
* 特定版本过期后支持，它可能不能通过 Azure 的门户。 下表指示 Azure 的门户网站上可用的版本。 群集版本仍将可以使用`Version`.NET SDK 及其否决日期之前 Windows PowerShell[新建 AzureHDInsightCluster](http://msdn.microsoft.com/library/dn593744.aspx)命令中的参数。

<table border="1">
<tr><th>HDInsight 版</th><th>HDP 版本</a><th>高可用性</th></th><th>发行日期</th><th>在 Azure 的门户网站上可用</th><th>支持到期日期</th><th>否决日期</th></tr>
<tr><td>HDI 3.2</td><td>HDP 2.2</td><td>是</td><td>2015 年 2/18</td><td>是</td><td></td><td></td></tr>
<tr><td>HDI 3.1</td><td>HDP 2.1</td><td>是</td><td>到 2014 6/24 年</td><td>是</td><td></td><td></td></tr>
<tr><td>HDI 3.0</td><td>HDP 2.0</td><td>是</td><td>到 2014 年 02/11 /</td><td>是</td><td>到 2014 年 09/17 /</td><td>2015 年 06/30</td></tr>
<tr><td>HDI 2.1</td><td>HDP 1.3</td><td>是</td><td>2013 年 10/28</td><td>否</td><td>到 2014 年 05/12 /</td><td>2015 年 05/31</td></tr>
<tr><td>HDI 1.6</td><td>HDP 1.1</td><td>否</td><td>2013 年 10/28</td><td>否</td><td>到 2014 年 04/26 /</td><td>2015 年 05/31</td></tr>
</table><br>

**非默认的群集的部署**

所以用户必须使用在门户中**创建自定义**选项来指定其他 HDInsight 群集版本，如果它们需要创建默认情况下，Hadoop 2.4 上创建 HDInsight 3.1 群集。

### 针对 HDInsight 群集版本的服务级别协议

按照"支持窗口"定义 SLA。 支持窗口指的时间是 HDInsight 群集版本支持的 Microsoft 客户服务和支持。 其版本是否**支持到期日期**晚于当前日期，HDInsight 群集将不在支持窗口。 上面的表中，可以找到支持 HDInsight 群集版本的列表。 给定的 HDInsight （一旦可用新版 X + 1） 的 X 版本支持到期日期计算为更高版本的︰  

- 公式 1︰ 添加到日期 HDInsight 群集版本发布 X 180 天。
- 公式 2︰ 添加到日期 HDInsight 90 天群集版本 X + 1 （X 之后的后续版本） 都可以在 Azure 的门户。

**否决日期**是不能其后在 HDInsight 中创建群集版本的日期。

> [AZURE.NOTE] HDInsight 2.1 和 3.0 群集运行在 Azure 来宾操作系统[家族 4](../cloud-services-guestos-update-matrix.md)，使用 64 位版本的 Windows Server 2012 R2 并支持.NET Framework 4.0，4.5。 然后 4.5.1。

## Hortonworks 发行 HDInsight 版本关联的说明##

* 3.2 版 HDInsight 群集使用 Hadoop 分布，基于[Hortonworks 数据平台 2.2][hdp-2-2]。

    * 对特定的 Apache 组件的[配置单元 0.14](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310843&version=12326450)、[猪 0.14](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310730&version=12326954)、 [HBase 0.98.4](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310753&version=12326810)、[菲尼克斯 4.2.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12315120&version=12327581)、 [M/R 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310941&version=12327180)， [HDFS 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310942&version=12327181)、 [YARN 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12313722&version=12327197)、[通用](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310240&version=12327179)、 [Tez 0.5.2](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12314426&version=12328742)、 [Ambari 2.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12312020&version=12327486)、[风暴 0.9.3](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12314820&version=12327112)、 [Oozie 4.1.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?version=12324960&projectId=12311620)发行说明。


* 3.1 版 HDInsight 群集使用 Hadoop 分布，基于[Hortonworks 数据平台 2.1.7][hdp-2-1-7]。 这是**默认**的 Hadoop 群集时 11/7/2014年后使用 Azure HDInsight 门户创建。 基于[Hortonworks 数据平台 2.1.1][hdp-2-1-1]11/7/2014年前创建的 HDInsight 3.1 群集。

* HDInsight 群集版本 3.0 使用 Hadoop 分布，基于[Hortonworks 数据平台 2.0][hdp 2 0 8]。

* 2.1 版 HDInsight 群集使用 Hadoop 分布，基于[Hortonworks 数据平台 1.3][hdp-1-3-0]。

* HDInsight 群集版本 1.6 使用 Hadoop 分布，基于[Hortonworks 数据平台 1.1][hdp-1-1-0]。


[图像 hdi 版本 versionscreen]: ./media/hdinsight-component-versioning/hdi-versioning-version-screen.png

[华盛顿论坛]: http://azure.microsoft.com/support/forums/

[连接的 excel-与-配置单元的 ODBC]: hdinsight-connect-excel-hive-ODBC-driver.md

[hdp-2-2]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.2.0/HDP_2.2.0_Release_Notes_20141202_version/index.html

[hdp 2 1 7]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html

[hdp-2-1-1]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.1/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.1.html

[hdp 2 0 8]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.8.0/bk_releasenotes_hdp_2.0/content/ch_relnotes-hdp2.0.8.0.html

[hdp-1-3-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.0/bk_releasenotes_hdp_1.x/content/ch_relnotes-hdp1.3.0_1.html

[hdp-1-1 0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-Win-1.1/bk_releasenotes_HDP-Win/content/ch_relnotes-hdp-win-1.1.0_1.html

[ambari 文档]: https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md

[zookeeper]: http://zookeeper.apache.org/

测试

---
ms.openlocfilehash: adbf528c06e83244352c6f2fb1f3f7e44ca80a9d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="Linux 教程︰ 开始使用 Hadoop 和配置单元 |Microsoft Azure"
    description="按照本 Linux 教程在 HDInsight 中使用 Hadoop 开始。 了解如何设置 Linux 群集和查询数据的配置单元。"
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="paulettm"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="08/07/2015"
    ms.author="nitinme"/>

# Hadoop 教程︰ 入门 Hadoop 使用 Linux （预览） 上的 HDInsight 中的配置单元

> [AZURE.SELECTOR]
- [Windows](hdinsight-hadoop-tutorial-get-started-windows.md)
- [Linux](hdinsight-hadoop-linux-tutorial-get-started.md)

本 Hadoop 教程获取您向您展示如何配置 Hadoop 群集在 Linux 上运行配置单元查询快速开始使用 Azure HDInsight 在 Linux 上。


> [AZURE.NOTE] 如果您是新手 Hadoop 和大数据，您可以阅读更多有关条款<a href="http://go.microsoft.com/fwlink/?LinkId=510084" target="_blank">Apache Hadoop</a>， <a href="http://go.microsoft.com/fwlink/?LinkId=510086" target="_blank">MapReduce</a>， <a href="http://go.microsoft.com/fwlink/?LinkId=510087" target="_blank">Hadoop 分布式文件系统 (HDFS)</a>，和<a href="http://go.microsoft.com/fwlink/?LinkId=510085" target="_blank">配置单元</a>。 为了理解 HDInsight 是如何让 Hadoop Azure 中的，请参阅[简介 Hadoop 在 HDInsight](hdinsight-hadoop-introduction.md)。


## 此教程 accomplish 什么？

假设您拥有大量的非结构化的数据集并且想要运行的查询来提取一些有意义的信息。 下面是如何达到此目的︰

   ![Hadoop 教程步骤︰ 创建存储帐户;配置 Hadoop 群集;查询数据，配置单元。](./media/hdinsight-hadoop-linux-tutorial-get-started/HDI.Linux.GetStartedFlow.png)


## 先决条件

本教程中 Linux 的 Hadoop 之前，必须具有下列︰

- **Azure 订阅**。 请参阅[获取 Azure 免费试用版](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)。
- **安全外壳协议 (SSH) 键**。 如果要为远程 Linux 群集到用而非密码密钥使用 SSH。 使用密钥是推荐的方法，因为这样做更安全。 有关如何生成的 SSH 密钥的说明，请参阅下列文章︰
    -  从 Linux 计算机-[使用 SSH 通过从 Linux、 Unix 或 OS X 的基于 Linux 的 HDInsight (Hadoop)](hdinsight-hadoop-linux-use-ssh-unix.md)。
    -  从 Windows 计算机[使用 SSH 与从 Windows 的基于 Linux 的 HDInsight (Hadoop)](hdinsight-hadoop-linux-use-ssh-windows.md)。

**估计完成时间︰** 30 分钟

## <a name="provision"></a>在 Linux 上的 HDInsight 群集配置

当您配置一个群集时，您将配置 Hadoop 和相关的应用程序包含的 Azure 计算资源。 在本节中，您将配置 HDInsight 3.2 版本的群集。 您还可以创建其他版本的 Hadoop 群集。 有关说明，请参阅[供应 HDInsight 群集使用的自定义选项][hdinsight 规定]。 其服务级别协议和 HDInsight 版本有关的信息，请参阅[HDInsight 组件版本控制](hdinsight-component-versioning.md)。

>[AZURE.NOTE]  您还可以创建运行 Windows 服务器操作系统的 Hadoop 群集。 有关说明，请参阅[开始使用在 Windows 上的 HDInsight](hdinsight-hadoop-tutorial-get-started-windows.md)。


**若要设置 HDInsight 群集**

1. 登录到[Azure 预览门户](https://ms.portal.azure.com/)。
2. 单击**新建**，单击**数据分析**，然后单击**HDInsight**。

    ![在 Azure 预览门户网站中创建一个新的群集](./media/hdinsight-hadoop-linux-tutorial-get-started/HDI.CreateCluster.1.png "Creating a new cluster in the Azure Preview Portal")

3. 输入的**群集名称**，选择**Hadoop** **群集类型**，并从**群集操作系统**下拉列表，选择**Ubuntu**。 如果可用，群集名称旁边将出现绿色复选标记。

    ![输入群集名称和类型](./media/hdinsight-hadoop-linux-tutorial-get-started/HDI.CreateCluster.2.png "Enter cluster name and type")

4. 如果您有多个订阅，请单击选择 Azure 用于群集的订阅的**订阅**条目。

5. 单击以查看现有资源组的列表，然后选择要创建的群集中的一个**资源组**。 或者，可以单击**新建**，然后输入新的资源组的名称。 绿色复选标记将显示以指示新组的名称是否可用。

    > [AZURE.NOTE] 如果有的话，此项将默认为您现有的资源组之一。

6. 单击**凭据**，然后输入管理员用户的密码。 此外必须输入**SSH 用户名**和**密码**或**公钥**，它将使用 SSH 用户进行身份验证。 使用公钥是推荐的方法。 在底部保存的凭据配置，请单击**选择**。

    ![提供群集凭据](./media/hdinsight-hadoop-linux-tutorial-get-started/HDI.CreateCluster.3.png "Provide cluster credentials")

    在 HDInsight 中使用 SSH 的详细信息，请参阅下列文章︰

    * [HDInsight 从 Linux、 Unix 或 OS X 上的基于 Linux 的 Hadoop 使用 SSH](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [在从 Windows HDInsight 基于 Linux 的 Hadoop 使用 SSH](hdinsight-hadoop-linux-use-ssh-windows.md)


7. 单击要选择的群集中，现有的数据源的**数据源**或创建一个新。 当您配置 Hadoop 群集在 HDInsight 中的时，您将指定 Azure 存储帐户。 从该帐户的特定 Blob 存储容器 Hadoop 分布式文件系统 (HDFS) 中指定为默认的文件系统，如。 默认情况下，HDInsight 群集配置中为您指定的存储帐户相同的数据中心。 有关详细信息，请参阅[使用 Azure Blob 存储与 HDInsight](hdinsight-use-blob-storage.md)

    ![数据源刀片式服务器](./media/hdinsight-hadoop-linux-tutorial-get-started/HDI.CreateCluster.4.png "Provide data source configuration")

    目前您可以用作数据源的 HDInsight 群集选择 Azure 存储帐户。 使用以下方法来了解**数据源**刀片式服务器上的项。

    - **选择方法**︰ 将此值设置为**所有订阅**允许浏览的所有订阅的存储帐户。 此设置为**访问键**如果想要输入的**存储名称**和现有存储帐户的**访问键**。

    - **选择存储帐户创建新 /**︰ 单击**选择存储帐户**，浏览并选择您希望群集相关联的现有存储帐户。 或者，单击**新建**以创建新的存储帐户。 使用字段显示输入存储帐户的名称。 如果的名称可用，则会出现绿色复选标记。

    - **选择默认容器**︰ 用于输入要用于群集的默认容器的名称。 您可以输入任何名称，我们建议为群集使用相同的名称，以便您可以轻松地识别该容器用于此特定群集。

    - **位置**︰ 存储帐户，或将在中创建的地理区域。

        > [AZURE.IMPORTANT] 选择默认的数据源的位置还会设置 HDInsight 群集的位置。 在同一区域必须位于群集和默认数据源。

    单击**选择**以保存数据源配置。

8. 单击**节点定价层**以显示将为此群集创建的节点的信息。 设置所需的群集的辅助节点数。 刀片式服务器中，将显示群集的估计的成本。

    ![节点定价层刀片式服务器](./media/hdinsight-hadoop-linux-tutorial-get-started/HDI.CreateCluster.5.png "Specify number of cluster nodes")

    单击**选择**要保存节点定价配置。

9. **新的 HDInsight 群集**刀片式服务器，请确保**附到 Startboard**被选中，然后单击**创建**。 这会创建群集，并到 Azure 门户网站 Startboard 为其添加一个图块。 该图标将指示群集资源调配，并完成资源调配后显示的 HDInsight 图标将会更改。

在资源调配时|设置完成
------------------|---------------------
    ![资源调配上 startboard 的指示器](./media/hdinsight-hadoop-linux-tutorial-get-started/provisioning.png)|![提供的群集平铺](./media/hdinsight-hadoop-linux-tutorial-get-started/provisioned.png)

> [AZURE.NOTE] 它将需要一些时间为群集创建，通常大约 15 分钟。 使用 Startboard 或**通知**条目左侧的页上平铺在资源调配过程检查。

完成设置后，单击 Startboard 启动群集刀片式服务器从群集的拼贴。

## <a name="hivequery"></a>提交在群集上的配置单元作业
既然您已经有一个 HDInsight Linux 群集配置下, 一步是运行查询示例数据 (sample.log) HDInsight 群集附带的示例配置单元作业。 示例数据包含日志的信息，包括跟踪、 警告、 信息、 和错误。 我们查询这些数据，可以检索与特定严重性的所有错误日志。 您必须执行以下步骤以在 HDInsight Linux 群集上运行配置单元查询︰

- 连接到 Linux 群集
- 运行配置单元作业



### 若要连接到群集

您可以从连接到 Linux 上 HDInsight 群集 Linux 计算机或基于 Windows 的计算机通过使用 SSH。

**从 Linux 计算机连接**

1. 打开终端，输入以下命令︰

        ssh <username>@<clustername>-ssh.azurehdinsight.net

    因为提供了快速创建选项的群集时，默认的 SSH 用户名称是**hdiuser**。 因此，该命令必须是︰

        ssh hdiuser@myhdinsightcluster-ssh.azurehdinsight.net

2. 出现提示时，输入群集资源调配时提供的密码。 成功建立连接后，提示将更改如下︰

        hdiuser@headnode-0:~$


**从基于 Windows 的计算机连接**

1. 下载<a href="http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html" target="_blank">PuTTY</a>的基于 Windows 的客户端。

2. 打开 PuTTY。 在**类别**中，单击**会话**。 从**基本的 PuTTY 会话选项**屏幕，在**主机名 （或 IP 地址）**字段中输入 HDInsight 服务器的 SSH 地址。 SSH 地址是您群集的名称后, 跟**-ssh.azurehdinsight.net**。 例如， **myhdinsightcluster-ssh.azurehdinsight.net**。

    ![连接到 linux 中使用 PuTTY HDInsight 群集](./media/hdinsight-hadoop-linux-tutorial-get-started/HDI.linux.connect.putty.png)

3. 要保存连接信息供将来使用，请输入下**保存会话**，此连接的名称，然后单击**保存**。 连接将被添加到已保存会话的列表中。

4. 单击**打开**到群集的连接。 当系统提示您输入用户名称，请输入**hdiuser**。 输入密码，输入群集资源调配时指定的口令。 成功建立连接后，提示将更改如下︰

        hdiuser@headnode-0:~$

### 若要运行配置单元作业

一旦连接到群集通过 SSH，可以使用以下命令来运行配置单元查询。

1. 通过在提示符下使用以下命令启动配置单元命令行界面 (CLI):

        hive

2. 使用 CLI，输入下列语句来创建新的表名**log4jLogs**为在群集上使用已经可用的示例数据︰

        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasb:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS cnt FROM log4jLogs WHERE t4 = '[ERROR]' GROUP BY t4;

    这些语句执行以下操作︰

    - **除去表**-删除表和数据文件，以防表已存在。
    - **创建外部表**-在配置单元中创建一个新的"外部"表。 外部表将只表定义存储在配置单元;将数据留在原来的位置。
    - **行格式**的说明配置单元设置数据的格式。 在这种情况下，由空格分隔每个日志中的字段。
    - **存储作为文本文件位置**-告诉配置单元数据的存储 （示例中的数据目录），并且它将存储为文本。
    - **选择**选择所有列 t4 其中包含值 [错误] 的行数。

    >[AZURE.NOTE] 期望通过外部源，例如自动化的数据上载过程中，或者通过 MapReduce 的另一个操作，更新基础数据时，应使用外部表，但总要配置单元查询使用最新的数据。 删除外部表 does*不*删除数据，仅表定义。

    这将返回以下输出︰

        Query ID = hdiuser_20150116000202_cceb9c6b-4356-4931-b9a7-2c373ebba493
        Total jobs = 1
        Launching Job 1 out of 1
        Number of reduce tasks not specified. Estimated from input data size: 1
        In order to change the average load for a reducer (in bytes):
          set hive.exec.reducers.bytes.per.reducer=<number>
        In order to limit the maximum number of reducers:
          set hive.exec.reducers.max=<number>
        In order to set a constant number of reducers:
          set mapreduce.job.reduces=<number>
        Starting Job = job_1421200049012_0006, Tracking URL = <URL>:8088/proxy/application_1421200049012_0006/
        Kill Command = /usr/hdp/2.2.1.0-2165/hadoop/bin/hadoop job  -kill job_1421200049012_0006
        Hadoop job information for Stage-1: number of mappers: 1; number of reducers: 1
        2015-01-16 00:02:40,823 Stage-1 map = 0%,  reduce = 0%
        2015-01-16 00:02:55,488 Stage-1 map = 100%,  reduce = 0%, Cumulative CPU 3.32 sec
        2015-01-16 00:03:05,298 Stage-1 map = 100%,  reduce = 100%, Cumulative CPU 5.62 sec
        MapReduce Total cumulative CPU time: 5 seconds 620 msec
        Ended Job = job_1421200049012_0006
        MapReduce Jobs Launched:
        Stage-Stage-1: Map: 1  Reduce: 1   Cumulative CPU: 5.62 sec   HDFS Read: 0 HDFS Write: 0 SUCCESS
        Total MapReduce CPU Time Spent: 5 seconds 620 msec
        OK
        [ERROR]    3
        Time taken: 60.991 seconds, Fetched: 1 row(s)

    请注意输出中包含**[错误] 3**，有三个包含该值的行。

3. 使用下面的语句创建名为**errorLogs**的新"内部"表︰

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]';


    这些语句执行以下操作︰

    - 如果不存在，则**创建表如果不存在**-创建一个表。 由于不使用**外部**关键字，则这种内部表，它存储在配置单元数据仓库和完全由配置单元。 与外部表，除去内部的表将删除基础数据。
    - **存储 AS ORC**的优化行纵栏式 (ORC) 格式存储的数据。 这是用于存储配置单元数据的高度优化和有效格式。
    - **插入覆盖...选择**-从包含 [错误] 的**log4jLogs**表中选择行并**errorLogs**表中插入数据。

4. 要验证仅行中列 t4 包含 [错误] 已存储到**errorLogs**表，请使用下面的语句从**errorLogs**返回的所有行︰

        SELECT * from errorLogs;

    应该在控制台上显示以下输出︰

        2012-02-03  18:35:34    SampleClass0    [ERROR]  incorrect      id
        2012-02-03  18:55:54    SampleClass1    [ERROR]  incorrect      id
        2012-02-03  19:25:27    SampleClass4    [ERROR]  incorrect      id
        Time taken: 0.987 seconds, Fetched: 3 row(s)

    返回的数据全部应对应于 [错误] 日志。

## <a name="nextsteps"></a>下一步行动
在 Linux 本教程中，您学习了如何调配与 HDInsight 的 Linux 上的 Hadoop 群集和使用 SSH 在其上运行的配置单元查询。 若要了解详细信息，请参阅下列文章︰

- [使用 Ambari 管理 HDInsight 群集](hdinsight-hadoop-manage-ambari.md)︰ 基于 Linux 的 HDInsight 群集使用 Ambari，用于管理和监视的 Hadoop 服务。 Https://CLUSTERNAME.azurehdinsight.net 在每个群集提供了 Ambari web 用户界面。

    > [AZURE.IMPORTANT] 虽然 Ambari 网站的很多部分都是直接通过 Internet 访问，web 界面的 Hadoop 服务如资源管理器或作业历史记录需要使用 SSH 隧道。 在 HDInsight 中使用 SSH 隧道的详细信息，请参阅下列文章︰
    >
    > * [HDInsight 从 Linux、 Unix 或 OS X 上的基于 Linux 的 Hadoop 使用 SSH](hdinsight-hadoop-linux-use-ssh-unix.md#tunnel)
    > * [在从 Windows HDInsight 基于 Linux 的 Hadoop 使用 SSH](hdinsight-hadoop-linux-use-ssh-windows.md#tunnel)

- [Linux 中使用的自定义选项设置 HDInsight](hdinsight-hadoop-provision-linux-clusters.md)︰ 了解有关如何设置 HDInsight 群集的详细信息。

- [使用 Linux 上的 HDInsight](hdinsight-hadoop-linux-information.md)︰ 如果您已经熟悉 Hadoop 在 Linux 平台上，这篇文档提供指导 Azure 的特定信息，如︰

    * 在集群上，Ambari 和 WebHCat 等承载服务的 Url
    * Hadoop 文件和示例在本地文件系统上的位置
    * 与默认的数据使用的 Azure 存储 (WASB) 而不是 HDFS 存储

- 在配置单元，或要了解猪和 MapReduce 的详细信息，请参阅以下资源︰

    - [使用 MapReduce 与 HDInsight][hdinsight-使用 mapreduce]
    - [使用 HDInsight 蜂巢][配置单元使用 hdinsight-]
    - [使用与 HDInsight 的小猪][使用猪的 hdinsight]

- 如何使用 HDInsight 群集使用 Azure 存储的详细信息，请参阅以下资源︰

    - [使用 HDInsight Azure Blob 存储](../hdinsight-use-blob-storage.md)
    - [上载到 HDInsight 的数据][hdinsight 上载数据]


[1]: ../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md

[hdinsight 规定]: hdinsight-provision-clusters.md
[hdinsight-管理 powershell]: hdinsight-administer-use-powershell.md
[hdinsight 上载数据]: hdinsight-upload-data.md
[hdinsight-使用 mapreduce]: hdinsight-use-mapreduce.md
[配置-单元使用 hdinsight]: hdinsight-use-hive.md
[使用猪的 hdinsight]: hdinsight-use-pig.md

[powershell 下载]: http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409
[powershell 安装配置]: ../install-configure-powershell.md
[powershell 开放]: ../install-configure-powershell.md#Install

[img 的 hdi 的仪表板]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.dashboard.png
[img 的 hdi 的仪表板的查询的选择]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.dashboard.query.select.png
[img-hdi-dashboard-query-select-result]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.dashboard.query.select.result.png
[img-hdi-dashboard-query-select-result-output]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.dashboard.query.select.result.output.png
[img-hdi-dashboard-query-browse-output]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.dashboard.query.browse.output.png
[图像的 hdi-clusterstatus]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.ClusterStatus.png
[图像的 hdi-gettingstarted-powerquery-导入数据]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.GettingStarted.PowerQuery.ImportData.png
[图像的 hdi-gettingstarted-powerquery-importdata2]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.GettingStarted.PowerQuery.ImportData2.png

测试

---
ms.openlocfilehash: 155a4bb66d95462d72b38adf2134eee3a6b363fd
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="Hadoop 教程︰ 开始使用 Hadoop 在 Windows |Microsoft Azure"
   description="开始使用 Hadoop HDInsight 中。 了解如何调配上 Windows 的 Hadoop 群集、 运行配置单元查询数据，并分析在 Excel 中的输出。"
   keywords="hadoop 教程，在 windows 中，hadoop 群集的 hadoop 了解 hadoop，配置单元查询"
   services="hdinsight"
   documentationCenter=""
   authors="nitinme"
   manager="paulettm"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="08/07/2015"
   ms.author="nitinme"/>


# Hadoop 教程︰ 开始使用 Hadoop 和 HDInsight 在 Windows 中的配置单元查询

> [AZURE.SELECTOR]
- [Windows](../hdinsight-hadoop-tutorial-get-started-windows-v1.md)
- [Linux](../hdinsight-hadoop-linux-get-started.md)

为了帮助您了解 Windows 上的 Hadoop 并开始使用 HDInsight，本教程演示如何在 Hadoop 群集中的非结构化数据上运行配置单元查询和分析的结果在 Microsoft Excel 中。

[AZURE.INCLUDE [hdinsight-azure-portal](../../includes/hdinsight-azure-portal.md)]

* [在 Windows 上的 HDInsight 的 Hadoop 入门](hdinsight-hadoop-tutorial-get-started-windows.md)

## 本 Hadoop 教程中实现什么？

假设您拥有大量的非结构化的数据集和要运行的配置单元查询以提取一些有意义的信息。 这正是我们打算在本教程中。 下面是我们如何达到这︰

   !["Hadoop 教程︰ 创建一个帐户;配置 Hadoop 群集;提交配置单元查询;分析在 Excel 中的数据。][image-hdi-getstarted-flow]

观看教程学习 Hadoop HDInsight 上的演示视频︰

![第一个的 Hadoop 教程的视频︰ 提交在 Hadoop 群集上，配置单元查询和分析在 Excel 中的结果。][img-hdi-getstarted-video]

**[在 YouTube 上的 HDInsight 监视 Hadoop 教程](https://www.youtube.com/watch?v=Y4aNjnoeaHA&list=PLDrz-Fkcb9WWdY-Yp6D4fTC1ll_3lU-QS)**


结合 Azure HDInsight 的一般可用性，Microsoft 还提供了 Azure，前身为*Microsoft HDInsight 开发者预览版*HDInsight 模拟器。 仿真程序开发人员方案的目标，只支持单节点部署。 有关使用 HDInsight 仿真程序的信息，请参阅[开始使用 HDInsight 仿真程序][hdinsight 仿真程序]。

> [AZURE.NOTE] 有关说明如何调配 HBase 群集，请参阅[HDInsight 中的规定 HBase 簇][hdinsight hbase 自定义设置]。 请参阅<a href="http://go.microsoft.com/fwlink/?LinkId=510237">Hadoop 和 HBase 之间的区别是什么？</a>以了解为什么您可能会选择在另一个数据库。

## 先决条件

为 Windows 上的 Hadoop 开始本教程之前，您必须具有以下︰


- **Azure 订阅**。 请参阅[获取 Azure 免费试用版](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)。
- 与 Office 2013 专业加、 Office 365 专业人员加上、 Excel 2013 独立或 Office 2010 专业版加**一个工作站计算机**。

**若要完成本教程的估计时间︰** 30 分钟



##<a name="storage"></a>创建一个 Azure 存储帐户

当您配置 Hadoop 群集在 HDInsight 中的时，您将指定 Azure 存储帐户。 从该帐户的特定 Blob 存储容器 Hadoop 分布式文件系统 (HDFS) 中指定为默认的文件系统，如。 默认情况下，HDInsight 群集配置中为您指定的存储帐户相同的数据中心。 有关详细信息，请参阅[使用 Azure Blob 存储与 HDInsight][hdinsight 存储]

>[AZURE.NOTE] 不要使用多个 Hadoop 群集共享默认 Blob 存储容器。

除了此存储帐户，还可以添加更多存储帐户，当您自定义的配置群集。 此附加的存储帐户可以从相同的 Azure 订阅或不同的 Azure 预订。 有关说明，请参阅[供应 HDInsight 群集使用的自定义选项][hdinsight 规定]。

本教程使用仅默认 blob 和默认的存储帐户。

**若要创建一个 Azure 存储帐户**

1. 登录到[Azure 门户][azure 管理门户]。
2. 单击左下角的**新建**，然后输入的值，如图所示。

    ![其中可用于快速创建新的存储帐户设置 azure 的门户。][image-hdi-storageaccount-quickcreate]

>[AZURE.NOTE]  请确保您在支持的群集中的某个位置中创建存储帐户。 它们是︰**东亚**、**东南亚**、**北欧**、**西欧**、**东亚美国**、**西部美国**、**美国中北部**、**南部美国中部**。

从列表中选择新的存储帐户，然后单击页面底部的**管理访问键**。 记下要**访问主键**(或**辅助快捷键**— 任一项工作)。  在本教程后面部分，将会需要它。 有关详细信息，请参阅[如何创建存储帐户][azure 创建 storageaccount] 。

##<a name="provision"></a>配置 Hadoop 群集

当您配置一个群集时，您将配置 Hadoop 和相关的应用程序包含的 Azure 计算资源。 在本节中，您将配置 HDInsight 3.1 版群集，它基于 Hadoop 2.4 版本。 此外可以通过使用 Azure 门户、 HDInsight PowerShell 的 cmdlet 或 HDInsight.NET SDK 来创建其他版本的 Hadoop 群集。 有关说明，请参阅[供应 HDInsight 群集使用的自定义选项][hdinsight 规定]。 其服务级别协议和 HDInsight 版本有关的信息，请参阅[HDInsight 组件版本控制](hdinsight-component-versioning.md)。

[AZURE.INCLUDE [provisioningnote](../../includes/hdinsight-provisioning.md)]


**若要配置 Hadoop 群集**

1. 登录到[Azure 门户][azure 管理门户]。

2. 单击左下角的**新建**，然后输入的值，如图所示。

    ![在 HDInsight 的 Hadoop 群集创建。][image-hdi-quickcreatecluster]

<!-- COMMENTED OUT TEXT BEGINS --

4. Enter or select the following values:


    <table border="1">
    <tr><th>Name</th><th>Value</th></tr>
    <tr><td>Cluster Name</td><td>Name of the cluster.</td></tr>
    <tr><td>Cluster Size</td><td>Number of data nodes you want to deploy. The default value is 4. But the option to use 1 or 2 data nodes is also available from the drop-down list. Any number of cluster nodes can be specified by using the <strong>Custom Create</strong> option. Pricing details about the billing rates for various cluster sizes are available. Click the <strong>?</strong> symbol above the drop-down list and follow the link that appears.</td></tr>
    <tr><td>Password</td><td>The password for the <i>admin</i> account. The cluster user name "admin" is specified when you are not using the <strong>Custom Create</strong> option. Note that this is NOT the Windows Administrator account for the VMs on which the clusters are provisioned. The account name can be changed by using the <strong>Custom Create</strong> wizard.</td></tr>
    <tr><td>Storage Account</td><td>Click the drop-down list, and select the storage account that you created. <br/>

    When a storage account is chosen, it cannot be changed. If the storage account is removed, the cluster will no longer be available for use.

    The HDInsight cluster is located in the same datacenter as the storage account.
    </td></tr>
    </table>

    Keep a copy of the cluster name. You will need it later in the tutorial.


5. Click **Create HDInsight Cluster**. When the provisioning completes, the  status column shows **Running**.

-- COMMENTED OUT TEXT ENDS -->

>[AZURE.NOTE] 这些步骤设置使用 3.1 版 HDInsight 群集。 若要创建与其他版本的群集，使用从门户**创建自定义**的方法或使用 Azure PowerShell。 有关每个版本之间的不同的信息，请参阅[由 HDInsight 提供的群集版本中的新增功能？][hdinsight 版本]。 有关使用**自定义创建**选项的信息，请参阅[使用自定义选项的设置 HDInsight 群集][hdinsight 规定]。


##<a name="sample"></a>从门户网站上运行的示例数据

已成功设置的 HDInsight 群集提供了包括入门的库，可以直接从门户运行示例查询控制台。 可以使用示例来学习如何使用 HDInsight 来通过逐步介绍一些基本情况。 这些示例附带所有必需的组件，如要分析的数据和在数据上运行查询。 若要了解有关入门教程库中的示例的详细信息，请参阅[了解的 Hadoop HDInsight 使用 HDInsight 前已启动库中](hdinsight-learn-hadoop-use-sample-gallery.md)。

**若要运行该示例**，从 Azure 的门户，单击要用来运行该示例，群集名称，然后单击页面底部的**查询控制台**。 打开该网页，从**前已启动库**选项卡，然后**样本**类别下，单击您想要运行该示例。 按照说明完成此示例网页。 下表列出了几个示例，并提供了有关哪些每个样本的作用的详细信息。

示例 | 其作用是什么？
------ | ---------------
[传感器数据分析][hdinsight 传感器数据示例] | 了解如何使用 HDInsight 来处理产生的加热，通风，空调 (HVAC) 系统来识别系统不能可靠地维护设置温度的历史数据。
[网站日志分析][网络日志的 hdinsight 示例] | 了解如何使用 HDInsight 来分析网站日志文件，以深入了解到网站外部网站，从一天的访问的频率以及用户体验的网站错误的摘要。
[Twitter 趋势分析](hdinsight-analyze-twitter-data.md) | 了解如何使用 HDInsight 来分析在 Twitter 趋势。



##<a name="hivequery"></a>从门户网站上运行配置单元查询
现在，您已设置 HDInsight 群集下, 一步是运行查询的示例配置单元表的配置单元作业。 我们将使用*hivesampletable*，其中附带了 HDInsight 群集。 表中包含有关移动设备制造商、 平台和模型的数据。 此表中的配置单元查询检索特定制造商的移动设备的数据。

> [AZURE.NOTE] HDInsight 工具 Visual Studio.NET 版本 2.5 或更高版本的带有 Azure SDK。 在 Visual Studio 中使用的工具，您可以连接到 HDInsight 群集、 创建配置单元表并运行配置单元查询。 有关详细信息，请参阅[开始使用 Visual Studio 的 HDInsight Hadoop 工具][1]。

**要从群集控制板运行配置单元作业**

1. 登录到[Azure 门户][azure 管理门户]。
2. 从左窗格中，单击**HDINSIGHT** 。 您将看到群集，包括刚才在上一节中创建的群集的列表。
3. 单击您想要使用运行配置单元作业中，该群集的名称，然后单击页面底部的**查询控制台**。
4. 在不同的浏览器选项卡中打开网页。 输入的 Hadoop 的用户帐户和密码。 默认的用户名是**admin**;该密码是配置群集时输入的内容。 仪表板如下所示︰

    ![在 HDInsight 群集仪表板配置单元编辑器选项卡。][img-hdi-dashboard]

    有几个选项卡页的顶部。 默认选项卡中为**配置单元编辑器**，以及其他选项卡是**作业历史记录**，**文件浏览器**。 通过使用仪表板，可以提交配置单元查询、 检查 Hadoop 作业日志，并浏览存储中的文件。

    > [AZURE.NOTE] 请注意，该网页的 URL 是*&lt;群集名称&gt;。 azurehdinsight.net*。 因此而不是从门户打开仪表板，可以使用 URL 来打开仪表板 web 浏览器中。

6. **配置单元编辑器**选项卡上，输入**查询名称**，输入**HTC20**。  该查询将职务。 在查询窗格中，输入配置单元查询，如图所示︰

    ![配置单元中的配置单元编辑器中的查询窗格中输入查询。][img-hdi-dashboard-query-select]

4. 单击**提交**。 花费一些时间来重新获得的结果。 屏幕刷新每隔 30 秒。 您还可以单击**刷新**来刷新屏幕。

    ![群集面板底部列出配置单元在查询的结果。][img-hdi-dashboard-query-select-result]

5. 状态显示在作业完成后，请单击在屏幕上查看输出的查询名称。 记下**作业开始时间 (UTC)**。 您将稍后需要它。

    ![作业开始时间 HDInsight 群集控制板的作业历史记录选项卡中列出。][img-hdi-dashboard-query-select-result-output]

    该页面还显示**作业输出**和**作业日志**。 您还可以选择将输出文件下载 (\_stdout) 和日志文件\(_stderr)。


**浏览到输出文件**

1. 在群集面板中，单击**文件浏览器**。
2. 单击您的存储帐户名称，单击容器名 （它是群集名称相同），然后单击**用户**。
3. 单击**管理员**，然后单击有上次的修改时间 （有点后作业开始的时间前面提到） 的 GUID。 复制此 GUID。 下一节中，您将需要它。


    ![配置单元查询输出的文件在文件浏览器选项卡中列出的 GUID。][img-hdi-dashboard-query-browse-output]


##<a name="powerquery"></a>对于 Excel 连接到 Microsoft 商业智能工具

您可以使用电源查询添加 Microsoft excel 从 HDInsight 的作业输出导入到 Excel 中，可以使用 Microsoft 商业智能工具来进一步分析结果。

您必须具有 Excel 2013 或 2010 安装完成本教程的这一部分。

**下载电源查询 Microsoft excel**

- Microsoft excel 从[Microsoft 下载中心](http://www.microsoft.com/download/details.aspx?id=39379)下载电源 Query 并安装它。

**HDInsight 数据导入**

1. 打开 Excel，并创建一个新工作簿。
3. 单击**电源查询**菜单，单击**自其他来源**，然后单击**从 Azure HDInsight**。

    ![Azure HDInsight 为打开的 Excel PowerQuery 导入菜单。][image-hdi-gettingstarted-powerquery-importdata]

3. 输入与群集，Azure Blob 存储帐户的**帐户名**，然后单击**确定**。 （这是先前在本教程中创建的存储帐户）。
4. Azure Blob 存储帐户，输入**帐户密码**，然后单击**保存**。
5. 在右窗格中，双击该 blob 名称。 默认情况下该 blob 名称是群集名称相同。

6. 在**名称**列中找到**标准输出**。 验证相应**文件夹路径**列中的 GUID 与先前复制的 GUID 相匹配。 匹配项表明输出数据对应于您提交的作业。 单击**二进制**列左侧的**标准输出**中。

    ![查找列表中的内容的 GUID 的数据输出。][image-hdi-gettingstarted-powerquery-importdata2]

9. 单击**关闭和负载**在左上角，以导入配置单元作业输出到 Excel 中。


##<a name="nextsteps"></a>下一步行动
在 Hadoop 教程中，您学习了如何配置 Hadoop 群集中 HDInsight，运行配置单元查询数据，并导入到 Excel 中，它们可以被进一步的结果处理和使用商业智能工具以图形方式显示在窗口上。 若要了解详细信息，请参阅下面的教程︰

- [开始使用 Visual Studio 的 HDInsight Hadoop 工具][1]
- [HDInsight 仿真程序入门][hdinsight 仿真程序]
- [HDInsight 使用 Azure Blob 存储][hdinsight 存储]
- [管理 HDInsight 使用 PowerShell][hdinsight-管理 powershell]
- [上载到 HDInsight 的数据][hdinsight 上载数据]
- [使用 MapReduce 与 HDInsight][hdinsight-使用 mapreduce]
- [使用 HDInsight 蜂巢][配置单元使用 hdinsight-]
- [使用与 HDInsight 的小猪][使用猪的 hdinsight]
- [与 HDInsight 使用 Oozie][hdinsight-使用 oozie]
- [Hadoop 开发 C# HDInsight 的流式处理程序][hdinsight 开发流]
- [HDInsight 的开发 Java MapReduce 程序][hdinsight 开发 mapreduce]


[1]: ../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md

[hdinsight 版本]: hdinsight-component-versioning.md


[hdinsight 规定]: hdinsight-provision-clusters.md
[hdinsight-管理 powershell]: hdinsight-administer-use-powershell.md
[hdinsight 上载数据]: hdinsight-upload-data.md
[hdinsight-使用 mapreduce]: hdinsight-use-mapreduce.md
[配置-单元使用 hdinsight]: hdinsight-use-hive.md
[使用猪的 hdinsight]: hdinsight-use-pig.md
[hdinsight-使用 oozie]: hdinsight-use-oozie.md
[hdinsight 存储]: hdinsight-hadoop-use-blob-storage.md
[hdinsight 仿真程序]: hdinsight-hadoop-emulator-get-started.md
[hdinsight 开发流]: hdinsight-hadoop-develop-deploy-streaming-jobs.md
[hdinsight 开发 mapreduce]: hdinsight-develop-deploy-java-mapreduce.md
[hadoop-hdinsight-简介]: hdinsight-hadoop-introduction.md
[网络日志的 hdinsight 示例]: hdinsight-hive-analyze-website-log.md
[hdinsight 传感器数据示例]: hdinsight-hive-analyze-sensor-data.md

[azure 的购买选项]: http://azure.microsoft.com/pricing/purchase-options/
[azure 的成员提供]: http://azure.microsoft.com/pricing/member-offers/
[azure 释放试验]: http://azure.microsoft.com/pricing/free-trial/
[azure 管理门户]: https://manage.windowsazure.com/
[azure 创建 storageaccount]: ../storage-create-storage-account.md

[apache hadoop]: http://go.microsoft.com/fwlink/?LinkId=510084
[apache 配置单元]: http://go.microsoft.com/fwlink/?LinkId=510085
[apache mapreduce]: http://go.microsoft.com/fwlink/?LinkId=510086
[apache hdfs]: http://go.microsoft.com/fwlink/?LinkId=510087
[hdinsight — hbase-自定义配置]: hdinsight-hbase-tutorial-get-started.md


[powershell 下载]: http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409
[powershell 安装配置]: ../install-configure-powershell.md
[powershell 开放]: ../install-configure-powershell.md#Install


[img 的 hdi 的仪表板]: ./media/hdinsight-hadoop-tutorial-get-started-windows-v1/HDI.dashboard.png
[img 的 hdi 的仪表板的查询的选择]: ./media/hdinsight-hadoop-tutorial-get-started-windows-v1/HDI.dashboard.query.select.png
[img-hdi-dashboard-query-select-result]: ./media/hdinsight-hadoop-tutorial-get-started-windows-v1/HDI.dashboard.query.select.result.png
[img-hdi-dashboard-query-select-result-output]: ./media/hdinsight-hadoop-tutorial-get-started-windows-v1/HDI.dashboard.query.select.result.output.png
[img-hdi-dashboard-query-browse-output]: ./media/hdinsight-hadoop-tutorial-get-started-windows-v1/HDI.dashboard.query.browse.output.png

[img hdi getstarted 视频]: ./media/hdinsight-hadoop-tutorial-get-started-windows-v1/hdi-get-started-video.png


[图像 hdi-storageaccount quickcreate]: ./media/hdinsight-hadoop-tutorial-get-started-windows-v1/HDI.StorageAccount.QuickCreate.png
[图像的 hdi-clusterstatus]: ./media/hdinsight-hadoop-tutorial-get-started-windows-v1/HDI.ClusterStatus.png
[图像的 hdi-quickcreatecluster]: ./media/hdinsight-hadoop-tutorial-get-started-windows-v1/HDI.QuickCreateCluster.png
[图像 hdi getstarted 流]: ./media/hdinsight-hadoop-tutorial-get-started-windows-v1/HDI.GetStartedFlow.png

[图像的 hdi-gettingstarted-powerquery-导入数据]: ./media/hdinsight-hadoop-tutorial-get-started-windows-v1/HDI.GettingStarted.PowerQuery.ImportData.png
[图像的 hdi-gettingstarted-powerquery-importdata2]: ./media/hdinsight-hadoop-tutorial-get-started-windows-v1/HDI.GettingStarted.PowerQuery.ImportData2.png
 
测试

---
ms.openlocfilehash: d675c10ba898fd2eb7dc8a5986994c3c2955df18
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="了解如何使用 Visual Studio 的 Hadoop 工具 HDInsight |Microsoft Azure"
    description="了解如何安装和使用 Visual Studio 的 Hadoop HDInsight 工具连接到 Hadoop 群集并运行配置单元查询。"
    keywords="hadoop 工具 visual studio 在配置单元查询"
    services="HDInsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="paulettm"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="07/28/2015"
    ms.author="jgao"/>

# 开始使用 Visual Studio 的 Hadoop HDInsight 工具运行配置单元查询

了解如何使用 Visual Studio 工具 HDInsight HDInsight 群集连接并提交查询，配置单元。 有关使用 HDInsight 的详细信息，请参阅[简介 HDInsight][hdinsight.introduction]和[入门 HDInsight][hdinsight.get.started]。 有关连接到风暴群集的详细信息，请参见[使用 Visual Studio 的 HDInsight 上的 Apache 风暴的拓扑开发 C#][hdinsight.storm.visual.studio.tools]。

>[AZURE.NOTE]最新版本引入了一些新的功能，如配置单元编辑器支持、 配置单元脚本本地验证和 YARN 登录访问。


## 先决条件

若要完成本教程，并在 Visual Studio 中使用 Hadoop 的工具，您需要︰

- Azure HDInsight 群集︰ 基于 Linux 的或基于 Windows 群集将使用本文中的步骤操作。 创建群集中看到以下信息之一︰

    - [开始使用基于 Linux 的 HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md)
    - [开始使用基于 Windows 的 HDInsight](hdinsight-hadoop-tutorial-get-started-windows.md)

- 具有以下软件的工作站︰

    - Windows 8.1、 Windows 8 或 Windows 7
    - Visual Studio （以下版本之一）︰
        - Visual Studio 2012 专业/高级/旗舰版与[更新 4](http://www.microsoft.com/download/details.aspx?id=39305)
        - Visual Studio 2013年社区/专业/高级/旗舰版与[更新 4](https://www.microsoft.com/download/details.aspx?id=44921)
        - Visual Studio 2015 年 （社区/企业）

    >[AZURE.NOTE] 目前，用于 Visual Studio 的 HDInsight 工具仅附带的英文版。


## Visual Studio 安装 HDInsight 工具

Visual Studio 的 HDInsight 工具是打包与 Microsoft Azure SDK 的.NET 2.5.1 版或更高版本。 可以通过使用[Web 平台安装程序](http://go.microsoft.com/fwlink/?LinkId=255386)来安装它。 您必须选择匹配您的 Visual Studio 版本。 此外将此 Hadoop 工具程序包安装 Microsoft 配置单元的 ODBC 驱动程序 （32 位和 64 位）。

![Hadoop 工具︰ HDinsight Visual Studio Web 平台安装程序工具。][1]


>[AZURE.NOTE] 如果您有 Visual Studio 2015年或 2012 年，并且安装了 Azure SDK 2.5，必须安装最新版本之前手动删除较旧的版本。 Visual Studio 2013年支持直接更新。

## 连接到 Azure 的订阅
Visual Studio 的 HDInsight 工具使您可以连接到您的 HDInsight 群集，执行一些基本的管理操作，并运行配置单元查询。

>[AZURE.NOTE] 有关连接到 HDInsight 仿真程序的信息，请参阅[入门 HDInsight 仿真程序](../hdinsight-get-started-emulator.md/#vstools)。

>[AZURE.NOTE] 有关连接到通用的 Hadoop 群集 （预览） 的信息，请参阅[编写并提交配置单元查询使用 Visual Studio](http://blogs.msdn.com/b/xiaoyong/archive/2015/05/04/how-to-write-and-submit-hive-queries-using-visual-studio.aspx)。


**若要连接到 Azure 订阅**

1.  打开 Visual Studio。
2.  从**视图**菜单上，单击**服务器资源管理器**打开服务器资源管理器窗口。
3.  展开**Azure**，然后再展开**HDInsight**。

    >[AZURE.NOTE]注意到应打开**HDInsight 任务列表**窗口。 如果您看不到它，从**视图**菜单上，单击**其他窗口**，然后单击**HDInsight 任务列表窗口**。  
4.  Azure 订阅您的凭据，请输入，然后单击**登录**。 这只是要求如果您从未连接到 Azure 预订从 Visual Studio 在此工作站上。
5.  在服务器资源管理器中，您将看到现有 HDInsight 群集的列表。 如果您没有任何群集，可以提供一个使用 Azure 预览门户，Azure PowerShell 或 HDInsight SDK。 有关详细信息，请参阅[群集调配 HDInsight][hdinsight 规定]。

    ![Hadoop 工具︰ HDInsight 工具 Visual Studio 的服务器资源管理器中的群集列表][5]
6.  展开一个 HDInsight 的群集。 您会看到**配置单元数据库**、 默认存储帐户、 链接的存储帐户和**Hadoop 服务日志**。 您可以进一步扩展实体。

您已连接到 Azure 订阅后，您可以执行以下︰

**从 Visual Studio 连接到 Azure 门户**

- 从服务器资源管理器中，展开**Azure** > **HDInsight**，HDInsight 群集中，用鼠标右键单击，然后单击**在 Azure 门户管理群集**。

**要提出问题，并从 Visual Studio 提供反馈**

- 从**工具**菜单上，单击**HDInsight**，然后单击**MSDN 论坛**提出问题，或单击**提供反馈**。

## 导航链接的资源

从服务器资源管理器，您可以看到默认的存储帐户和任何链接的存储帐户。 如果您扩展默认存储帐户，您可以看到容器存储帐户上。 默认的存储帐户和默认容器标记。 您还可以右击任何容器以查看内容。

![HDInsight 工具 Visual Studio 的服务器资源管理器中的群集列表][2]

## 运行配置单元查询
[Apache 配置单元][apache.hive]是一个基于 Hadoop 提供数据汇总、 查询和分析的数据仓库基础结构。 Visual Studio 的 HDInsight 工具支持 Visual Studio 中的运行配置单元查询。 有关配置单元的详细信息，请参阅[使用配置单元 HDInsight 与][hdinsight.hive]。

它是花费很多时间来测试对 HDInsight 群集配置单元脚本。 它可能需要几分钟或更多。 Visual Studio 的 HDInsight 工具都能够本地验证脚本配置单元，而无需连接到活动群集。

HDInsight 工具 Visual Studio 还使用户能够看到该配置单元作业通过收集和曲面的某些配置单元作业 YARN 日志。

### 查看**hivesampletable**
所有 HDInsight 群集都附带一个名为*hivesampletable*的示例配置单元表。 我们将使用此表来向您展示如何列出配置单元表，查看表的架构，并列出配置单元表中的行。



**列出配置单元表并查看配置单元表架构**

1.  从**服务器资源管理器中**，展开**Azure** > **HDInsight** > 您所选的群集 >**配置单元数据库** > **默认** > **hivesampletable**来查看表架构。
4.  **Hivesampletable**，用鼠标右键单击，然后单击**视图顶部 100 行**列出这些行。 它等效于运行以下配置单元查询使用配置单元的 ODBC 驱动程序︰

        SELECT * FROM hivesampletable LIMIT 100

    您可以自定义的行计数。

    ![Hadoop 工具︰ HDinsight 配置单元 Visual Studio 架构查询][6]

### 创建配置单元表

您可以使用 GUI 创建配置单元表或使用配置单元查询。 有关使用配置单元查询的信息，请参阅[运行配置单元查询](#run.queries)。

**若要创建配置单元表**

1. 从**服务器资源管理器中**，展开**Azure** > **HDInsight 群集**HDInsight 群集 >**配置单元数据库**，然后用鼠标右键单击**默认值**，并单击**创建表**。
2. 配置表。
3. 单击**创建表**提交作业以创建新的配置单元表。

    ![Hadoop 工具︰ hdinsight visual studio 工具创建配置单元表][7]

### <a name="run.queries"></a>验证和运行配置单元查询
有两种方法来创建和运行查询配置单元︰

- 创建临时的查询
- 创建配置单元的应用程序

**若要创建、 验证和运行 ad hoc 查询**

1. 从**服务器资源管理器中**，展开**Azure**，然后再展开**HDInsight 群集**。
2. 右键单击要用来运行该查询，的群集，然后单击**写入配置单元查询**。
3. 输入的配置单元查询。 注意配置单元编辑器支持 IntelliSense。 Visual Studio 的 HDInsight 工具支持编辑配置单元脚本时加载远程的元数据。 例如，当您键入"选择 * 发件人"，IntelliSense 将列出所有建议的表名称。 指定一个表名时，IntelliSense 按列出的列名称。 该工具支持几乎所有的配置单元 DML 语句、 子查询和内置的 Udf。

    ![Hadoop 工具︰ HDInsight Visual Studio 工具 IntelliSense][13]

    ![Hadoop 工具︰ HDInsight Visual Studio 工具 IntelliSense][14]

    > [AZURE.NOTE] 将建议只有元数据的群集在 HDInsight 工具栏中选择。
4. （可选）︰ 单击**验证脚本**检查脚本的语法错误。

    ![Hadoop 工具︰ hdinsight Visual Studio 本地验证工具][10]

4. 单击**提交**或**提交 （高级）**。 使用高级的提交选项，您将脚本配置**作业名称**、**参数**、**其他配置**和**状态目录**︰

    ![hdinsight hadoop 配置单元查询][9]

    提交作业后，您会看到**配置单元作业摘要**窗口。

    ![HDInsight Hadoop 配置单元查询摘要][8]
5. 使用**刷新**按钮来更新状态，直到作业状态更改为**已完成**。
6. 单击底部才能看到下面的链接︰**查询作业**、**作业输出**、**作业日志**或**Yarn 日志**。



**若要创建并运行一个配置单元的解决方案**

1. 从**文件**菜单上，单击**新建**，然后单击**项目**。
2. 从左窗格中选择**HDInsight** ，中间窗格中，选择**配置单元的应用程序**、 输入属性，然后单击**确定**。

    ![Hadoop 工具︰ hdinsight visual studio 工具新建配置单元项目][11]
3. 从**解决方案资源管理器中**，双击**Script.hql**以将其打开。
4. 若要验证该配置单元脚本，可以单击**验证脚本**按钮，或右键单击配置单元编辑器中的脚本，然后从上下文菜单中单击**验证脚本**。


### 查看配置单元作业
您可以查看作业的查询、 作业输出、 作业日志和配置单元作业 Yarn 日志。 有关详细信息，请参阅上面的屏幕快照。

该工具的最新版本可以看得到里面的配置单元作业通过收集和曲面 YARN 日志。 YARN 日志可以帮助您调查性能问题。 HDInsight 如何收集 YARN 日志的详细信息，请参阅[访问 HDInsight 应用程序日志以编程方式][hdinsight.access.application.logs]。

**若要查看配置单元作业**

1. 从**服务器资源管理器中**，展开**Azure**，然后再展开**HDInsight**。
2. HDInsight 群集中，用鼠标右键单击，然后单击**查看配置单元作业**。 您将看到已运行在群集配置单元作业的列表。
3. 作业在作业列表中选择它，请单击，然后使用**配置单元作业摘要**窗口打开**查询作业**、**作业输出**、**作业日志**或**Yarn 日志**。

    ![Hadoop 工具︰ HDInsight Visual Studio 工具查看配置单元作业][12]

### Tez 配置单元作业性能图

显示配置单元作业的性能曲线图的 HDInsight Visual Studio 工具支持运行 Tez 执行引擎。 有关启用 Tez 的信息，请参阅[使用 HDInsight 中的配置单元][hdinsight.hive]。 Visual Studio 中的配置单元作业提交后，Visual Studio 显示关系图作业完成时。  您可能需要单击**刷新**按钮以获得最新的作业状态。

> [AZURE.NOTE] 此功能的 HDInsight 3.2.4.593，上面的群集版本才可使用，并可以只适用于已完成的作业。 这适用于这两个窗口和基于 Linux 的群集。

![hadoop 配置单元 tez 性能图](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.hive.tez.performance.graph.png)

## 运行 Pig 脚本

Visual Studio 的 HDInsight 工具支持创建和提交到 HDInsight 群集 Pig 脚本。 用户可以创建模板，请从猪的项目，然后提交到 HDInsight 群集脚本。

## 下一步行动
在本文中，您学习了如何连接到 HDInsight 群集从 Visual Studio，使用 Hadoop 工具包，以及如何运行配置单元查询。 有关详细信息，请参阅︰

- [在 HDInsight 中使用 Hadoop 配置单元][hdinsight.hive]
- [开始使用 Hadoop 在 HDInsight][hdinsight.get.started]
- [在 HDInsight 中的提交 Hadoop 作业][hdinsight.submit.jobs]
- [使用 Hadoop 在 HDInsight 中使用 Twitter，分析数据][hdinsight.analyze.twitter.data]


<!--Anchors-->
[安装]: #installation
[连接到 Azure 订阅]: #connect-to-your-azure-subscription
[导航链接的资源]: #navigate-the-linked-resources
[运行配置单元查询]: #run-hive-queries
[下一步行动]: #next-steps

<!--Image references-->
[1]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.wpi.png
[2]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.linked.resources.png
[5]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.server.explorer.png
[6]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.hive.schema.png
[7]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.create.hive.table.png
[8]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.run.hive.job.summary.png
[9]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.submit.jobs.advanced.png
[10]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.validate.hive.script.png
[11]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.new.hive.project.png
[12]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.view.hive.jobs.png
[13]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.intellisense.table.names.png
[14]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.intellisense.column.names.png


<!--Link references-->
[hdinsight 规定]: ../hdinsight/hdinsight-provision-clusters.md
[hdinsight.introduction]: ../hdinsight-introduction.md
[hdinsight.get.started]: ../hdinsight-get-started.md
[hdinsight.hive]: ../hdinsight/hdinsight-use-hive.md
[hdinsight.submit.jobs]: ../hdinsight/hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight.analyze.twitter.data]: ../hdinsight/hdinsight-analyze-twitter-data.md
[hdinsight.storm.visual.studio.tools]: ../hdinsight/hdinsight-storm-develop-csharp-visual-studio-topology.md
[hdinsight.access.application.logs]: ../hdinsight/hdinsight-hadoop-access-yarn-app-logs.md

[apache.hive]: http://hive.apache.org

测试

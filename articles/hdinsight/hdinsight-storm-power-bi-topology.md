---
ms.openlocfilehash: 2a1fc07f0394eae64746321533599706bf4447e6
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
 pageTitle="数据写入从 Apache 冲击电源 BI |Microsoft Azure"
 description="数据写入 HDInsight 在 Apache 风暴群集上运行的 C# 拓扑电源 BI。 此外，创建一份报告和使用双电源的实时控制板。"
 services="hdinsight"
 documentationCenter=""
 authors="Blackmist"
 manager="paulettm"
 editor="cgronlun"
    tags="azure-portal"/>

<tags
 ms.service="hdinsight"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="big-data"
 ms.date="07/17/2015"
 ms.author="larryfr"/>

# 电源 BI （预览） 用于可视化的 Apache 风暴拓扑结构中的数据

电源 BI 预览功能允许您以可视方式将数据显示为报表或面板。 使用电源 BI REST API，您可以轻松地使用数据从电源 BI HDInsight 群集上的 Apache 风暴上运行一个拓扑结构。

在本文中，您将学习如何使用电源双风暴拓扑所创建的数据创建报告和仪表板。

## 先决条件

- Azure 的订阅。 请参阅[获取 Azure 免费试用版](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)。

* 具有[电源双向](https://powerbi.com)访问的 Azure Active Directory 用户

* Visual Studio （以下版本之一）

    * Visual Studio 2012 与[更新 4](http://www.microsoft.com/download/details.aspx?id=39305)

    * Visual Studio 2013 年[更新 4](http://www.microsoft.com/download/details.aspx?id=44921)或[Visual Studio 2013年社区](http://go.microsoft.com/fwlink/?linkid=517284&clcid=0x409)

    * Visual Studio 2015 [CTP6](http://visualstudio.com/downloads/visual-studio-2015-ctp-vs)

* Visual Studio HDInsight 工具︰ 信息，请参阅[开始使用 Visual Studio 的 HDInsight 工具](../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md)上安装的信息。

## 它的工作原理

该示例包含一个 C# 风暴拓扑随机生成句子、 拆分成几个单词的句子，计数的单词，并将与计数发送到 BI REST API，电源。 [PowerBi.Api.Client](https://github.com/Vtek/PowerBI.Api.Client) Nuget 程序包用于与电源双向通信。

此项目中的下列文件实现的电源 BI 特定功能︰

* **PowerBiBolt.cs**︰ 实现冲击螺栓，将数据发送到电源 BI

* **Data.cs**︰ 描述数据对象/行，将发送给电源 BI

> [AZURE.WARNING] 电源双似乎允许创建具有相同名称的多个数据集。 如果数据集不存在，并且您的拓扑创建电源双螺栓的多个实例，这会发生。 若要避免此问题，将螺栓的并行提示设置为 1 （像本示例一样，） 或部署拓扑之前创建的数据集。
>
> 举例说明如何创建数据集拓扑之外提供了此解决方案中包含的**CreateDataset**控制台应用程序。

## 将电源 BI 应用程序注册

1. 请按照注册电源双[电源双向快速入门](https://msdn.microsoft.com/en-US/library/dn931989.aspx)中的步骤操作。

2. 请按照[注册应用程序](https://msdn.microsoft.com/en-US/library/dn877542.aspx)创建一个应用程序的注册。 这将使用访问电源 BI REST API 时。

    > [AZURE.IMPORTANT] 保存应用程序注册**的客户端 ID** 。

## 下载示例

下载[HDInsight 电源 BI C# 风暴的示例](https://github.com/Blackmist/hdinsight-csharp-storm-powerbi)。 若要下载它，分叉/克隆使用[git](http://git-scm.com/)，或者使用的**下载**链接以下载.zip 存档。

## 配置示例

1. 在 Visual Studio 中打开示例。 从**解决方案资源管理器**，打开**SCPHost.exe.config**文件，然后查找**< OAuth.../ >**元素。 为此元素的以下属性输入值。

    * **客户**︰ 您在前面创建的应用程序注册的客户端 ID。

    * **用户**︰ Azure Active Directory 帐户有权访问电源 BI。

    * **密码**︰ Azure Active Directory 帐户的密码。

2. （可选）。 此项目所使用的默认数据集名称是**词**。 若要更改此设置，右键单击**解决方案资源管理器**中的**字数统计**项目，选择**属性**，然后选择**设置**。 将**数据集名称**条目更改为所需的值。

2. 保存并关闭该文件。

## 部署示例

1. 从**解决方案资源管理器中**，用鼠标右键单击**字数统计**项目并选择**提交到 HDInsight 上的冲击**。 从**风暴群集**下拉列表对话框中选择 HDInsight 群集。

    > [AZURE.NOTE] 它可能需要数秒钟，**风暴群集**下拉列表来填充的服务器名称。
    >
    > 如果出现提示，请输入 Azure 订阅的登录凭据。 如果您有多个订阅，登录到包含您在 HDInsight 群集上的冲击。

2. 已成功提交拓扑结构，应显示为群集风暴的拓扑。 从列表以查看有关正在运行的拓扑信息选择 WordCount 拓扑。

    ![具有选定的 WordCount 拓扑拓扑](./media/hdinsight-storm-power-bi-topology/topologysummary.png)

    > [AZURE.NOTE] 您还可以查看从服务器资源管理器的电流冲击拓扑︰ 展开 Azure，HDInsight，风暴在 HDInsight 群集上，用鼠标右键单击，然后选择视图风暴拓扑。

3. 滚动查看**拓扑摘要**，直到您看到**螺栓**部分。 在本部分中，记下**PowerBI**螺栓的**执行**列。 在页的顶部使用刷新按钮刷新之前的值更改为零。 当此数量开始增加时，它指示正在写入到电源 BI 项目。

## 创建报表

1. 在浏览器中访问[https://PowerBI.com](https://powerbi.com)。 登录您的帐户。

2. 在该页的左侧，展开**数据集**。 选择的**词**项。 这是数据集创建的示例拓扑。

    ![单词数据集项](./media/hdinsight-storm-power-bi-topology/words.png)

3. 在**字段**区域中，展开**字数统计**。 将**计数**和**字**条目拖到该页面的中间部分。 这将创建新的图表，其中显示的栏的每个字表明该单词出现的次数。

    ![字数统计图表](./media/hdinsight-storm-power-bi-topology/wordcountchart.png)

4. 从左上角的页上，选择**保存**以创建新的报表。 输入报表的名称**字数统计**。

5. 选择要返回到仪表板的电源 BI 徽标。 **字数统计**报表现在显示在**报告**下。

## 创建活动仪表板

1. 旁边**的仪表板**，选择**+**图标可创建新的仪表板。 命名新的仪表板**实时字数统计**。

2. 选择您在前面创建的**字数统计**报告。 显示时，选择该图表，然后选择该图表的右上角的图钉图标。 您应该收到通知它被固定到仪表板。

    ![用图钉显示图表](./media/hdinsight-storm-power-bi-topology/pushpin.png)

2. 选择要返回到仪表板的电源 BI 徽标。 选择**实时字数统计**仪表板。 它包含的字数统计图表，并从 HDInsight 上运行 WordCount 拓扑结构图表将自动更新为新条目发送到电源 BI。

    ![活动仪表板](./media/hdinsight-storm-power-bi-topology/dashboard.png)

## 停止的 WordCount 拓扑

拓扑结构将继续运行，直到您停止它或删除 HDInsight 群集上的风暴。 执行以下步骤以停止拓扑结构。

1. 在 Visual Studio 中，打开字数统计拓扑结构的**拓扑摘要**窗口。 如果拓扑摘要尚未打开，转到**服务器资源管理器**，展开的**Azure**和**HDInsight**项、 HDInsight 群集上风暴右击，选择**视图风暴拓扑**。 最后，选择的**字数统计**拓扑。

2. 选择**取消**按钮停止**WordCount**拓扑。

    ![取消按钮上拓扑图摘要](./media/hdinsight-storm-power-bi-topology/killtopology.png)

## 下一步行动

在本文中，您学习了如何从风暴拓扑结构的数据发送到使用其余的电源 BI。 有关如何使用其他 Azure 的技术信息，请参阅以下信息︰

* [在 HDInsight 上的风暴的示例拓扑](hdinsight-storm-example-topology.md)

测试

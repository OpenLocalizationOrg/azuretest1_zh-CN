---
ms.openlocfilehash: 1bfe186977c776853a934c294ab255a757506d94
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="将 Excel 连接到 Hadoop 使用电源查询 |Microsoft Azure"
    description="了解如何利用商业智能组件并使用 Excel 来访问数据存储在 HDInsight 上的 Hadoop 电源查询。"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="paulettm"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/09/2015"
    ms.author="jgao"/>


#Excel 使用连接到 Hadoop 的电源查询

Microsoft 的大数据解决方案的一个关键特征是使用 Hadoop 群集在 Azure HDInsight Microsoft 商业智能 (BI) 组件的集成。 这种集成的一个主要例子是将 Excel 连接到 Azure 存储帐户包含 Excel 外接使用 Query 电源与 Hadoop 群集关联的数据的能力。 本文讲解如何设置和使用电源查询与 Hadoop 群集管理与 HDInsight 相关联的查询数据。

> [AZURE.NOTE] 虽然这篇文章中的步骤可以用的 Linux 或基于 Windows HDInsight 群集，Windows 是所必需的客户端工作站。

## 先决条件

在开始这篇文章之前，您必须具有以下︰

- **HDInsight 群集**。 一个配置，请参阅[开始使用 Azure HDInsight][hdinsight 获取启动]。
- **一个工作站**运行 Windows 7，Windows Server 2008 R2 或更高版本的操作系统。
- **办公室 2013年专业加号，Office 365 ProPlus，Excel 2013 独立或 Office 2010 专业版 +**。


## <a id="InstallPowerQuery"></a>安装电源 Microsoft excel 的查询

电源查询可以用于从各种来源到 Microsoft Excel 中，它可以打开电源如 PowerPivot 和 Power View 的 BI 工具导入数据。 特别是，电源查询可以导入的具有已输出或在 HDInsight 群集上运行 Hadoop 作业已生成的数据。

从[Microsoft 下载中心][下载 powerquery] excel 下载电源 Query 并安装它。

## <a id="ImportData"></a>HDInsight 数据导入 Excel

电源查询加载项以 Excel 很容易从 HDInsight 群集导入 Excel，其中 BI 工具如 PowerPivot 和电源映射可用于检查，分析，和将数据导入数据。

**从 HDInsight 群集导入数据**

1. 打开 Excel。

2. 创建一个新的空白工作簿。

3. 单击**电源查询**菜单，单击**自其他来源**，然后单击**从 Azure HDInsight**。

    ![HDI。PowerQuery.SelectHdiSource][image-hdi-powerquery-hdi-source]

    注意︰ 如果看不到**电源查询**菜单，请转到**文件** > **选项** > **加载项**，并选择**COM 加载**页面底部的下拉列表**管理**中。 选择**转到...**按钮，验证已选中了针对 Excel 外接电源查询框。

3. **帐户名**，输入与您的群集，Azure Blob 存储帐户的名称，然后单击**确定**。

4. **帐户密码**，输入的密钥 Blob 存储帐户，然后单击**保存**。 （您需要这样做才第一次访问该存储区。

5. 在查询编辑器的左侧**导航**窗格中，双击 Blob 存储容器名称。 默认情况下，容器名称是群集名称相同的名称。

6. （文件夹路径是**...**名称**列中找到**HiveSampleData.txt**/ 配置单元/仓库/hivesampletable/**)，然后单击**二进制**HiveSampleData.txt 左侧。

    ![HDI。PowerQuery.ImportData][image-hdi-powerquery-importdata]

7. 如果需要，您可以重命名列的名称。 准备就绪后，单击**应用和关闭**。

    ![HDI。PowerQuery.ImportedTable][image-hdi-powerquery-imported-table]

## <a id="NextSteps"></a>下一步行动

在本文中，您学习了如何使用电源查询来从 HDInsight 中检索数据导入 Excel。 同样，可以从 HDInsight 到 Azure SQL 数据库检索数据。 就还可以将数据上传到 HDInsight。 若要了解详细信息，请参阅下列文章︰

* [连接到 Microsoft 配置单元 ODBC 驱动程序的 HDInsight 的 Excel][hdinsight ODBC]
* [上载到 HDInsight 的数据][hdinsight 上载数据]

[hdinsight ODBC]: hdinsight-connect-excel-hive-ODBC-driver.md
[hdinsight--入门]: ../hdinsight-get-started.md
[hdinsight 上载数据]: hdinsight-upload-data.md

[图像的 hdi-powerquery-hdi 的源]: ./media/hdinsight-connect-excel-power-query/HDI.PowerQuery.SelectHdiSource.png
[图像的 hdi-powerquery-导入数据]: ./media/hdinsight-connect-excel-power-query/HDI.PowerQuery.ImportData.png
[图像的 hdi-powerquery-导入的表]: ./media/hdinsight-connect-excel-power-query/HDI.PowerQuery.ImportedTable.PNG

[powerquery 下载]: http://go.microsoft.com/fwlink/?LinkID=286689

测试

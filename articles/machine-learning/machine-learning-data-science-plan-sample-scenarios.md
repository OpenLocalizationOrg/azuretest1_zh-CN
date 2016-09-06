---
ms.openlocfilehash: dfef880678558552a542d63b71bdcac38a50ec82
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="高级分析功能的方案流程和在 Azure 的机器学习技术 |Microsoft Azure"
    description="在 Azure 机器学习中选择高级预测分析过程的适当方案。"
    services="machine-learning"
    documentationCenter=""
    authors="msolhab"
    manager="paulettm"
    editor="" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na" 
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/22/2015"
    ms.author="msolhab;bradsev" />


# 在 Azure 机器学习中的高级分析功能的方案

这篇文章概括介绍了样本数据源和目标方案可与高级分析过程和技术 （调整） 在 Azure 机器学习来处理的各种。 它演示了处理序列依赖于数据的特点、 源位置和目标在 Azure 中的存储库中可用的选项。

**决策树中**选择示例方案适合于您的数据和目标也会出现在最后一节。

>[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


以下各部分提供了一个示例方案。 对于每个方案，一个可能的数据科学或高级分析功能流和支持 Azure 的资源列出。

>[AZURE.NOTE] **对于所有以下情况，您需要︰**

*   [创建存储帐户。](storage-whatis-account.md)
*   [创建 Azure ML 区](machine-learning/machine-learning-create-workspace.md)




## <a name="smalllocal"></a>方案\#1︰ 小型到中型表格式本地文件中的数据集

![小型到中型的本地文件][1]

#### Azure 的其他资源︰ 无

1.  登录到[Azure 机器学习 Studio](https://studio.azureml.net/)。

2.  上载数据集。

3.  生成开始上载 dataset(s) Azure 机器学习实验流程。

## <a name="smalllocalprocess"></a>方案\#2︰ 小到中型数据集需要处理的本地文件

![小型到中型的本地文件处理][2]

#### Azure 的其他资源︰ Azure 的虚拟机 （IPython 笔记本服务器）

1.  创建运行 IPython 笔记本 Azure 虚拟机。

2.  将数据上载到 Azure 存储容器。

3.  预先处理，清洁 IPython 笔记本，从 Azure 存储容器访问数据中的数据。

4.  转换数据清洗，表格形式。

5.  将转换后的数据保存在 Azure 的 blob。

6.  登录到[Azure 机器学习 Studio](https://studio.azureml.net/)。

7.  从 Azure blob 使用[读取器][读取器]模块中读取数据。

8. 生成启动与 ingested dataset(s) Azure 机器学习实验流程。

## <a name="largelocal"></a>方案\#3︰ 大型数据集的本地文件，针对 Azure Blob

![大型的本地文件][3]

#### Azure 的其他资源︰ Azure 的虚拟机 （IPython 笔记本服务器）

1.  创建运行 IPython 笔记本 Azure 虚拟机。

2.  将数据上载到 Azure 存储容器。

3.  预先处理，清洁 IPython 笔记本，访问从 Azure blob 数据中的数据。

4.  如果需要，转换数据清洗，表格形式。

5.  浏览数据，并根据需要创建功能。

6.  小到中型数据样本中提取。

7.  将采样的数据保存在 Azure 的 blob。

8. 登录到[Azure 机器学习 Studio](https://studio.azureml.net/)。

9. 从 Azure blob 使用[读取器][读取器]模块中读取数据。

10. 生成开始 ingested dataset(s) Azure ML 实验流程。


## <a name="smalllocaltodb"></a>方案\#4︰ 小到中型数据集的本地文件，针对 SQL Server 在 Azure Virtal 机

![小型到中型本地文件复制到 SQL Azure 中的数据库][4]

#### Azure 的其他资源︰ Azure 的虚拟机 (SQL Server / IPython 笔记本服务器)

1.  创建运行 SQL Server + IPython 笔记本 Azure 虚拟机。

2.  将数据上载到 Azure 存储容器。

3.  预先处理，清洁 IPython 笔记本使用 Azure 存储容器中的数据。

4.  如果需要，转换数据清洗，表格形式。

5.  将数据保存到 VM 本地文件 （在虚拟机上运行 IPython 笔记本、 本地驱动器，请参阅虚拟机驱动器）。

6.  将数据加载到 Azure 虚拟机上运行的 SQL Server 数据库。

    一。  选项\#1︰ 使用 SQL Server 管理 Studio。

        i.  Login to SQL Server VM
        ii. Run SQL Server Management Studio.
        iii. Create database and target tables.
        iv. Use one of the bulk import methods to load the data from VM-local files.

    b。  选项\#2︰ 使用 IPython 笔记本-不建议为中型和较大的数据集

        i.  Use ODBC connection string to access SQL Server on VM.
        ii. Create database and target tables.
        iii. Use one of the bulk import methods to load the data from VM-local files.

7.  浏览数据，根据需要创建功能。 请注意，功能不需要在数据库表中具体化。 只注意必要的查询来创建它们。

8. 如果需要并且/或者需要，确定大小的数据的示例。

9. 登录到[Azure 机器学习 Studio](https://studio.azureml.net/)。

10. 直接从 SQL Server 使用的[读取器][读取器]模块中读取数据。 粘贴所需查询提取字段、 造成功能，如果需要[读取器][读取器]查询中直接采样数据。

11. 生成开始 ingested dataset(s) Azure ML 实验流程。

## <a name="largelocaltodb"></a>方案\#5︰ 大型数据集在本地文件中，针对 SQL Server 在 Azure VM

![大本地文件复制到 SQL Azure 中的数据库][5]

#### Azure 的其他资源︰ Azure 的虚拟机 (SQL Server / IPython 笔记本服务器)

1.  创建运行 SQL Server，IPython 笔记本服务器 Azure 虚拟机。

2.  将数据上载到 Azure 存储容器。

3.  （可选）预先处理和清理数据。

    一。  预先处理，清洁 IPython 笔记本，访问从 Azure blob 数据中的数据。

    b。  如果需要，转换数据清洗，表格形式。

    c。  将数据保存到 VM 本地文件 （在虚拟机上运行 IPython 笔记本、 本地驱动器，请参阅虚拟机驱动器）。

4.  将数据加载到 Azure 虚拟机上运行的 SQL Server 数据库。

    一。  登录到 SQL Server 虚拟机。

    b。  如果没有已保存的数据，请将从 Azure 存储容器数据文件下载到本地虚拟机文件夹。

    c。  运行 SQL Server 管理 Studio。

    d。  创建数据库和目标表。

    电子。  使用批量导入方法来加载数据。

    f。  如果需要表联接，创建索引来加速联接。

 > [AZURE.NOTE] 对于较大的数据规模的更快的加载，建议创建已分区的表和批量导入的数据并行。 有关详细信息，请参阅[SQL 分区表并行数据导入](machine-learning/machine-learning-data-science-parallel-load-sql-partitioned-tables.md)。

5.  浏览数据，根据需要创建功能。 请注意，功能不需要在数据库表中具体化。 只注意必要的查询来创建它们。

6.  如果需要并且/或者需要，确定大小的数据的示例。

7.  登录到[Azure 机器学习 Studio](https://studio.azureml.net/)。

8. 直接从 SQL Server 使用的[读取器][读取器]模块中读取数据。 粘贴所需查询提取字段、 造成功能，如果需要[读取器][读取器]查询中直接采样数据。

9. 简单的 Azure ML 试验流量开始上载的数据集

## <a name="largedbtodb"></a>方案\#6︰ 在 SQL Server 数据库上的 prem，针对 SQL Server 在 Azure 虚拟机的大型数据集

![大型 SQL 数据库上的 prem 向 SQL Azure 中的数据库][6]

#### Azure 的其他资源︰ Azure 的虚拟机 (SQL Server / IPython 笔记本服务器)

1.  创建运行 SQL Server，IPython 笔记本服务器 Azure 虚拟机。

2.  使用一个数据的导出方法，将数据从 SQL Server 导出到转储文件。

    一。  注意︰ 如果您决定将所有数据从上 prem 移动数据库，备用 （快） 方法将完整数据库移到 Azure 中的 SQL Server 实例。 跳过步骤来导出数据、 创建数据库，并加载/导入到目标数据库的数据和按照备选方法。

3.  转储文件上传到 Azure 存储容器。

4.  加载到 Azure 虚拟机上运行的 SQL Server 数据库的数据。

    一。  登录到 SQL Server 虚拟机。

    b。  从 Azure 存储容器的数据文件下载到本地虚拟机文件夹中。

    c。  运行 SQL Server 管理 Studio。

    d。  创建数据库和目标表。

    电子。  使用批量导入方法来加载数据。

    f。  如果需要表联接，创建索引来加速联接。

> [AZURE.NOTE] 提高加载速度较大的数据的大小，则可以创建已分区的表和批量导入的数据并行。 有关详细信息，请参阅[SQL 分区表并行数据导入](machine-learning/machine-learning-data-science-parallel-load-sql-partitioned-tables.md)。

5.  浏览数据，根据需要创建功能。 请注意，功能不需要在数据库表中具体化。 只注意必要的查询来创建它们。

6.  如果需要并且/或者需要，确定大小的数据的示例。

7.  登录到[Azure 机器学习 Studio](https://studio.azureml.net/)。

8. 直接从 SQL Server 使用的[读取器][读取器]模块中读取数据。 粘贴所需查询提取字段、 造成功能，如果需要[读取器][读取器]查询中直接采样数据。

9. 简单的 Azure ML 试验流量开始上载的数据集。

### 替代方法将完整数据库从本地 SQL Server 复制到 SQL Azure 数据库

![本地数据库中分离并将附加到 SQL Azure 中的数据库][7]

#### Azure 的其他资源︰ Azure 的虚拟机 (SQL Server / IPython 笔记本服务器)

复制 SQL Server VM 中的整个 SQL Server 数据库，您应该复制数据库从每个服务器一个位置到另一个，假设该数据库可将暂时处于脱机状态。 在 SQL Server 管理 Studio 对象资源管理器图形用户界面，或使用相同的事务处理 SQL 命令来执行此操作。

1. 分离数据库中的源位置。 有关详细信息，请参见[分离数据库](https://technet.microsoft.com/library/ms191491(v=sql.110).aspx)。
2. 在 Windows 资源管理器或 Windows 命令提示符窗口中，分离的数据库文件或文件和日志文件或文件复制到 Azure 中的 SQL Server VM 的目标位置中。
3. 将复制的文件附加到目标 SQL Server 实例。 有关详细信息，请参阅[附加数据库](https://technet.microsoft.com/library/ms190209(v=sql.110).aspx)。

[移动数据库使用分离和附加 (事务处理 SQL)](https://technet.microsoft.com/library/ms187858(v=sql.110).aspx)

## <a name="largedbtohive"></a>方案\#7︰ 大本地文件中的数据以在 Azure HDInsight Hadoop 群集配置单元数据库为目标

![本地目标配置单元中的重要数据][9]

#### Azure 的其他资源︰ Azure HDInsight Hadoop 群集和 Azure 虚拟机 （IPython 笔记本服务器）

1.  创建运行 IPython 笔记本服务器 Azure 虚拟机。

2.  创建 Azure HDInsight Hadoop 群集。

3.  （可选）预先处理和清理数据。

    一。  预先处理，清洁 IPython 笔记本，访问从 Azure blob 数据中的数据。

    b。  如果需要，转换数据清洗，表格形式。

    c。  将数据保存到 VM 本地文件 （在虚拟机上运行 IPython 笔记本、 本地驱动器，请参阅虚拟机驱动器）。

4.  将数据上载到的 Hadoop 群集在步骤 2 中选择的默认容器。

5.  将数据加载到 Azure HDInsight Hadoop 群集中配置单元数据库。

    一。  登录到 Hadoop 群集的头节点

    b。  打开 Hadoop 命令行。

    c。  输入命令的配置单元根目录`cd %hive_home%\bin`在 Hadoop 命令行。

    d。  运行配置单元查询来创建数据库和表，并将数据从 blob 存储加载配置单元表。

    > [AZURE.NOTE] 如果数据是重要的用户可以创建的配置单元表分区。 然后，用户可以使用`for`循环中 Hadoop 命令行在头节点上数据加载到配置单元表分区的分区。

6.  浏览数据并根据需要在 Hadoop 命令行创建功能。 请注意，功能不需要在数据库表中具体化。 只注意必要的查询来创建它们。

    一。  登录到 Hadoop 群集的头节点

    b。  打开 Hadoop 命令行。

    c。  输入命令的配置单元根目录`cd %hive_home%\bin`在 Hadoop 命令行。

    d。  浏览数据，并根据需要创建功能的 Hadoop 群集的头节点上运行的配置单元查询在 Hadoop 命令行。

7.  如果需要并且/或者需要，取样数据以适应在 Azure 机器学习 Studio。

8.  登录到[Azure 机器学习 Studio](https://studio.azureml.net/)。

9. 读取的数据直接从`Hive Queries`使用[读取器][读取器]模块。 粘贴所需查询提取字段、 造成功能，如果需要[读取器][读取器]查询中直接采样数据。

10. 简单的 Azure ML 试验流量开始上载的数据集。

## <a name="decisiontree"></a>方案选择的决策树
------------------------

下图总结了上述方案的高级分析过程和技术所做的每个明细的方案可供选择。 请注意，数据处理、 探索、 功能工程和取样可能需要放置在一个或多个方法/环境--中间，对源和/或目标环境--并可能继续根据需要反复进行。 图仅用作举例说明了一些可能的流，并不提供详尽的枚举。

![DS 过程演练方案示例][8]

### 高级分析操作示例

采用高级分析过程和使用公共数据集技术的端到端 Azure 机器学习演练，请参见︰


* [高级分析流程和技术操作︰ 使用 SQL Server](machine-learning/machine-learning-data-science-process-sql-walkthrough.md)。
* [高级分析流程和技术操作︰ 使用 HDInsight Hadoop 群集](machine-learning/machine-learning-data-science-process-hive-walkthrough.md)。


[1]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-small-in-aml.png
[2]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-with-processing.png
[3]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-large.png
[4]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-to-db.png
[5]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-large-to-db.png
[6]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-db-to-db.png
[7]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-attach-db.png
[8]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-sample-scenarios.png
[9]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-to-hive.png


<!-- Module References -->
[读取器]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/

测试

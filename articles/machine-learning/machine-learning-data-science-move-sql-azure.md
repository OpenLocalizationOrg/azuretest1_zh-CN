---
ms.openlocfilehash: ce129d95b8155b1d420bf36bc47e3423e0332808
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="将 Azure 机器学习数据移到 Azure SQL 数据库 |Azure" 
    description="创建 SQL 表并加载到 SQL 表的数据" 
    metaKeywords="" 
    services="machine-learning" 
    solutions="" 
    documentationCenter="" 
    authors="fashah" 
    manager="jacob.spoelstra" 
    editor="" 
    videoId="" 
    scriptId="" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/10/2015" 
    ms.author="fashah;bradsev" /> 

# 将数据移到 Azure SQL Azure 机器学习数据库


本主题中，我们概述从平面文件 （CSV 或 TSV 格式） 或数据存储到 SQL Azure 数据库后端 SQL Server 在移动数据的选项。 这些任务将数据移动到云是高级分析过程和技术 （调整） 提供的 Azure 机器学习的一部分。

概述了用于机器学习为将数据移动到后端 SQL Server 选项的主题，请参阅[移动到 Azure 的虚拟机上的 SQL Server 数据](machine-learning-data-science-move-sql-server-virtual-machine.md)。

下表概述了将数据移到 SQL Azure 数据库的选项。
<table>

<tr>
<td><b>来源</b></td>
<td colspan="2"><b>目的︰ SQL Azure 数据库</b></td>
</tr>

<tr>
  <td><b>平面文件 （CSV 或 TSV 格式）</b></td>  

  <td>
    1. <a href="#bulk-insert-sql-query">大容量插入 SQL 查询 </td>
</tr>

<tr>
  <td><b>后端 SQL Server</b></td>

  <td>
    1. <a href="#export-flat-file">将导出到平面文件<br>
    2. <a href="#insert-tables-bcp">SQL 数据库迁移向导<br>
    3. <a href="#db-migration">数据库备份和恢复<br>
    4. <a href="#adf">Azure 数据工厂 </td>
</tr>

</table>


## <a name="prereqs"></a>先决条件
此处所述此过程要求您具有︰

* **Azure 的订阅**。 如果没有预订，您可以注册[免费试用版](https://azure.microsoft.com/pricing/free-trial/)。
* **Azure 存储帐户**。 对于将数据存储在本教程中，您将使用 Azure 存储帐户。 如果没有 Azure 存储帐户，请参阅文章[创建存储帐户](storage-create-storage-account.md#create-a-storage-account)。 创建存储帐户后，您需要获取帐户密码用来访问存储。 请参阅[查看、 复制和重新生成存储访问键](storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys)。
* 对**SQL Azure 数据库**的访问。 如果您必须先设置一个 Azure 的 SQL 数据库，[开始使用 Microsoft Azure SQL 数据库](sql-database-get-started.md)提供有关如何设置 SQL Azure 数据库的新实例信息。
* 安装和配置**Azure PowerShell**本地。 有关说明，请参阅[如何安装和配置 Azure PowerShell](powershell-install-configure.md)。

**数据**︰ 迁移过程演示了使用[纽约出租车上的数据集](http://chriswhong.com/open-data/foil_nyc_taxi/)。 纽约出租车上数据集包含行程数据和博览会上和信息可用，提到该张贴内容，Azure 的 blob 存储在[此处](http://www.andresmh.com/nyctaxitrips/)。 [纽约出租车上行程数据集描述](machine-learning-data-science-process-sql-walkthrough.md#dataset)中提供了示例和说明这些文件。
 
可以调整到您自己的数据集此处描述的过程，也可以按照如下所述使用纽约出租车上数据集。 若要将纽约出租车上数据集传到后端 SQL Server 数据库，请按照[批量导入到 SQL Server 数据库的数据](machine-learning-data-science-process-sql-walkthrough.md#dbload)中所述的过程。 这些说明是针对 SQL Server Azure 虚拟机上，但将上载到后端 SQL Server 的过程是相同的。

## <a name="file-to-azure-sql-database"></a> 将数据从平面文件源移到 SQL Azure 数据库

平面文件 （CSV 或 TSV 格式） 中的数据可以移动到 SQL Azure 数据库使用大容量插入 SQL 查询。

### <a name="bulk-insert-sql-query"></a> 大容量插入 SQL 查询

使用大容量插入 SQL 查询的过程的步骤是在 Azure VM 数据从平面文件源移动到 SQL Server 的各节中介绍的内容相似。 有关详细信息，请参阅[批量插入 SQL 查询](machine-learning-data-science-move-sql-server-virtual-machine.md#insert-tables-bulkquery)。

##<a name="sql-on-prem-to-sazure-sql-database"></a> 将数据从后端 SQL Server 移到 SQL Azure 数据库

如果源数据存储在后端 SQL Server 中，有各种可能的将数据移到 SQL Azure 数据库︰

1. [将导出到平面文件](#export-flat-file) 
2. [SQL 数据库迁移向导](#insert-tables-bcp)
3. [数据库备份和恢复](#db-migration)
4. [Azure 数据工厂](#adf)

前三个步骤是非常类似于那些章节涵盖了这些相同的过程中[将数据移到 Azure 的虚拟机上的 SQL Server](machine-learning-data-science-move-sql-server-virtual-machine.md) 。 下面提供了指向该主题中的相应部分。

###<a name="export-flat-file"></a>将导出到平面文件

此导出到平面文件的步骤是类似于那些已覆盖[到平面文件导出](machine-learning-data-science-move-sql-server-virtual-machine.md#export-flat-file)。

###<a name="insert-tables-bcp"></a>SQL 数据库迁移向导

使用 SQL 数据库迁移向导的步骤是类似于那些已覆盖[SQL 数据库迁移向导](machine-learning-data-science-move-sql-server-virtual-machine.md#sql-migration)。

###<a name="db-migration"></a>数据库备份和恢复

使用数据库备份和还原步骤是类似于那些已覆盖[数据库备份和还原](machine-learning-data-science-move-sql-server-virtual-machine.md#sql-backup)。

###<a name="adf"></a>Azure 数据工厂

[将数据从 SQL Azure 使用 Azure 数据工厂的后端 SQL 服务器移动](machine-learning-data-science-move-sql-azure-adf.md)主题中提供将数据移到 SQL Azure 数据库 Azure 数据工厂 (ADF) 的过程。本主题演示如何将数据从移动的后端 SQL Server 数据库 SQL Azure 数据库到 Azure Blob 存储通过使用 ADF。 

考虑使用 ADF 时需要不断地在混合方案中，访问后端和云资源迁移数据时进行事务处理的数据，或需要被修改或已添加到它在迁移过程中的业务逻辑。 ADF 允许进行调度和监控的使用简单的 JSON 脚本管理的定期数据移动作业。 ADF 还具有其他功能，如支持复杂的操作。





测试

---
ms.openlocfilehash: 0c2ac69f177084a47a8605ba56f5bbcd64a288ff
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="Hadoop 群集使用 Azure CLI 管理 |Microsoft Azure"
    description="如何使用 Azure CLI 管理中 HDIsight 的 Hadoop 群集"
    services="hdinsight"
    editor="cgronlun"
    manager="paulettm"
    authors="mumian"
    tags="azure-portal"
    documentationCenter=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/08/2015"
    ms.author="jgao"/>

# 通过使用 Azure 命令行界面 (Azure CLI) 管理中 HDInsight 的 Hadoop 群集

[AZURE.INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

了解如何使用 Azure CLI 管理 Azure HDInsight 在 Hadoop 群集。 在 Node.js 实现 Azure CLI。 可以在任何支持 Node.js，包括 Windows、 Mac 和 Linux 的平台上使用它。

Azure CLI 是开源。 源代码管理中在 GitHub <a href= "https://github.com/azure/azure-xplat-cli">https://github.com/azure/azure-xplat-cli</a>。

本文介绍了仅使用 HDInsight Azure CLI。 有关如何使用 Azure CLI 的一般指南，请参阅[如何使用 Azure CLI] [azure 命令行工具]。


##先决条件

在开始这篇文章之前，您必须具有以下︰

- **Azure 订阅**。 请参阅[获取 Azure 免费试用版](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)。

- **Azure CLI** -请参阅[安装和配置 Azure CLI](../xplat-cli.md)的安装和配置信息。

##安装

如果您没有使用的[安装和配置 Azure CLI](../xplat-cli.md)的文档来安装和配置 Azure CLI。

##条款 HDInsight 群集

[AZURE.INCLUDE [provisioningnote](../../includes/hdinsight-provisioning.md)]


HDInsight 使用 Azure Blob 存储容器作为默认的文件系统。 您可以创建一个 HDInsight 群集之前，Azure 存储帐户是必需的。

在导入的 publishsettings 文件之后，可以使用下面的命令来创建存储帐户︰

    azure account storage create [options] <StorageAccountName>


> [AZURE.NOTE] 必须在数据中心中的 HDInsight 与搭配使用存储帐户。


通过使用 Azure 预览门户创建 Azure 存储帐户的信息，请参阅[创建、 管理或删除存储帐户][azure 创建 storageaccount]。

如果您已经拥有存储帐户，但不是知道用户名和帐户密码，您可以使用以下命令检索的信息︰

    -- Lists Storage accounts
    azure account storage list
    -- Shows a Storage account
    azure account storage show <StorageAccountName>
    -- Lists the keys for a Storage account
    azure account storage keys list <StorageAccountName>

通过使用 Azure 预览门户中获取的信息的详细信息，请参阅[创建、 管理或删除存储帐户][azure 创建 storageaccount]的"查看、 复制和重新生成存储访问键"部分。


如果它不存在， **azure hdinsight 群集创建**命令创建容器。 如果您选择预先创建的容器，您可以使用下面的命令︰

    azure storage container create --account-name <StorageAccountName> --account-key <StorageAccountKey> [ContainerName]

一旦您有存储帐户和准备的 Blob 容器，您就可以创建群集︰

    azure hdinsight cluster create --clusterName <ClusterName> --storageAccountName <StorageAccountName> --storageAccountKey <storageAccountKey> --storageContainer <StorageContainer> --nodes <NumberOfNodes> --location <DataCenterLocation> --username <HDInsightClusterUsername> --clusterPassword <HDInsightClusterPassword>

![HDI。CLIClusterCreation][image-cli-clustercreation]

















##通过使用配置文件设置 HDInsight 群集
通常情况下，提供 HDInsight 群集、 运行作业，并删除群集来削减成本。 命令行界面可以保存到一个文件中，配置的选项，以便您可以重复使用它每次提供群集。  

    azure hdinsight cluster config create <file>

    azure hdinsight cluster config set <file> --clusterName <ClusterName> --nodes <NumberOfNodes> --location "<DataCenterLocation>" --storageAccountName "<StorageAccountName>.blob.core.windows.net" --storageAccountKey "<StorageAccountKey>" --storageContainer "<BlobContainerName>" --username "<Username>" --clusterPassword "<UserPassword>"

    azure hdinsight cluster config storage add <file> --storageAccountName "<StorageAccountName>.blob.core.windows.net"
           --storageAccountKey "<StorageAccountKey>"

    azure hdinsight cluster config metastore set <file> --type "hive" --server "<SQLDatabaseName>.database.windows.net"
           --database "<HiveDatabaseName>" --user "<Username>" --metastorePassword "<UserPassword>"

    azure hdinsight cluster config metastore set <file> --type "oozie" --server "<SQLDatabaseName>.database.windows.net"
           --database "<OozieDatabaseName>" --user "<SQLUsername>" --metastorePassword "<SQLPassword>"

    azure hdinsight cluster create --config <file>



![HDI。CLIClusterCreationConfig][image-cli-clustercreation-config]


##列出并显示群集的详细信息
使用以下命令列出并显示群集的详细信息︰

    azure hdinsight cluster list
    azure hdinsight cluster show <ClusterName>

![HDI。CLIListCluster][image-cli-clusterlisting]


##删除群集
使用下面的命令删除群集︰

    azure hdinsight cluster delete <ClusterName>




##下一步行动
在本文中，您学习了如何来执行不同的 HDInsight 群集管理任务。 若要了解详细信息，请参阅下列文章︰

* [通过使用 Azure 预览门户管理 HDInsight] [hdinsight 管理门户]
* [通过使用 Azure PowerShell 管理 HDInsight] [hdinsight-管理 powershell]
* [开始使用 Azure HDInsight] [hdinsight--入门]
* [如何使用 Azure CLI] [azure 命令行工具]


[azure 命令行工具]: ../xplat-cli.md
[azure 创建 storageaccount]: ../storage-create-storage-account.md
[azure 的购买选项]: http://azure.microsoft.com/pricing/purchase-options/
[azure 的成员提供]: http://azure.microsoft.com/pricing/member-offers/
[azure 释放试验]: http://azure.microsoft.com/pricing/free-trial/


[hdinsight 管理门户]: hdinsight-administer-use-management-portal.md
[hdinsight-管理 powershell]: hdinsight-administer-use-powershell.md
[hdinsight--入门]: ../hdinsight-get-started.md

[图像-cli--下载-导入客户]: ./media/hdinsight-administer-use-command-line/HDI.CLIAccountDownloadImport.png
[图像-cli-clustercreation]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreation.png
[图像 cli clustercreation 配置]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreationConfig.png
[图像-cli-clusterlisting]: ./media/hdinsight-administer-use-command-line/HDI.CLIListClusters.png "列表和显示群集"

测试

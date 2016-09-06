---
ms.openlocfilehash: 1e8718586755974744c1c2311f32c018219d1a5d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="自定义 HDInsight 群集使用脚本操作 |Microsoft Azure" 
    description="了解如何自定义 HDInsight 群集使用脚本的操作。" 
    services="hdinsight" 
    documentationCenter="" 
    authors="nitinme" 
    manager="paulettm" 
    editor="cgronlun"/> 

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/07/2015" 
    ms.author="nitinme"/> 

# 自定义 HDInsight 群集使用脚本操作

[AZURE.INCLUDE [hdinsight-azure-portal](../../includes/hdinsight-azure-portal.md)]

* [自定义 HDInsight 群集使用脚本操作](hdinsight-hadoop-customize-cluster.md)

HDInsight 提供了一个名为**脚本操作**，用于调用定义在资源调配过程中群集上执行的自定义项的自定义脚本的配置选项。 要在群集上安装其他软件或更改群集上的应用程序的配置，可以使用这些脚本。 


> [AZURE.NOTE] 将操作脚本保存在 HDInsight 群集版本 3.1 或更高版本的 Windows 操作系统上才支持。  HDInsight 群集版本的详细信息，请参阅[HDInsight 群集版本](hdinsight-component-versioning.md)。
> 
> 将操作脚本保存为可作为不额外收费的标准 Azure HDInsight 订阅的一部分。

HDInsight 群集可以自定义多种其他方式，如包括其他 Azure 存储帐户，更改 Hadoop 配置文件 （核心 site.xml，配置单元 site.xml，等等），或添加到群集中的常见位置共享库 （例如，配置单元，Oozie）。 这些自定义项可以通过 Azure PowerShell，Azure HDInsight.NET SDK 或 Azure 的门户。 有关详细信息，请参阅[配置 Hadoop 群集中使用的自定义选项的 HDInsight][hdinsight 配置群集]。

## 脚本操作中群集资源调配过程

在创建群集时仅使用脚本的操作。 下图说明在资源调配过程中执行脚本操作时︰

![HDInsight 群集进行自定义和群集资源调配过程的阶段][img-hdi-cluster-states] 

当运行该脚本时，群集将进入**ClusterCustomization**舞台。 在此阶段，该脚本下系统管理员帐户在群集中，所有指定的节点上并行运行并在该节点上提供完全的管理员权限。 

> [AZURE.NOTE] 因为**ClusterCustomization**阶段在群集节点上有管理员权限，可以使用脚本来执行操作，如停止和启动服务，包括与 Hadoop 相关的服务。 因此，作为一部分的脚本，您必须确保 Ambari 服务和其它与 Hadoop 相关服务启动并运行该脚本完成运行之前。 这些服务需要在创建时成功地确定群集的状态和正常运行。 如果您更改任何这些服务会影响群集上的配置，必须使用提供帮助器函数。 对有关 helper 函数的详细信息，请参阅[HDInsight 的开发脚本操作脚本][hdinsight 写脚本]。

输出和错误日志中的脚本存储在群集中指定的默认存储帐户。 日志都存储在一个表的名称**u < \cluster-name-fragment >< \time-stamp > setuplog**。 这些都是聚合日志中的所有节点上 （head 节点和辅助节点） 在群集中运行的脚本。


每个群集可以接受指定它们的顺序调用多个脚本操作。 可以在头节点和 / 或辅助节点上运行脚本。 

## 调用脚本操作脚本

脚本操作脚本可用来从 Azure PowerShell 或 HDInsight.NET SDK 的 Azure 的入口。

HDInsight 提供几个脚本 HDInsight 群集上安装以下组件︰

名称 | Script
----- | -----
**安装触发** | https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1。 请参阅[安装和使用 HDInsight 群集上的触发][hdinsight-安装触发]。
**R 安装** | https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1。 请参阅[安装和使用 HDInsight 群集上的 R][hdinsight-安装-r]。
**安装 Solr** | https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1。 [Solr 在 HDInsight 上的安装和使用群集](hdinsight-hadoop-solr-install.md)，请参阅。
- **安装 Giraph** | https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1。 请参阅[Giraph 在 HDInsight 上的安装和使用群集](hdinsight-hadoop-giraph-install.md)。



**从 Azure 门户**

1. 开始[调配使用自定义选项的群集](hdinsight-provision-clusters.md#portal)在所述使用的**自定义**选项，配置群集。 
2. 在向导的**脚本操作**页上，单击**添加脚本操作**以提供详细信息的脚本操作，如下所示︰

    ![将操作脚本保存用于自定义群集](./media/hdinsight-hadoop-customize-cluster-v1/HDI.CustomProvision.Page6.png "Use Script Action to customize a cluster")
    
    <table border='1'>
        <tr><th>属性</th><th>值</th></tr>
        <tr><td>名称</td>
            <td>指定脚本动作的名称。</td></tr>
        <tr><td>脚本的 URI</td>
            <td>指定调用自定义群集脚本的 URI。 s</td></tr>
        <tr><td>节点类型</td>
            <td>指定在其运行的自定义脚本的节点。 您可以选择<b>所有节点</b>，<b>头节点仅</b>，或<b>辅助节点仅</b>。
        <tr><td>参数</td>
            <td>指定的参数，如果所需的脚本。</td></tr>
    </table>

    您可以添加多个安装在群集上的多个组件的脚本操作。 

3. 单击以开始设置群集的选中标记。 
  
**从 Azure PowerShell cmdlet**

使用 HDInsight 的 Azure PowerShell 命令来运行单个脚本操作或多个脚本操作。 您可以使用**<a href = "http://msdn.microsoft.com/library/dn858088.aspx" target="_blank">添加 AzureHDInsightScriptAction</a>**用于调用自定义脚本。 若要使用这些 cmdlet，您必须安装和配置的 Azure PowerShell。 有关配置工作站运行 HDInsight Azure PowerShell cmdlet 的信息，请参阅[安装和配置 Azure PowerShell][powershell 安装配置]。

使用以下的 Azure PowerShell 命令时部署 HDInsight 群集运行单个脚本操作︰

    $config = New-AzureHDInsightClusterConfig –ClusterSizeInNodes 4

    $config = Add-AzureHDInsightScriptAction -Config $config –Name MyScriptActionName –Uri http://uri.to/scriptaction.ps1 –Parameters MyScriptActionParameter -ClusterRoleCollection HeadNode,DataNode

    New-AzureHDInsightCluster -Config $config

使用以下的 Azure PowerShell 命令部署 HDInsight 群集时运行多个脚本操作︰

    $config = New-AzureHDInsightClusterConfig –ClusterSizeInNodes 4

    $config = Add-AzureHDInsightScriptAction -Config $config –Name MyScriptActionName1 –Uri http://uri.to/scriptaction1.ps1 –Parameters MyScriptAction1Parameters -ClusterRoleCollection HeadNode,DataNode | Add-AzureHDInsightScriptAction -Config $config –Name MyScriptActionName2 –Uri http://uri.to/scriptaction2.ps1 -Parameters MyScriptAction2Parameters -ClusterRoleCollection HeadNode

    New-AzureHDInsightCluster -Config $config

**从 HDInsight.NET SDK**

HDInsight.NET SDK 提供了<a href="http://msdn.microsoft.com/library/microsoft.windowsazure.management.hdinsight.clusterprovisioning.data.scriptaction.aspx" target="_blank">ScriptAction</a>类来调用自定义脚本。 若要使用 HDInsight.NET SDK:

1. 创建 Visual Studio 应用程序，然后再从 NuGet 安装 SDK。 从**工具**菜单上，单击**Nuget 程序包管理器**，然后单击**程序包管理器控制台**。 在控制台安装包中运行以下命令︰

        Install-Package Microsoft.WindowsAzure.Management.HDInsight

2. 使用 SDK 来创建一个群集。 有关说明，请参阅[使用.NET SDK 提供 HDInsight 群集](hdinsight-provision-clusters.md#sdk)。

3. 使用**ScriptAction**类可以调用自定义脚本，如下所示︰

        
        var clusterInfo = new ClusterCreateParameters()
        {
            // Provide the cluster information, like
            // name, Storage account, credentials,
            // cluster size, and version            
            ...
            ...
        };

        // Add the script action to install Spark
        clusterInfo.ConfigActions.Add(new ScriptAction(
            "MyScriptActionName", // Name of the config action
            new ClusterNodeType[] { ClusterNodeType.HeadNode }, // List of nodes to install the component on
            new Uri("http://uri.to/scriptaction.ps1"), // Location of the script to install the component
            "MyScriptActionParameter" //Parameters, if any, required by the script
        ));



## 对 HDInsight 群集上使用的开放源码软件的支持
Microsoft Azure HDInsight 服务是使您能够通过使用 Hadoop 周围形成的开放源代码技术体系构建大数据在云中的应用程序的灵活平台。 Microsoft Azure 的开源技术，提供一般意义上的支持，**支持范围**一节中所述<a href="http://azure.microsoft.com/support/faq/" target="_blank">Azure 支持常见问题网站</a>。 HDInsight 服务提供额外的支持的某些组件，如下所述。

有两种类型的 HDInsight 服务中可用的开源的组件︰

- **内置组件**-HDInsight 群集上预先安装了这些组件，并提供核心功能的群集。 例如，YARN ResourceManager、 配置单元查询语言 (HiveQL) 和 Mahout 库都属于此类别。 群集组件的完整列表位于<a href="http://azure.microsoft.com/documentation/articles/hdinsight-component-versioning/" target="_blank">由 HDInsight 提供的 Hadoop 群集版本中的新增功能？</a>.
- **自定义组件**，以在群集中，用户可以安装或使用您的工作负载在社区中提供或由您创建的任何组件。

完全支持内置组件，并可以帮助 Microsoft 技术支持来隔离并解决与这些组件相关的问题。

自定义组件接收商业上合理的支持，以帮助您进一步排查该问题。 这可能导致解决问题或要求您能够进行深入的专业技能，为该技术在其中找到的开放源代码技术可用的频道。 例如，有许多社区站点可以使用，如︰ <a href ="https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight" target="_blank">HDInsight 的 MSDN 论坛</a>和<a href="http://stackoverflow.com" target="_blank">堆栈溢出</a>。 此外，Apache 项目对项目网站<a href="http://apache.org" target="_blank">Apache.org</a>;例如， <a href="http://hadoop.apache.org/" target="_blank">Hadoop</a>和<a href="http://spark.apache.org/" target="_blank">触发</a>。

HDInsight 服务提供了多种方法用于自定义组件。 不管如何使用或在群集上安装组件，适用于相同级别的支持。 下面是最常见的方式，可以在 HDInsight 群集上使用自定义组件的列表︰

1. 可以将作业提交的 Hadoop 或其他类型的作业的执行或使用自定义组件提交到群集。
2. 群集的自定义-在群集创建过程中您可以指定其他设置和自定义将在群集节点安装的组件。
3. 示例-常见的自定义组件、 Microsoft 和其他公司可能会提供如何在 HDInsight 群集上使用这些组件的示例。 不支持的情况下提供了这些示例。

## 开发脚本操作脚本

[HDInsight 的开发脚本操作脚本][hdinsight 写脚本]，请参阅。 


## 请参见

- [在使用自定义选项的 HDInsight 提供 Hadoop 群集][hdinsight 提供群集]如何使用其他自定义选项来配置 HDInsight 群集提供指导。
- [HDInsight 的开发脚本操作脚本][hdinsight 写脚本]
- [安装和使用 HDInsight 群集上激励][hdinsight-安装触发]
- [安装和使用 R HDInsight 群集上][hdinsight-安装-r]
- [Solr 在 HDInsight 上的安装和使用群集](hdinsight-hadoop-solr-install.md)。
- [Giraph 在 HDInsight 上的安装和使用群集](hdinsight-hadoop-giraph-install.md)。

[hdinsight-安装触发]: hdinsight-hadoop-spark-install.md
[hdinsight-安装-r]: hdinsight-hadoop-r-scripts.md
[hdinsight 写脚本]: hdinsight-hadoop-script-actions.md
[hdinsight 提供群集]: hdinsight-provision-clusters.md
[powershell 安装配置]: ../install-configure-powershell.md


[img hdi 群集状态]: ./media/hdinsight-hadoop-customize-cluster-v1/HDI-Cluster-state.png "在群集资源调配过程的阶段"
 
测试

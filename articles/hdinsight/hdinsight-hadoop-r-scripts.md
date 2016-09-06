---
ms.openlocfilehash: 2c27e025ef68bf8603478e411d269b5bcaedda52
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="HDInsight 自定义群集中的使用 R |Microsoft Azure"
    description="了解如何安装和使用 R 自定义 Hadoop 群集。"
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
    ms.date="07/28/2015"
    ms.author="jgao"/>

# 上安装和使用 R HDInsight Hadoop 群集

通过使用**脚本操作**群集的自定义项，可以在任何类型的 HDInsight 在 Hadoop 群集上安装 R。 这使得数据科学家和分析家使用 R 来部署功能强大的 MapReduce/YARN 编程框架来处理大量的数据在 HDInsight 中所部署的 Hadoop 群集上。

脚本操作允许您运行脚本自定义群集上，请在创建群集时只。 有关详细信息，请参见[使用脚本操作的自定义 HDInsight 群集][hdinsight 群集自定义]。


## R 是什么？

<a href="http://www.r-project.org/" target="_blank">用于统计计算的 R 项目</a>是一种开放源代码语言和环境的统计计算。 R 提供数百个生成中的统计函数和自己相结合方面的功能和面向对象的编程的编程语言。 它还提供了丰富的图形功能。 R 是大多数专业统计学家和科学家在各种不同的字段中首选的编程环境。

R 脚本可以在 HDInsight 中使用脚本操作时创建安装 R 环境自定义的 Hadoop 群集上运行。 R 是兼容与 Azure Blob 存储 (WASB)，以便可以在 HDInsight 上使用 R 处理其中所存储的数据。  

## R 安装

只读的 blob Azure 存储中提供了[示例脚本](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1)在 HDInsight 群集上安装 R。 本节提供有关如何在资源调配使用 Azure 预览门户群集时使用的示例脚本的说明。

> [AZURE.NOTE] 使用 HDInsight 群集版本 3.1 引入了示例脚本。 有关 HDInsight 群集版本的详细信息，请参阅[HDInsight 群集版本](../hdinsight-component-versioning/)。

1. 提供从预览门户 HDInsight 群集，请单击**可选配置**，然后单击**脚本操作**。
2. 在**脚本操作**页中，输入以下值︰

    ![将操作脚本保存用于自定义群集](./media/hdinsight-hadoop-r-scripts/hdi-r-script-action.png "Use Script Action to customize a cluster")

    <table border='1'>
        <tr><th>属性</th><th>值</th></tr>
        <tr><td>名称</td>
            <td>为指定名称的脚本操作，例如，<b>安装 R</b>。</td></tr>
        <tr><td>脚本的 URI</td>
            <td>指定调用自定义群集，例如，该脚本的 URI <i>https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1</i></td></tr>
        <tr><td>节点类型</td>
            <td>指定在其运行的自定义脚本的节点。 您可以选择<b>所有节点</b>，<b>头节点仅</b>，或<b>辅助节点</b>只。
        <tr><td>参数</td>
            <td>指定的参数，如果所需的脚本。 但是，要安装 R 的脚本不需要任何参数，因此您可以将其留空。</td></tr>
    </table>

    您可以添加多个安装在群集上的多个组件的脚本操作。 添加脚本后，请单击复选标记以开始设置群集。

您可以使用该脚本使用 Azure PowerShell 或 HDInsight.NET SDK 安装在 HDInsight R。 本文后面提供了有关这些过程的说明。

## 运行 R 脚本
本部分介绍如何运行 Hadoop 群集与 HDInsight R 脚本。

1. **建立一个远程桌面连接到群集**︰ 从预览门户，启用了远程桌面使用 R 安装，创建群集，然后再连接到群集。 有关说明，请参阅<a href="http://azure.microsoft.com/documentation/articles/hdinsight-administer-use-management-portal/#rdp" target="_blank">与 HDInsight 群集使用 RDP 连接</a>。

2. **打开 R 控制台**︰ R 安装将 R 控制台链接放在头节点的桌面上。 单击它以打开 R 控制台。

3. **运行 R 脚本**︰ R 脚本可以直接从 R 控制台运行通过粘贴它、 选择它，然后按 ENTER。 这里有一个简单的示例脚本，生成编号 1 至 100，然后将其乘以 2。

        library(rmr2)
        library(rhdfs)
        ints = to.dfs(1:100)
        calc = mapreduce(input = ints, map = function(k, v) cbind(v, 2*v))
        from.dfs(calc)

前两行调用 RHadoop 库安装与。最后一行将打印到控制台的结果。 输出应如下所示︰

    [1,]  1 2
    [2,]  2 4
    .
    .
    .
    [98,]  98 196
    [99,]  99 198
    [100,] 100 200

## 安装使用 Azure PowerShell 的 R

在本节中，我们使用**<a href = "http://msdn.microsoft.com/library/dn858088.aspx" target="_blank">添加 AzureHDInsightScriptAction</a>**的 cmdlet 来调用脚本，方法是使用脚本的操作自定义群集。 继续之前，请确保您已安装并配置 Azure PowerShell。 有关配置工作站运行 HDInsight Powershell cmdlet 的信息，请参阅[安装和配置 Azure PowerShell][powershell 安装配置]。

执行以下步骤︰

1. 打开 Azure PowerShell 控制台并声明以下变量︰

        # PROVIDE VALUES FOR THESE VARIABLES
        $subscriptionName = "<SubscriptionName>"        # Name of the Azure subscription
        $clusterName = "<HDInsightClusterName>"         # HDInsight cluster name
        $storageAccountName = "<StorageAccountName>"    # Azure storage account that hosts the default container
        $storageAccountKey = "<StorageAccountKey>"      # Key for the storage account
        $containerName = $clusterName
        $location = "<MicrosoftDataCenter>"             # Location of the HDInsight cluster. It must be in the same data center as the storage account.
        $clusterNodes = <ClusterSizeInNumbers>          # The number of nodes in the HDInsight cluster.
        $version = "<HDInsightClusterVersion>"          # HDInsight version, for example "3.1"

2. 指定的配置值 （例如，群集中的节点） 和要使用的默认存储。

        # SPECIFY THE CONFIGURATION OPTIONS
        Select-AzureSubscription $subscriptionName
        $config = New-AzureHDInsightClusterConfig -ClusterSizeInNodes $clusterNodes
        $config.DefaultStorageAccount.StorageAccountName="$storageAccountName.blob.core.windows.net"
        $config.DefaultStorageAccount.StorageAccountKey=$storageAccountKey
        $config.DefaultStorageAccount.StorageContainerName=$containerName

3. 使用**添加 AzureHDInsightScriptAction** cmdlet 来调用示例脚本安装 R，例如︰

        # INVOKE THE SCRIPT USING THE SCRIPT ACTION
        $config = Add-AzureHDInsightScriptAction -Config $config -Name "Install R"  -ClusterRoleCollection HeadNode,DataNode -Uri https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1


    **添加 AzureHDInsightScriptAction** cmdlet 的参数如下︰

    <table style="border-color: #c6c6c6; border-width: 2px; border-style: solid; border-collapse: collapse;">
    <tr>
    <th style="border-color: #c6c6c6; border-width: 2px; border-style: solid; border-collapse: collapse; width:90px; padding-left:5px; padding-right:5px;">参数</th>
    <th style="border-color: #c6c6c6; border-width: 2px; border-style: solid; border-collapse: collapse; width:550px; padding-left:5px; padding-right:5px;">定义</th></tr>
    <tr>
    <td style="border-color: #c6c6c6; border-width: 2px; border-style: solid; border-collapse: collapse; padding-left:5px;">配置</td>
    <td style="border-color: #c6c6c6; border-width: 2px; border-style: solid; border-collapse: collapse; padding-left:5px; padding-right:5px;">操作信息添加到哪个脚本的配置对象</td></tr>
    <tr>
    <td style="border-color: #c6c6c6; border-width: 2px; border-style: solid; border-collapse: collapse; padding-left:5px;">名称</td>
    <td style="border-color: #c6c6c6; border-width: 2px; border-style: solid; border-collapse: collapse; padding-left:5px;">脚本操作的名称</td></tr>
    <tr>
    <td style="border-color: #c6c6c6; border-width: 2px; border-style: solid; border-collapse: collapse; padding-left:5px;">ClusterRoleCollection</td>
    <td style="border-color: #c6c6c6; border-width: 2px; border-style: solid; border-collapse: collapse; padding-left:5px;">指定在其运行的自定义脚本的节点。 有效的值是**HeadNode** （要在头节点上安装） 或**DataNode** （若要在数据的所有节点上安装）。 您可以使用任意一个或两个值。</td></tr>
    <tr>
    <td style="border-color: #c6c6c6; border-width: 2px; border-style: solid; border-collapse: collapse; padding-left:5px;">参数</td>
    <td style="border-color: #c6c6c6; border-width: 2px; border-style: solid; border-collapse: collapse; padding-left:5px;">所需的脚本参数
    </td></tr>
    <tr>
    <td style="border-color: #c6c6c6; border-width: 2px; border-style: solid; border-collapse: collapse; padding-left:5px;">Uri</td>
    <td style="border-color: #c6c6c6; border-width: 2px; border-style: solid; border-collapse: collapse; padding-left:5px;">指定执行该脚本的 URI</td></tr>
    </table>

4. 最后，设置有 R 安装自定义群集。  

        # PROVISION A CLUSTER WITH R INSTALLED
        New-AzureHDInsightCluster -Config $config -Name $clusterName -Location $location -Version $version

出现提示时，输入群集的凭据。 它可能需要几分钟才能创建群集。


## 安装使用.NET SDK 的 R

HDInsight.NET SDK 提供了.NET 客户端库，便于从.NET 应用程序与 HDInsight 合作。

执行以下过程以设置使用 SDK HDInsight 群集︰

- [安装 HDInsight.NET SDK](#installSDK)
- [创建自行签署式证书](#createCert)
- [在 Visual Studio 中创建.NET 应用程序](#createApp)
- [运行应用程序](#runApp)

以下各节说明如何执行这些步骤。

**若要安装 HDInsight.NET SDK**

您可以从[NuGet](http://nuget.codeplex.com/wikipage?title=Getting%20Started)安装最新版本已发布的版本的 SDK。 说明将显示在下一个过程。

**若要创建自签名的证书**

创建自签名的证书，将其安装在您的工作站，并将其上载到 Azure 订购。 有关说明，请参阅[创建自签名证书](http://go.microsoft.com/fwlink/?LinkId=511138)。


**若要在 Visual Studio 中创建.NET 应用程序**

1. 打开 Visual Studio 2013年。

2. 从**文件**菜单上，单击**新建**，然后单击**项目**。

3. 从**新建项目**，键入或选择以下值︰

    <table style="border-color: #c6c6c6; border-width: 2px; border-style: solid; border-collapse: collapse;">
    <tr>
    <th style="border-color: #c6c6c6; border-width: 2px; border-style: solid; border-collapse: collapse; width:90px; padding-left:5px; padding-right:5px;">属性</th>
    <th style="border-color: #c6c6c6; border-width: 2px; border-style: solid; border-collapse: collapse; width:90px; padding-left:5px; padding-right:5px;">值</th></tr>
    <tr>
    <td style="border-color: #c6c6c6; border-width: 2px; border-style: solid; border-collapse: collapse; padding-left:5px;">Category</td>
    <td style="border-color: #c6c6c6; border-width: 2px; border-style: solid; border-collapse: collapse; padding-left:5px; padding-right:5px;">模板/视频 C# / 窗口</td></tr>
    <tr>
    <td style="border-color: #c6c6c6; border-width: 2px; border-style: solid; border-collapse: collapse; padding-left:5px;">Template</td>
    <td style="border-color: #c6c6c6; border-width: 2px; border-style: solid; border-collapse: collapse; padding-left:5px;">控制台应用程序</td></tr>
    <tr>
    <td style="border-color: #c6c6c6; border-width: 2px; border-style: solid; border-collapse: collapse; padding-left:5px;">名称</td>
    <td style="border-color: #c6c6c6; border-width: 2px; border-style: solid; border-collapse: collapse; padding-left:5px;">CreateRCluster</td></tr>
    </table>

4. 单击**确定**以创建项目。

5. 从**工具**菜单上，单击**Nuget 程序包管理器**，然后单击**程序包管理器控制台**。

6. 在控制台安装包中运行以下命令。

        Install-Package Microsoft.WindowsAzure.Management.HDInsight

    此命令将添加.NET 库和对它们从当前 Visual Studio 项目的引用。

7. 从**解决方案资源管理器中**，双击**Program.cs**以将其打开。

8. 将以下**using**语句添加到文件的顶部︰

        using System.Security.Cryptography.X509Certificates;
        using Microsoft.WindowsAzure.Management.HDInsight;
        using Microsoft.WindowsAzure.Management.HDInsight.ClusterProvisioning;
        using Microsoft.WindowsAzure.Management.HDInsight.Framework.Logging;

9. 在**main （）**函数中，粘贴以下代码，并为这些变量提供值︰

        var clusterName = args[0];

        // PROVIDE VALUES FOR THE VARIABLES
        string thumbprint = "<CertificateThumbprint>";  
        string subscriptionId = "<AzureSubscriptionID>";
        string location = "<MicrosoftDataCenterLocation>";
        string storageaccountname = "<AzureStorageAccountName>.blob.core.windows.net";
        string storageaccountkey = "<AzureStorageAccountKey>";
        string username = "<HDInsightUsername>";
        string password = "<HDInsightUserPassword>";
        int clustersize = <NumberOfNodesInTheCluster>;

        // PROVIDE THE CERTIFICATE THUMBPRINT TO RETRIEVE THE CERTIFICATE FROM THE CERTIFICATE STORE
        X509Store store = new X509Store();
        store.Open(OpenFlags.ReadOnly);
        X509Certificate2 cert = store.Certificates.Cast<X509Certificate2>().First(item => item.Thumbprint == thumbprint);

        // CREATE AN HDINSIGHT CLIENT OBJECT
        HDInsightCertificateCredential creds = new HDInsightCertificateCredential(new Guid(subscriptionId), cert);
        var client = HDInsightClient.Connect(creds);
        client.IgnoreSslErrors = true;

        // PROVIDE THE CLUSTER INFORMATION
        var clusterInfo = new ClusterCreateParameters()
        {
            Name = clusterName,
            Location = location,
            DefaultStorageAccountName = storageaccountname,
            DefaultStorageAccountKey = storageaccountkey,
            DefaultStorageContainer = clusterName,
            UserName = username,
            Password = password,
            ClusterSizeInNodes = clustersize,
            Version = "3.1"
        };

10. 将下面的代码附加到**main （）**函数，以使用[ScriptAction](http://msdn.microsoft.com/library/microsoft.windowsazure.management.hdinsight.clusterprovisioning.data.scriptaction.aspx)类来调用自定义脚本来安装。

        // ADD THE SCRIPT ACTION TO INSTALL R

        clusterInfo.ConfigActions.Add(new ScriptAction(
            "Install R",
            new ClusterNodeType[] { ClusterNodeType.HeadNode, ClusterNodeType.DataNode },
            new Uri("https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1"), null
            ));

11. 最后，创建群集︰

        client.CreateCluster(clusterInfo);

11. 将更改保存到应用程序并生成解决方案。

**若要运行该应用程序**

打开 Azure PowerShell 控制台、 定位到您保存项目的位置、 导航到 \bin\debug 目录内的项目，然后运行下面的命令︰

    .\CreateRCluster <cluster-name>

提供群集的名称并按 enter 键以 R 安装与配置群集。

## 请参见

- [安装和使用 HDInsight 群集上激励]有关如何使用自定义群集上安装和使用触发 HDInsight Hadoop 群集说明[hdinsight-安装触发]。 触发是并行处理的开放源码框架支持内存中处理，以提高大数据分析应用程序的性能。
- [在 HDInsight 群集上安装 Giraph](../hdinsight-hadoop-giraph-install)。 使用自定义群集在 HDInsight Hadoop 群集上安装 Giraph。 Giraph 允许您执行使用 Hadoop，图形处理和使用 Azure HDInsight。
- [在 HDInsight 群集上安装 Solr](../hdinsight-hadoop-solr-install)。 使用自定义群集在 HDInsight Hadoop 群集上安装 Solr。 Solr 允许您执行存储数据的功能强大的搜索操作。

[powershell 安装配置]: ../install-configure-powershell.md
[hdinsight 规定]: ../hdinsight-provision-clusters/
[hdinsight 群集自定义]: ../hdinsight-hadoop-customize-cluster
[hdinsight-安装触发]: ../hdinsight-hadoop-spark-install/

测试

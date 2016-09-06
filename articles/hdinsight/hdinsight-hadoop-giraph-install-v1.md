---
ms.openlocfilehash: b40856edc7358d9b78fd069d49fb02ef6fcddce8
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="安装和使用 Hadoop 群集在 HDInsight 上的 Giraph |Microsoft Azure" 
    description="了解如何自定义 Giraph 与 HDInsight 群集。 您将使用一个脚本操作配置选项以使用脚本来安装 Giraph。" 
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

# 在 HDInsight Hadoop 群集上安装 Giraph，并使用 Giraph 来处理大规模的关系图

通过使用**脚本操作**群集的自定义项，可以在任何类型的 Azure HDInsight 在 Hadoop 群集上安装 Giraph。 脚本操作允许您运行脚本自定义群集上，请在创建群集时只。 有关详细信息，请参阅[自定义 HDInsight 群集使用脚本操作][hdinsight 群集自定义]。

在本主题中，您将学习如何通过脚本操作安装 Giraph。 一旦您安装了 Giraph，您还将学习如何使用 Giraph 为最典型的应用程序，这是处理大规模的关系图。

[AZURE.INCLUDE [hdinsight-azure-portal](../../includes/hdinsight-azure-portal.md)]

* [在 HDInsight 群集上安装 Giraph](hdinsight-hadoop-giraph-install.md)

## <a name="whatis"></a>Giraph 是什么？

<a href="http://giraph.apache.org/" target="_blank">Apache Giraph</a>允许您执行图形使用 Hadoop，处理和使用 Azure HDInsight。 关系图的模型对象，如大型互联网，如网络上的路由器或上社交网络 （有时称为社交图） 的人之间的关系之间的连接之间的关系。 图形处理允许您为图中的对象之间的关系有关的原因如︰

- 根据您当前的关系识别潜在的朋友。
- 用于标识网络中的两台计算机之间最短的路线。
- 计算网页的网页排名。

   
## <a name="install"></a>如何安装 Giraph？

只读的 Azure 存储 blob，在[https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1)提供了在 HDInsight 群集上安装 Giraph 的示例脚本。 本节说明如何在使用 Azure 门户配置群集时使用的示例脚本。 

> [AZURE.NOTE] 示例脚本仅适用于 HDInsight 群集版本 3.1。 HDInsight 群集版本的详细信息，请参阅[HDInsight 群集版本](hdinsight-component-versioning.md)。

1. 开始[调配使用自定义选项的群集](hdinsight-provision-clusters.md#portal)在所述使用的**自定义**选项，配置群集。 
2. 在向导的**脚本操作**页上，单击**添加脚本操作**以提供详细信息的脚本操作，如下所示︰

    ![将操作脚本保存用于自定义群集](./media/hdinsight-hadoop-giraph-install-v1/hdi-script-action-giraph.png "Use Script Action to customize a cluster")
    
    <table border='1'>
        <tr><th>属性</th><th>值</th></tr>
        <tr><td>名称</td>
            <td>指定脚本动作的名称。 例如，<b>安装 Giraph</b>。</td></tr>
        <tr><td>脚本的 URI</td>
            <td>指定脚本调用自定义群集的统一资源标识符 (URI)。 例如， <i>https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1</i></td></tr>
        <tr><td>节点类型</td>
            <td>指定在其运行的自定义脚本的节点。 您可以选择<b>所有节点</b>，<b>头节点仅</b>，或<b>辅助节点仅</b>。
        <tr><td>参数</td>
            <td>指定的参数，如果所需的脚本。 要安装 Giraph 的脚本不需要任何参数，因此您可以将其留空。</td></tr>
    </table>    

    您可以添加多个安装在群集上的多个组件的脚本操作。 添加脚本后，单击以启动群集资源调配的选中标记。

您还可以使用脚本使用 Azure PowerShell 或 HDInsight.NET SDK 在 HDInsight 上安装 Giraph。 在本主题后面提供了有关这些过程的说明。

## <a name="usegiraph"></a>如何在 HDInsight 中使用 Giraph？

我们使用 SimpleShortestPathsComputation 示例演示基本<a href = "http://people.apache.org/~edwardyoon/documents/pregel.pdf">Pregel</a>寻找图中的对象之间的最短路径的实现。 使用以下步骤上载示例数据和示例 jar，SimpleShortestPathsComputation 的例子，来运行作业，然后查看结果。

1. 将示例数据文件上载到 Azure Blob 存储。 在您的本地工作站上创建一个名为**tiny_graph.txt**的新文件。 它应该包含以下行︰

        [0,0,[[1,1],[3,3]]]
        [1,0,[[0,1],[2,2],[3,1]]]
        [2,0,[[1,2],[4,4]]]
        [3,0,[[0,3],[1,1],[4,4]]]
        [4,0,[[3,4],[2,4]]]

    将 tiny_graph.txt 文件上传到 HDInsight 群集的主存储。 有关如何上载数据的说明，请参阅[HDInsight 中的 Hadoop 作业的数据上载](hdinsight-upload-data.md)。

    此数据描述了有向图，使用的格式中的对象之间的关系 [源\_id、 源\_值，[[目标\_id]，[边缘\_值]，...]]。 每行表示之间的关系**源\_id**对象和一个或多个**目标\_id**对象。 **边缘\_值**（或粗细） 可以被认为是强度或**source_id**之间的连接的距离和**目标\_id**。

    绘制出、 并用作对象之间的距离的值 （或称重），上面的数据可能如下所示︰

    ![绘制圆行不同距离为 tiny_graph.txt](./media/hdinsight-hadoop-giraph-install-v1/giraph-graph.png)

    

4. 运行 SimpleShortestPathsComputation 的示例。 使用下面的 Azure PowerShell cmdlet 要运行该示例通过使用 tiny_graph.txt 文件作为输入。 这要求您已安装并配置了[Azure PowerShell][powershell 安装]。

        $clusterName = "clustername"
        # Giraph examples jar
        $jarFile = "wasb:///example/jars/giraph-examples.jar"
        # Arguments for this job
        $jobArguments = "org.apache.giraph.examples.SimpleShortestPathsComputation",
                        "-ca", "mapred.job.tracker=headnodehost:9010",
                        "-vif", "org.apache.giraph.io.formats.JsonLongDoubleFloatDoubleVertexInputFormat",
                        "-vip", "wasb:///example/data/tiny_graph.txt",
                        "-vof", "org.apache.giraph.io.formats.IdWithValueTextOutputFormat",
                        "-op",  "wasb:///example/output/shortestpaths",
                        "-w", "2"
        # Create the definition
        $jobDefinition = New-AzureHDInsightMapReduceJobDefinition
          -JarFile $jarFile
          -ClassName "org.apache.giraph.GiraphRunner"
          -Arguments $jobArguments
        
        # Run the job, write output to the Azure PowerShell window
        $job = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $jobDefinition
        Write-Host "Wait for the job to complete ..." -ForegroundColor Green
        Wait-AzureHDInsightJob -Job $job
        Write-Host "STDERR"
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $job.JobId -StandardError
        Write-Host "Display the standard output ..." -ForegroundColor Green
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $job.JobId -StandardOutput

    在上面的示例中，您已安装的 Giraph 的 HDInsight 群集的名称替换**群集名称**。

5. 查看结果。 完成作业后，结果将存储在中的两个输出文件中__wasb: / 示例/出/shotestpaths__文件夹。 这些文件称为__m 的一部分 00001__和__部件-m-00002__。 执行以下步骤来下载并查看输出︰

        $subscriptionName = "<SubscriptionName>"       # Azure subscription name
        $storageAccountName = "<StorageAccountName>"   # Azure Storage account name
        $containerName = "<ContainerName>"             # Blob storage container name

        # Select the current subscription
        Select-AzureSubscription $subscriptionName
        
        # Create the Storage account context object
        $storageAccountKey = Get-AzureStorageKey $storageAccountName | %{ $_.Primary }
        $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

        # Download the job output to the workstation
        Get-AzureStorageBlobContent -Container $containerName -Blob example/output/shortestpaths/part-m-00001 -Context $storageContext -Force
        Get-AzureStorageBlobContent -Container $containerName -Blob example/output/shortestpaths/part-m-00002 -Context $storageContext -Force

    这将在您的工作站上的当前目录中创建的__示例/输出/shortestpaths__目录结构，并将两个输出文件下载到该位置。

    使用__Cat__ cmdlet 来显示文件的内容︰ 

        Cat example/output/shortestpaths/part*

    输出应类似于以下内容︰


        0   1.0
        4   5.0
        2   2.0
        1   0.0
        3   1.0

    示例是硬编码在开头使用 SimpleShortestPathComputation 对象 ID 为 1，并查找其他对象的最短路径。 因此应作为读取输出`destination_id distance`，其中距离是旅游过的对象 ID 1 和目标 ID 之间的边缘值 （或称重）
    
    直观显示这种情况，可以通过旅行 ID 1 和所有其他对象之间的最短路径验证结果。 请注意，ID 1 和 ID 4 之间的最短路径 5。 这是之间的总距离<span style="color:orange">ID 1 和 3</span>，然后<span style="color:red">ID 3 和 4</span>。

    ![用绘制之间的最短路径为圆形绘制的对象](./media/hdinsight-hadoop-giraph-install-v1/giraph-graph-out.png) 


## <a name="usingPS"></a>通过使用 Azure PowerShell HDInsight Hadoop 群集上安装 Giraph

在本节中，我们使用**<a href = "http://msdn.microsoft.com/library/dn858088.aspx" target="_blank">添加 AzureHDInsightScriptAction</a>**的 cmdlet 来调用脚本，方法是使用脚本的操作自定义群集。 继续之前，请确保您已安装并配置 Azure PowerShell。 有关配置工作站运行 HDInsight Azure Powershell cmdlet 的信息，请参阅[安装和配置 Azure PowerShell][powershell 安装]。

执行以下步骤︰

1. 打开 Azure PowerShell 窗口并声明以下变量︰

        # Provide values for these variables
        $subscriptionName = "<SubscriptionName>"        # Name of the Azure subscription
        $clusterName = "<HDInsightClusterName>"         # HDInsight cluster name
        $storageAccountName = "<StorageAccountName>"    # Azure Storage account that hosts the default container
        $storageAccountKey = "<StorageAccountKey>"      # Key for the Storage account
        $containerName = $clusterName
        $location = "<MicrosoftDataCenter>"             # Location of the HDInsight cluster. It must be in the same data center as the Storage account.
        $clusterNodes = <ClusterSizeInNumbers>          # Number of nodes in the HDInsight cluster
        $version = "<HDInsightClusterVersion>"          # For example, "3.1"
    
2. 指定的配置值，例如在群集和要使用的默认存储节点︰

        # Specify the configuration options
        Select-AzureSubscription $subscriptionName
        $config = New-AzureHDInsightClusterConfig -ClusterSizeInNodes $clusterNodes
        $config.DefaultStorageAccount.StorageAccountName="$storageAccountName.blob.core.windows.net"
        $config.DefaultStorageAccount.StorageAccountKey=$storageAccountKey
        $config.DefaultStorageAccount.StorageContainerName=$containerName
    
3. 使用**添加 AzureHDInsightScriptAction** cmdlet 将脚本操作添加到群集配置。 以后，创建群集时，会执行脚本操作。 

        # Add a script action to the cluster configuration
        $config = Add-AzureHDInsightScriptAction -Config $config -Name "Install Giraph" -ClusterRoleCollection HeadNode -Uri https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1

    **添加 AzureHDInsightScriptAction** cmdlet 的参数如下︰

    <table style="border-color: #c6c6c6; border-width: 2px; border-style: solid; border-collapse: collapse;">
    <tr>
    <th style="border-color: #c6c6c6; border-width: 2px; border-style: solid; border-collapse: collapse; width:90px; padding-left:5px; padding-right:5px;">参数</th>
    <th style="border-color: #c6c6c6; border-width: 2px; border-style: solid; border-collapse: collapse; width:550px; padding-left:5px; padding-right:5px;">定义</th></tr>
    <tr>
    <td style="border-color: #c6c6c6; border-width: 2px; border-style: solid; border-collapse: collapse; padding-left:5px;">配置</td>
    <td style="border-color: #c6c6c6; border-width: 2px; border-style: solid; border-collapse: collapse; padding-left:5px; padding-right:5px;">操作信息添加到哪个脚本的配置对象。</td></tr>
    <tr>
    <td style="border-color: #c6c6c6; border-width: 2px; border-style: solid; border-collapse: collapse; padding-left:5px;">名称</td>
    <td style="border-color: #c6c6c6; border-width: 2px; border-style: solid; border-collapse: collapse; padding-left:5px;">脚本操作的名称。</td></tr>
    <tr>
    <td style="border-color: #c6c6c6; border-width: 2px; border-style: solid; border-collapse: collapse; padding-left:5px;">ClusterRoleCollection</td>
    <td style="border-color: #c6c6c6; border-width: 2px; border-style: solid; border-collapse: collapse; padding-left:5px;">在其运行的自定义脚本的节点。 有效的值是 HeadNode （要在头节点上安装） 或 DataNode （若要在数据的所有节点上安装）。 您可以使用任意一个或两个值。</td></tr>
    <tr>
    <td style="border-color: #c6c6c6; border-width: 2px; border-style: solid; border-collapse: collapse; padding-left:5px;">Uri</td>
    <td style="border-color: #c6c6c6; border-width: 2px; border-style: solid; border-collapse: collapse; padding-left:5px;">为执行该脚本的 URI。</td></tr>
    <tr>
    <td style="border-color: #c6c6c6; border-width: 2px; border-style: solid; border-collapse: collapse; padding-left:5px;">参数</td>
    <td style="border-color: #c6c6c6; border-width: 2px; border-style: solid; border-collapse: collapse; padding-left:5px;">所需的脚本的参数。 使用本主题中的示例脚本不需要任何参数，并且因此看不到上述代码段中的此参数。
    </td></tr>
    </table>
    
4. 最后，开始用 Giraph 安装中设置自定义的群集︰  
    
        # Start provisioning a cluster with Giraph installed
        New-AzureHDInsightCluster -Config $config -Name $clusterName -Location $location -Version $version 

出现提示时，输入群集的凭据。 它可能需要几分钟才能创建群集。


## <a name="usingSDK"></a>通过使用.NET SDK HDInsight Hadoop 群集上安装 Giraph

HDInsight.NET SDK 提供了更方便地使用.NET Framework 应用程序中的 HDInsight 的.NET 客户端库。 本节说明如何使用 SDK 中的脚本操作来配置已安装的 Giraph 群集。 必须执行以下过程︰

- 安装 HDInsight.NET SDK
- 创建自行签署式证书
- 创建一个控制台应用程序
- 运行应用程序


**若要安装 HDInsight.NET SDK**

您可以从[NuGet](http://nuget.codeplex.com/wikipage?title=Getting%20Started)安装 SDK 的已发布的版本。 说明将显示在下一个过程。

**若要创建自签名的证书**

创建自签名的证书，将其安装在您的工作站，并将其上载到 Azure 订购。 有关说明，请参阅[创建自签名证书](http://go.microsoft.com/fwlink/?LinkId=511138)。 


**若要创建一个 Visual Studio 应用程序**

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
    <td style="border-color: #c6c6c6; border-width: 2px; border-style: solid; border-collapse: collapse; padding-left:5px;">CreateGiraphCluster</td></tr>
    </table>

4. 单击**确定**以创建项目。

5. 从**工具**菜单上，单击**Nuget 程序包管理器**，然后单击**程序包管理器控制台**。

6. 在控制台安装包中运行以下命令︰

        Install-Package Microsoft.WindowsAzure.Management.HDInsight

    此命令将添加.NET 库和对它们从当前 Visual Studio 项目的引用。

7. 从解决方案资源管理器中，双击**Program.cs**以将其打开。

8. 添加以下使用语句到文件的顶部︰

        using System.Security.Cryptography.X509Certificates;
        using Microsoft.WindowsAzure.Management.HDInsight;
        using Microsoft.WindowsAzure.Management.HDInsight.ClusterProvisioning;
        using Microsoft.WindowsAzure.Management.HDInsight.Framework.Logging;
    
9. 在 main （） 函数中，复制并粘贴下面的代码，并为变量提供值︰
        
        var clusterName = args[0];

        // Provide values for the variables
        string thumbprint = "<CertificateThumbprint>";  
        string subscriptionId = "<AzureSubscriptionID>";
        string location = "<MicrosoftDataCenterLocation>";
        string storageaccountname = "<AzureStorageAccountName>.blob.core.windows.net";
        string storageaccountkey = "<AzureStorageAccountKey>";
        string username = "<HDInsightUsername>";
        string password = "<HDInsightUserPassword>";
        int clustersize = <NumberOfNodesInTheCluster>;

        // Provide the certificate thumbprint to retrieve the certificate from the certificate store 
        X509Store store = new X509Store();
        store.Open(OpenFlags.ReadOnly);
        X509Certificate2 cert = store.Certificates.Cast<X509Certificate2>().First(item => item.Thumbprint == thumbprint);

        // Create an HDInsight client object
        HDInsightCertificateCredential creds = new HDInsightCertificateCredential(new Guid(subscriptionId), cert);
        var client = HDInsightClient.Connect(creds);
        client.IgnoreSslErrors = true;
        
        // Provide the cluster information
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

10. 将下面的代码附加到用于调用自定义脚本来安装 Giraph [ScriptAction](http://msdn.microsoft.com/library/microsoft.windowsazure.management.hdinsight.clusterprovisioning.data.scriptaction.aspx)类的 main （） 函数︰

        // Add the script action to install Giraph
        clusterInfo.ConfigActions.Add(new ScriptAction(
          "Install Giraph", // Name of the config action
          new ClusterNodeType[] { ClusterNodeType.HeadNode }, // List of nodes to install Giraph on
          new Uri("https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1"), // Location of the script to install Giraph
          null //Because the script used does not require any parameters
        ));

11. 最后，创建群集︰

        client.CreateCluster(clusterInfo);

12. 将更改保存到应用程序并生成解决方案。 

**若要运行该应用程序**

打开一个控制台，Azure PowerShell、 定位到您保存的 Visual Studio 项目的位置、 导航到 \bin\debug 目录内的项目，然后运行下面的命令︰

    .\CreateGiraphCluster <cluster-name>

提供群集的名称并按 enter 键以 Giraph 安装与配置群集。


## 请参见##
- [安装和使用 HDInsight 群集上激励]有关说明如何使用自定义群集安装和使用在 HDInsight Hadoop 群集上触发[hdinsight-安装触发]。 触发是开源的并行处理支持内存中处理大数据分析应用程序的性能提高的框架。
- [在 HDInsight 群集上安装 R][hdinsight-安装-r]如何使用自定义群集上安装和使用 R HDInsight Hadoop 群集提供指导。 R 是一种开源的语言和环境统计计算的。 它提供了数百个内置统计函数和自己相结合方面的功能和面向对象的编程的编程语言。 它还提供了丰富的图形功能。
- [在 HDInsight 群集上安装 Solr](hdinsight-hadoop-solr-install.md)。 使用自定义群集在 HDInsight Hadoop 群集上安装 Solr。 Solr 允许您执行存储数据的功能强大的搜索操作。



[工具]: https://github.com/Blackmist/hdinsight-tools
[接入点]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/

[powershell 安装]: ../powershell-install-configure.md
[hdinsight 规定]: hdinsight-provision-clusters.md
[hdinsight-安装-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-安装触发]: hdinsight-hadoop-spark-install.md
[hdinsight 群集自定义]: hdinsight-hadoop-customize-cluster.md
 
测试

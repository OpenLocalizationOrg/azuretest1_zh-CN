---
ms.openlocfilehash: 8be2e42c18da2076a3465ccf471503ddad6f4ea8
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="自定义 HDInsight 群集使用脚本操作 |Microsoft Azure" 
    description="了解如何自定义 HDInsight 群集使用脚本的操作。" 
    services="hdinsight" 
    documentationCenter="" 
    authors="Blackmist" 
    manager="paulettm" 
    editor="cgronlun"
    tags="azure-portal"/> 

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/21/2015" 
    ms.author="larryfr"/> 

# 自定义 HDInsight 群集使用脚本操作

HDInsight 提供了一个名为**脚本操作**，用于调用定义在资源调配过程中群集上执行的自定义项的自定义脚本的配置选项。 要在群集上安装其他软件或更改群集上的应用程序的配置，可以使用这些脚本。 

> [AZURE.NOTE] 这篇文章中的信息是特定于基于 Linux 的 HDInsight 群集。 本文基于 Windows 群集特定的版本，请参阅[自定义 HDInsight 群集使用脚本操作 (Windows)](hdinsight-hadoop-customize-cluster-linux.md)

## 脚本操作中群集资源调配过程

在创建群集时仅使用脚本的操作。 下图说明在资源调配过程中执行脚本操作时︰

![HDInsight 群集进行自定义和群集资源调配过程的阶段][img-hdi-cluster-states] 

HDInsight 配置时，运行该脚本。 在此阶段，该脚本显示群集中的所有指定节点上并行运行并在该节点上，使用超级用户权限运行。 

> [AZURE.NOTE] 因为您有群集节点时，该脚本就是根用户权限运行，您可以执行操作，如停止和启动服务，包括与 Hadoop 相关的服务。 如果停止服务，则必须确保 Ambari 服务和其它与 Hadoop 相关服务是否启动并运行该脚本完成运行之前。 这些服务需要在创建时成功地确定群集的状态和正常运行。

每个群集可以接受指定它们的顺序调用多个脚本操作。 可以在头节点和 / 或辅助节点上运行脚本。

> [AZURE.IMPORTANT] 脚本操作必须在 15 分钟内，完成或他们将会超时。

## 示例脚本操作脚本

从 Azure 预览门户、 Azure PowerShell 或 HDInsight.NET SDK，可以使用脚本操作脚本。 本文介绍如何从门户使用脚本的操作。 若要了解如何使用 PowerShell 和.NET SDK 来使用脚本操作，查看下表中列出的示例。

HDInsight 提供几个脚本 HDInsight 群集上安装以下组件︰

名称 | Script
----- | -----
**安装色调** | https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv01/install-hue-uber-v01.sh。 请参阅[色调在 HDInsight 上的安装和使用群集](hdinsight-hadoop-hue-linux.md)。
**安装触发** | https://hdiconfigactions.blob.core.windows.net/linuxsparkconfigactionv01/spark-installer-v01.sh。 请参阅[安装和使用 HDInsight 群集上的激励](hdinsight-hadoop-spark-install-linux.md)。
**R 安装** | https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh。 请参阅[安装和使用 R HDInsight 群集上](hdinsight-hadoop-r-scripts-linux.md)。
**安装 Solr** | https://hdiconfigactions.blob.core.windows.net/linuxsolrconfigactionv01/solr-installer-v01.sh。 [Solr 在 HDInsight 上的安装和使用群集](hdinsight-hadoop-solr-install-linux.md)，请参阅。
**安装 Giraph** | https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh。 请参阅[Giraph 在 HDInsight 上的安装和使用群集](hdinsight-hadoop-giraph-install-linux.md)。

##使用脚本的操作从 Azure 预览门户

1. 开始[调配使用自定义选项的群集](hdinsight-provision-clusters.md#portal)在所述配置群集。 

2. 在__可选配置__，**脚本操作**刀片式服务器，请单击**添加脚本操作**以提供详细信息的脚本操作，如下所示︰

    ![将操作脚本保存用于自定义群集](./media/hdinsight-hadoop-customize-cluster-linux/HDI.CreateCluster.8.png "Use Script Action to customize a cluster")
    
  	| 属性 | 值 |
  	| -------- | ----- |
  	| 名称 | 指定脚本动作的名称。 |
  	| 脚本的 URI | 指定调用自定义群集脚本的 URI。 |
  	| 头/工作人员 | 在运行自定义脚本时指定节点 （**头**、**工作人员**或**ZooKeeper**）。 |
  	| 参数 | 指定的参数，如果所需的脚本。 |

    按回车键以添加要在群集上安装多个组件的多个脚本操作。 

3. 单击**选择**保存脚本操作配置并继续与群集资源调配。 

##使用脚本的操作从 Azure 资源管理器模板

在本节中，我们使用 Azure 资源管理器 (ARM) 的模板来设置 HDInsight 群集，并且使用脚本操作在群集上安装自定义组件 (在此示例中，R)。 本部分提供示例 ARM 模板提供群集使用脚本的操作。 

### 在开始之前

* 有关配置工作站运行 HDInsight Powershell cmdlet 的信息，请参阅[安装和配置 Azure PowerShell](../powershell-install-configure.md)。 
* 有关如何创建 ARM 模板的说明，请参阅[创作 Azure 资源管理器模板](resource-group-authoring-templates.md)。 
* 如果您先前没有使用 Azure PowerShell 与资源管理器，请参阅[使用 Azure PowerShell 使用 Azure 资源管理器中](powershell-azure-resource-manager)。

### 使用脚本的操作设置群集

1. 将下面的模板复制到您的计算机上的某个位置。 此模板将在 headnode 上安装 R 以及工作人员与群集中的节点。 您还可以验证 JSON 模板是否有效。 将模板内容粘贴到[JSONLint](http://jsonlint.com/)，在线的 JSON 验证程序工具。

            {
            "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
            "contentVersion": "1.0.0.0",
            "parameters": {
                "clusterLocation": {
                    "type": "string",
                    "defaultValue": "West US",
                    "allowedValues": [ "West US" ]
                },
                "clusterName": {
                    "type": "string"
                },
                "clusterUserName": {
                    "type": "string",
                    "defaultValue": "admin"
                },
                "clusterUserPassword": {
                    "type": "securestring"
                },
                "sshUserName": {
                    "type": "string",
                    "defaultValue": "hdiuser"
                },
                "sshPassword": {
                    "type": "securestring"
                },
                "clusterStorageAccountName": {
                    "type": "string"
                },
                "clusterStorageAccountResourceGroup": { 
                    "type": "string"
                },
                "clusterStorageType": {
                    "type": "string",
                    "defaultValue": "Standard_LRS",
                    "allowedValues": [
                        "Standard_LRS",
                        "Standard_GRS",
                        "Standard_ZRS"
                    ]
                },
                "clusterStorageAccountContainer": {
                    "type": "string"
                },
                "clusterHeadNodeCount": {
                    "type": "int",
                    "defaultValue": 1
                },
                "clusterWorkerNodeCount": {
                    "type": "int",
                    "defaultValue": 2
                }
            },
            "variables": {
            },
            "resources": [
                {
                    "name": "[parameters('clusterStorageAccountName')]",
                    "type": "Microsoft.Storage/storageAccounts",
                    "location": "[parameters('clusterLocation')]",
                    "apiVersion": "2015-05-01-preview",
                    "dependsOn": [ ],
                    "tags": { },
                    "properties": {
                        "accountType": "[parameters('clusterStorageType')]"
                    }
                },
                {
                    "name": "[parameters('clusterName')]",
                    "type": "Microsoft.HDInsight/clusters",
                    "location": "[parameters('clusterLocation')]",
                    "apiVersion": "2015-03-01-preview",
                    "dependsOn": [
                        "[concat('Microsoft.Storage/storageAccounts/', parameters('clusterStorageAccountName'))]"
                    ],
                    "tags": { },
                    "properties": {
                        "clusterVersion": "3.2",
                        "osType": "Linux",
                        "clusterDefinition": {
                            "kind": "hadoop",
        
                            "configurations": {
                                "gateway": {
                                    "restAuthCredential.isEnabled": true,
                                    "restAuthCredential.username": "[parameters('clusterUserName')]",
                                    "restAuthCredential.password": "[parameters('clusterUserPassword')]"
                                }
                            }
                        },
                        "storageProfile": {
                            "storageaccounts": [
                                {
                                    "name": "[concat(parameters('clusterStorageAccountName'),'.blob.core.windows.net')]",
                                    "isDefault": true,
                                    "container": "[parameters('clusterStorageAccountContainer')]",
                                    "key": "[listKeys(resourceId(parameters('clusterStorageAccountResourceGroup'), 'Microsoft.Storage/storageAccounts', parameters('clusterStorageAccountName')), providers('Microsoft.Storage', 'storageAccounts').apiVersions[0]).key1]"
                                }
                            ]
                        },
                        "computeProfile": {
                            "roles": [
                                {
                                    "name": "headnode",
                                    "targetInstanceCount": "[parameters('clusterHeadNodeCount')]",
                                    "hardwareProfile": {
                                        "vmSize": "Large"
                                    },
                                    "osProfile": {
                                        "linuxOperatingSystemProfile": {
                                            "username": "[parameters('sshUserName')]",
                                            "password": "[parameters('sshPassword')]"
                                        }
                                    },
                                    "scriptActions": [
                                        {
                                            "name": "installR",
                                            "uri": "https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh",
                                            "parameters": ""
                                        }
                                    ]
                                },
                                {
                                    "name": "workernode",
                                    "targetInstanceCount": "[parameters('clusterWorkerNodeCount')]",
                                    "hardwareProfile": {
                                        "vmSize": "Large"
                                    },
                                    "osProfile": {
                                        "linuxOperatingSystemProfile": {
                                            "username": "[parameters('sshUserName')]",
                                            "password": "[parameters('sshPassword')]"
                                        }
                                    },
                                    "scriptActions": [
                                        {
                                            "name": "installR",
                                            "uri": "https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh",
                                            "parameters": ""
                                        }
                                    ]
                                }
                            ]
                        }
                    }
                }
            ],
            "outputs": {
                "cluster":{
                    "type" : "object",
                    "value" : "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
                }
            }
        }


    
2. 到 Azure 帐户启动 Azure PowerShell 和日志中。 在让您的凭据后，该命令将返回有关您的帐户信息。

        Add-AzureAccount
    
        Id                             Type       ...
        --                             ----    
        someone@example.com            User       ...   

3. 如果您有多个订阅，提供您想要用于部署的订阅 id。

        Select-AzureSubscription -SubscriptionID <YourSubscriptionId>

4. 切换到 Azure 资源管理器模块。

        Switch-AzureMode AzureResourceManager

5. 如果您没有现有资源组，创建新的资源组。 提供的资源组，您需要为您的解决方案的位置的名称。 返回新的资源组的摘要。

        New-AzureResourceGroup -Name myresourcegroup -Location "West US"

        ResourceGroupName : myresourcegroup
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                            Actions  NotActions
                            =======  ==========
                            *
        ResourceId        : /subscriptions/######/resourceGroups/ExampleResourceGroup


6. 要创建新部署的资源组，请运行**新建 AzureResourceGroupDeployment**命令并提供必需的参数。 参数将包括您的部署，您的资源组的路径或 URL 的名称为您创建的模板的名称。 如果您的模板所需的任何参数，则必须传递这些参数也。 在这种情况下，要安装在群集上的 R 的脚本操作不需要任何参数。


        New-AzureResourceGroupDeployment -Name mydeployment -ResourceGroupName myresourcegroup -TemplateFile <PathOrLinkToTemplate>


    系统将提示您在模板中定义的参数提供值。

7. 在已部署的资源组中，您将看到部署的摘要。

          DeploymentName    : mydeployment
          ResourceGroupName : myresourcegroup
          ProvisioningState : Succeeded
          Timestamp         : 8/17/2015 7:00:27 PM
          Mode              : Incremental
          ... 

8. 如果部署失败，可以使用以下 cmdlet 以获取有关失败的信息。

        Get-AzureResourceGroupLog -ResourceGroup myresourcegroup -Status Failed

    有关部署失败的详细信息，使用以下 cmdlet。

        Get-AzureResourceGroupLog -ResourceGroup myresourcegroup -Status Failed -DetailedOutput

##使用脚本的操作从 Azure PowerShell

在本节中，我们使用**<a href = "http://msdn.microsoft.com/library/dn858088.aspx" target="_blank">添加 AzureHDInsightScriptAction</a>**的 cmdlet 来调用脚本，方法是使用脚本的操作自定义群集。 继续之前，请确保您已安装并配置 Azure PowerShell。 有关配置工作站运行 HDInsight Powershell cmdlet 的信息，请参阅[安装和配置 Azure PowerShell](../powershell-install-configure.md)。

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
    
3. 使用**添加 AzureHDInsightScriptAction** cmdlet 来调用该脚本。 下面的示例使用在群集上安装 R 的脚本︰ 

        # INVOKE THE SCRIPT USING THE SCRIPT ACTION
        $config = Add-AzureHDInsightScriptAction -Config $config -Name "Install R"  -ClusterRoleCollection HeadNode,WorkerNode,ZookeeperNode -Uri https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh


    **添加 AzureHDInsightScriptAction** cmdlet 的参数如下︰

  	| 参数 | 定义 |
  	| --------- | ---------- |
  	| 配置 | 操作信息添加到哪个脚本的配置对象。 |
  	| 名称 | 脚本操作的名称。 |
  	| ClusterRoleCollection | 指定在其运行的自定义脚本的节点。 有效的值是**HeadNode** （要安装在头节点上）， **WorkerNode** （若要在数据的所有节点上安装），或**ZookeeperNode** （以 zookeeper 节点上安装）。 您可以使用其中一种或所有值。 |
  	| 参数 | 所需的脚本的参数。 |
  	| Uri | 指定执行该脚本的 URI。 |
    
4. 最后，配置该群集︰
    
        New-AzureHDInsightCluster -Config $config -Name $clusterName -Location $location -Version $version 

出现提示时，输入群集的凭据。 它可能需要几分钟才能创建群集。

##使用脚本的操作从 HDInsight.NET SDK

HDInsight.NET SDK 提供了便于更方便地使用 HDInsight 从.NET 应用程序的客户端库。 以下步骤演示了如何使用脚本自定义 HDInsight.NET SDK 中的群集。


> [AZURE.IMPORTANT] 必须首先创建一个自签名的证书，将其安装在您的工作站，然后将其上载到 Azure 订购。 有关说明，请参阅[创建自签名证书](http://go.microsoft.com/fwlink/?LinkId=511138)。 


###创建一个 Visual Studio 项目

1. 打开 Visual Studio 2013年或 2015年。

2. 从**文件**菜单上，单击**新建**，然后单击**项目**。

3. 从**新建项目**，键入或选择以下值︰
    
  	| 属性 | 值 |
  	| -------- | ----- |
  	| Category | 模板/视频 C# / 窗口 |
  	| Template | 控制台应用程序 |
  	| 名称 | ScriptActionCluster |

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

10. 将下面的代码附加到**main （）**函数。 此代码调用脚本的操作;在此示例中，要在群集上安装 R 的脚本︰

        // ADD THE SCRIPT ACTION TO INSTALL R

        clusterInfo.ConfigActions.Add(new ScriptAction(
            "Install R",
            new ClusterNodeType[] { ClusterNodeType.HeadNode, ClusterNodeType.DataNode },
            new Uri("https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh"), null
            ));

11. 最后，创建群集︰

        client.CreateCluster(clusterInfo);

11. 将更改保存到应用程序并生成解决方案。 

###运行应用程序

打开 Azure PowerShell 控制台、 定位到您保存项目的位置、 导航到 \bin\debug 目录内的项目，然后运行下面的命令︰

    .\ScriptActionCluster <cluster-name>

提供群集名称并按 enter 键来配置群集。

## 对 HDInsight 群集上使用的开放源码软件的支持

Microsoft Azure HDInsight 服务是使您能够通过使用 Hadoop 周围形成的开放源代码技术体系构建大数据在云中的应用程序的灵活平台。 Microsoft Azure [Azure 支持常见问题网站](http://azure.microsoft.com/support/faq/)的**支持范围**部分中所述，开放源码技术，提供一般意义上的支持。 HDInsight 服务提供额外的支持的某些组件，如下所述。

有两种类型的 HDInsight 服务中可用的开源的组件︰

- **内置组件**-HDInsight 群集上预先安装了这些组件，并提供核心功能的群集。 例如，YARN ResourceManager、 配置单元查询语言 (HiveQL) 和 Mahout 库都属于此类别。 群集组件的完整列表位于[由 HDInsight 提供的 Hadoop 群集版本中的新增功能？](hdinsight-component-versioning.md)。

- **自定义组件**，以在群集中，用户可以安装或使用您的工作负载在社区中提供或由您创建的任何组件。

完全支持内置组件，并可以帮助 Microsoft 技术支持来隔离并解决与这些组件相关的问题。

自定义组件接收商业上合理的支持，以帮助您进一步排查该问题。 这可能导致解决问题或要求您能够进行深入的专业技能，为该技术在其中找到的开放源代码技术可用的频道。 例如，有许多社区站点可以使用，如︰ 

* [HDInsight 的 MSDN 论坛](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight)

* [堆栈溢出](https://stackoverflow.com)

此外，Apache 项目具有项目网站上[Apache.org](https://apache.org);例如， [Hadoop](http://hadoop.apache.org/)并[触发](http://spark.apache.org/)。

HDInsight 服务提供了多种方法用于自定义组件。 不管如何使用或在群集上安装组件，适用于相同级别的支持。 下面是最常见的方式，可以在 HDInsight 群集上使用自定义组件的列表︰

1. 可以将作业提交的 Hadoop 或其他类型的作业的执行或使用自定义组件提交到群集。

2. 群集的自定义-在群集创建过程中您可以指定其他设置和自定义将在群集节点安装的组件。

3. 示例-常见的自定义组件、 Microsoft 和其他公司可能会提供如何在 HDInsight 群集上使用这些组件的示例。 不支持的情况下提供了这些示例。

##疑难解答

Ambari web 用户界面可用于查看群集资源调配过程中由脚本记录的信息。

1. 在浏览器中，导航到 https://CLUSTERNAME.azurehdinsight.net。 群集名称替换 HDInsight 群集的名称。

    出现提示时，输入群集 （管理员） 的管理员帐户名和密码。 您可能需要重新输入 web 窗体中的管理员凭据。
    
2. 从页面顶部的栏，选择__操作__项。 这将显示当前和以前通过 Ambari 群集上执行的操作的列表。

    ![Ambari web UI 栏以选定的操作](./media/hdinsight-hadoop-customize-cluster-linux/ambari-nav.png)
    
3. 查找条目具有__运行\_customscriptaction__在__操作__列中。 这些脚本操作在运行时创建。

    ![操作的屏幕抓图](./media/hdinsight-hadoop-customize-cluster-linux/ambariscriptaction.png)

    选择此项，并向下钻取链接以查看生成脚本时的标准输出和 STDERR 输出运行在群集上。

## 下一步行动

有关创建和使用脚本自定义群集，请参阅下列信息和示例︰

- [HDInsight 为开发脚本操作脚本](hdinsight-hadoop-script-actions-linux.md)
- [安装和使用 HDInsight 群集上触发](hdinsight-hadoop-spark-install-linux.md)
- [上安装和使用 R HDInsight 群集](hdinsight-hadoop-r-scripts-linux.md)
- [上安装和使用 Solr HDInsight 群集](hdinsight-hadoop-solr-install-linux.md)
- [安装和使用 HDInsight 群集上的 Giraph](hdinsight-hadoop-giraph-install-linux.md)



[img hdi 群集状态]: ./media/hdinsight-hadoop-customize-cluster-linux/HDI-Cluster-state.png "在群集资源调配过程的阶段"
 
测试

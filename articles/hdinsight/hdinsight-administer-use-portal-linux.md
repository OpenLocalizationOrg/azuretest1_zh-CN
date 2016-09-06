---
ms.openlocfilehash: 4ef8d5d4a7373cc717920cac73ebb3774726cd13
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="管理 Hadoop 群集中使用 Azure 门户 HDInsight |Microsoft Azure"
    description="了解如何管理 HDInsight 服务。 创建 HDInsight 群集，打开交互式的 JavaScript 控制台，打开 Hadoop 命令控制台。"
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
    ms.date="08/06/2015"
    ms.author="larryfr"/>

# 通过使用 Azure 预览门户管理中 HDInsight 的 Hadoop 群集

[AZURE.INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]


使用[Azure 预览门户网站][预览门户]，可以调配和管理基于 Linux 的 Hadoop 群集在 Azure HDInsight。

> [AZURE.NOTE] 此文档中的步骤适用于使用基于 Linux 的 Hadoop 群集。 使用基于 Windows 群集的信息，请参阅[管理 Hadoop 群集中使用 Azure 预览门户 HDInsight](hdinsight-administer-use-management-portal.md)


[AZURE.INCLUDE [preview-portal](../../includes/hdinsight-azure-preview-portal-nolink.md)]


## 其他工具，以便管理 HDInsight
还有其他工具可用除了 Azure 门户管理 HDInsight。

- [使用 Azure CLI 管理 HDInsight](hdinsight-administer-use-command-line.md): Azure CLI 是一个跨平台命令行工具，允许您管理 Azure 服务

- [使用 Azure PowerShell 管理 HDInsight](hdinsight-administer-use-powershell.md): Azure PowerShell 提供 PowerShell cmdlet 管理 Azure 服务

##先决条件

在开始这篇文章之前，您必须具有以下︰

- **Azure 订阅**。 请参阅[获取 Azure 免费试用版](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)

##条款 HDInsight 群集

可以通过使用以下步骤设置从 Azure 门户 HDInsight 群集︰

1. 登录到[Azure 预览门户网站][预览门户]。

2. 选择**新建**，选择__数据分析__，然后选择__HDInsight__

    ![在 Azure 预览门户网站中创建一个新的群集](./media/hdinsight-administer-use-portal-linux/new-cluster.png)

3. 输入的__群集名称__，然后选择您想要创建的__群集类型__。 如果可用，__群集名称__的旁边将出现绿色复选标记。

    ![群集名称、 群集类型和操作系统类型](./media/hdinsight-administer-use-portal-linux/clustername.png)
    
    > [AZURE.NOTE] Hadoop 群集类型是可用的基于 Linux 的 HDInsight 预览。

4. 如果您有多个订阅，则选择__订阅__条目选择 Azure 用于群集的订阅。

5. __资源组__，您可以选择查看现有资源组的列表，然后选择要创建的群集中的一个条目。 或者，您可以选择__新建__，然后输入新的资源组的名称。 绿色复选标记将显示以指示新组的名称是否可用。

    > [AZURE.NOTE] 如果有的话，此项将默认为您现有的资源组之一。

6. 选择__凭据__，然后输入__群集登录用户名__的__群集的登录密码__。 此外必须输入__SSH 用户名__和__密码__或__公钥__，它将使用 SSH 用户进行身份验证。 最后，使用__选择__按钮以设置凭据。 远程桌面将不采用在此文档中，以便您可以将其禁用。

    ![群集的凭据刀片式服务器](./media/hdinsight-administer-use-portal-linux/clustercredentials.png)

    在 HDInsight 中使用 SSH 的详细信息，请参阅下列文章︰

    * [HDInsight 从 Linux、 Unix 或 OS X 上的基于 Linux 的 Hadoop 使用 SSH](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [在从 Windows HDInsight 基于 Linux 的 Hadoop 使用 SSH](hdinsight-hadoop-linux-use-ssh-windows)

6. 为__数据源__，可以选择的项中选择现有的数据源，或新建一个。

    ![数据源刀片式服务器](./media/hdinsight-administer-use-portal-linux/datasource.png)
    
    目前您可以用作数据源的 HDInsight 群集选择 Azure 存储帐户。 使用以下方法来了解__数据源__刀片式服务器上的项。
    
    - __选择方法__︰ 将此值设置为__所有订阅__启用浏览在您的订阅的存储帐户。 如果您要输入的__存储名称__和现有存储帐户的__访问键__，设置为__访问键__。
    
    - __新建__︰ 用于创建新的存储帐户。 使用字段显示输入存储帐户的名称。 如果的名称可用，则会出现绿色复选标记。
    
    - __选择默认容器__︰ 用于输入要用于群集的默认容器的名称。 您可以输入任何名称，我们建议为群集使用相同的名称，以便您可以轻松地识别该容器用于此特定群集。
    
    - __位置__︰ 存储帐户将地理区域中，或将被创建。
    
        > [AZURE.IMPORTANT] 选择默认的数据源的位置还会设置 HDInsight 群集的位置。 在同一区域必须位于群集和默认数据源。
        
    - __选择__︰ 用于保存数据源配置。
    
7. 选择要显示的节点，则为此群集创建信息__节点定价层__。 默认情况下，辅助节点数量将设置为__4__。 

    此刀片的底部将显示群集的估计的成本。

    ![节点定价层刀片式服务器](./media/hdinsight-administer-use-portal-linux/nodepricingtiers.png)
    
    使用__选择__按钮以保存__节点定价层__的信息。

8. 选择__可选的配置__。 此刀片式服务器允许您配置下列各项︰

    * __HDInsight 版本__︰ HDInsight 群集所用的版本。 HDInsight 版本控制的详细信息，请参阅[HDInsight 组件版本控制](hdinsight-component-versioning.md)
    * __外部的 Metastores__︰ 这使您可以选择一个 SQL 数据库，它将用于存储 Oozie 并配置单元的配置信息。 这使您可以重用配置时删除并重新创建群集，而无需重新创建该配置单元和 Oozie 配置每次。
    *__Azure 存储键__︰ 这使您可以将附加的存储帐户与 HDInsight 服务器相关联。
    
        > [AZURE.NOTE] HDInsight 只能访问 Azure 存储帐户用作默认数据存储区，配置此部分中，通过添加或可公开访问。

    ![可选配置刀片式服务器](./media/hdinsight-administer-use-portal-linux/optionalconfiguration.png)

9. 确保__锁定到 Startboard__选中，然后选择__创建__。 这会创建群集，并到 Azure 门户网站 Startboard 为其添加一个图块。 该图标将指示群集资源调配，并完成资源调配后显示的 HDInsight 图标将会更改。

  	| 在资源调配时 | 设置完成 |
  	| ------------------ | --------------------- |
  	| ![资源调配上 startboard 的指示器](./media/hdinsight-administer-use-portal-linux/provisioning.png) | ![提供的群集平铺](./media/hdinsight-administer-use-portal-linux/provisioned.png) |

    > [AZURE.NOTE] 它将需要一些时间为群集创建，通常大约 15 分钟。 使用 Startboard 或__通知__条目左侧的页上平铺在资源调配过程检查。

##管理群集

从 Azure 预览门户选择群集将显示有关该群集，如名称、 资源组、 操作系统和群集控制板 （用于 Linux 群集访问 Ambari 网站。） 的 URL 的基本信息

![群集的详细信息](./media/hdinsight-administer-use-portal-linux/clusterdetails.png)

使用以下方法来理解此刀片式服务器，然后在__重点__与__快速链接__部分顶部的图标︰

* __设置__和__所有设置__︰ 显示__设置__刀片式服务器群集，以便您可以访问该群集的详细的配置信息。

* __仪表板__、__群集的仪表板__和__URL__︰ 这些是访问群集控制板，用于基于 Linux 的群集是 Ambari 网站的所有方法。

* __安全外壳__︰ 访问群集使用 SSH 所需的信息。

* __扩展群集__︰ 允许您更改工作人员为此群集的节点数。

* __删除__︰ 删除 HDInsight 群集。

* __快速入门 (![云和 thunderbolt 图标 = 快速入门](./media/hdinsight-administer-use-portal-linux/quickstart.png))__︰ 显示的信息将帮助您开始使用 HDInsight。

* __用户 (![用户图标](./media/hdinsight-administer-use-portal-linux/users.png))__︰ 允许您在 Azure 订阅为其他用户设置_门户_管理该群集的权限。

    > [AZURE.IMPORTANT] 这_只_影响访问和向在 Azure 预览门户中，该群集的权限，谁可以连接到或将作业提交到 HDInsight 群集没有影响。

* __标记 (![标记图标](./media/hdinsight-administer-use-portal-linux/tags.png))__︰ 标记允许您设置键/值对来定义自定义分类的云服务。 例如，可能会创建一个密钥，该密钥命名__项目__，然后与特定项目关联的所有服务都使用一个公共值。

* __文档__︰ Azure HDInsight 的文档的链接。

> [AZURE.IMPORTANT] 要管理所提供的 HDInsight 群集服务，必须使用 Ambari Web 或 Ambari REST API。 使用 Ambari 的详细信息，请参阅[管理 HDInsight 群集使用 Ambari](hdinsight-hadoop-manage-ambari.md)。

##监视群集

__使用__HDInsight 群集刀片明 dislays 核心供使用 HDInsight，您订购的数量，以及分配给该群集和它们为此群集中的节点的分配方式的内核数有关的信息。

![使用信息](./media/hdinsight-administer-use-portal-linux/usage.png)

> [AZURE.IMPORTANT] 若要监视 HDInsight 群集所提供的服务，必须使用 Ambari Web 或 Ambari REST API。 使用 Ambari 的详细信息，请参阅[使用 Ambari 管理 HDInsight 群集](hdinsight-hadoop-manage-ambari.md)

##下一步行动
在本文中，您学习了如何创建一个 HDInsight 群集使用 Azure 的门户，以及如何打开 Hadoop 命令行工具。 若要了解详细信息，请参阅下列文章︰

* [管理使用 Azure PowerShell 的 HDInsight](hdinsight-administer-use-powershell.md)
* [HDInsight 使用 Azure CLI 管理](hdinsight-administer-use-command-line.md)
* [条款 HDInsight 群集](hdinsight-provision-clusters.md)
* [以编程方式提交 Hadoop 作业](hdinsight-submit-hadoop-jobs-programmatically.md)
* [开始使用 Azure HDInsight](../hdinsight-get-started.md)
* [Hadoop 的版本是 Azure HDInsight？](hdinsight-component-versioning.md)

[预览门户]: https://portal.azure.com
测试t

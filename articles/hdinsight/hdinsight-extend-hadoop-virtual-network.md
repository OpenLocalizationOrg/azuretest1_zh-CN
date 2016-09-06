---
ms.openlocfilehash: 581c2b5f0120707df7aaaf1215a4aa58989fde6c
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="扩展虚拟网络与 HDInsight |Microsoft Azure"  
    description="了解如何使用 Azure 虚拟网络连接到其他云资源或在您的数据中心的资源的 HDInsight"
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="paulettm"
    editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="07/06/2015"
   ms.author="larryfr"/>


#通过使用 Azure 虚拟网络扩展 HDInsight 功能

Azure 的虚拟网络允许您扩展您的 Hadoop 解决方案来结合本地资源，如 SQL Server 或创建安全的专用网络之间在云中的资源。

> [AZURE.NOTE] HDInsight 不支持基于相似性的 Azure 的虚拟网络。 在使用 HDInsight 时，您必须使用基于位置的虚拟网络。

##<a id="whatis"></a>什么是 Azure 虚拟网络？

[Azure 虚拟网络](/documentation/services/virtual-network/)允许您创建安全、 持久网络包含针对您的解决方案所需的资源。 虚拟的网络，您可以︰

* 连接在一起在专用网络 （仅云） 云资源。

    ![仅云配置的关系图](media/hdinsight-extend-hadoop-virtual-network/cloud-only.png)

    使用虚拟网络链接使用 Azure HDInsight Azure 服务启用以下方案︰

    * **调用 HDInsight 服务或作业**从 Azure 网站或 Azure 的虚拟机中运行的服务。

    * HDInsight 和 SQL Azure 数据库，SQL Server 或另一个虚拟机上运行的数据存储解决方案之间的**直接数据传输**。

    * 到一个解决方案中**组合多个 HDInsight 服务器**。 一个示例是使用 HDInsight 风暴服务器以使用传入的数据，然后存储到 HDInsight HBase 服务器处理的数据。 原始数据也可能到 HDInsight Hadoop 服务器以便以后分析使用 MapReduce 存储。

* 通过使用虚拟专用网络 (VPN) 连接到您的本地数据中心网络 （站点到站点或点到站点） 云资源。

    站点的配置允许您将多个资源从您的数据中心连接到 Azure 的虚拟网络，通过硬件 VPN 或路由和远程访问服务。

    ![图中的站点对站点配置](media/hdinsight-extend-hadoop-virtual-network/site-to-site.png)

    点到站点配置允许您通过使用 VPN 软件连接到 Azure 的虚拟网络的特定资源。

    ![图中的点到站点配置](media/hdinsight-extend-hadoop-virtual-network/point-to-site.png)

    使用虚拟网络链接云，您的数据中心，使到仅云配置类似的方案。 但是，而不再局限于使用云中的资源，您还可以使用资源在您的数据中心。

    * HDInsight 和数据中心之间的**直接数据传输**。 一个示例使用 Sqoop 来将 SQL Server 或读取数据生成的业务线 (LOB) 应用程序之间的数据传输。

    * **调用 HDInsight 服务或作业**从 LOB 应用程序。 一个示例使用 HBase Java Api 来存储和检索数据来自 HDInsight HBase 群集。

虚拟网络的功能、 好处和功能的详细信息，请参阅[Azure 虚拟网络概述](../virtual-network/virtual-networks-overview.md)。

> [AZURE.NOTE] 资源调配 HDInsight 群集之前，必须创建 Azure 的虚拟网络。 有关详细信息，请参阅[虚拟网络配置任务](/documentation/services/virtual-network/)。
>
> Azure HDInsight 支持仅基于位置的虚拟网络，并且当前不使用基于相似性组的虚拟网络。
>
> 强烈建议将为每个群集的单个子网。

资源调配虚拟网络上的 HDInsight 群集的详细信息，请参阅[HDInsight 中的资源调配 Hadoop 群集](hdinsight-provision-clusters.md)。

##<a id="tasks"></a>任务和信息

本部分包含常见任务的信息和虚拟的网络中使用 HDInsight 时，您可能需要的信息。

###确定 FQDN

HDInsight 群集将被分配的虚拟网络接口特定完全合格的域名称 (FQDN)。 这是连接到群集中虚拟的网络上的其他资源时，应使用的地址。 要确定 FQDN，请使用以下 URL 查询 Ambari 管理服务︰

    https://<clustername>.azurehdinsight.net/ambari/api/v1/clusters/<clustername>.azurehdinsight.net/services/<servicename>/components/<componentname>

> [AZURE.NOTE] 在 Ambari 中使用 HDInsight 的详细信息，请参阅[监视 Hadoop 群集中使用 Ambari API 的 HDInsight](hdinsight-monitor-use-ambari-api.md)。

必须指定群集名称、 服务和群集，如 YARN 资源管理器上运行的组件。

> [AZURE.NOTE] 返回的数据是信息的 JavaScript 对象符号 (JSON) 文档，其中包含了大量有关组件。 要提取只 FQDN，应使用 JSON 解析器来检索`host_components[0].HostRoles.host_name`值。

例如，若要返回从 HDInsight Hadoop 群集的 FQDN，可用于以下方法之一 YARN 资源管理器中检索数据︰

* [Azure PowerShell](../powershell-install-configure.md)

        $ClusterDnsName = <clustername>
        $Username = <cluster admin username>
        $Password = <cluster admin password>
        $DnsSuffix = ".azurehdinsight.net"
        $ClusterFQDN = $ClusterDnsName + $DnsSuffix

        $webclient = new-object System.Net.WebClient
        $webclient.Credentials = new-object System.Net.NetworkCredential($Username, $Password)

        $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/services/yarn/     components/resourcemanager"
        $Response = $webclient.DownloadString($Url)
        $JsonObject = $Response | ConvertFrom-Json
        $FQDN = $JsonObject.host_components[0].HostRoles.host_name
        Write-host $FQDN

* [弯曲](http://curl.haxx.se/)和[jq](http://stedolan.github.io/jq/)

        curl -G -u <username>:<password> https://<clustername>.azurehdinsight.net/ambari/api/v1/clusters/<clustername>.azurehdinsight.net/services/yarn/components/resourcemanager | jq .host_components[0].HostRoles.host_name

###连接到 HBase

远程连接到 HBase 使用 Java API，您必须确定 HBase 群集 ZooKeeper 仲裁地址并指定此应用程序中。

要获取 ZooKeeper 仲裁地址，请使用以下方法之一来查询 Ambari 管理服务︰

* [Azure PowerShell](../powershell-install-configure.md)

        $ClusterDnsName = <clustername>
        $Username = <cluster admin username>
        $Password = <cluster admin password>
        $DnsSuffix = ".azurehdinsight.net"
        $ClusterFQDN = $ClusterDnsName + $DnsSuffix

        $webclient = new-object System.Net.WebClient
        $webclient.Credentials = new-object System.Net.NetworkCredential($Username, $Password)

        $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.quorum"
        $Response = $webclient.DownloadString($Url)
        $JsonObject = $Response | ConvertFrom-Json
        Write-host $JsonObject.items[0].properties.'hbase.zookeeper.quorum'

* [弯曲](http://curl.haxx.se/)和[jq](http://stedolan.github.io/jq/)

        curl -G -u <username>:<password> "https://<clustername>.azurehdinsight.net/ambari/api/v1/clusters/<clustername>.azurehdinsight.net/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.quorum" | jq .items[0].properties[]

> [AZURE.NOTE] 在 Ambari 中使用 HDInsight 的详细信息，请参阅[监视 Hadoop 群集中使用 Ambari API 的 HDInsight](hdinsight-monitor-use-ambari-api.md)。

一旦仲裁信息，则客户端应用程序中使用它。

例如，对于使用 HBase API 的 Java 应用程序，将**hbase site.xml**文件添加到项目并指定仲裁信息文件中，如下所示︰

```
<configuration>
  <property>
    <name>hbase.cluster.distributed</name>
    <value>true</value>
  </property>
  <property>
    <name>hbase.zookeeper.quorum</name>
    <value>zookeeper0.address,zookeeper1.address,zookeeper2.address</value>
  </property>
  <property>
    <name>hbase.zookeeper.property.clientPort</name>
    <value>2181</value>
  </property>
</configuration>
```

###验证网络连接性

某些服务，例如，SQL Server 可以将传入的网络连接限制。 这将防止 HDInsight 成功地使用这些服务。

如果您遇到从 HDInsight 访问服务时出现问题，请咨询服务，以确保您已启用了网络访问的文档。 您可以通过创建相同的虚拟网络，Azure 的虚拟机来验证网络访问和使用客户端实用程序来验证该虚拟机可以连接到该服务在虚拟的网络上。

##<a id="nextsteps"></a>下一步行动

下面的示例演示如何使用 HDInsight Azure 虚拟网络︰

* [分析传感器数据与风暴和 HBase 中 HDInsight](hdinsight-storm-sensor-data-analysis.md) -演示如何配置在虚拟网络中，风暴和 HBase 群集，以及如何从风暴向 HBase 远程写入数据。

* [Azure 虚拟网络上的资源调配 HBase 群集](hdinsight-hbase-provision-vnet.md)-提供有关资源调配 HBase 群集 Azure 的虚拟网络上的信息。

* [在 HDInsight 中的提供 Hadoop 群集](hdinsight-provision-clusters.md)-提供有关配置 Hadoop 群集，包括使用 Azure 虚拟网络信息的信息。

* [使用 Sqoop 与 HDInsight 在 Hadoop](hdinsight-use-sqoop.md) -提供有关使用 Sqoop 与 SQL Server 通过虚拟网络的数据传输。

若要了解有关 Azure 的虚拟网络的详细信息，请参阅[Azure 虚拟网络概述](../virtual-network/virtual-networks-overview.md)。

测试

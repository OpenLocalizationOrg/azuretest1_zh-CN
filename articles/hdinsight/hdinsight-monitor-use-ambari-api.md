---
ms.openlocfilehash: 0ff0e472a65d9f71825d63e429ab6a8b2f40a907
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="监视 Hadoop 群集中使用 Ambari API 的 HDInsight |Microsoft Azure"
    description="进行资源调配、 管理和监视 Hadoop 群集使用 Apache Ambari Api。 直观的运算符工具和 Api 隐藏 Hadoop 的复杂性。"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    editor="cgronlun"
    manager="paulettm"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2015"
    ms.author="jgao"/>

# HDInsight 使用 Ambari API 中的监视 Hadoop 群集

了解如何使用 Ambari Api 监视 HDInsight 群集版本 3.1 和 2.1。

> [AZURE.NOTE] 这篇文章中的信息是主要用于基于 Windows 的 HDInsight 群集，提供 Ambari REST API 的只读版本。 基于 Linux 的群集，请参阅[管理 Hadoop 群集使用 Ambari](hdinsight-hadoop-manage-ambari.md)。

## <a id="whatisambari"></a> Ambari 是什么？

[Apache Ambari][ambari 总部]使用的资源调配、 管理，以及监控 Apache Hadoop 群集。 它包括一个直观的运算符工具集合和强大的一组 Api，这些隐藏的 Hadoop，简化的群集操作的复杂性。 有关 Api 的详细信息，请参阅[Ambari API 参考][ambari api 参考]。


HDInsight 目前支持只 Ambari 的监视功能。 HDInsight 版本 3.0 和 2.1 群集支持 Ambari API 1.0。 本文介绍了 HDInsight 版本 3.1 和 2.1 群集访问 Ambari Api。 两者之间的关键区别是某些组件已更改引入了新功能 （如作业历史记录服务器上）。


##<a id="prerequisites"></a>先决条件

在开始本教程之前，您必须具有以下︰

- **带有 Azure PowerShell 的工作站**。 请参阅[安装和使用 Azure PowerShell](http://azure.microsoft.com/documentation/videos/install-and-use-azure-powershell/)。


- （可选）[卷曲][卷曲]。 若要安装此更新，请参阅[卷曲发布和下载][卷曲下载]。

    >[AZURE.NOTE] 在 Windows 中，使用双引号而不是单引号都开始选项值时使用 cURL 命令。

- **Azure HDInsight 群集**。 有关群集配置的说明，请参阅[开始使用 HDInsight][hdinsight 获取启动]或[规定 HDInsight 群集][hdinsight 规定]。 您将需要以下数据以转到教程︰

群集属性|Azure PowerShell 变量名|值|说明
---|---|---|---
HDInsight 群集名称|$clusterName||HDInsight 群集的名称。
群集用户名|$clusterUsername||指定在资源调配群集用户名称。
群集的密码|$clusterPassword||群集的用户密码。

    > [AZURE.NOTE] 填充表中的值。 这将有助于通过本教程。



##<a id="jumpstart"></a>快速入门

有多种方法使用 Ambari 来监视 HDInsight 群集。

**使用 Azure PowerShell**

下面是 Azure PowerShell 脚本以使 MapReduce 作业跟踪程序信息*HDInsight 3.1 群集中。*  关键的区别是我们拉从 YARN 服务 （而非 MapReduce） 这些详细信息。

    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterPassword>"

    $ambariUri = "https://$clusterName.azurehdinsight.net:443/ambari"
    $uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName.azurehdinsight.net/services/yarn/components/resourcemanager"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    $response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

    $response.metrics.'yarn.queueMetrics'

以下是用于 MapReduce 作业跟踪程序信息*HDInsight 2.1 群集中*获取 Azure PowerShell 脚本︰

    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterPassword>"

    $ambariUri = "https://$clusterName.azurehdinsight.net:443/ambari"
    $uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName.azurehdinsight.net/services/mapreduce/components/jobtracker"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    $response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

    $response.metrics.'mapred.JobTracker'

输出为︰

![Jobtracker 输出][img-jobtracker-output]

**使用卷曲**

以下是使用 cURL 获取群集信息的示例︰

    curl -u <username>:<password> -k https://<ClusterName>.azurehdinsight.net:443/ambari/api/v1/clusters/<ClusterName>.azurehdinsight.net

输出为︰

    {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/",
     "Clusters":{"cluster_name":"hdi0211v2.azurehdinsight.net","version":"2.1.3.0.432823"},
     "services"[
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/services/hdfs",
        "ServiceInfo":{"cluster_name":"hdi0211v2.azurehdinsight.net","service_name":"hdfs"}},
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/services/mapreduce",
        "ServiceInfo":{"cluster_name":"hdi0211v2.azurehdinsight.net","service_name":"mapreduce"}}],
     "hosts":[
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/hosts/headnode0",
        "Hosts":{"cluster_name":"hdi0211v2.azurehdinsight.net",
                 "host_name":"headnode0"}},
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/hosts/workernode0",
        "Hosts":{"cluster_name":"hdi0211v2.azurehdinsight.net",
                 "host_name":"headnode0.{ClusterDNS}.azurehdinsight.net"}}]}

**10/8/2014年的释放**︰

当使用 Ambari 端点，"https://{clusterDns}.azurehdinsight.net/ambari/api/v1/clusters/{clusterDns}.azurehdinsight.net/services/{servicename}/components/{componentname}"， *host_name*字段返回完全限定的域名称 (FQDN) 的节点，而不是主机名。 10/8/2014年版本中，本示例返回之前只是"**headnode0**"。 10/8/2014年发布后，您将获得 FQDN"**headnode0。 {ClusterDNS}.azurehdinsight.net**"，如前面的示例中所示。 此更改才能促进的方案 （如 HBase 和 Hadoop） 的多个群集类型可以部署一个虚拟网络 (VNET) 中的位置。 发生这种情况，例如，当使用 Hadoop 作为后端平台 HBase。

##<a id="monitor"></a>Ambari 监控 Api

下表列出了一些最常见的 Ambari 监控 API 调用。 有关 API 的详细信息，请参阅[Ambari API 参考][ambari api 参考]。

监视器 API 调用|URI|说明
---|---|---
获取群集|`/api/v1/clusters`|
获取群集信息。|`/api/v1/clusters/&lt;ClusterName&gt;.azurehdinsight.net`|群集服务主机
获取服务|`/api/v1/clusters/&lt;ClusterName&gt;.azurehdinsight.net/services`|服务包括︰ hdfs，mapreduce
获取服务的信息。|`/api/v1/clusters/&lt;ClusterName&gt;.azurehdinsight.net/services/&lt;ServiceName&gt;`|
获得服务组件|`/api/v1/clusters/&lt;ClusterName&gt;.azurehdinsight.net/services/&lt;ServiceName&gt;/components`|HDFS: namenode datanode<br/>MapReduce: jobtracker;tasktracker
获取组件信息。|`/api/v1/clusters/&lt;ClusterName&gt;.azurehdinsight.net/services/&lt;ServiceName&gt;/components/&lt;ComponentName&gt;`|ServiceComponentInfo，主机组件、 衡量标准
获取主机|`/api/v1/clusters/&lt;ClusterName&gt;.azurehdinsight.net/hosts`|headnode0 workernode0
获取主机的信息。|`/api/v1/clusters/&lt;ClusterName&gt;.azurehdinsight.net/hosts/&lt;HostName&gt;`|
获取主机组件|`/api/v1/clusters/&lt;ClusterName&gt;.azurehdinsight.net/hosts/&lt;HostName&gt;/host_components`|namenode resourcemanager
获取主机组件的信息。|`/api/v1/clusters/&lt;ClusterName&gt;.azurehdinsight.net/hosts/&lt;HostName&gt;/host_components/&lt;ComponentName&gt;`|HostRoles，组件、 主机、 度量标准
获取配置|`/api/v1/clusters/&lt;ClusterName&gt;.azurehdinsight.net/configurations`|配置类型︰ 核心站点、 hdfs 站点，mapred 网站、 网站用户配置单元
获取配置信息。|`/api/v1/clusters/&lt;ClusterName&gt;.azurehdinsight.net/configurations?type=&lt;ConfigType&gt;&tag=&lt;VersionName&gt;`|配置类型︰ 核心站点、 hdfs 站点，mapred 网站、 网站用户配置单元


##<a id="nextsteps"></a>下一步行动

现在您已了解如何使用 Ambari 监控 API 调用。 若要了解详细信息，请参阅︰

- [使用 Azure 预览门户管理 HDInsight 群集][hdinsight 管理门户]
- [使用 Azure PowerShell 管理 HDInsight 群集][hdinsight-管理 powershell]
- [使用命令行界面管理 HDInsight 群集][hdinsight-管理-cli]
- [HDInsight 文档][hdinsight-文档]
- [开始使用 HDInsight][hdinsight--入门]



[ambari-主页]: http://ambari.apache.org/
[ambari api 参考]: https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md

[卷曲]: http://curl.haxx.se
[卷曲下载]: http://curl.haxx.se/download.html

[microsoft 的 hadoop SDK]: http://hadoopsdk.codeplex.com/wikipage?title=Ambari%20Monitoring%20Client

[powershell 安装]: ../install-configure-powershell.md
[powershell 脚本]: http://technet.microsoft.com/library/ee176949.aspx

[hdinsight-管理 powershell]: hdinsight-administer-use-powershell.md
[hdinsight 管理门户]: hdinsight-administer-use-management-portal.md
[hdinsight-管理-cli]: hdinsight-administer-use-command-line.md
[hdinsight-文档]: /documentation/services/hdinsight/
[hdinsight--入门]: ../hdinsight-get-started.md
[hdinsight 规定]: hdinsight-provision-clusters.md

[img jobtracker 输出]: ./media/hdinsight-monitor-use-ambari-api/hdi.ambari.monitor.jobtracker.output.png

测试

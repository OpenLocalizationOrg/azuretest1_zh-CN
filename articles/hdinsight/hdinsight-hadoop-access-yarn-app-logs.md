---
ms.openlocfilehash: 9219180a4ea0385ea00d86ddc02665e26d9e580a
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="访问 Hadoop YARN 应用程序日志，以编程方式 |Microsoft Azure"
    description="访问应用程序以编程方式记录在 Hadoop 群集在 HDInsight 上。"
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
    ms.date="07/09/2015"
    ms.author="jgao"/>

# 访问 YARN 应用程序登录 HDInsight 在 Hadoop 以编程方式

本主题说明如何以编程方式枚举完在 Hadoop 群集在 Azure HDInsight YARN （尚未另一个资源谈判者） 应用程序以及如何以编程方式访问应用程序日志，而无需使用远程桌面协议 (RDP) 连接到群集。 具体来说，已添加新的组件和一个新的 API:

  1. HDInsight 群集上的通用应用程序历史记录服务器已启用。 它是内 YARN 日程表服务器处理的存储和检索有关已完成的应用程序的通用信息的组件。
  2. 在 Azure HDInsight.NET SDK 中的 Api 是可用来以编程方式枚举已在群集运行的应用程序和下载 （纯文本） 中相关的特定于应用程序或特定于容器的日志以帮助调试发生任何应用程序问题。


## 先决条件

Azure HDInsight SDK 需要使用.NET Framework 应用程序中的此主题中介绍的代码。 最近发布的版本 SDK 是[NuGet](http://nuget.codeplex.com/wikipage?title=Getting%20Started)在可用。

要从 Visual Studio 应用程序安装 HDInsight SDK，转到**工具**菜单上，单击**Nuget 程序包管理器**，然后单击**程序包管理器控制台**。 在控制台中以安装软件包运行下面的命令︰

        Install-Package Microsoft.WindowsAzure.Management.HDInsight

此命令将 HDInsight 添加.NET 库并向当前 Visual Studio 项目中添加对它们的引用。


## <a name="YARNTimelineServer"></a>YARN 日程表服务器

<a href="http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html" target="_blank">YARN 日程表服务器</a>完成的应用程序以及框架特定应用程序信息，通过两个不同的接口提供一般信息。 具体来说︰

* HDInsight 群集的一般应用程序信息的存储和检索已启用版本 3.1.1.374 或更高版本。
* 时间线服务器的特定于框架的应用程序信息组件当前不可用 HDInsight 群集上。


应用程序的通用信息包括以下类型的数据︰

* 应用程序 ID，而应用程序的唯一标识符
* 启动应用程序的用户
* 尝试完成应用程序的信息
* 任何给定的应用程序尝试使用容器

在 HDInsight 群集，此信息将被存储 Azure 资源管理器到默认 Azure 存储帐户的默认容器中的历史记录存储。 通过 REST API 可以检索此泛型数据对已完成的应用程序︰

    GET on https://<cluster-dns-name>.azurehdinsight.net/ws/v1/applicationhistory/apps

我们已添加到 HDInsight.NET SDK，可以方便地以编程方式检索此数据的新的 Api。 请注意，在通用数据，还可以通过直接在群集节点上运行 YARN 命令行界面 (CLI) 命令，（通过使用 RDP 连接到群集） 后检索。

## <a name="YARNAppsAndLogs"></a>YARN 应用程序和日志

YARN 通过将资源管理的应用程序时间安排/监视支持多个编程模型 (MapReduce 是其中之一)。 这是通过全局*ResourceManager* (RM)、 每个工作节点*NodeManagers* (NMs) 和每个应用程序*ApplicationMasters* (AMs)。 每个应用程序上午协商资源 （CPU、 内存、 磁盘） 网络安装与运行您的应用程序 RM 使用 NMs 授予这些资源作为*容器*授予工作。 上午负责跟踪进度的 rm 分配给它的容器 应用程序可能需要很多的容器，这取决于应用程序的性质。

此外，每个应用程序可能包含多个*应用程序尝试*为完成存在崩溃时或由于上午之间的通信丢失和 rm。 因此，容器授予应用程序的具体尝试。 在某种意义上，容器提供的上下文 YARN 应用程序，执行工作的基本单位，在容器的上下文中完成的所有工作都执行单个辅助节点分配给该容器上。 请参阅[YARN 概念][YARN 概念]供进一步参考。

应用程序日志 （和关联的容器日志） 中调试问题的 Hadoop 应用程序至关重要。 YARN 提供用于收集、 聚合和存储[日志聚合][日志聚合]功能的应用程序日志的很好框架。 日志聚合功能使访问应用程序日志更具有确定性，因为它在辅助节点上的所有容器聚合日志并将它们存储为一个聚合日志文件每个工作节点上的默认文件系统中，应用程序完成之后。 您的应用程序可能使用数百或数千的容器，但总是会聚集到一个文件中，从而导致每个应用程序所使用的辅助节点的一个日志文件的日志中的所有单个辅助节点上运行的容器。 默认情况下，对 HDInsight 群集启用日志聚合 (版本 3.0 及以上)，和聚合的日志可以在位于以下位置的群集的默认容器中找到︰

    wasb:///app-logs/<user>/logs/<applicationId>

在该位置，*用户*启动该应用程序的用户的名称，并且*applicationId* YARN rm 指派的应用程序的唯一标识符

聚合的日志并不直接可读的与它们用[TFile][T 文件]，按容器进行索引的[二进制格式][二进制格式]编写。 YARN 提供了 CLI 工具转储这些日志以纯文本格式的应用程序或感兴趣的容器。 当 （通过 RDP 连接到它） 后，通过运行以下 YARN 之一的纯文本命令直接在群集节点上，您可以查看这些日志︰

    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application>
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application> -containerId <containerId> -nodeAddress <worker-node-address>

下一节谈到了如何访问特定于应用程序或特定于容器的日志以编程的方式，而无需使用 RDP 连接到您的 HDInsight 群集。

## <a name="enumerate-and-download"></a>枚举的应用程序，然后以编程方式下载日志

若要使用下面的代码示例，必须满足上述下载最新版本的 HDInsight.NET SDK 的前提条件。 那里提供的说明，请参阅。

下面的代码阐释了如何使用新的 Api 来枚举的应用程序和下载已完成的应用程序日志。

> [AZURE.NOTE] 下面的 Api 将处理仅针对"运行"Hadoop 群集使用版本 3.1.1.374 或更高版本。 添加以下指令︰

    using Microsoft.Hadoop.Client;
    using Microsoft.WindowsAzure.Management.HDInsight;

这些引用下面的代码中新定义的 Api。 下面的代码段创建在您的订阅中针对"正在运行"群集应用程序历史记录客户端︰

    string subscriptionId = "<your-subscription-id>";
    string clusterName = "<your-cluster-name>";
    string certName = "<your-subscription-management-cert-name>";

    // Create an HDInsight client
    X509Store store = new X509Store(StoreName.My, StoreLocation.LocalMachine);
    store.Open(OpenFlags.ReadOnly);
    X509Certificate2 cert = store.Certificates.Cast<X509Certificate2>()
                                .Single(x => x.FriendlyName == certName);

    HDInsightCertificateCredential creds =
                new HDInsightCertificateCredential(new Guid(subscriptionId), cert);

    IHDInsightClient client = HDInsightClient.Connect(creds);

    // Get the cluster on which your applications were run
    // The cluster needs to be in the "Running" state
    ClusterDetails cluster = client.GetCluster(clusterName);

    // Create an Application History client against your cluster
    IHDInsightApplicationHistoryClient appHistoryClient =
                cluster.CreateHDInsightApplicationHistoryClient(TimeSpan.FromMinutes(5));


现在可以使用客户端应用程序的历史记录列出已完成的应用程序、 基于您的条件，并下载相关的应用程序日志的应用程序筛选器。 下面的代码段显示了如何完成此操作通过编程方式︰

    // Local download folder location where the logs will be placed
    string downloadLocation = "E:\\YarnApplicationLogs";

    // List completed applications on your cluster that were submitted in the last 24 hours but failed
    // Search for applications based on application name
    string appNamePrefix = "your-app-name-prefix";
    DateTime endTime = DateTime.UtcNow;
    DateTime startTime = endTime.AddHours(-24);
    IEnumerable<ApplicationDetails> applications = appHistoryClient
                    .ListCompletedApplications(startTime, endTime)
                    .Where(app =>
                        app.GetApplicationFinalStatusAsEnum() == ApplicationFinalStatus.Failed
                        && app.Name.StartsWith(appNamePrefix));

    // Download logs for failed or killed applications
    // This will generate one log file for each application
    foreach (ApplicationDetails application in applications)
    {
        appHistoryClient.DownloadApplicationLogs(application, downloadLocation);
    }

上面的代码列表/查找感兴趣的应用程序通过使用历史记录应用程序客户端，然后将下载这些应用程序的本地文件夹的日志。

此外，以下代码片段下载应用程序 ID 已知的应用程序日志。 应用程序 ID 是应用程序的全局唯一标识符分配给它的 rm。 它被构造使用 RM，以及向其提交的应用程序的单调递增计数器的开始时间。 应用程序 ID 是窗体的"应用程序\_&lt;RM 开始时间&gt;\_&lt;计数器&gt;"。 请注意，应用程序 ID 和作业 ID 不同。 作业 ID 为 MapReduce 框架中，为特定的概念，而应用程序 ID 是一个框架不可知 YARN 概念。 在 YARN，作业 ID 标识特定的 MapReduce 作业，由 MapReduce 应用程序提交到 rm。 上午处理

    // Download application logs for an application whose application ID is known
    string applicationId = "application_1416017767088_0028";
    ApplicationDetails someApplication = appHistoryClient.GetApplicationDetails(applicationId);
    appHistoryClient.DownloadApplicationLogs(someApplication, downloadLocation);

如果需要还可以为每个下载日志容器 （或任何特定的容器），它由一个应用程序，如下所示。

    ApplicationDetails someApplication = appHistoryClient.GetApplicationDetails(applicationId);

    // Download logs separately for each container of application(s) of interest
    // This will generate one log file per container
    IEnumerable<ApplicationAttemptDetails> applicationAttempts =
                appHistoryClient.ListApplicationAttempts(someApplication);

    ApplicationAttemptDetails finalAttempt = applicationAttempts
                .Single(x => x.ApplicationAttemptId == someApplication.LatestApplicationAttemptId);

    IEnumerable<ApplicationContainerDetails> containers =
                appHistoryClient.ListApplicationContainers(finalAttempt);

    foreach (ApplicationContainerDetails container in containers)
    {
        appHistoryClient.DownloadApplicationLogs(container, downloadLocation);
    }



[YARN 日程表服务器]:http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html
[日志聚合]:http://hortonworks.com/blog/simplifying-user-logs-management-and-access-in-yarn/
[T-文件]:https://issues.apache.org/jira/secure/attachment/12396286/TFile%20Specification%2020081217.pdf
[二进制文件格式]:https://issues.apache.org/jira/browse/HADOOP-3315
[YARN 概念]:http://hortonworks.com/blog/apache-hadoop-yarn-concepts-and-applications/

测试

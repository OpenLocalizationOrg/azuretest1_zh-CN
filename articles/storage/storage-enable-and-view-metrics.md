---
ms.openlocfilehash: 17ac05fc70deb943fa5b0ca77f8cfb4c4a59c563
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="存储分析" 
    description="如何管理并发性斑点、 队列、 表和文件服务" 
    services="storage" 
    documentationCenter="" 
    authors="tamram" 
    manager="adinah" 
    editor=""/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="06/18/2015" 
    ms.author="tamram"/>

# 启用存储度量值和度量数据查看

默认情况下，存储服务未启用存储标准。 您可以启用使用 Azure 管理门户网站，Windows PowerShell 监视或通过存储 API 以编程方式。

启用存储标准时，您必须选择数据的保留期︰ 这段时间内决定多长时间存储的情况下，服务保持指标与要求您的空间来存储它们的费用。 通常情况下，应使用较短的保留周期每分钟每小时指标比指标由于分钟指标所需的大量额外空间。 您应选择保留期，以便有足够的时间来分析数据和下载任何度量标准，您想要保留以供离线分析或报告的目的。 请记住，您也将从您的存储帐户下载度量数据记入。

## 如何启用存储标准使用 Azure 的管理门户

在 Azure 管理门户中，您使用配置页中为控件存储标准的存储帐户。 进行监视，您可以以天为单位为每个 blob、 表和队列设置级别和保留期。 在每种情况下，级别是下列项之一︰


- 关闭 — — 这意味着没有度量值被收集。

- 最少 — — 收集一套基本指标如入口/出口、 可用性、 延迟和成功百分比，Blob、 表和队列服务聚合的存储标准。

- 详细 — — 存储度量标准收集一套完整的指标，包括每个存储 API 操作，以及服务级别指标的同一指标。 详细的指标可以更仔细地分析应用程序操作过程中出现的问题。

请注意，管理门户不会当前启用您可以在您的存储帐户; 配置分钟指标您必须启用使用 PowerShell 分钟指标或以编程方式。


## 如何启用使用 PowerShell 的存储标准

可以使用在您的本地计算机上 PowerShell 使用 Azure PowerShell cmdlet Get-AzureStorageServiceMetricsProperty 检索当前设置，并设置 AzureStorageServiceMetricsProperty 该 cmdlet 以更改当前设置在您的存储帐户配置存储标准。

控制存储标准的 cmdlet 使用以下参数︰

- MetricsType 可能的值为小时和分钟。

- 服务类型的可能值是 Blob、 队列和表。

- MetricsLevel 可能的值为 None （等效于在管理门户关闭），服务 （相当于最低的管理门户中） 和 ServiceAndApi （相当于管理门户中详细）。

例如，以下命令切换对分钟度量数据的默认存储帐户中的 blob 服务使用的保留期设置为五天︰

`Set-AzureStorageServiceMetricsProperty -MetricsType Minute -ServiceType Blob -MetricsLevel ServiceAndApi  -RetentionDays 5`

以下命令将检索当前的小时工资标准级别和保留天默认存储帐户中的 blob 服务︰

`Get-AzureStorageServiceMetricsProperty -MetricsType Hour -ServiceType Blob`

有关如何配置 Azure PowerShell cmdlet 使用 Azure 订阅以及如何选择默认的存储帐户要使用，请参阅︰[如何安装和配置 Azure PowerShell](../install-configure-powershell.md)。

## 如何以编程方式启用存储标准

除了使用 Azure 管理门户或控制存储标准的 Azure PowerShell cmdlet，您还可以使用 Azure 存储 Api 的其中之一。 例如，如果您使用.NET 语言可以使用客户端存储库。

CloudBlobClient、 CloudQueueClient 和 CloudTableClient 所有这类具有如 SetServiceProperties 和 SetServicePropertiesAsync ServiceProperties 对象作为参数的方法。 可以使用 ServiceProperties 对象来配置存储标准。 例如，下面的 C# 代码段演示如何更改每小时队列度量指标级别和保留天数︰

    var storageAccount = CloudStorageAccount.Parse(connStr);
    var queueClient = storageAccount.CreateCloudQueueClient();
    var serviceProperties = queueClient.GetServiceProperties();
     
    serviceProperties.HourMetrics.MetricsLevel = MetricsLevel.Service;
    serviceProperties.HourMetrics.RetentionDays = 10;
     
    queueClient.SetServiceProperties(serviceProperties);
    
## 查看存储标准

配置存储标准来监视您的存储帐户后，它会记录您的存储帐户中的已知表的一组指标。 可以为您的存储帐户管理门户中使用监视器页面以俟在图表中查看的小时工资标准。 在此页中管理门户，您可以︰

- 选择要绘制在图表中 （可用指标的选择将取决于您是否选择详细或最少的监视服务配置页面上） 的指标。


- 选择的度量标准，在图表中显示的时间范围。


- 选择使用绝对或相对的比例来绘制指标。


- 配置电子邮件警报，在一个特定指标达到一定值时通知您。


如果您希望下载用于长期存储的标准或本地对其进行分析，您需要使用一种工具或编写一些代码来读取表。 您必须下载的分钟指标进行分析。 如果所有表都列入您的存储帐户，但您可以按名称直接访问它们不显示这些表。 许多第三方存储浏览工具已意识到这些表并使您可以直接查看它们 （请参阅博客文章[Microsoft Azure 存储资源管理器](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx)可用工具的列表）。

### 按小时度量标准
- $MetricsHourPrimaryTransactionsBlob
- $MetricsHourPrimaryTransactionsTable
- $MetricsHourPrimaryTransactionsQueue

### 每分钟的标准
- $MetricsMinutePrimaryTransactionsBlob
- $MetricsMinutePrimaryTransactionsTable
- $MetricsMinutePrimaryTransactionsQueue

### 容量
- $MetricsCapacityBlob

可以查找这些表[存储分析指标表架构](https://msdn.microsoft.com/library/azure/hh343264.aspx)在架构的完整详细信息。 下面的示例行显示只可用列的子集，但说明了存储标准保存这些计量数据的方式的一些重要功能︰

| PartitionKey  |       RowKey       |                    Timestamp | TotalRequests | TotalBillableRequests | TotalIngress | TotalEgress | 可用性 | AverageE2ELatency | AverageServerLatency | PercentSuccess |
|---------------|:------------------:|-----------------------------:|---------------|-----------------------|--------------|-------------|--------------|-------------------|----------------------|----------------|
| 20140522T1100 |      用户;所有      | 2014-05-22T11:01:16.7650250Z | 7             | 7                     | 4003         | 46801       | 100          | 104.4286          | 6.857143             | 100            |
| 20140522T1100 | 用户;QueryEntities | 2014-05-22T11:01:16.7640250Z | 5             | 5                     | 2694         | 45951       | 100          | 143.8             | 7.8                  | 100            |
| 20140522T1100 |  用户;QueryEntity  | 2014-05-22T11:01:16.7650250Z | 1             | 1                     | 538          | 633         | 100          | 3                 | 3                    | 100            |
| 20140522T1100 | 用户;UpdateEntity  | 2014-05-22T11:01:16.7650250Z | 1             | 1                     | 771          | 217         | 100          | 9                 | 6                    | 100               |

在此示例分钟指标数据中，分区键以每分钟分辨率使用时间。 行键标识存储在行中的信息的类型和组成的信息、 访问类型和请求类型的两个部分︰

- 访问类型为用户或系统，其中用户是指存储服务，所有用户请求和系统是都指由存储分析的请求。

- 请求类型是都在这种情况下，它是一个摘要行，或者其标识特定的 API，如 QueryEntity 或 UpdateEntity。


上面的示例数据单 1 分钟 （可在上午 11:00 开始） 中显示的所有记录，UpdateEntity 这样的 QueryEntities 请求的数目加上 QueryEntity 的请求数以及请求添加多达七个，这是总的用户︰ 所有行所示。 同样，可以通过计算派生用户︰ 所有行上的平均端到端延迟 104.4286 ((143.8 * 5) + 3 + 9) / 7。

应当考虑设置在监视器页面管理门户中的警报，以便存储标准可以自动通知您的存储服务的行为的任何重要变化。如果您使用存储资源管理器工具下载此度量数据分隔的格式，可以使用 Microsoft Excel 数据进行分析。 博客文章[Microsoft Azure 存储资源管理器](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx)可用的存储资源管理器工具的列表，请参阅。



## 以编程方式访问度量数据

下面的列表显示了示例 C# 代码，访问各种分钟分钟指标，并将结果显示在控制台窗口中。 它使用 Azure 存储库包含简化了访问的指标表中存储的 CloudAnalyticsClient 类的版本 4。

    private static void PrintMinuteMetrics(CloudAnalyticsClient analyticsClient, DateTimeOffset startDateTime, DateTimeOffset endDateTime)
    {
    // Convert the dates to the format used in the PartitionKey
    var start = startDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");
    var end = endDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");
    
    var services = Enum.GetValues(typeof(StorageService));
    foreach (StorageService service in services)
    {
    Console.WriteLine("Minute Metrics for Service {0} from {1} to {2} UTC", service, start, end);
    var metricsQuery = analyticsClient.CreateMinuteMetricsQuery(service, StorageLocation.Primary);
    var t = analyticsClient.GetMinuteMetricsTable(service);
    var opContext = new OperationContext();
    var query =
    from entity in metricsQuery
    // Note, you can't filter using the entity properties Time, AccessType, or TransactionType
    // because they are calculated fields in the MetricsEntity class.
    // The PartitionKey identifies the DataTime of the metrics.
    where entity.PartitionKey.CompareTo(start) >= 0 && entity.PartitionKey.CompareTo(end) <= 0 
    select entity;
    
    // Filter on "user" transactions after fetching the metrics from Table Storage.
    // (StartsWith is not supported using LINQ with Azure table storage)
    var results = query.ToList().Where(m => m.RowKey.StartsWith("user"));
    var resultString = results.Aggregate(new StringBuilder(), (builder, metrics) => builder.AppendLine(MetricsString(metrics, opContext))).ToString();
    Console.WriteLine(resultString);
    }
    }
    
    private static string MetricsString(MetricsEntity entity, OperationContext opContext)
    {
    var entityProperties = entity.WriteEntity(opContext);
    var entityString =
    string.Format("Time: {0}, ", entity.Time) +
    string.Format("AccessType: {0}, ", entity.AccessType) +
    string.Format("TransactionType: {0}, ", entity.TransactionType) +
    string.Join(",", entityProperties.Select(e => new KeyValuePair<string, string>(e.Key.ToString(), e.Value.PropertyAsObject.ToString())));
    return entityString;
    
    }




## 当您启用存储标准不要导致什么费用？

编写创建指标表实体的请求适用于所有的 Azure 存储操作的标准费率收费。

通过对指标数据的客户端读取和删除请求也是以标准费率收费。 如果您已配置数据保留策略，则不收取当 Azure 存储中删除旧的度量数据。 但是，如果您删除分析数据，您的帐户被收取的删除操作。

使用标准表的容量也是收费︰ 您可以使用以下估计用于度量数据存储容量的︰

- 如果每小时服务利用了每个服务中的每个 API，大约 148 KB 的数据会存储指标交易记录表中的每个小时，如果您已启用服务和 API 级别摘要。

- 如果每小时服务利用了每个服务中的每个 API，大约 12 KB 的数据会存储指标交易记录表中的每个小时，如果您已启用只是服务级别摘要。

- Blob 的容量表已添加每一天 （前提是用户已经选择的日志） 的两行︰ 这意味着，每一天此表的大小增加到大约 300 字节。

## 下一步的步骤如下︰
[启用存储日志和访问日志数据](https://msdn.microsoft.com/library/dn782840.aspx)
 
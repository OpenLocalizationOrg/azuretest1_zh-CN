---
ms.openlocfilehash: 428305bd4b3449e2bbe1748fd5100c4812d3138d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="在 Azure 批高效列表查询 |Microsoft Azure"
    description="了解如何减少返回的数据量和 Azure 批池、 作业、 任务、 计算节点和多个查询时提高性能。"
    services="batch"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="Batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="big-compute"
    ms.date="08/27/2015"
    ms.author="davidmu;v-marsma"/>

# 有效的批次列表查询

Azure 的批次是大计算、 并在生产环境中，像作业、 任务和计算节点的实体可以编号以千为单位。 获取有关这些项的信息因此可以生成大量的数据，必须在每个查询转移。 限制项目的数量以及每个返回的信息类型将提高查询的速度，因此您的应用程序的性能。

下面的[批处理.NET](https://msdn.microsoft.com/library/azure/mt348682.aspx) API 方法通常经常是实际上使用 Azure 批次每个应用程序必须执行的操作的示例︰

- [ListTasks](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listtasks.aspx)
- [ListJobs](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx)
- [ListPools](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listpools.aspx)
- [ListComputeNodes](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listcomputenodes.aspx)

监视是一个常见的用例 — 确定容量和池状态要求所有计算节点 (Vm) 中查询池，例如。 另一个例子是查询的作业任务，确定是否任何这些任务仍然在排队。 在某些情况下，一组丰富的数据是必需的但其他情况下，只有总项目数的计数或处于某种状态的项的集合是必需的。

请务必注意，同时返回的项目数，表示这些项目所需的数据量可能会非常大。 只需查询大量的项目产生较大的响应可能会导致大量的问题︰

- 批处理 API 的响应时间可能会变得太慢。 项目数越多、 越长批服务所需的查询时间。 大量项目有分割成块的大小，并因此客户端库可能需要进行多个 API 调用到服务来获取所有单个列表中的项目。
- 通过调用批处理应用程序处理的 API 时有更多的项目来处理长。
- 由应用程序调用批处理时，有更多和/或更大的项目，将消耗更多的内存。
- 更多和/或更大的项目会导致网络通信量增加。 这将花费更长的传输时间，具体取决于应用程序的体系结构，可能会导致数据传输批帐户该地区以外的增加的网络费用。

对于所有的批处理 Api 满足以下条件︰

- 每个属性名称是一个字符串，它将映射到该对象的属性
- 所有属性名称都是区分大小写，但属性值都是区分大小写
- 属性名称和大小写是因为元素显示在批处理 REST API，
- 日期/时间字符串可以用两种格式之一指定和前面必须加上日期时间
    - W3CDTF (例如*creationTime gt 日期时间"2011年-05-08T08:49:37Z'*)
    - RFC1123 (例如*creationTime gt DateTime'Sun，2011 年 8 5 月 08:49:37 格林威治标准时间*)
- 布尔值的字符串"为 true"或"假"
- 指定了无效的属性或运算符将导致"400 （错误请求）"错误

## 高效查询批处理.NET 中

批.NET API 提供了以列表形式返回的能力以减少物料的数量和信息量为每个项返回通过指定查询[DetailLevel](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.detaillevel.aspx) 。 DetailLevel 是一个抽象基类，并[ODATADetailLevel](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.aspx)对象实际需要创建并作为参数传递给适当的方法。

ODataDetailLevel 对象有三个可以在构造函数或一组直接指定的公共字符串属性︰

- [FilterClause](#filter) – 筛选并可能减少返回的项的数目
- [SelectClause](#select) – 指定对于每个项，缩减项目和响应中返回的属性值的子集
- [ExpandClause](#expand) – 返回所有必需的数据在一个调用到多个调用，而非

> [AZURE.TIP] DetailLevel 的实例配置选择，也可以传递展开子句如[PoolOperations.GetPool](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.getpool.aspx)来限制数据量返回相应的 Get 方法。

### <a id="filter"></a> FilterClause

筛选器字符串可以减少返回的项目数。 可以指定限定符与一个或多个属性值，以确保返回与查询相关的项。 例如，也许您想要列出的作业，只有正在运行的任务或列出已准备好运行任务的计算节点。

 [FilterClause](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.filterclause.aspx)是一个表达式，该表达式包含*属性名称*、*运算符*和*值*的一个或多个表达式组成的字符串。 可以指定的属性是特定于每个 API 调用，所支持的每个属性的运算符。 可以使用逻辑运算符****和**or**组合多个表达式。

例如，此筛选器字符串返回唯一正在运行的任务的*显示名称*以"MyTask"开头︰

    startswith(displayName, 'MyTask') and (state eq 'Running')

下面的每个批处理 REST API，文章包含一个表，指定所支持的属性和这些属性上不同的列表操作的操作。

- [列出某一科目中的池](https://msdn.microsoft.com/library/azure/dn820101.aspx)
- [列出库中的计算节点](https://msdn.microsoft.com/library/azure/dn820159.aspx)
- [列出某一科目中的作业](https://msdn.microsoft.com/library/azure/dn820117.aspx)
- [列出的作业准备和作业的作业发布任务的状态](https://msdn.microsoft.com/library/azure/mt282170.aspx)
- [列出某一科目中的作业调度](https://msdn.microsoft.com/library/azure/mt282174.aspx)
- [列出与作业调度的作业](https://msdn.microsoft.com/library/azure/mt282169.aspx)
- [列出与作业关联的任务](https://msdn.microsoft.com/library/azure/dn820187.aspx)
- [列出与任务关联的文件](https://msdn.microsoft.com/library/azure/dn820142.aspx)
- [列表中的帐户的证书](https://msdn.microsoft.com/library/azure/dn820154.aspx)
- [列出在某个节点上的文件](https://msdn.microsoft.com/library/azure/dn820151.aspx)

> [AZURE.IMPORTANT] 在指定属性中的任何三个子句类型时，请确保属性名称和大小写匹配的批处理 REST API 元素对应。 例如，当使用.NET [CloudTask](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask)，即使.NET 属性是[CloudTask.State](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.state)必须指定**状态**而不是**状态**。 例如，要验证正确的名称和**状态**属性的情况下，将检查中[获取有关任务的信息](https://msdn.microsoft.com/library/azure/dn820133.aspx)批 REST API 文档中的元素名称。

### <a id="select"></a> SelectClause

可以通过选择字符串限制为每个项返回的属性值。 可以指定的项的属性的列表，并仅这些属性值然后返回。

[SelectClause](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.selectclause.aspx)是由组成的以逗号分隔的属性名称列表的字符串。 可以指定可用于列表操作所返回的项的属性的任意组合。

    "name, state, stateTransitionTime"

### <a id="expand"></a> ExpandClause

API 调用的次数可以减少展开子句。 每个列表项的详细的信息可以获得一次 API 调用，而不是第一次获得该列表，然后循环访问该列表，使每个项的调用列表中。

[ExpandClause](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.expandclause.aspx)的类似于 select 子句，在于它控制是否在结果中返回特定的数据。 在展开子句是只支持作业列表、 任务列表和池列表中，并且当前仅支持统计信息。 当所有属性都是必需的并且没有 select 子句指定了时，则必须用于展开子句来获取统计信息。 如果 select 子句用于获取属性的一个子集，然后"统计数据"也都可以在 select 子句中指定并展开子句可以保留为空。

## 高效查询示例

下面您可以找到使用高效地查询批处理.NET API 的代码段批服务统计的一组特定的池。 在这种情况下，批处理用户测试和生产池，其测试池 Id 前缀与"测试"和生产池 Id 前缀为"prod"。 在段中， *myBatchClient*是[BatchClient](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient)的正确初始化的实例。

    // First we need an ODATADetailLevel instance on which to set the expand, filter, and select
    // clause strings
    ODATADetailLevel detailLevel = new ODATADetailLevel();

    // Specify the ExpandClause so that the .NET API pulls the statistics for the CloudPools in a single
    // underlying REST API call. Note that we use the pool's REST API element name "stats" here as opposed
    // to "Statistics" as it appears in the .NET API (CloudPool.Statistics)
    detailLevel.ExpandClause = "stats";

    // We want to pull only the "test" pools, so we limit the items returned by using a Filterclause and
    // specifying that the pool IDs must start with "test"
    detailLevel.FilterClause = "startswith(id, 'test')";

    // To further limit the data that crosses the wire, configure the SelectClause to limit the
    // properties returned on each CloudPool object to only CloudPool.Id and CloudPool.Statistics
    detailLevel.SelectClause = "id, stats";

    // Now get our collection of pools, minimizing the amount of data returned by specifying the
    // detail level we configured above
    List<CloudPool> testPools = myBatchClient.PoolOperations.ListPools(detailLevel).ToList();

> [AZURE.TIP]
> 我们建议，您*总是*使用筛选器和 select 子句的列表 API 调用以确保最大效率和您的应用程序的最佳性能。

## 下一步行动

1. 如果还没有的话，请务必检查出开发方案与相关的批处理 API 文档
    - [批处理的剩余部分](https://msdn.microsoft.com/library/azure/dn820158.aspx)
    - [批次.NET](https://msdn.microsoft.com/library/azure/dn865466.aspx)
2. 抓取[Azure 批次样本](https://github.com/Azure/azure-batch-samples)在 GitHub 上并深入代码

测试

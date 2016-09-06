---
ms.openlocfilehash: e39db29bca92d6b3457ef543a16eb9e4a66fef4e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="创建流分析输入 |Microsoft Azure" 
    description="了解如何连接和配置的输入的源的流分析解决方案。"
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72" 
    manager="paulettm" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="08/19/2015" 
    ms.author="jeffstok"/>

# 创建流分析的输入

## 了解输入流分析
---
分析输入流定义为与数据源的连接。 流分析都有第一类与 Azure 源事件中心的集成和 Blob 存储从内部和外部作业订阅。 当数据发送到该数据源时，它是由流分析作业，实时处理的。 输入可分为两种不同类型︰ 数据输入流和引用数据输入。

## 数据流输入
---
流的分析作业必须包含至少一个数据流输入，使用与转换作业。 Azure Blob 存储和 Azure 事件集线器支持作为数据流输入源。 Azure 事件集线器用于从多个设备和服务，如社会媒体活动源、 股票交易信息或来自传感器的数据收集事件流。
或者，可以为接收大容量数据作为输入源使用 Azure Blob 存储。

## 引用数据输入
---
流分析支持第二种称为引用数据的输入类型。 这是通常用于执行关联和查找，辅助数据，这里的数据通常是静态的或不经常更改。 Azure Blob 存储目前仅支持的引用数据的输入的源。 引用数据源 blob 被限定为 50 MB 的大小。

## 创建事件中心数据输入的流
---
### 事件集线器的概述
事件集线器的高度可扩展的事件 ingestor，并且数据摄取到流分析作业的最常用方法。 事件集线器和流分析一起为客户提供端到端解决方案的实时分析--事件集线器允许客户实时送入 Azure 事件和流分析作业可实时处理它们。 例如，客户可以将网站点击，传感器的读数，在线日志事件发送给事件集线器并创建事件集线器用作输入的数据流的实时过滤、 聚合和加入流分析作业。 可以为数据出口还使用事件集线器。 更多事件集线器上的详细信息请参阅[事件集线器](https://azure.microsoft.com/services/event-hubs/ "事件集线器")文档。

### 使用者组
每个流分析事件中心输入应将配置为具有其自己的使用者组。 当作业中包含自联接或多个输入时，多个读取器下游领域，这将影响的单一使用者组中的读者数可能会读取一些输入。  为了避免超出事件中心 5 读者每个使用者组，每个分区限制，则指定每个流分析作业的使用者组最佳做法。 注意还有 20 每事件中心的使用者组的限制。 有关详细信息，请参阅[事件集线器编程指南](https://msdn.microsoft.com/library/azure/dn789972.aspx "事件集线器编程指南")。

## 创建事件中心输入的数据流
---
### 数据流输入形式添加事件中心  ###

1. 在流分析作业的输入选项卡上，单击**添加输入**然后选择默认选项中，**数据流**，并单击右侧的按钮。

    ![image1](./media/stream-analytics-connect-data-event-inputs/01-stream-analytics-create-inputs.png)  

2. 接下来选择**事件中心**。

    ![image6](./media/stream-analytics-connect-data-event-inputs/06-stream-analytics-create-inputs.png)  

3. 键入或选择下列字段并单击右侧的按钮︰

    - **输入别名**︰ 将作业查询中用来引用此输入的友好名称  
    - **服务总线 Namespace**: 服务总线命名空间是一组邮件传递实体容器。 当您创建新的事件中心时，也可以创建服务总线命名空间。  
    - **事件的中心**︰ 输入您事件中心的名称  
    - **事件的中心策略名称**︰ 共享的访问策略，可以创建在事件集线器配置选项卡。 每个共享的访问策略具有一个名称，您设置，权限和访问键。  
    - **事件使用者组中心**（可选）︰ 要摄取事件中心的数据的使用者组。 如果未指定，流分析的作业将使用默认的使用者组摄取事件中心中的数据。 建议为每个流分析作业中使用不同的使用者组。  

    ![image7](./media/stream-analytics-connect-data-event-inputs/07-stream-analytics-create-inputs.png)  

4. 指定下列设置︰

    - **事件序列化格式**︰ 若要确保您查询的工作预期，需要知道哪些序列化格式 （JSON，CSV 或 Avro） 流分析的方法使用传入的数据流量。  
    - **编码**︰ utf-8 是这次唯一支持的编码格式。  

    ![image8](./media/stream-analytics-connect-data-event-inputs/08-stream-analytics-create-inputs.png)  

5. 单击检查按钮完成向导并验证流分析可以成功地连接到该事件的中心。

## 创建一个 Blob 存储数据的流输入
---
对于方案中大量的非结构化数据存储在云中，Blob 存储提供了经济高效且可扩展的解决方案。 Blob 存储中的数据通常被视为"静态"数据，但它可以作为数据流处理，通过流分析。 Blob 存储输入流分析的一个常见方案是日志处理，其中遥测系统中捕获，并且需要进行分析和处理，以提取有意义的数据。 请务必注意流分析中的 Blob 存储事件的默认时间戳是 blob 的创建时间戳。 作为流事件负载中使用时间戳处理的数据，必须使用[时间戳的](https://msdn.microsoft.com/library/azure/dn834998.aspx)关键字。
Blob 存储的详细信息请参阅[Blob 存储](http://azure.microsoft.com/services/storage/blobs/)文档。

### 添加输入数据流形式的 Blob 存储  ###

1. 在流分析作业的输入选项卡上，单击**添加输入**然后选择默认选项中，**数据流**，并单击右侧的按钮。

    ![image1](./media/stream-analytics-connect-data-event-inputs/01-stream-analytics-create-inputs.png)  

2. 选择**Blob 存储**，然后单击右侧的按钮。

    ![image2](./media/stream-analytics-connect-data-event-inputs/02-stream-analytics-create-inputs.png)  

3. 键入或选择以下字段︰

    - **输入别名**︰ 将作业查询中用来引用此输入的友好名称  
    - **存储帐户**︰ 如果存储帐户是中流作业比其他订阅存储帐户名称，将需要存储帐户密钥。  
    - **存储容器**︰ 容器提供的 blob 存储在 Microsoft Azure Blob 服务的逻辑分组。 Blob 上载到 Blob 服务后，您必须指定该 blob 的容器。  

    ![image3](./media/stream-analytics-connect-data-event-inputs/03-stream-analytics-create-inputs.png)  

4. 单击选项，可配置自定义路径中的路径读取 blob 前缀模式**配置高级设置**框。 如果未指定此字段，则流分析将读取所有 blob 容器中。

    ![image4](./media/stream-analytics-connect-data-event-inputs/04-stream-analytics-create-inputs.png)  

5. 选择以下设置︰

    - **事件序列化格式**︰ 若要确保您查询的工作预期，需要知道哪些序列化格式 （JSON，CSV 或 Avro） 流分析的方法使用传入的数据流量。  
    - **编码**︰ utf-8 是这次唯一支持的编码格式。  


    ![image5](./media/stream-analytics-connect-data-event-inputs/05-stream-analytics-create-inputs.png)  

6. 单击检查按钮完成向导并验证流分析可以成功地连接到 Blob 存储帐户。

## 创建 Blob 存储引用数据
---
Blob 存储可以用于定义流分析作业的引用数据。  这是用于执行查找或关联数据的静态或缓慢变化数据。
支持用于刷新的引用数据可以通过使用 {日期} 输入配置中指定的路径模式启用和 {时间} 标记。 流分析将更新引用数据定义基于此路径模式。 例如，某种模式的`"/sample/{date}/{time}/products.csv"`"YYYY-月-日"和时间的 date 格式"hh: mm"的格式通知流分析要领取更新 blob`"/sample/2015-04-16/17:30/products.csv"`在 4 月 16 日 2015 UTC 下午 5:30 时区。

> [AZURE.NOTE] 当前流分析作业查找引用刷新数据 blob 仅在时间中的 blob 名称编码的时间重合时︰ 例如作业寻找下午 5:30 之间的 /sample/2015-04-16/17:30/products.csv 和 5:30:59.999999999 PM 在 4 月 16 日 2015 UTC 时区。 当时钟降临下午 5:31 它停止寻找 /sample/2015-04-16/17:30/products.csv，并开始寻找 /sample/2015-04-16/17:31/products.csv。

只有被认为是以前的引用数据 blob 时作业启动时。 此时，作业寻找 blob 具有最新的日期/时间编码作业比前值名称中开始的时间 （最新的引用数据斑点从作业开始时间之前）。 这样做是为了确保有一个非空引用数据集作业的开始。 如果不能找到一个，作业将失败，并向用户显示一个诊断的通知︰

### 将 Blob 存储添加为引用数据  ###

1. 在流分析作业的输入选项卡上，单击**添加输入**然后选择**引用的数据**并单击右侧的按钮。

    ![image9](./media/stream-analytics-connect-data-event-inputs/09-stream-analytics-create-inputs.png)  

2.  键入或选择以下字段︰

    - **输入别名**︰ 将作业查询中用来引用此输入的友好名称  
    - **存储帐户**︰ 如果存储帐户是中流作业比其他订阅存储帐户名称，将需要存储帐户密钥。  
    - **存储容器**︰ 容器提供的 blob 存储在 Microsoft Azure Blob 服务的逻辑分组。 Blob 上载到 Blob 服务后，您必须指定该 blob 的容器。  
    - **路径模式**︰ 用来找到您指定容器中的 blob 的文件路径。 在路径中，您可以选择指定一个或多个以下 2 变量实例: {日期} {时间}  

    ![image10](./media/stream-analytics-connect-data-event-inputs/10-stream-analytics-create-inputs.png)  

3. 选择以下设置︰

    - **事件序列化格式**︰ 若要确保您查询的工作预期，需要知道哪些序列化格式 （JSON，CSV 或 Avro） 流分析的方法使用传入的数据流量。  
    - **编码**︰ utf-8 是这次唯一支持的编码格式。  

    ![image12](./media/stream-analytics-connect-data-event-inputs/12-stream-analytics-create-inputs.png)  

4.  单击检查按钮完成向导并验证流分析可以成功地连接到 Blob 存储帐户。

    ![image11](./media/stream-analytics-connect-data-event-inputs/11-stream-analytics-create-inputs.png)  


## 获取帮助
进一步的帮助，请尝试我们的[Azure 流分析论坛](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## 下一步行动

- [Azure 流分析简介](stream-analytics-introduction.md)
- [开始使用 Azure 流分析](stream-analytics-get-started.md)
- [小数位数 Azure 流分析作业](stream-analytics-scale-jobs.md)
- [Azure 流分析查询语言参考](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure 流分析管理 REST API 参考](https://msdn.microsoft.com/library/azure/dn835031.aspx)

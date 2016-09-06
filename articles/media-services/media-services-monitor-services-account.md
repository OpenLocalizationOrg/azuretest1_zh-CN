---
ms.openlocfilehash: e7a09d9ae03feccce062cf9bf6a76a521e5898fc
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="监控介质服务帐户" 
    description="介绍了如何配置监视您在 Azure 中的介质服务帐户。" 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/11/2015"
    ms.author="juliako"/>



#<a id="monitormediaservicesaccount"></a>如何监视媒体服务帐户

Azure 媒体服务仪表板提供了可用于管理您的介质服务帐户的使用指标和帐户信息。

您可以监视数排队编码作业、 失败的编码任务，由编码器，输入和输出数据的有效编码作业以及媒体服务帐户相关联的 blob 存储使用情况。 此外，如果正在传输给客户的内容，您可以检索各种流指标。 您可以选择最后一次 6 小时，24 小时或 7 天监控数据。
 
>[AZURE.NOTE] 其他成本是与监视 Azure 管理门户中的存储数据。 有关详细信息，请参阅[存储分析和记帐](http://go.microsoft.com/fwlink/?LinkId=256667)。

##<a id="configuremonitoring"></a>如何︰ 监视媒体服务帐户

1. 在[管理门户网站](http://go.microsoft.com/fwlink/?LinkID=256666)中，单击**媒体服务**，然后单击介质服务帐户名称，以打开仪表板。 

    ![MediaServices_Dashboard][dashboard]

2. 要监视您的编码作业或数据，只是开始编码将作业提交到介质服务，或启动流到客户通过使用 Azure 媒体点播流的内容。 您应该开始查看监视后大约一小时的仪表板上的数据。

##<a id="configuringstorage"></a>如何︰ 监视 blob 存储使用情况 （可选）
1. 在**快速浏览**部分下单击**存储帐户**名称。
2. 在存储帐户页中，单击**配置页**链接，并向下滚动到 Blob、 表和队列服务，如下所示的**监视**设置。

    >[AZURE.NOTE] Blob 是媒体服务中唯一受支持的存储类型。

    ![StorageOptions][storage_options_scoped]

3. 在**监控**，设置 Blob 的监视级别和数据保留策略︰

-  若要监视级别设置，请选择下列项之一︰

      **最小**-收集度量标准，如入口/出口、 可用性、 延迟和成功百分比，Blob、 表和队列服务聚合。

      除了最小的指标，**详细**的收集一组相同的 Azure 存储服务 API 中每个存储操作的指标。 详细的指标可以更仔细地分析应用程序操作过程中出现的问题。 

      **关闭**-关闭监视。 现有的监控数据保持到末尾的保留期。

- 若要设置数据保留策略，在**保持期 （天）**，键入数据从 1 到 365 天的保留天数。 如果您不想将保留策略设置，则输入零。 如果没有保留策略，则完全取决于您要删除监视数据。 我们建议设置一个基于多长时间您想保留存储分析数据，为您的帐户，以便可以通过免费的系统删除旧的和未使用分析数据的保留策略。

4. 完成监视配置后，单击**保存**。
类似于媒体服务指标，首先应看监视后大约一小时的仪表板上的数据。
度量值存储在名为 $MetricsTransactionsBlob、 $MetricsTransactionsTable、 $MetricsTransactionsQueue 和 $MetricsCapacityBlob 的四个表中的存储帐户。 有关详细信息，请参阅[存储分析指标](http://go.microsoft.com/fwlink/?LinkId=256668)。


<!-- Images -->
[仪表板]: ./media/media-services-monitor-services-account/media-services-dashboard.png
[storage_options_scoped]: ./media/media-services-monitor-services-account/storagemonitoringoptions_scoped.png

 
测试

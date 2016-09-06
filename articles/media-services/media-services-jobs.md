---
ms.openlocfilehash: bbca0c1d783d517eeaf0f35ea5b97a67f7cd0739
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="作业使用 Azure 媒体服务" 
    description="本主题概述了如何管理管理 Azure 媒体服务工作。" 
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

#作业使用 Azure 媒体服务

**作业**包含有关要执行的处理的元数据。 每个**作业**中包含一个或多个**任务**指定原子的处理任务，其输入资产、 输出资产、 媒体处理器和其关联的设置。 编码器设置的详细信息，请参阅编码器参考线和编码器的架构。

编码作业通常合并其他处理，例如，包装，或加密的资产或资产生成的编码器。 作业中的任务可以链接在一起，一个任务的输出资产作为输入资产提供给下一步的任务。 以这种方式有一个作业可以包含所有所需的媒体演示文稿处理。

此部分提供了使用 Jobs\Tasks 时可以执行的常见任务的链接。

>[AZURE.NOTE]目前有 30 个任务每个作业任务的限制。 如果需要链接 30 多个任务，请创建包含任务的多个作业。


##获取媒体处理器

与**.NET**或**REST API**获得媒体处理器。

[AZURE.INCLUDE [media-services-selector-get-media-processor](../../includes/media-services-selector-get-media-processor.md)]

##创建作业 

作业是实体包含有关一组的任务 （例如，编码或索引） 的元数据。 每个任务可以执行原子操作输入资产上。 有关如何创建编码作业的示例，请参阅︰

[AZURE.INCLUDE [media-services-selector-encode](../../includes/media-services-selector-encode.md)]

##索引

[AZURE.INCLUDE [media-services-selector-index-content](../../includes/media-services-selector-index-content.md)]

##编码 

**Azure Media 编码器**使用**Azure 管理门户**、 **.NET**或**REST API，**对进行编码。
 
[AZURE.INCLUDE [media-services-selector-encode](../../includes/media-services-selector-encode.md)]

##监视作业进度

使用**Azure 管理门户网站**、 **.NET**或**REST API，**监视作业进度。

[AZURE.INCLUDE [media-services-selector-job-progress](../../includes/media-services-selector-job-progress.md)]

##相关的链接

[配额和限制](media-services-quotas-and-limitations.md)– 用于描述配额和限制的媒体服务编码器
 
测试

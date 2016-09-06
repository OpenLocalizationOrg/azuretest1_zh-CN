---
ms.openlocfilehash: b50b23e1780277faf52ab07eace3f5ca1bc75e65
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="提供媒体点播与 Azure 媒体服务" 
    description="本主题将讨论提供媒体点播 Azure 媒体服务与公共方案。" 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
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


#提供媒体点播与 Azure 媒体服务

##概述

本主题描述了典型的 Azure 媒体服务 (AMS) 视频点播流的步骤。 每个步骤链接到相关的主题。 可以使用不同的技术实现的任务，有一些按钮链接到您选择 （例如，.NET 或其他） 的技术。   

请注意，您可以将介质服务集成与您现有的工具和过程。 例如，编码内容现场，然后将上传到转换的媒体服务转换为多种格式，并通过 Azure CDN 或第三方的 CDN 提供。 

下图显示在视频中按需工作流媒体服务平台所涉及到的主要部件。
![VoD 的工作流][vod-overview]

##<a id="vod_scenarios"></a>常见的方案︰ 提供媒体点播

###保护存储中的内容并提供流媒体以明文 （非加密）

1. 高质量的夹层文件上载到某项资产。
    
    建议将存储加密选项应用于您的资产，以保护您的内容在上载过程中，同时存储中的其余部分。 
1. 对到 MP4 设置的自适应比特率进行编码。 

    建议将存储加密选项应用于输出资产，以保护您的静态内容。
    
1. 配置资产交付策略 （使用动态打包）。 
    
    如果您的资产存储加密，您**必须**配置资产交付策略。 

1. 通过创建按需定位程序发布资产。

    请确保至少有一个流保留的单位要流内容的流终结点。

1. 流已发布的内容。

###保护存储中的内容，提供动态加密流式媒体  

若要能够使用动态加密，必须先要以流式传输加密的内容的流终结点获取至少一个流保留的单位。

1. 高质量的夹层文件上载到某项资产。 应用于该资产存储加密选项。
1. 对到 MP4 设置的自适应比特率进行编码。 将存储加密选项应用到输出的资产。
1. 创建加密内容资产要在播放过程中动态加密密钥。
2. 配置内容密钥的授权策略。
1. 配置资产交付策略 （使用动态打包和动态加密）。
1. 通过创建按需定位程序发布资产。
1. 流已发布的内容。 

###索引内容

1. 高质量的夹层文件上载到某项资产。
1. 索引内容。

    索引作业将生成可作为关闭字幕 (CC) 在视频播放的文件。 它还生成文件，您可以在视频搜索，跳转到视频的确切位置。 

1. 使用索引的内容。


###提供渐进式下载 

1. 高质量的夹层文件上载到某项资产。
1. 自适应的比特率设置 MP4 或单个 MP4 编码。
1. 通过创建按需或 SAS 定位器发布资产。

    如果使用按需定位程序，请确保至少有一个流保留的单位打算以渐进式下载内容的流终结点。

    如果使用 SAS 定位器，从 Azure blob 存储下载内容。 在这种情况下，您不需要具有流保留的单位。
  
1. 渐进式下载内容。

本文说明如何设置您的开发环境并执行上面提到的任务的主题的链接。


##概念

与按需提供您的内容相关的概念，请参阅[媒体服务的概念](media-services-concepts.md)。

##常见任务︰ 提供媒体点播

###创建介质服务帐户

使用**Azure 管理门户**创建[Azure 媒体服务帐户](media-services-create-account.md)。 

###设置开发环境  

选择**.NET**或**REST API，**为您的开发环境。

[AZURE.INCLUDE [media-services-selector-setup](../../includes/media-services-selector-setup.md)]

###以编程方式连接  

选择**.NET**或**REST API**以编程方式连接到 Azure 媒体服务。

[AZURE.INCLUDE [media-services-selector-connect](../../includes/media-services-selector-connect.md)]


###配置流式处理的终结点

有关流终结点以及如何管理它们的信息的概述，请参阅[如何在媒体的服务帐户管理流式传输端点](media-services-manage-origins.md)。

###上传媒体 

使用**Azure 管理门户网站**、 **.NET**或**REST API，**文件上传。

[AZURE.INCLUDE [media-services-selector-upload-files](../../includes/media-services-selector-upload-files.md)]

###创建作业/任务

作业是实体包含有关一组的任务 （例如，编码或索引） 的元数据。 每个任务可以执行原子操作输入资产上。 有关如何创建编码作业的示例，请参阅︰

有关概述，请参见[使用 Azure 媒体服务工作](media-services-jobs.md)。

获得媒体处理器适用于**.NET**或**REST API，**您的任务。

[AZURE.INCLUDE [media-services-selector-get-media-processor](../../includes/media-services-selector-get-media-processor.md)]

下面的示例使用**Azure 管理门户**、 **.NET**或**REST API**创建编码作业。

[AZURE.INCLUDE [media-services-selector-encode](../../includes/media-services-selector-encode.md)]

####索引

[AZURE.INCLUDE [media-services-selector-index-content](../../includes/media-services-selector-index-content.md)]

####编码 

**概述**︰ 

- [动态打包概述](media-services-dynamic-packaging-overview.md)
- [编码的点播内容与 Azure 的介质服务](media-services-encode-asset.md)。

**Azure Media 编码器**使用**Azure 管理门户**、 **.NET**或**REST API，**对进行编码。
 
[AZURE.INCLUDE [media-services-selector-encode](../../includes/media-services-selector-encode.md)]

高级**媒体编码器高级工作流**使用**.NET**与编码。 

[AZURE.INCLUDE [media-services-selector-advanced-encoding](../../includes/media-services-selector-advanced-encoding.md)]

####监视作业进度

使用**Azure 管理门户网站**、 **.NET**或**REST API，**监视作业进度。

[AZURE.INCLUDE [media-services-selector-job-progress](../../includes/media-services-selector-job-progress.md)]

###保护内容 

**概述**︰ 

[内容保护概述](media-services-content-protection-overview.md)

如果您想要使用高级加密标准 (AES) （使用 128 位加密密钥） 或 PlayReady DRM 加密资产，您需要创建一个内容项。

使用**.NET**或**REST API**来创建键。

[AZURE.INCLUDE [media-services-selector-create-contentkey](../../includes/media-services-selector-create-contentkey.md)]

一旦您创建该内容项，您可以配置使用**.NET**或**REST API**密钥的授权策略。

[AZURE.INCLUDE [media-services-selector-content-key-auth-policy](../../includes/media-services-selector-content-key-auth-policy.md)]


配置资产交付策略使用**.NET**或**REST API**。

[AZURE.INCLUDE [media-services-selector-asset-delivery-policy](../../includes/media-services-selector-asset-delivery-policy.md)]


####与合作伙伴集成

[使用 castLabs 来将 DRM 许可证传送到 Azure 媒体服务](media-services-castlabs-integration.md)

###发布和提供资产

动态打包概述 

> [AZURE.SELECTOR]
- [概述](media-services-dynamic-packaging-overview.md)


提供内容概述 

> [AZURE.SELECTOR]
- [概述](media-services-deliver-content-overview.md)

（通过创建定位器） 发布资产使用**Azure 管理门户**、 **.NET**或**REST API**。

[AZURE.INCLUDE [media-services-selector-publish](../../includes/media-services-selector-publish.md)]

###启用 Azure CDN

媒体服务支持与 Azure CDN 的集成。 有关如何启用 Azure CDN 的信息，请参阅[如何在媒体的服务帐户管理流式传输端点](media-services-manage-origins.md#enable_cdn)。

###扩展的媒体服务帐户

您可以通过指定数**流保留单位**和**编码保留单位**，要将您的帐户以使用配置缩放**介质服务**。 

您还可以通过向它添加存储帐户缩放您的介质服务帐户。 每个存储帐户仅限于 500 TB。 展开存储到默认限制之外，您可以将多个存储帐户附加到单个媒体服务帐户。

[本](media-services-how-to-scale.md)主题链接到相关的主题。

###播放您的内容与现有的玩家

有关详细信息，请参阅[播放您的内容与现有的玩家](media-services-playback-content-with-existing-players.md)。


[vod 概述]: ./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png
 

测试

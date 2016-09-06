---
ms.openlocfilehash: f69503aa53de9da94a1cd50207bfe1990b53d250
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="Azure 媒体服务概述和常见方案" 
    description="本主题概述 Azure 媒体服务" 
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
    ms.date="08/20/2015"
    ms.author="juliako"/>

#Azure 媒体服务概述和常见方案

Microsoft Azure 媒体服务是一个可扩展云平台，使开发人员能够构建可扩展媒体管理和交付应用程序。 媒体服务基于 REST Api，使您安全地上载、 存储、 编码和打包为按需和实时流式传送到不同的客户端 （例如，电视、 电脑和移动设备） 的视频或音频内容。

您可以生成完全使用介质服务的端到端工作流。 您还可以选择为您的工作流的某些部分使用第三方组件。 例如，使用第三方编码器进行编码。 然后，将上载、 保护，打包，提供使用介质服务。


下面的海报展示了 Azure 媒体服务工作流程，从媒体创建到消耗。 您可以从此处下载海报︰ [Azure 媒体服务标牌](http://www.microsoft.com/download/details.aspx?id=38195)。

![概述][overview]

您可以选择您的内容实时或按需提供的内容的流。 本主题介绍您内容的[实时](media-services-overview.md#live_scenarios)或[按需](media-services-overview.md#vod_scenarios)提供的常见方案。 该主题还链接到其他相关主题。

若要生成媒体服务解决方案，您可以使用︰

- [媒体服务 REST API，](https://msdn.microsoft.com/library/azure/hh973617.aspx)
- 可用的客户端 Sdk 之一︰ [.net Azure 媒体服务 SDK](https://github.com/Azure/azure-sdk-for-media-services)， [Java 的 Azure SDK](https://github.com/Azure/azure-sdk-for-java)， [Node.js Azure 媒体服务](https://github.com/fritzy/node-azure-media)， [Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php)
- 现有的工具︰ [Azure 管理门户](http://manage.windowsazure.com/)或[浏览器 Azure 媒体服务的](https://github.com/Azure/Azure-Media-Services-Explorer)。


##先决条件

要开始使用 Azure 介质服务，您应该︰
 
3. Azure 帐户。 如果您没有帐户，您可以在几分钟创建免费的试用帐户。 有关详细信息，请参阅[Azure 免费试用版](azure.microsoft.com)。
2. Azure 媒体服务帐户。 使用 Azure 管理门户、.NET 或 REST API 来创建 Azure 媒体服务帐户。 有关详细信息，请参阅[创建帐户](media-services-create-account.md)。
3. （可选）设置开发环境。 选择.NET 或 REST API，为您的开发环境。 有关详细信息，请参阅[设置环境](media-services-dotnet-how-to-use.md)。 

    此外，了解如何以编程方式连接[连接](media-services-dotnet-connect_programmatically.md)。
4. （推荐）分配一个或多个刻度单位。 建议在生产环境中的应用程序分配一个或多个刻度单位。   有关详细信息，请参阅[管理流式处理的终结点](media-services-manage-origins.md)。

##概念

有关详细信息，请参阅[概念](media-services-concepts.md)。


##<a id="vod_scenarios"></a>提供媒体点播与 Azure 媒体服务︰ 常见方案和任务

本部分描述常见的方案，并提供指向相关主题的链接。 下图显示在提供媒体服务平台所涉及到的主要部件上请求的内容。 

![VoD 的工作流][vod-overview]


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

###另请参阅

- [如何将内容上载](media-services-manage-content.md#upload)
- [如何获取媒体处理器](media-services-get-media-processor.md)
- [如何对内容进行编码](media-services-manage-content.md#encode)
- [如何监视作业](media-services-portal-check-job-progress.md)
- [如何索引内容](media-services-manage-content.md#index)
- [如何保护内容](media-services-manage-content.md#encrypt)
- [如何保护发布](media-services-manage-content.md#publish)
- [如何扩展编码](media-services-portal-encoding-units.md)

##<a id="live_scenarios"></a>提供使用 Azure 媒体服务的实时流事件

在实时流处理通常涉及以下组件︰ 

- 照相机是用来广播事件。
- 将信号发送到实时流服务的流转换为从摄像机实时视频编码器。 
  
    或者，多个实时编码器。 对于某些关键实时事件，需求非常高的可用性和质量的经验，建议采用主动-主动冗余编码器，以实现无缝的故障切换不丢失数据。
- 实时的流服务，使您可以执行以下操作︰ 
    - 摄取实况内容使用各种实时流协议 （例如 RTMP 或平滑流式处理） 
    - 编码到自适应比特率流的流
    - 预览您实时流
    - ingested 的内容存储以便进行流式处理更高 （视频点播）
    - 直接向您的客户，或链接进行进一步分发内容传递网络 (CDN) 提供通过常用流的协议 （例如，MPEG 划线、 平滑、 HLS、 HDS） 的内容。 
    
        
**Microsoft Azure 媒体服务**(AMS) 能够摄取、 编码、 预览、 存储和传递实时流式内容。

当交付给客户的内容您的目标是将高品质的视频传送到不同的网络条件下的各种设备。 为了兼顾质量和网络条件，使用实时编码器编码到多比特率 （自适应比特率） 视频流流。  流在不同设备上的关照，可使用介质服务[动态打包](media-services-dynamic-packaging-overview.md)进行动态重新打包到不同的协议流。 媒体服务支持传送流技术下自适应比特率︰ HTTP 实时流 (HLS)、 平滑流式处理、 MPEG 划线和 HDS （对 Adobe 至此/访问许可证持有者只）。

在 Azure 媒体服务、**渠道**、**程序**和**StreamingEndpoints**处理所有的实时流功能包括接收，格式，DVR、 安全性、 可伸缩性和冗余。 

一个**通道**表示用于处理实时流式内容的管道。 目前，通道可以接收实时输入流方式如下︰


- 内部实时编码器发送一个比特率流通道启用执行实时编码的媒体服务中使用以下格式之一︰ RTP (MPEG-TS)、 RTMP，或平滑流式处理 (碎片 MP4)。 通道然后执行传入的实时编码到多的比特率 （随样性） 视频流的单个的比特率流。 在请求时，介质服务为客户提供流。

    编码的实时流媒体服务是在**预览**。
- 内部部署实时编码器发送多比特率**RTMP**或**平滑流式处理**(碎片 MP4) 通道。 您可以使用下面的实时编码器输出多比特率平滑流式处理︰ 基本，Envivio，Cisco。  下面的实时编码器输出 RTMP: Adobe Flash 动态、 Telestream Wirecast 和 Tricaster 不知所终。 Ingested 的流通过**通道**s，而无需任何进一步的处理。 您实时编码器还可以发送一个比特率流到没有启用实时编码，但不是建议使用的通道。 在请求时，介质服务为客户提供流。


###允许进行实时编码使用 Azure 媒体服务的渠道与合作


下图显示实时流式的流通道中启用执行实时编码与介质服务中涉及 AMS 平台的主要部件。  

![实时流][live-overview1]

有关详细信息，请参见[使用都适用于执行实时编码与 Azure 媒体服务的渠道](media-services-manage-live-encoder-enabled-channels.md)。 


###从内部编码器接收多比特率实时流的渠道与合作


下图显示此实时流的工作流中涉及的 AMS 平台的主要部件。

![实时流][live-overview2]

有关详细信息，请参阅[该接收多通道处理的比特率从内部编码器的实时流](media-services-manage-channels-overview.md)。 

##使用该内容

Azure 媒体服务提供各种工具，您需要创建丰富的、 动态的客户端播放器应用程序对于大多数平台包括︰ iOS 设备、 Android 设备，Windows、 Windows Phone，Xbox 和顶框。 以下主题提供了 Sdk 和播放机框架可以用来开发自己的客户端应用程序可以使用从介质服务中的流式媒体链接。

[开发视频播放器应用程序](media-services-develop-video-players.md)

##启用 Azure CDN

媒体服务支持与 Azure CDN 的集成。 有关如何启用 Azure CDN 的信息，请参阅[如何在媒体的服务帐户管理流式传输端点](media-services-manage-origins.md#enable_cdn)。

##扩展的媒体服务帐户

您可以通过指定数**流保留单位**和**编码保留单位**，要将您的帐户以使用配置缩放**介质服务**。 

您还可以通过向它添加存储帐户缩放您的介质服务帐户。 每个存储帐户仅限于 500 TB。 展开存储到默认限制之外，您可以将多个存储帐户附加到单个媒体服务帐户。

[本](media-services-how-to-scale.md)主题链接到相关的主题。

##支持

[Azure 支持](http://azure.microsoft.com/support/options/)提供 Azure，包括媒体服务支持的选项。

##模式和实践指南

[模式和实践指导](https://wamsg.codeplex.com/)
[联机文档](https://msdn.microsoft.com/library/dn735912.aspx)
[下载电子书](https://www.microsoft.com/download/details.aspx?id=42629)


##服务级别协议 (SLA)

- 媒体服务编码，我们保证 99.9%的可用性的 REST API 事务。
- 对于流，我们将成功服务请求提供 99.9%的可用性保证为现有媒体内容时至少一个流的计价单位的采购。
- 对于实时信道，我们保证，运行通道将具有外部连接至少 99.9%的时间。
- 内容保护，我们保证，我们将成功地完成密钥请求至少 99.9%的时间。
- 对于索引器，我们将成功地服务索引器任务请求处理与编码保留单位 99.9%的时间。

    有关详细信息，请参阅[Microsoft Azure SLA](http://azure.microsoft.com/support/legal/sla/)。



<!-- Images -->
[概述]: ./media/media-services-overview/media-services-overview.png
[vod 概述]: ./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png
[live overview1]: ./media/media-services-live-streaming-workflow/media-services-live-streaming-new.png
[live overview2]: ./media/media-services-live-streaming-workflow/media-services-live-streaming-current.png
 

测试

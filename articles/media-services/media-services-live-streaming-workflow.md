---
ms.openlocfilehash: 128cfe49996ca3f9d377c2e4e586f03581ce1a99
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="提供实时流与 Azure 媒体服务" 
    description="本主题概述了活动流中的主要组件 involoved。" 
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


#提供使用 Azure 媒体服务的实时流事件

##概述

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


##允许进行实时编码使用 Azure 媒体服务的渠道与合作


下图显示实时流式的流通道中启用执行实时编码与介质服务中涉及 AMS 平台的主要部件。  

![实时流][live-overview1]

有关详细信息，请参见[使用都适用于执行实时编码与 Azure 媒体服务的渠道](media-services-manage-live-encoder-enabled-channels.md)。 


##从内部编码器接收多比特率实时流的渠道与合作


下图显示此实时流的工作流中涉及的 AMS 平台的主要部件。

![实时流][live-overview2]

有关详细信息，请参阅[该接收多通道处理的比特率从内部编码器的实时流](media-services-manage-channels-overview.md)。 


##相关的主题

[媒体服务的概念](media-services-concepts.md)

[Azure 媒体服务零碎的 MP4 实时摄取规范](media-services-fmp4-live-ingest-overview.md)





[live overview1]: ./media/media-services-live-streaming-workflow/media-services-live-streaming-new.png

[live overview2]: ./media/media-services-live-streaming-workflow/media-services-live-streaming-current.png
 
测试

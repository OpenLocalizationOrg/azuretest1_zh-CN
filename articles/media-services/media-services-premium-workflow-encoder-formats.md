---
ms.openlocfilehash: e5d6ad9d5e23f43e853fd6c993af6cbb01876e42
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="媒体编码器高级流格式和编解码器" 
    description="本主题概括介绍媒体编码器高级流格式格式和编解码器" 
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

#媒体编码器高级流格式和编解码器


**请注意**本主题中讨论的媒体编码器高级工作流媒体处理器不可用于中国。 

本文档包含的输入和输出文件格式和编解码器支持**媒体编码器高级工作流**编码器的公共预览版本的列表。

[媒体编码器特优 Worflow 输入格式和编解码器](#input_formats)

[媒体编码器特优 Worflow 输出格式和编解码器](#output_formats)

**媒体编码器高级工作流**支持字幕[这](#closed_captioning)一节中所述。 


##<a id="input_formats"></a>媒体编码器高级工作流输入格式和编解码器

下面的部分列出了此媒体处理器支持作为输入的编解码器和文件格式。

###输入容器中的文件格式

- Adobe® Flash® F4V
- MXF/SMPTE 377 米
- GXF
- Mpeg-2 传输流
- MPEG-2 程序流
- MPEG-4/MP4
- Windows 媒体/ASF
- AVI （未压缩 8 位/10 位）

###输入的视频编解码器

- AVC 8 位/10 位，最多支持 4:2:2，包括 AVCIntra
- Avid DNxHD （以 MXF)
- DVCPro/DVCProHD （以 MXF)
- JPEG2000
- MPEG-2 （到 422 配置文件和高级别; 包括如 XDCAM、 XDCAM HD、 IMX XDCAM、 CableLabs® 和 D10 的变体）
- MPEG-1
- Windows 媒体视频/VC-1

###输入的音频编解码器

- AES （SMPTE 331 M 和 302 M、 AES3-2003年）
- E Dolby®
- Dolby® 数字 (AC3)
- AAC (AAC LC、 AAC-他和 AAC-HEv2; 达 5.1)
- MPEG 第 2 层
- MP3 （mpeg-1 音频第 3 层）
- Windows Media 音频
- WAV/PCM
 
##<a id="output_format"></a>媒体编码器高级工作流输出格式和编解码器

下面的部分列出了作为输出从该媒体处理器支持的编解码器和文件格式。

###容器/文件格式

- Adobe® Flash® F4V
- MXF （OP1a、 XDCAM 和 AS02）
- DPP （包括 AS11）
- GXF
- MPEG-4/MP4
- Windows 媒体/ASF
- AVI （未压缩 8 位/10 位）
- 平滑流式文件格式 (PIFF 1.3)
- MPEG TS 


###输出的视频编解码器

- AVC (H.264; 8 位; 最引人注目，多级别 5.2; 4 K 超高清晰度;AVC 内）
- Avid DNxHD （以 MXF)
- DVCPro/DVCProHD （以 MXF)
- MPEG-2 （到 422 配置文件和高级别; 包括如 XDCAM、 XDCAM HD、 IMX XDCAM、 CableLabs® 和 D10 的变体）
- MPEG-1
- Windows 媒体视频/VC-1
- JPEG 缩略图的创建

###输出音频编解码器

- AES （SMPTE 331 M 和 302 M、 AES3-2003年）
- Dolby® 数字 (AC3)
- Dolby® 数字加上 (E AC3) 到 7.1
- AAC (AAC LC、 AAC-他和 AAC-HEv2; 达 5.1)
- MPEG 第 2 层
- MP3 （mpeg-1 音频第 3 层）
- Windows Media 音频

##<a id="closed_captioning"></a>支持字幕

在摄取，**媒体编码器高级工作流**支持︰

1. SCC 文件
1. SMPTE TT 文件
1. CEA-608/CEA-708-用户数据 （SEI 消息的 H.264 基本流，ATSC/53，SCTE20） 作为执行或作为 MXF/GXF 文件中的辅助数据传输
1. STL 字幕文件

在输出时，提供了以下选项︰

1. CEA-608 CEA 708 翻译
1. （嵌入的 H.264 基本流，SEI 邮件中或执行中 MXF 文件作为辅助数据） 通过 CEA 608 CEA 708
1. 源代码管理
1. SMPTE 计时的文本 （从源 CEA 608 每 SMPTE RP2052; 包括创建 DFXP 文件）
1. SRT 字幕文件
1. DVB 的副标题

注意︰ 不是所有上面的输出格式都支持通过 Azure 媒体服务中流的传送。

##已知的问题

如果不包含您输入的视频字幕，输出资产仍将包含一个空的 TTML 文件。 测试

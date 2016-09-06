---
ms.openlocfilehash: eb268d376c1195ececcc3616b2f1cd0ee5e20c5d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="媒体编码标准格式和编解码器" 
    description="本主题概述了 Azure 媒体编码标准格式和编解码器。" 
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

#媒体编码标准格式和编解码器


本文档包含最常见的导入和导出文件格式，您可以使用与媒体编码标准的列表。


##输入容器中的文件格式

文件格式 （文件扩展名）|支持
---|---|---|---
（使用 H.264，AAC 编码解码器） 的 FLV (.flv)          |是 
MXF (.mxf)                  |是 
GXF (.gxf)                  |是 
MPEG2 PS，MPEG2 的 TS 3GP （.ts、.ps、.3gp）    |是 
Windows 媒体视频 (WMV) / ASF （.wmv、.asf） |是 
AVI （未压缩 8 位/10 位） (.avi)|是 
MP4/ISMV (.ismv)|是 
[Microsoft 的数字视频 Recording(DVR-MS)](https://msdn.microsoft.com/library/windows/desktop/dd692984)(.dvr-ms) |是 
Matroska/WebM (.mkv)        |是 
波/WAV (.wav) |是 
 

##输入的视频编解码器

输入的视频编解码器|支持
---|---|---|---
AVC 8 位/10 位，最多支持 4:2:2，包括 AVCIntra   |8 位 4:2:0 和 4:2:2 
Avid DNxHD （以 MXF)                                 |是 
DVCPro/DVCProHD （以 MXF)                            |是 
JPEG 2000                                           |是 
MPEG-2 （到 422 配置文件和高级别; 包括如 XDCAM、 XDCAM HD、 IMX XDCAM、 CableLabs® 和 D10 的变体）|达 422 配置文件 
MPEG-1                                              |是 
VC-1/WMV9                                           |是 
Canopus HQ/HQX                                      |否 
Mpeg-4 Part 2                                       |是 
[Theora](https://en.wikipedia.org/wiki/Theora)      |是 
压缩后的 YUV420 或夹层                   |是


##输入的音频编解码器

输入的音频编解码器|支持
---|---|---|---
AAC (AAC LC、 AAC-他和 AAC-HEv2; 达 5.1)|是 
MPEG 第 2 层|是 
MP3 （mpeg-1 音频第 3 层）|是 
Windows Media 音频|是 
WAV/PCM|是 
[FLAC](https://en.wikipedia.org/wiki/FLAC)</a>|是 
[作品](https://en.wikipedia.org/wiki/Opus _(audio_format) |是 
[Vorbis](https://en.wikipedia.org/wiki/Vorbis)</a>|是 
AMR （自适应多速率）|是
AES （SMPTE 331 M 和 302 M、 AES3-2003年）        |否 
E Dolby®                                    |否 
Dolby® 数字 (AC3)                        |否 
Dolby® 数字再加上 (E AC3)                 |否 


##输出格式和编解码器

下表列出了支持的导出的编解码器和文件格式。


文件格式|视频编解码器|音频编解码器
---|---|---
MP4 (*.mp4)<br/><br/>（包括多比特率 MP4 容器） |H.264 （高、 主和基线配置文件）|AAC LC，他 AAC v1、 v2 他 AAC 
MPEG2 TS |H.264 （高、 主和基线配置文件）|AAC LC，他 AAC v1、 v2 他 AAC 

##请参见

[编码的点播内容与 Azure 媒体服务](media-services-encode-asset.md)

[如何与媒体编码标准进行编码](media-services-dotnet-encode-with-media-encoder-standard.md)

测试

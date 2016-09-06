---
ms.openlocfilehash: 1480347cc4444f4476624181216e02ec3cd6df56
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="Azure 媒体编码格式和编解码器" 
    description="本主题概述了 Azure 媒体编码格式和编解码器。" 
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
    ms.date="08/30/2015" 
    ms.author="juliako"/>

#Azure 媒体编码格式和编解码器

本文档列出最常见的输入和输出文件格式以及可以使用 Azure Media 编码器的编码解码器。


##输入的视频文件格式 （容器）
 
文件格式 （文件扩展名）|支持
---|---
3GPP，3GPP2 （.3gp、.3g2、.3gp2） |是
高级的流格式 (ASF) (.asf)    |是
高级视频编码高清 (AVCHD) [mpeg-2 传输流] （.mts，.m2ts） |是
音频视频交错 (AVI) (.avi)    |是
便携式数字摄像机 MPEG-2 (MOD) (.mod)   |是
DVD 传输流 (TS) 文件 (.ts)    |是
DVD 视频对象 (VOB) 文件 (.vob)  |是
表达式编码器屏幕捕获编解码器文件 (.xesc)    |是
MP4 (.mp4)  |是
Mpeg-1 系统流 （.mpeg、.mpg）  |是
Mpeg-2 视频文件 (.m2v)    |是
平滑流式文件格式 (PIFF 1.3) (.ismv) |是
Windows 媒体视频 (WMV) (.wmv)    |是
Adobe® Flash® F4V           |否     
MXF/SMPTE 377 米              |有限 
GXF                         |否      
[Microsoft 的数字视频 Recording(DVR-MS)](https://msdn.microsoft.com/library/windows/desktop/dd692984)|否
Matroska/WebM               |否

支持某些未压缩的格式。 有关详细信息，请参阅[支持未压缩的视频格式](#uncompressed)

##输入音频文件格式

文件格式 （文件扩展名）|支持
---|---
交流-3 （杜比数字） audio(.ac3)|是
音频交换文件格式 (AIFF)(.aiff)|是
广播的波 Format(.bwf)|是
MP3 (mpeg-1 音频层 3)(.mp3)|是
MP4 audio(.m4A)|是
Mpeg-4 音频 book(.m4b)|是
波 file(.wav)|是
Windows 媒体 Audio(.wma)|是


##输入的视频编解码器

输入的视频编解码器|支持
---|--- 
H.264 （基线，主和高配置文件）           |是
AVC 8 位/10 位，最多支持 4:2:2，包括 AVCIntra   |唯一的 8 位 4:2:0
Avid DNxHD （以 MXF)                                 |否
DVCPro/DVCProHD （以 MXF)                            |否
JPEG2000                                            |否
MPEG-2 (简单和主配置文件和 4:2:2 的配置文件)  |最多 4:2:2 的配置文件
Mpeg-1 (包括 MPEG-PS)                          |是
Windows 媒体视频/VC-1                            |是
Canopus HQ/HQX                                      |是
Mpeg-4 v2 （简单的可视化配置文件和高级简单配置文件）   |是
[Theora](https://en.wikipedia.org/wiki/Theora)      |否
VC-1 （简单，主，和高级配置文件）          |是
Windows 媒体视频 （简单，主，和高级配置文件）   |是
DV DVC、 DVHD、 DVSD (DVSL）                          |是
草谷总部/HQX                                 |是
 

##输入的音频编解码器

输入的音频编解码器|支持
---|---
AES （SMPTE 331 M 和 302 M、 AES3-2003年）        |否
E Dolby®                                    |否
Dolby® 数字 (AC3)                        |是
Dolby® 数字再加上 (E AC3)                 |否
AAC (AAC LC，AAC LC 核心和他 AAC v2 AAC LC 核心; 高达 5.1 他 AAC v1)|是
MPEG 第 2 层|是|是|是
MP3 （mpeg-1 音频第 3 层）|是
Windows Media 音频 9 （Windows 媒体音频标准、 Windows 媒体音频的专业人员，以及 Windows Media 音频无损）    |是
WAV/PCM|是
[FLAC](https://en.wikipedia.org/wiki/FLAC)|否
[作品](https://en.wikipedia.org/wiki/Opus _(audio_format) |否
[Vorbis](https://en.wikipedia.org/wiki/Vorbis)|否


##输入图像文件格式

文件格式 （文件扩展名） | 支持
---|---
位图 (.bmp) | 是
GIF 动画 GIF (.gif)| 是
JPEG （.jpeg、.jpg）| 是
PNG (.png)| 是
TIFF (.tif)| 是
WPF 画布 XAML (.xaml)| 是


##输出格式和编解码器

下表列出了支持的导出的编解码器和文件格式。

文件格式|视频编解码器|音频编解码器
---|---|---
Windows Media (* .wmv;* .wma)|VC-1 （高级，主，和简单的配置文件）|Windows 媒体音频标准 Windows 媒体音频专业版、 Windows 媒体音频语音、 无损的 Windows Media 音频
MP4 (*.mp4)|H.264 （高、 主和基线配置文件）|AAC LC，他 AAC v1、 他 AAC v2、 杜比数字加上
平滑流式文件格式 (PIFF 1.1) (* .ismv;* .isma)|VC-1 （高级配置文件）<p>H.264 （高、 主和基线配置文件） |Windows 媒体音频标准，Windows 音频媒体专业人员<p><p>AAC LC，他 AAC v1、 v2 他 AAC

有关其他支持的编解码器和媒体服务中的筛选器，请参阅[Windows DirectShow 的筛选器](https://msdn.microsoft.com/library/windows/desktop/dd375464.aspx)。

##<a id="uncompressed"></a>支持的未压缩的视频格式 

Azure 媒体服务导入压缩视频数据提供支持。

这是所支持的未压缩格式的部分列表。

未压缩的视频格式|说明
---|---
标准的 YVU9 格式未压缩数据|平面的 YUV 格式。 在每个像素，则 Y 样本和 V 水平在每一行; 每四个像素的示例在每个垂直的线条，则 Y 样本和每像素位数在每个垂直第四 line.9 V 示例。
YUV 411 设置数据格式|在每个像素，则 Y 样本和 V 水平在每一行; 每四个像素的示例每个采样的垂直线条。 字节排序 （首先为最小） 是 U0、 Y0、 V0，Y1、 U4，Y2、 V4、 Y3、 Y4、 Y5、 Y6、 Y7，其中后缀 0 是最左边的像素，越来越多的像素增加从左到右。 每个 12 字节块为 8 的图像像素。
Y41P 数据的格式|在每个像素，则 Y 样本和 V 水平在每一行; 每四个像素的示例每个采样的垂直线条。字节排序 （首先为最小） 是 U0、 Y0、 V0，Y1、 U4，Y2、 V4、 Y3、 Y4、 Y5、 Y6、 Y7，其中后缀 0 是最左边的像素，越来越多的像素增加从左到右。 每个 12 字节块为 8 的图像像素。
YUY2 数据的格式|同为 UYVY，但使用不同的像素顺序。 字节排序 （首先为最小） 是 Y0，U0、 Y1，V0，Y2、 U2、 Y3，V2、 Y4、 U4，Y5，V4，其中后缀 0 是最左边的像素，越来越多的像素增加从左到右。 每个 4 字节块为 2 图像像素。
YVYU 数据的格式|打包的 YUV 格式。 同为 UYVY，但使用不同的像素顺序。 字节排序 （首先为最小） 为 Y0、 V0，Y1、 U0、 Y2，V2、 Y3、 U2、 Y4、 V4、 Y5、 U4，其中后缀 0 是最左边的像素，越来越多的像素增加从左到右。 每个 4 字节块为 2 图像像素。
UYVY 数据的格式|打包的 YUV 格式。 在每个像素，则 Y 样本和 V 示例在每第二个像素水平在每一行中;每个采样的垂直线条。 最受欢迎的各种 YUV 4:2:2 的格式。 字节排序 （首先为最小） 是 U0、 Y0、 V0，Y1，U2、 Y2，V2、 Y3、 U4，Y4、 V4，Y5，其中后缀 0 是最左边的像素，越来越多的像素增加从左到右。 每个 4 字节块为 2 图像像素。
YUV 211 设置数据格式|打包的 YUV 格式。 在每个第二个像素，则 Y 样本和 V 水平在每一行; 每四个像素的示例每个采样的垂直线条。字节排序 （首先为最小） 为 Y0，U0、 Y2、 V0，Y4、 U4、 Y6、 V4、 Y8、 U8、 Y10，V8，其中后缀 0 是最左边的像素，越来越多的像素增加从左到右。 每个 4 字节块为 4 图像像素。
Cirrus 逻辑职位需求 YUV 411 格式|少于 8 位每个 Y，U 和 V 的采样 cirrus 逻辑职位需求 YUV 411 格式。 在每个像素，则 Y 样本和 V 水平在每一行; 每四个像素的示例每个采样的垂直线条。
Indeo 产生 YVU9 格式|Indeo 产生 YVU9 格式的最后一帧的区别的其他信息。 但报告 9 9.5 位 / 像素。

测试

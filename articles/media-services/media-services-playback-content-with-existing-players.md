---
ms.openlocfilehash: d000fead4bfff7ac6becaa8967f66eea38b3855c
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="播放您的内容" 
    description="本主题列出了现有的播放器，您可以使用播放您的内容。" 
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


#播放您的内容与现有的玩家

Azure 媒体服务支持许多流行的流格式，如平滑流式处理、 实时流式 HTTP，和 MPEG-短线。 本主题列出了可用来测试您的流中的现有队员的。  

>[AZURE.NOTE]对播放动态打包或动态加密的内容，请确保您打算将内容传递的流终结点获取至少一个流的计价单位。 有关扩展流单元的信息，请参阅︰[如何缩放流式处理单位](media-services-manage-origins.md#scale_streaming_endpoints)。

###Azure 管理门户媒体服务内容播放

**Azure 管理门户**提供了可用来测试您的视频内容的播放机。

单击所需的视频上 （确保它被[发布](media-services-manage-content.md#publish)），然后单击**播放**按钮，底部的门户。 
 
一些注意事项︰

- **媒体服务内容播放机**播放从默认的流终结点。 如果您想要从非默认流终结点播放，使用另一个播放机。 例如，为[Azure 服务媒体播放](http://amsplayer.azurewebsites.net/azuremediaplayer.html)。
 

![AMSPlayer][AMSPlayer]

###Azure 服务媒体播放

使用[Azure 媒体服务播放器](http://amsplayer.azurewebsites.net/azuremediaplayer.html)播放在下列格式中的任何内容 （清除或受保护）︰

- 平滑流式处理
- MPEG 短划线
- HLS
- 渐进式的 MP4


###Flash 播放器

####AES 加密令牌 

[http://aestoken.azurewebsites.net]("http://aestoken.azurewebsites.net)

###Silverlight 播放器

####监视

[http://smf.cloudapp.net/healthmonitor](http://smf.cloudapp.net/healthmonitor)

####PlayReady 与标记

[http://sltoken.azurewebsites.net](http://sltoken.azurewebsites.net)

### 短划线机

[http://dashplayer.azurewebsites.net](http://dashplayer.azurewebsites.net)

[http://dashif.org](http://dashif.org)

###其他

若要测试 HLS Url，您也可以使用︰

- **Safari**在 iOS 设备上或
- 在 Windows **3ivx HLS 播放机**。

##开发视频播放器

有关如何开发自己的播放器的信息，请参阅[开发视频播放器](media-services-develop-video-players.md)
 
[AMSPlayer]: ./media/media-services-playback-content-with-existing-players/media-services-portal-player.png 
测试t

---
ms.openlocfilehash: 4582941806d5a299be4a53fe572d131a5614aa47
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="在 DASH.js 的 HTML5 应用程序中嵌入 MPEG-短线自适应流式视频" 
    description="本主题演示如何在 DASH.js 的 HTML5 应用程序中嵌入 MPEG-短线自适应流式视频。" 
    authors="Juliako" 
    manager="dwrede" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/11/2015" 
    ms.author="juliako"/>


#在 DASH.js 的 HTML5 应用程序中嵌入 MPEG-短线自适应流式视频

##概述

MPEG 划线是带来明显的好处，对于那些希望提供高质量的、 自适应视频流输出的视频内容自适应流式处理的 ISO 标准。 与 MPEG-短线，视频流将自动放到一个较低的定义，当网络变得拥塞。 这样可以减少出现"暂停"视频时播放机下载播放 （亦即缓冲） 下一步几秒钟的查看器的可能性。 同时减少了网络拥塞，视频播放器将又返回到更高的质量流。 这种能力来适应所需的带宽也会导致视频更快的启动时间。 这意味着可以在快到下载的低质量段播放前的几秒钟，缓冲逐步增加高质量的一次足够内容后再。

Dash.js 是用 JavaScript 编写的一个开源 MPEG-短线视频播放器。 其目标是提供可靠、 跨平台玩家可以自由地重新使用需要视频播放的应用程序。 它提供了在任何浏览器支持 W3C 媒体源扩展 (MSE) 今天是铬和 IE11 （其他浏览器已经表示他们希望支持 MSE） 的 MPEG-短线播放。 有关 DASH.js，js 请参阅 GitHub dash.js 存储库。


##创建一个基于浏览器的流式视频播放器

要创建简单的 web 页显示与预期一个视频播放器控件这样播放、 暂停、 后退等，您将需要︰

1. 创建一个 HTML 页面
1. 添加视频标记
1. 添加 dash.js 播放器
1. 初始化播放机
1. 添加一些 CSS 样式
1. 实现 MSE 的浏览器中查看结果

刚才的几个 JavaScript 代码行中，可以完成初始化播放机。 使用 dash.js，它真的就这么简单 MPEG-短线视频嵌入您基于浏览器的应用程序。


##创建 HTML 页

第一步是创建标准的 HTML 页包含 <video> 元素，保存该文件作为 basicPlayer.html，如下面的示例所示︰
    
    <!DOCTYPE html>
    <html>
      <head><title>Adaptive Streaming in HTML5</title></head>
      <body>
        <h1>Adaptive Streaming with HTML5</h1>
        <video id="videoplayer" controls></video>
      </body>
    </html>

##添加 DASH.js 播放器

若要向应用程序添加 dash.js 参考实现，您需要获取来自 dash.js 项目的 1.0 版本的 dash.all.js 文件。 这应在 JavaScript 应用程序的文件夹中保存。 此文件是齐心协力到单个文件的所有必要的 dash.js 代码方便文件。 如果您有一看周围 dash.js 存储库时，将发现个别文件，测试代码，如此等等，但如果所有您想要做的是使用 dash.js 然后 dash.all.js 文件就是您所需要。

若要向应用程序添加 dash.js 播放器，请向 basicPlayer.html 的 head 部分添加脚本标记︰

    <!-- DASH-AVC/265 reference implementation -->
    < script src="js/dash.all.js"></script>


接下来，创建一个函数在页面加载时初始化播放机。 在其中加载 dash.all.js 的行后面添加下面的脚本︰

    <script>
    // setup the video element and attach it to the Dash player
    function setupVideo() {
      var url = "http://wams.edgesuite.net/media/MPTExpressionData02/BigBuckBunny_1080p24_IYUV_2ch.ism/manifest(format=mpd-time-csf)";
      var context = new Dash.di.DashContext();
      var player = new MediaPlayer(context);
                      player.startup();
                      player.attachView(document.querySelector("#videoplayer"));
                      player.attachSource(url);
    }
    </script>

此函数首先创建 DashContext。 这用来配置应用程序的特定运行时环境。 从技术的角度来看，它定义了依赖关系注入框架构建应用程序时应使用的类。 在大多数情况下，您将使用 Dash.di.DashContext。

接下来，实例化的 dash.js framework MediaPlayer 的主要类。 此类包含的方法如需播放和暂停、 管理与视频元素之间的关系和还可管理的媒体演示文稿说明 (MPD) 文件，它描述了要播放的视频的解释的核心。

MediaPlayer 类 startup() 函数以确保播放机就可以播放视频。 在其它事物之间此函数可以确保，所有需要的类 （按定义上下文） 已加载。 播放机后，您可以使用 attachView() 函数为其附加视频元素。 这使得视频流注入元素和控制必要时播放 MediaPlayer。

MediaPlayer 到传递 MPD 文件的 URL，以便它能够知道关于视频有望播放。刚创建的 setupVideo() 函数需要页面完全加载后执行。 使用 onload 事件的 body 元素执行此操作。 更改您<body>元素︰

    <body onload="setupVideo()">

最后，设置使用 CSS 的视频元素的大小。 在自适应流式处理环境中，这一点尤其重要，因为正在播放的视频的大小可能会随播放适应不断变化的网络条件。 在这个简单的演示只是强制视频元素通过将下面的 CSS 添加到网页的 head 部分中为 80%的可用的浏览器窗口︰
    
    <style>
    video {
      width: 80%;
      height: 80%;
    }
    </style>

##播放视频

若要播放的视频，basicPlayback.html 文件在您浏览器指向并单击显示的视频播放器上播放。

##请参见

[开发视频播放器应用程序](media-services-develop-video-players.md)

[GitHub dash.js 资料库](https://github.com/Dash-Industry-Forum/dash.js)测试

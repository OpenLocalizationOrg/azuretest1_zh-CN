---
ms.openlocfilehash: 2931651df6e6d5717baa954eff8b3fb8fdb7be4d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="打开源媒体框架用于平滑流式插件。" 
    description="了解如何使用 Adobe 开放源媒体框架 Azure 媒体服务平滑流式处理插件。" 
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
    ms.date="06/17/2015" 
    ms.author="juliako"/>



# 如何使用 Microsoft 平滑流式 Adobe 开源媒体框架的插件 #

##概述 ##


打开源介质 Framework 2.0 (为 OSMF SS) 的 Microsoft 平滑流式处理插件扩展 OSMF 的默认功能，并向新的和现有的 OSMF 播放器添加 Microsoft 平滑流式内容回放。 该插件还将平滑流式播放功能添加到闸门媒体回放 (SMP)。

对于 OSMF SS 包括两个版本的插件︰

- OSMF (.swc) 的静态平滑流式处理插件

- OSMF (.swf) 的动态平滑流式处理插件

本文档假定读者已常规了解 OSMF 和 OSMF 插件。 有关 OSMF 的详细信息，请参阅[官方 OSMF 站点](http://osmf.org/)上的文档。

###用于 OSMF 2.0 平滑流式插件。

该插件支持加载和播放点播平滑流式内容具有以下功能︰

- 按需平滑流式播放 （播放，暂停，搜寻，停止）
- 实时平滑流式播放 （播放）
- 实时的 DVR 功能 （暂停、 搜寻、 DVR 回放，实时转）
- 对视频编解码器的 H.264 支持
- 支持的音频编解码器的 AAC
- OSMF 内置 Api 切换功能的多个音频语言
- OSMF 内置 Api 与最大播放质量选项
- 与 OSMF 标题插件附属字幕
- Adobe 拾&reg;闪存&reg;11.4 或更高版本的播放机。
- 此版本仅支持 OSMF 2.0。

## 受支持的功能和已知的问题

有关受支持的功能，不支持的功能和已知的问题的完整列表，请参阅[本文档](http://download.microsoft.com/download/3/1/B/31B63D97-574E-4A8D-BF8D-170744181724/Smooth_Streaming_Plugin_for_OSMF.pdf)。


## 加载插件
OSMF 插件可以加载静态 （在编译时） 或动态 （运行时）。 OSMF 下载的平滑流式处理插件包含动态和静态版本。

- 静态加载︰ 静态加载，静态库 (SWC) 文件是必需的。 在编译时静态的插件添加作为参考的项目和内部的最终输出文件的合并。

- 动态加载︰ 以动态加载，预编译 (SWF) 文件是必需的。 动态插件在运行时加载并不包括在项目输出。 （编译后的输出）可以使用 HTTP 和文件协议加载动态插件。

静态和动态加载的详细信息，请参阅官方[OSMF 插件页](http://osmf.org/dev/osmf/OtherPDFs/osmf_plugin_dev_guide.pdf)。

###SS OSMF 静态加载
以下代码片段显示了如何为 OSMF 静态加载 SS 插件和使用 OSMF MediaFactory 类基本视频播放。 包括 OSMF 代码 SS 之前, 请确保项目引用包括静态"MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swc"插件。

```
package 
{
    
    import com.microsoft.azure.media.AdaptiveStreamingPluginInfo;
    
    import flash.display.*;
    import org.osmf.media.*;
    import org.osmf.containers.MediaContainer;
    import org.osmf.events.MediaErrorEvent;
    import org.osmf.events.MediaFactoryEvent;
    import org.osmf.events.MediaPlayerStateChangeEvent;
    import org.osmf.layout.*;
    
    
    
    [SWF(width="1024", height="768", backgroundColor='#405050', frameRate="25")]
    public class TestPlayer extends Sprite
    {        
        public var _container:MediaContainer;
        public var _mediaFactory:DefaultMediaFactory;
        private var _mediaPlayerSprite:MediaPlayerSprite;
        

        public function TestPlayer( )
        {
            stage.quality = StageQuality.HIGH;

            initMediaPlayer();

        }
    
        private function initMediaPlayer():void
        {
        
            // Create the container (sprite) for managing display and layout
            _mediaPlayerSprite = new MediaPlayerSprite();    
            _mediaPlayerSprite.addEventListener(MediaErrorEvent.MEDIA_ERROR, onPlayerFailed);
            _mediaPlayerSprite.addEventListener(MediaPlayerStateChangeEvent.MEDIA_PLAYER_STATE_CHANGE, onPlayerStateChange);
            _mediaPlayerSprite.scaleMode = ScaleMode.NONE;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;
            //Adds the container to the stage
            addChild(_mediaPlayerSprite);
            
            // Create a mediafactory instance
            _mediaFactory = new DefaultMediaFactory();
            
            // Add the listeners for PLUGIN_LOADING
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD,onPluginLoaded);
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD_ERROR, onPluginLoadFailed );
            
            // Load the plugin class 
            loadAdaptiveStreamingPlugin( );  
            
        }
        
        private function loadAdaptiveStreamingPlugin( ):void
        {
            var pluginResource:MediaResourceBase;
            
            pluginResource = new PluginInfoResource(new AdaptiveStreamingPluginInfo( )); 
            _mediaFactory.loadPlugin( pluginResource ); 
        }
        
        private function onPluginLoaded( event:MediaFactoryEvent ):void
        {
            // The plugin is loaded successfully.
            // Your web server needs to host a valid crossdomain.xml file to allow plugin to download Smooth Streaming files.
        loadMediaSource("http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest")
        
        }
        
        private function onPluginLoadFailed( event:MediaFactoryEvent ):void
        {
            // The plugin is failed to load ...
        }
        
        
        private function onPlayerStateChange(event:MediaPlayerStateChangeEvent) : void
        {
            var state:String;
            
            state =  event.state;
            
            switch (state)
            {
                case MediaPlayerState.LOADING: 
                    
                    // A new source is started to load.
                    
                    break;
                
                case  MediaPlayerState.READY :   
                    // Add code to deal with Player Ready when it is hit the first load after a source is loaded. 
                    
                    break;
                
                case MediaPlayerState.BUFFERING :
                    
                    break;
                
                case  MediaPlayerState.PAUSED :
                    break;      
                // other states ...          
            }
        }
        
        private function onPlayerFailed(event:MediaErrorEvent) : void
        {
            // Media Player is failed .           
        }
        
        private function loadMediaSource(sourceURL : String):void 
        {
            // Take an URL of SmoothStreamingSource's manifest and add it to the page.
            
            var resource:URLResource= new URLResource( sourceURL );
            
            var element:MediaElement = _mediaFactory.createMediaElement( resource );
            _mediaPlayerSprite.scaleMode = ScaleMode.LETTERBOX;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;
            
            // Add the media element
            _mediaPlayerSprite.media = element;
        }     
        
    }
}
```


###SS OSMF 动态加载

下面的代码段演示如何为 OSMF 动态加载 SS 插件并播放一个基本视频使用 OSMF MediaFactory 类。 之前包括 SS OSMF 代码，将"MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf"动态插件复制到项目文件夹中，如果您想要加载使用文件协议，或者复制 web 服务器的 HTTP 负载下。 没有必要要包含在项目引用中的"MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swc"。

```
package 
{
    
    import flash.display.*;
    import org.osmf.media.*;
    import org.osmf.containers.MediaContainer;
    import org.osmf.events.MediaErrorEvent;
    import org.osmf.events.MediaFactoryEvent;
    import org.osmf.events.MediaPlayerStateChangeEvent;
    import org.osmf.layout.*;
    import flash.events.Event;
    import flash.system.Capabilities;

    
    //Sets the size of the SWF
    
    [SWF(width="1024", height="768", backgroundColor='#405050', frameRate="25")]
    public class TestPlayer extends Sprite
    {        
        public var _container:MediaContainer;
        public var _mediaFactory:DefaultMediaFactory;
        private var _mediaPlayerSprite:MediaPlayerSprite;
        
        
        public function TestPlayer( )
        {
            stage.quality = StageQuality.HIGH;
            initMediaPlayer();
        }
        
        private function initMediaPlayer():void
        {
            
            // Create the container (sprite) for managing display and layout
            _mediaPlayerSprite = new MediaPlayerSprite();    
            _mediaPlayerSprite.addEventListener(MediaErrorEvent.MEDIA_ERROR, onPlayerFailed);
            _mediaPlayerSprite.addEventListener(MediaPlayerStateChangeEvent.MEDIA_PLAYER_STATE_CHANGE, onPlayerStateChange);

            //Adds the container to the stage
            addChild(_mediaPlayerSprite);
            
            // Create a mediafactory instance
            _mediaFactory = new DefaultMediaFactory();
            
            // Add the listeners for PLUGIN_LOADING
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD,onPluginLoaded);
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD_ERROR, onPluginLoadFailed );
            
            // Load the plugin class 
            loadAdaptiveStreamingPlugin( );  
            
        }
        
        private function loadAdaptiveStreamingPlugin( ):void
        {
            var pluginResource:MediaResourceBase;
            var adaptiveStreamingPluginUrl:String;

            // Your dynamic plugin web server needs to host a valid crossdomain.xml file to allow loading plugins.

            adaptiveStreamingPluginUrl = "http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf";
            pluginResource = new URLResource(adaptiveStreamingPluginUrl);
            _mediaFactory.loadPlugin( pluginResource ); 

        }
        
        private function onPluginLoaded( event:MediaFactoryEvent ):void
        {
            // The plugin is loaded successfully.

            // Your web server needs to host a valid crossdomain.xml file to allow plugin to download Smooth Streaming files.

    loadMediaSource("http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest")
        }
        
        private function onPluginLoadFailed( event:MediaFactoryEvent ):void
        {
            // The plugin is failed to load ...
        }
        
        
        private function onPlayerStateChange(event:MediaPlayerStateChangeEvent) : void
        {
            var state:String;
            
            state =  event.state;
            
            switch (state)
            {
                case MediaPlayerState.LOADING: 
                    
                    // A new source is started to load.
                    
                    break;
                
                case  MediaPlayerState.READY :   
                    // Add code to deal with Player Ready when it is hit the first load after a source is loaded. 
                    
                    break;
                
                case MediaPlayerState.BUFFERING :
                    
                    break;
                
                case  MediaPlayerState.PAUSED :
                    break;      
                // other states ...          
            }
        }
        
        private function onPlayerFailed(event:MediaErrorEvent) : void
        {
            // Media Player is failed .           
        }
        
        private function loadMediaSource(sourceURL : String):void 
        {
            // Take an URL of SmoothStreamingSource's manifest and add it to the page.
            
            var resource:URLResource= new URLResource( sourceURL );
            
            var element:MediaElement = _mediaFactory.createMediaElement( resource );
            _mediaPlayerSprite.scaleMode = ScaleMode.LETTERBOX;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;
            // Add the media element
            _mediaPlayerSprite.media = element;
        }     
        
    }
}
```

##明暗闪动媒体播放 SS ODMF 动态插件
OSMF 动态插件的平滑流式处理后[闸门媒体回放 (SMP)](http://osmf.org/strobe_mediaplayback.html)兼容。 可以使用 OSMF 插件的 SS SMP 向添加平滑流式内容回放。 要做到这一点，将在 web 服务器的 HTTP 负载可以通过以下步骤复制"MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf":

1.  浏览[闪光媒体回放设置页面](http://osmf.org/dev/2.0gm/setup.html)。 
2.  将 src 设置为一个平滑流式处理源，(例如 http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest) 
3.  进行所需的配置更改，然后单击预览和更新。
 
    **请注意**您的内容的 web 服务器都需要有效的 crossdomain.xml。 
4.  复制并粘贴到一个简单的 HTML 页面，使用您喜爱的文本编辑器，如下面的示例中的代码︰



        <html>
        <body>
        <object width="920" height="640"> 
        <param name="movie" value="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf"></param>
        <param name="flashvars" value="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest &autoPlay=true"></param>
        <param name="allowFullScreen" value="true"></param>
        <param name="allowscriptaccess" value="always"></param>
        <param name="wmode" value="direct"></param>
        <embed src="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf" 
            type="application/x-shockwave-flash" 
            allowscriptaccess="always" 
            allowfullscreen="true" 
            wmode="direct" 
            width="920" 
            height="640" 
            flashvars=" src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true">
        </embed>
        </object>
        </body>
        </html>



5. 将平滑流式传输的 OSMF 插件添加到嵌入代码并保存。

        <html>
        <object width="920" height="640"> 
        <param name="movie" value="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf"></param>
        <param name="flashvars" value="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true&plugin_AdaptiveStreamingPlugin=http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf&AdaptiveStreamingPlugin_retryLive=true&AdaptiveStreamingPlugin_retryInterval=10"></param>
        <param name="allowFullScreen" value="true"></param>
        <param name="allowscriptaccess" value="always"></param>
        <param name="wmode" value="direct"></param>
        <embed src="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf" 
            type="application/x-shockwave-flash" 
            allowscriptaccess="always" 
            allowfullscreen="true" 
            wmode="direct" 
            width="920" 
            height="640" 
            flashvars="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true&plugin_AdaptiveStreamingPlugin=http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf&AdaptiveStreamingPlugin_retryLive=true&AdaptiveStreamingPlugin_retryInterval=10">
        </embed>
        </object>
        </html>


6.  将 HTML 页保存并发布到 web 服务器。 浏览到已发布的 web 页使用您最喜爱的闪存&reg;播放器启用 Internet 浏览器 (Internet Explorer，镶边，Firefox，等等)。
7.  享受在 Adobe 平滑流式内容&reg;闪存&reg;播放器。

常规的 OSMF 开发的详细信息，请参阅官方[OSMF 开发页面](http://osmf.org/resources.html)。


##请参见

[自适应流式插件 OSMF 更新 Microsoft](http://azure.microsoft.com/blog/2014/10/27/microsoft-adaptive-streaming-plugin-for-osmf-update/) 

测试

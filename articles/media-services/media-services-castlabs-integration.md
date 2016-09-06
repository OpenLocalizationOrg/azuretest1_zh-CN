---
ms.openlocfilehash: 3e30e62fb42e8b77369dff4ea83fb4dc05805cea
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="使用 castLabs 来将 DRM 许可证传送到 Azure 媒体服务" 
    description="本文介绍如何使用 Azure 媒体服务 (AMS) 提供动态加密通过 PlayReady 和 Widevine DRMs AMS 的流。 PlayReady 许可证都来自于媒体服务 PlayReady 许可证服务器和 Widevine 许可证提供的 castLabs 许可证服务器。" 
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


#使用 castLabs 来将 DRM 许可证传送到 Azure 媒体服务

##概述

本文介绍如何使用 Azure 媒体服务 (AMS) 提供动态加密通过 PlayReady 和 Widevine DRMs AMS 的流。 PlayReady 许可证都来自于媒体服务 PlayReady 许可证服务器和 Widevine 许可证提供的**castLabs**许可证服务器。

下图演示了高层 Azure 媒体服务和 castLabs 集成体系结构。

![集成](./media/media-services-castlabs-integration/media-services-castlabs-integration.png)

##典型的系统设置

- 媒体内容存储在 AMS。
- 内容密钥的密钥 Id 存储在 castLabs 和 AMS。
- castLabs 和 AMS 都有内置的令牌身份验证。 以下各节讨论了身份验证令牌。 
- 当视频流的客户端请求时，内容是使用**常见的加密**(CENC) 动态加密并且动态打包通过平滑流式处理和短划线的 AMS。 我们还提供了 PlayReady M2TS 的 HLS 流协议的基本流加密。
- 从 AMS 许可证服务器中检索 PlayReady 许可证和 Widevine 许可证检索从 castLabs 许可证服务器。 
- 媒体播放器会自动决定哪种许可获取根据客户端平台功能。 

##有关获取许可证的身份验证令牌生成

CastLabs 和 AMS 支持 JWT （JSON Web 标记） 令牌格式用于授权许可证。 

###在 AMS JWT 标记 

下表描述在 AMS JWT 令牌。 

颁发者|从所选的颁发者字符串安全令牌服务 (STS)
---|---
受众|观众从使用 STS 的字符串
索赔|一组声明
NotBefore|开始标记的有效性
到期|最终有效性的标记
SigningCredentials|在许可证服务器和 STS，castLabs 的 PlayReady 许可证服务器之间共享的密钥可以是对称或非对称密钥。

###在 castLabs JWT 标记

下表描述了 JWT 标记 castLabs 中。 

名称|说明
---|---
optData|JSON 字符串，其中包含关于您的信息。 
crt|JSON 字符串包含信息资产，其许可信息和播放权。
iat|日期在当前日期时间。
jti|有关此标记 （每个标记可以只使用一次 castLabs 系统中） 的唯一标识符。

##设置示例解决方案 

此[示例解决方案](https://github.com/AzureMediaServicesSamples/CastlabsIntegration)包括两个项目︰

-   控制台应用程序，可以用于已 ingested 资产，DRM 限制设置为 PlayReady 和 Widevine。
-   手出可能被视为非常简化的 STS 版本的标记一个 Web 应用程序。


若要使用控制台应用程序︰

1.  更改的控件设置了 AMS 凭据、 castLabs 凭据、 STS 配置和共享的密钥。
2.  上载到 AMS 的资产。
3.  从上载资产，获取 UUID 和更改行中的 Program.cs 文件的 32:

         var objIAsset = _context.Assets.Where(x => x.Id == "nb:cid:UUID:dac53a5d-1500-80bd-b864-f1e4b62594cf").FirstOrDefault();

4.  使用 AssetId 的 castLabs 系统 (行 44 Program.cs 文件中) 中命名该资产。

    必须将 AssetId 设置为**castLabs**;它必须是一个唯一的字母数字字符串。

5.  运行该程序。


若要使用 Web 应用程序 (STS):

1.  更改安装程序 castlabs 商家到 web.config ID、 STS 配置和共享的密钥。
2.  将部署到 Azure 网站。
3.  导航到该网站。

##播放视频

播放视频用通用加密 (PlayReady)，您可以使用[Azure 媒体播放器](http://amsplayer.azurewebsites.net/azuremediaplayer.html)。 当运行控制台应用程序，回显内容密钥 ID 和清单 URL。

1.  打开一个新的选项卡，然后启动您 STS: http://[yourStsName].azurewebsites.net/api/token/assetid/[yourCastLabsAssetId]/contentkeyid/[thecontentkeyid]。
2.  转到[Azure 媒体播放器](http://amsplayer.azurewebsites.net/azuremediaplayer.html)。
3.  将粘贴的流的 URL 中。
4.  单击**高级选项**复选框。
5.  在**保护**下拉列表中选择 PlayReady。
6.  粘贴你从您在标记文本框的 STS 的标记。
7.  更新播放机。
8.  应播放视频。

有关播放受保护的视频与铬的 HTML5 中的 castLabs 播放器，请联系 yanmf@microsoft.com 才能访问到播放机。 当您具有访问权限时，有 2 件事要注意︰

1.  CastLabs 播放器需求的 MPEG-短线清单文件的访问权限，因此追加 (格式 = mpd-时间-csf) 对清单文件以获取 MPEG-短线清单文件，而不是默认的平滑流式处理一个。

2.  CastLab 许可证服务器不需要"持有者 ="前缀标记的前面。 因此提交该标记之前，请移除它。

 

测试

---
ms.openlocfilehash: 5ae2649943f29271af0bb97b27115a84b7138ffb
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="配置使用门户的内容密钥授权策略" 
    description="了解如何配置内容密钥的授权策略。" 
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



#配置内容主要授权策略 
[AZURE.INCLUDE [media-services-selector-content-key-auth-policy](../../includes/media-services-selector-content-key-auth-policy.md)]


##概述

Microsoft Azure 媒体服务使您能够提供您使用高级加密标准 (AES) （使用 128 位加密密钥） 和 PlayReady DRM 加密的内容。 介质服务还提供了**Key\License 传递服务**从其客户端可以获得密钥或许可证来播放加密的内容。 

本主题演示如何使用**Azure 管理门户**配置内容密钥的授权策略。 以后可以使用密钥动态加密您的内容。 请注意，当前可以加密下面流格式︰ HLS、 MPEG 划线和平滑流式处理。 无法加密流格式，HDS 或渐进式下载。
 
当一个玩家请求设置要动态加密的流时，介质服务使用配置的密钥动态加密您使用 AES 或 PlayReady 加密的内容。 要解密流，播放机将请求密钥传递服务密钥。 若要确定用户有权获取密钥，服务评估您为密钥指定的授权策略。


如果您计划有多个内容密钥，或者想要指定**Key\License 传递服务**URL 以外媒体服务密钥传递服务，使用介质服务.NET SDK 或 REST Api。

[配置使用介质服务.NET SDK 的内容密钥授权策略](media-services-dotnet-configure-content-key-auth-policy.md)

[配置使用 REST API，媒体服务内容密钥授权策略](media-services-rest-configure-content-key-auth-policy.md)

###一些注意事项︰

- 若要能够使用动态打包和动态加密，您必须确保至少有一个流保留的单位。 有关详细信息，请参阅[如何调整介质服务](media-services-manage-origins.md#scale_streaming_endpoints)。 
- 您的资产必须包含一组自适应比特率 MP4s 或自适应的比特率平滑流式处理文件。 有关详细信息，请参阅[编码资产](media-services-encode-asset.md)。  
- 键传递服务缓存 15 分钟 ContentKeyAuthorizationPolicy 和其相关的对象 （策略选项和限制）。  如果您创建 ContentKeyAuthorizationPolicy，并指定要使用"的令牌"的限制，然后测试它，然后更新的策略，"开放"的限制，需要大约 15 分钟前策略切换到"开放"的策略版本。


##如何︰ 配置密钥的授权策略

要配置密钥的授权策略，请选择**内容保护**页面。
    
媒体服务支持多种方法进行关键请求的用户进行身份验证。 内容密钥授权策略可以**打开**、**标记**或**IP**授权限制 （与其余部分或.NET SDK，就可以配置**IP** ）。 

###打开限制

**打开**限制意味着系统会将密钥传递给密钥申请的人。 这种限制可能适用于测试目的。

![OpenPolicy][open_policy]

###限制标记

若要选择受限制的令牌策略，请按**标记**按钮。

**标记**限制策略必须伴随颁发的**安全令牌服务**(STS) 的标记。 媒体服务支持的**简单 Web 标记**([SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)) 格式和**JSON Web 标记**(JWT) 的格式标记。 有关信息，请参阅[JWT 令牌的身份验证](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)。

媒体服务不提供**安全令牌服务**。 您可以创建自定义的 STS 或利用 Microsoft Azure ACS 对问题标记。 必须配置 STS 创建签名与指定标记限制配置中指定的密钥和问题索赔的标记。 如果令牌无效并且令牌中的声明匹配那些配置为内容的密钥，介质服务密钥传递服务将向客户端返回加密密钥。 有关详细信息，请参阅[对问题标记使用 Azure ACS](http://mingfeiy.com/acs-with-key-services)。

配置**标记**限制时策略，您必须设置值为**验证密钥**、**颁发者**和**观众**。 主要验证密钥包含与签名令牌的密钥，颁发者已颁发令牌的安全令牌服务。 （有时称为作用域） 的观众介绍了该标记的目的或资源标记授予的访问权限。 媒体服务密钥传递服务验证令牌中的这些值与该模板中的值相匹配。  

###PlayReady

保护您的**PlayReady**的内容，您需要授权策略中指定的任务之一时，PlayReady 许可证模板定义一个 XML 字符串。 默认情况下，设置以下策略︰
        
    <PlayReadyLicenseResponseTemplate xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1">
      <LicenseTemplates>
        <PlayReadyLicenseTemplate><AllowTestDevices>true</AllowTestDevices>
          <ContentKey i:type="ContentEncryptionKeyFromHeader" />
          <LicenseType>Nonpersistent</LicenseType>
          <PlayRight>
            <AllowPassingVideoContentToUnknownOutput>Allowed</AllowPassingVideoContentToUnknownOutput>
          </PlayRight>
        </PlayReadyLicenseTemplate>
      </LicenseTemplates>
    </PlayReadyLicenseResponseTemplate>

可以单击**导入策略 xml**按钮并提供对 XML 架构定义的不同的 XML，这符合[此处](https://msdn.microsoft.com/library/azure/dn783459.aspx)。

##下一步行动
现在，您已经配置内容密钥授权策略，请转到[如何︰ 启用加密，使用 Azure 管理门户](../media-services-manage-content#encrypt/)主题。


[open_policy]: ./media/media-services-portal-configure-content-key-auth-policy/media-services-protect-content-with-open-restriction.png
[token_policy]: ./media/media-services-key-authorization-policy/media-services-protect-content-with-token-restriction.png

 
测试

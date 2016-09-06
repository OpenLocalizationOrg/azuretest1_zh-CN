---
ms.openlocfilehash: 1ef4ff6cc4732583724491a962052fb657ba6bf2
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="保护内容概述" 
    description="本文概括了与介质服务内容保护。" 
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

#保护内容概述


Microsoft Azure 媒体服务使您能够保护您的媒体则将保留您的计算机通过存储、 处理和传递的时间。 媒体服务使您能够提供您使用高级加密标准 (AES) （使用 128 位加密密钥） 和 PlayReady DRM 加密动态的内容。 介质服务还提供了用于提供键和 PlayReady 许可证授权客户端的服务。 

![内容保护][content-protection]

若要能够使用动态加密，必须先要以流式传输加密的内容的流终结点获取至少一个流保留的单位。

##概念

###资产加密选项

根据您要上载、 存储和提供的内容的类型，介质服务提供了可供选择的各种加密选项。

**无**不使用加密。 这是默认值。 注意，使用此选项时您的内容不受在传输过程中或在存储中存放。

如果您打算提供 MP4 使用渐进式下载，使用此选项可将上载您的内容。

**StorageEncrypted** -此选项用于加密明文内容使用 AES 256 位加密本地，然后将其上载到 Azure 存储存储位置存放时加密。 未自动加密和放在编码之前加密的文件系统使用存储加密保护的资产，而且 （可选） 重新加密之前上载回作为一个新的输出资产。 您想要高质量输入的媒体文件安全使用强加密存放在磁盘上时，针对存储加密的主要的用例。 

为了提供存储加密的资产，您必须配置资产的交付策略，这样媒体服务就会知道要如何将内容传递。 您的资产可以进行流式处理之前，流式服务器移除存储加密和流式处理您使用指定的传递策略 （例如，AES、 PlayReady 或未加密） 的内容。 

**CommonEncryptionProtected** -使用此选项如果您希望加密 （或上载已加密） 与通用加密或 PlayReady （例如，平滑流式处理用 PlayReady DRM 保护） 的 DRM 内容。

**EnvelopeEncryptionProtected** – 如果您想要保护 （或上载已保护） 此选项的使用 HTTP 实时流 (HLS) 使用高级加密标准 (AES) 加密。 请注意，是否您要上载 HLS 已经使用 AES 加密，它必须已经加密转换管理器。



###动态加密

Microsoft Azure 媒体服务使您能够提供您使用高级加密标准 (AES) （使用 128 位加密密钥） 和 PlayReady DRM 加密动态的内容。 

目前，您可以加密下面流格式︰ HLS、 MPEG 划线和平滑流式处理。 无法加密流格式，HDS 或渐进式下载。

如果要对资产进行加密的介质服务，您需要您的资产相关联的加密密钥 （CommonEncryption 或 EnvelopeEncryption） 还配置密钥的授权策略。

您还需要配置资产的交付策略。 如果您要传输的存储加密的资产，请确保指定您希望如何提供它通过配置资产交付策略。  

当玩家通过请求流时，介质服务使用指定的键来动态加密使用 AES 或 PlayReady 加密内容。 要解密流，播放机将请求密钥传递服务密钥。 若要确定用户有权获取密钥，服务评估您为密钥指定的授权策略。

>[AZURE.NOTE]利用动态加密，必须首先从其计划提供加密的内容的流终结点获取至少一个点播流计价单位。 有关详细信息，请参阅[如何规模的媒体服务](media-services-manage-origins.md#scale_streaming_endpoints)。

###PlayReady DRM 许可证和 AES 清除密钥传递服务

媒体服务提供一种服务提供 PlayReady 许可证和 AES 清除授权客户端的密钥。 可以使用 Azure 管理门户，REST API 或.net 的媒体服务 SDK 配置为您的许可证和密钥的授权和身份验证策略。

注意是否您使用的门户，您可以配置一个 AES 政策 （它将应用于 AES 加密的所有内容） 和一个 PlayReady 策略 （它将应用于所有 PlayReady 加密的内容）。 如果您希望更好地控制配置.NET 使用介质服务 SDK。

###PlayReady 许可证模板

媒体服务提供了用于提供 PlayReady 许可证服务。 当最终用户播放机 (例如，Silverlight) 尝试播放受保护的 PlayReady 内容时，请求发送到许可证交付服务以获取许可证。 如果许可证服务批准了请求，它会发出发送到客户端，并可用于解密并播放指定的内容的许可证。

许可证包含的权利和所需 PlayReady DRM 运行时可以在用户试图播放受保护内容时强制执行的限制。 媒体服务提供了 Api，使您可以配置您的 PlayReady 许可证。 有关详细信息，请参阅[媒体服务 PlayReady 许可证模板概述](https://msdn.microsoft.com/library/azure/dn783459.aspx)

###限制标记

内容密钥授权策略可以有一个或多个授权限制︰ 打开，标记限制或 IP 限制。 受限制的令牌策略必须伴随颁发的安全令牌服务 (STS) 的标记。 媒体服务支持的简单 Web 标记 (SWT) 格式和 JSON Web 标记 (JWT) 格式标记。 媒体服务不提供安全令牌服务。 您可以创建自定义的 STS 或利用 Microsoft Azure ACS 对问题标记。 必须配置 STS 创建签名与指定标记限制配置中指定的密钥和问题索赔的标记。 媒体服务密钥传递服务将返回到客户端令牌是否有效并在标记匹配的那些配置键 （或许可） 索赔请求的键 （或许可）。
当配置标记限制策略时，必须指定主要验证密钥、 颁发者和观众的参数。 主要验证密钥包含与签名令牌的密钥，颁发者已颁发令牌的安全令牌服务。 （有时称为作用域） 的观众介绍了该标记的目的或资源标记授予的访问权限。 媒体服务密钥传递服务验证令牌中的这些值与该模板中的值相匹配。

##常见方案

###保护存储中的内容，提供动态加密的流媒体，使用 AMS key\license 提供服务  

1. 到资产接收高质量的夹层文件。 应用于该资产存储加密选项。
2. 配置流式处理的终结点。
1. 对到 MP4 设置的自适应比特率进行编码。 将存储加密选项应用到输出的资产。
1. 创建加密内容资产要在播放过程中动态加密密钥。
2. 配置内容密钥的授权策略。
1. 配置资产交付策略 （使用动态打包和动态加密）。
1. 通过创建按需定位程序发布资产。
1. 流已发布的内容。

###对自己加密和流式服务使用介质服务密钥和许可证交付服务

有关详细信息，请参阅[如何集成流加密器/服务器的 Azure PlayReady 许可证服务](http://mingfeiy.com/integrate-azure-playready-license-service-encryptorstreaming-server)。

###与合作伙伴集成

[使用 castLabs 来将 DRM 许可证传送到 Azure 媒体服务](media-services-castlabs-integration.md)


##内容加密的常见任务

>[AZURE.NOTE]如果您的内容存储加密，请务必配置资产交付策略。

###配置流式处理的终结点

有关流终结点以及如何管理它们的信息的概述，请参阅[如何在媒体的服务帐户管理流式传输端点](media-services-manage-origins.md)。

###创建内容密钥

创建内容密钥所需加密使用**.NET**或**REST API，**您的资产。

[AZURE.INCLUDE [media-services-selector-create-contentkey](../../includes/media-services-selector-create-contentkey.md)]

###配置内容主要授权策略 

配置内容保护和使用**.NET**或**REST API**密钥的授权策略。

[AZURE.INCLUDE [media-services-selector-content-key-auth-policy](../../includes/media-services-selector-content-key-auth-policy.md)]

###配置资产交付策略

配置资产交付策略使用**.NET**或**REST API**。

[AZURE.INCLUDE [media-services-selector-asset-delivery-policy](../../includes/media-services-selector-asset-delivery-policy.md)]


##相关的链接

[作为服务并使用 Azure 媒体服务的 AES 动态加密宣布 PlayReady](http://mingfeiy.com/playready)

[Azure 媒体服务 PlayReady 许可证交付定价解释](http://mingfeiy.com/playready-pricing-explained-in-azure-media-services)

[如何调试 aes 加密流的 Azure 媒体服务](http://mingfeiy.com/debug-aes-encrypted-stream-azure-media-services)

[JWT 标记 authenitcation](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)

[将集成 Azure 媒体服务 OWIN MVC 基于 Azure 活动目录的应用程序和限制基于 JWT 索赔的内容密钥传递](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/)。

[使用 Azure ACS 对问题标记](http://mingfeiy.com/acs-with-key-services)。

[内容保护]: ./media/media-services-content-protection-overview/media-services-content-protection.png
 
测试

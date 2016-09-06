---
ms.openlocfilehash: 9d106be7926757277975e3a91feec918189fdf49
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="配置使用.NET 的资产交付策略" 
    description="本主题演示如何配置不同的资产交付策略。" 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/11/2015"  
    ms.author="juliako"/>

#如何︰ 配置资产交付策略
[AZURE.INCLUDE [media-services-selector-asset-delivery-policy](../../includes/media-services-selector-asset-delivery-policy.md)]

如果您计划对加密传递资产，媒体服务内容传送工作流中的步骤之一配置资产的交付策略。 资产交付策略会告诉媒体服务您希望如何为您的资产要交付︰ 哪个流协议应您的资产动态打包 （如 MPEG 划线、 HLS、 平滑流式处理或全部），无论是否想要动态加密您的资产以及如何为 （信封或公用加密）。 

本主题讨论为什么以及如何创建和配置资产的交付策略。 

>[AZURE.NOTE]若要能够使用动态打包和动态加密，您必须确保具有至少一个刻度单位 （也称为流单位）。 有关详细信息，请参阅[如何调整介质服务](media-services-manage-origins.md#scale_streaming_endpoints)。 
>
>此外，您的资产必须包含一组自适应比特率 MP4s 或自适应的比特率平滑流式处理文件。      

您可以将不同策略应用到相同的资产。 例如，可以对 MPEG 短划线和 HLS 平滑流式处理和 AES 信封加密应用 PlayReady 加密。 在传递策略中没有定义任何协议 （例如，添加一个策略，它只能作为协议指定 HLS） 将阻止流。 此规则的例外是如果您必须定义根本没有资产交付策略。 然后，将以明文允许的所有协议。

请注意，是否提供一个存储您要加密的资产，则必须配置资产的交付策略。 您的资产可以进行流式处理之前，流式服务器移除存储加密和流式处理您使用指定的传递策略的内容。 例如，为了提供您使用高级加密标准 (AES) 信封加密密钥加密的资产，将策略类型设置为**DynamicEnvelopeEncryption**。 要删除存储加密并流以明文资产，设置为**NoDynamicEncryption**的策略类型。 按照示例，说明如何配置这些策略类型。 

根据您如何配置资产交付策略您将能够动态打包、 动态加密和传输以下流协议︰ 平滑流式处理、 HLS、 MPEG 划线和 HDS 流。  

下面的列表显示了您使用流平滑、 HLS、 短划线和 HDS 的格式。  

平滑流式传输︰

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

HLS:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

MPEG 短划线

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf) 

HDS

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=f4m-f4f)

如何发布一项资产并生成流的 URL 上的说明，请参阅[生成流的 URL](media-services-deliver-streaming-content.md)。

##清晰的资产交付策略 

下面的**ConfigureClearAssetDeliveryPolicy**方法指定，可以提供在任何下列协议流不应用动态加密︰ MPEG 划线、 HLS，和平滑流式处理协议。 您可能想要将此策略应用于加密的存储资产。
  
创建 AssetDeliveryPolicy 时可以指定什么值的信息，请参阅[定义 AssetDeliveryPolicy 时使用的类型](#types)部分。 

    static public void ConfigureClearAssetDeliveryPolicy(IAsset asset)
    {
        IAssetDeliveryPolicy policy =
            _context.AssetDeliveryPolicies.Create("Clear Policy",
            AssetDeliveryPolicyType.NoDynamicEncryption, 
            AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.Dash, null);

        asset.DeliveryPolicies.Add(policy);
    }

##DynamicCommonEncryption 资产交付策略 


下面的**CreateAssetDeliveryPolicy**方法创建平滑流协议 （从流将阻止其他协议） 配置为应用动态通用加密 (**DynamicCommonEncryption**) **AssetDeliveryPolicy** 。 该方法采用两个参数︰**资产**（资产所需应用的交付策略） 和**IContentKey** ( **CommonEncryption**类型的内容密钥的详细信息，请参阅︰[创建内容密钥](media-services-dotnet-create-contentkey.md#common_contentkey))。

创建 AssetDeliveryPolicy 时可以指定什么值的信息，请参阅[定义 AssetDeliveryPolicy 时使用的类型](#types)部分。 


    static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
    {
        Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);

        Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
            new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
        {
            {AssetDeliveryPolicyConfigurationKey.PlayReadyLicenseAcquisitionUrl, acquisitionUrl.ToString()},
        };

        var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
            AssetDeliveryPolicyType.DynamicCommonEncryption,
            AssetDeliveryProtocol.SmoothStreaming,
            assetDeliveryPolicyConfiguration);

        // Add AssetDelivery Policy to the asset
        asset.DeliveryPolicies.Add(assetDeliveryPolicy);

        Console.WriteLine();
        Console.WriteLine("Adding Asset Delivery Policy: " +
            assetDeliveryPolicy.AssetDeliveryPolicyType);
    }



##DynamicEnvelopeEncryption 资产交付策略 

下面的**CreateAssetDeliveryPolicy**方法创建被配置为对 HLS 应用动态信封加密 (**DynamicEnvelopeEncryption**) 的**AssetDeliveryPolicy**和短划线 （从流将阻止其他协议） 的协议。 该方法采用两个参数︰**资产**（资产所需应用的交付策略） 和**IContentKey** ( **EnvelopeEncryption**类型的内容密钥的详细信息，请参阅︰[创建内容密钥](media-services-dotnet-create-contentkey.md#envelope_contentkey))。


创建 AssetDeliveryPolicy 时可以指定什么值的信息，请参阅[定义 AssetDeliveryPolicy 时使用的类型](#types)部分。   

    private static void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
    {
        
        //  Get the Key Delivery Base Url by removing the Query parameter.  The Dynamic Encryption service will
        //  automatically add the correct key identifier to the url when it generates the Envelope encrypted content
        //  manifest.  Omitting the IV will also cause the Dynamice Encryption service to generate a deterministic
        //  IV for the content automatically.  By using the EnvelopeBaseKeyAcquisitionUrl and omitting the IV, this
        //  allows the AssetDelivery policy to be reused by more than one asset.
        //
        Uri keyAcquisitionUri = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.BaselineHttp);
        UriBuilder uriBuilder = new UriBuilder(keyAcquisitionUri);
        uriBuilder.Query = String.Empty;
        keyAcquisitionUri = uriBuilder.Uri;

        // The following policy configuration specifies: 
        //   key url that will have KID=<Guid> appended to the envelope and
        //   the Initialization Vector (IV) to use for the envelope encryption.
        Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
            new Dictionary<AssetDeliveryPolicyConfigurationKey, string> 
        {
            {AssetDeliveryPolicyConfigurationKey.EnvelopeBaseKeyAcquisitionUrl, keyAcquisitionUri.ToString()},
        };

        IAssetDeliveryPolicy assetDeliveryPolicy =
            _context.AssetDeliveryPolicies.Create(
                        "AssetDeliveryPolicy",
                        AssetDeliveryPolicyType.DynamicEnvelopeEncryption,
                        AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.Dash,
                        assetDeliveryPolicyConfiguration);

        // Add AssetDelivery Policy to the asset
        asset.DeliveryPolicies.Add(assetDeliveryPolicy);

        Console.WriteLine();
        Console.WriteLine("Adding Asset Delivery Policy: " + assetDeliveryPolicy.AssetDeliveryPolicyType);
    }


##<a id="types"></a>类型定义 AssetDeliveryPolicy 时使用

###<a id="AssetDeliveryProtocol"></a>AssetDeliveryProtocol 

    /// <summary>
    /// Delivery protocol for an asset delivery policy.
    /// </summary>
    [Flags]
    public enum AssetDeliveryProtocol
    {
        /// <summary>
        /// No protocols.
        /// </summary>
        None = 0x0,

        /// <summary>
        /// Smooth streaming protocol.
        /// </summary>
        SmoothStreaming = 0x1,

        /// <summary>
        /// MPEG Dynamic Adaptive Streaming over HTTP (DASH)
        /// </summary>
        Dash = 0x2,

        /// <summary>
        /// Apple HTTP Live Streaming protocol.
        /// </summary>
        HLS = 0x4,

        /// <summary>
        /// Adobe HTTP Dynamic Streaming (HDS)
        /// </summary>
        Hds = 0x8,

        /// <summary>
        /// Include all protocols.
        /// </summary>
        All = 0xFFFF
    }

###<a id="AssetDeliveryPolicyType"></a>AssetDeliveryPolicyType

    /// <summary>
    /// Policy type for dynamic encryption of assets.
    /// </summary>
    public enum AssetDeliveryPolicyType
    {
        /// <summary>
        /// Delivery Policy Type not set.  An invalid value.
        /// </summary>
        None,

        /// <summary>
        /// The Asset should not be delivered via this AssetDeliveryProtocol. 
        /// </summary>
        Blocked, 

        /// <summary>
        /// Do not apply dynamic encryption to the asset.
        /// </summary>
        /// 
        NoDynamicEncryption,  

        /// <summary>
        /// Apply Dynamic Envelope encryption.
        /// </summary>
        DynamicEnvelopeEncryption,

        /// <summary>
        /// Apply Dynamic Common encryption.
        /// </summary>
        DynamicCommonEncryption
    }

###<a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType

    /// <summary>
    /// Delivery method of the content key to the client.
    /// </summary>
    public enum ContentKeyDeliveryType
    {
        /// <summary>
        /// None.
        /// </summary>
        None,

        /// <summary>
        /// Use PlayReady License acquistion protocol
        /// </summary>
        PlayReadyLicense,

        /// <summary>
        /// Use MPEG Baseline HTTP key protocol.
        /// </summary>
        BaselineHttp
    }

###<a id="AssetDeliveryPolicyConfigurationKey"></a>AssetDeliveryPolicyConfigurationKey

    /// <summary>
    /// Keys used to get specific configuration for an asset delivery policy.
    /// </summary>
    public enum AssetDeliveryPolicyConfigurationKey
    {
        /// <summary>
        /// No policies.
        /// </summary>
        None,

        /// <summary>
        /// Exact Envelope key URL.
        /// </summary>
        EnvelopeKeyAcquisitionUrl,

        /// <summary>
        /// Base key url that will have KID=<Guid> appended for Envelope.
        /// </summary>
        EnvelopeBaseKeyAcquisitionUrl,
        
        /// <summary>
        /// The initialization vector to use for envelope encryption in Base64 format.
        /// </summary>
        EnvelopeEncryptionIVAsBase64,

        /// <summary>
        /// The PlayReady License Acquisition Url to use for common encryption.
        /// </summary>
        PlayReadyLicenseAcquisitionUrl,

        /// <summary>
        /// The PlayReady Custom Attributes to add to the PlayReady Content Header
        /// </summary>
        PlayReadyCustomAttributes,

        /// <summary>
        /// The initialization vector to use for envelope encryption.
        /// </summary>
        EnvelopeEncryptionIV,
    } 
测试

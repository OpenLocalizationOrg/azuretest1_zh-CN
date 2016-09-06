---
ms.openlocfilehash: 8e009c5ccce69e43a4df2a79663514cec49c01ff
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="使用 PlayReady DRM 动态加密和许可证交付服务"
    description="Microsoft Azure 媒体服务使您能够使用 Microsoft PlayReady 受 DRM 保护的 MPEG-短划线、 平滑流式处理和 Http 实时流 (HLS) 流。 本主题演示如何动态与 PlayReady DRM 加密，并使用密钥传递服务。"
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
    ms.topic="get-started-article" 
    ms.date="08/14/2015"
    ms.author="juliako"/>

#使用 PlayReady DRM 动态加密和许可证交付服务

> [AZURE.SELECTOR]
- [.NET](media-services-protect-with-drm.md)
- [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)

Microsoft Azure 媒体服务使您能够使用[Microsoft PlayReady 受 DRM](https://www.microsoft.com/playready/overview/)保护的 MPEG-短划线、 平滑流式处理和 Http 实时流 (HLS) 流。

媒体服务现在提供了用于提供 Microsoft PlayReady 许可证服务。 媒体服务也提供了 Api，使您可以配置的权利和所需的 PlayReady DRM 运行时在用户尝试播放受保护的内容时强制执行的限制。 当用户请求查看 PlayReady 受保护的内容时，客户机应用程序将请求从 Azure 媒体服务内容。 Azure 媒体服务然后将客户端进行身份验证和授权的用户访问内容的 Azure 媒体服务 PlayReady 授权服务器重定向。 PlayReady 许可证包含客户端播放机可以用来解密并流式传输内容的解密密钥。

媒体服务支持多种方法进行关键请求的用户进行身份验证。 内容密钥授权策略可以有一个或多个授权限制︰ 打开，标记限制或 IP 限制。 受限制的令牌策略必须伴随颁发的安全令牌服务 (STS) 的标记。 媒体服务支持的[简单 Web 标记](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)(SWT) 格式和[JSON Web 标记](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3)(JWT) 的格式标记。 有关详细信息，请参阅配置内容密钥的授权策略。

利用动态加密，您需要包含多比特率 MP4 文件或多比特率平滑流式处理源代码文件的一组资产。 您还需要配置资产 （在本主题后面部分所述） 的交付策略。 然后，基于流的 URL 中指定的格式，按需流式处理服务器将确保流传递中已选择的协议。 因此，您只需将存储并支付中单个存储格式的文件和媒体服务将生成并提供适当的响应，基于客户机的请求。

本主题将向应用程序提供受保护的媒体工作的开发人员非常有用。 本主题演示如何将 PlayReady 许可证交付服务配置授权策略，以便只有授权的客户端可能会收到 PlayReady 许可证。 它还演示了如何使用动态加密。

>[AZURE.NOTE]要开始使用动态加密，必须先获取至少一个刻度单位 （也称为流单元）。 有关详细信息，请参阅[如何调整介质服务](media-services-manage-origins.md#scale_streaming_endpoints)。

##PlayReady 动态加密和 PlayReady 许可证交付服务的工作流

以下是需要执行保护您的资产与 PlayReady，使用介质服务许可证交付服务，以及使用动态加密时的一般步骤。

1. 创建资产和将文件上传到该资产。 
1. 对该资产包含对自适应的比特率设置 MP4 文件进行编码。
1. 创建内容密钥并将其与编码的资产。 在媒体服务内容密钥包含资产的加密密钥。 有关详细信息，请参阅 ContentKey。
1. 配置内容密钥的授权策略。 必须由您配置并满足内容密钥以便客户端传递到客户端的内容密钥的授权策略。 
1. 配置资产的交付策略。 配置传递策略包括︰ 传递协议 （例如，MPEG 划线、 HLS、 HDS、 平滑流式处理或全部）、 动态加密 （例如，公用加密），PlayReady 许可证获取 URL 类型。 
 
    可以为每种协议在同一资产上应用不同的策略。 例如，可以应用到平滑划线和 AES 信封与 HLS PlayReady 加密。 在传递策略中没有定义任何协议 （例如，添加一个策略，它只能作为协议指定 HLS） 将阻止流。 此规则的例外是如果您必须定义根本没有资产交付策略。 然后，将以明文允许的所有协议。
1. 创建按需定位，以获取流的 URL。

您会发现一个完整的.NET 示例的主题的结尾处。

下面的图像演示上面所述的工作流。 此标记用于身份验证。

![PlayReady 与保护](./media/media-services-content-protection-overview/media-services-content-protection-with-playready.png)

本主题的其余部分提供了详细的解释和代码示例，向您展示如何完成上述任务的主题的链接。 

##当前的限制

如果您添加或更新资产的交付策略，必须删除现有的定位器 （如果有的话） 并创建新的定位程序。

##创建资产和将文件上传到资产

为了管理、 编码，并且流视频，必须先将您的内容上载到 Microsoft Azure 媒体服务。 一旦上传，您的内容是安全地存储在进一步处理和流云。 

下面的代码段演示如何创建资产，并将指定的文件上传到该资产。

    static public IAsset UploadFileAndCreateAsset(string singleFilePath)
    {
        if(!File.Exists(singleFilePath))
        {
            Console.WriteLine("File does not exist.");
            return null;
        }
    
        var assetName = Path.GetFileNameWithoutExtension(singleFilePath);
        IAsset inputAsset = _context.Assets.Create(assetName, AssetCreationOptions.StorageEncrypted);
    
        var assetFile = inputAsset.AssetFiles.Create(Path.GetFileName(singleFilePath));
    
        Console.WriteLine("Created assetFile {0}", assetFile.Name);
    
        var policy = _context.AccessPolicies.Create(
                                assetName,
                                TimeSpan.FromDays(30),
                                AccessPermissions.Write | AccessPermissions.List);
    
        var locator = _context.Locators.CreateLocator(LocatorType.Sas, inputAsset, policy);
    
        Console.WriteLine("Upload {0}", assetFile.Name);
    
        assetFile.Upload(singleFilePath);
        Console.WriteLine("Done uploading {0}", assetFile.Name);
    
        locator.Delete();
        policy.Delete();
    
        return inputAsset;
    }

##对该资产包含对自适应的比特率设置 MP4 文件进行编码

采用动态加密只需是创建包含多比特率 MP4 文件或多比特率平滑流式处理源代码文件的一组资产。 然后，根据清单或片段请求，点播流中指定格式，服务器将确保您在您选择的协议接收流。 因此，您只需将存储并支付中单个存储格式的文件和媒体服务将生成并提供适当的响应，基于客户机的请求。 有关详细信息，请参阅[动态打包概述](media-services-dynamic-packaging-overview.md)主题。

下面的代码段显示了如何进行编码与自适应比特率 MP4 设置资产︰

    static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset inputAsset)
    {
        var encodingPreset = "H264 Adaptive Bitrate MP4 Set 720p";
    
        IJob job = _context.Jobs.Create(String.Format("Encoding into Mp4 {0} to {1}",
                                inputAsset.Name,
                                encodingPreset));
    
        var mediaProcessors = 
            _context.MediaProcessors.Where(p => p.Name.Contains("Media Encoder")).ToList();
    
        var latestMediaProcessor = 
            mediaProcessors.OrderBy(mp => new Version(mp.Version)).LastOrDefault();
    
    
    
        ITask encodeTask = job.Tasks.AddNew("Encoding", latestMediaProcessor, encodingPreset, TaskOptions.None);
        encodeTask.InputAssets.Add(inputAsset);
        encodeTask.OutputAssets.AddNew(String.Format("{0} as {1}", inputAsset.Name, encodingPreset), AssetCreationOptions.StorageEncrypted);
    
        job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
        job.Submit();
        job.GetExecutionProgressTask(CancellationToken.None).Wait();
    
        return job.OutputMediaAssets[0];
    }
    
    static private void JobStateChanged(object sender, JobStateChangedEventArgs e)
    {
        Console.WriteLine(string.Format("{0}\n  State: {1}\n  Time: {2}\n\n",
            ((IJob)sender).Name,
            e.CurrentState,
            DateTime.UtcNow.ToString(@"yyyy_M_d__hh_mm_ss")));
    }

##<a id="create_contentkey"></a>创建内容密钥并将其与编码的资产

在媒体服务内容密钥包含要加密与资产的键。

有关详细信息，请参阅[创建内容密钥](media-services-dotnet-create-contentkey.md)。


##<a id="configure_key_auth_policy"></a>配置内容密钥的授权策略

媒体服务支持多种方法进行关键请求的用户进行身份验证。 内容密钥授权策略必须由您配置并满足由客户端 （玩家） 的密钥传送给客户端。 内容密钥授权策略可以有一个或多个授权限制︰ 打开，标记限制或 IP 限制。

有关详细信息，请参阅[配置内容密钥授权策略](media-services-dotnet-configure-content-key-auth-policy.md#playready-dynamic-encryption)。

##<a id="configure_asset_delivery_policy"></a>配置资产交付策略 

配置您的资产的交付策略。 资产交付策略配置中包括的一些事项︰

- PlayReady 许可证获取 URL。 
- 资产交付协议 （例如，MPEG 划线、 HLS、 HDS、 平滑流式处理或全部）。 
- （在这种情况下，通用加密） 的动态加密类型。 

有关详细信息，请参阅[配置资产交付策略](media-services-rest-configure-asset-delivery-policy.md)。

##<a id="create_locator"></a>创建流以获取流 URL 定位位置

您需要为您的用户提供流 URL 平滑、 破折号或 HLS。

>[AZURE.NOTE]如果您添加或更新资产的交付策略，必须删除现有的定位器 （如果有的话） 并创建新的定位程序。

如何发布一项资产并生成流的 URL 上的说明，请参阅[生成流的 URL](media-services-deliver-streaming-content.md)。

##获取测试标记

获取测试标记基于令牌用于密钥的授权策略的限制。

    // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
    // back into a TokenRestrictionTemplate class instance.
    TokenRestrictionTemplate tokenTemplate = 
        TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);
    
    // Generate a test token based on the data in the given TokenRestrictionTemplate.
    //The GenerateTestToken method returns the token without the word “Bearer” in front
    //so you have to add it in front of the token string. 
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate);
    Console.WriteLine("The authorization token is:\nBearer {0}", testToken);

    
[AMS 播放机](http://amsplayer.azurewebsites.net/azuremediaplayer.html)可用于测试流。

##<a id="example"></a>示例

1. 创建新的控制台项目。
1. NuGet 可用于安装和添加 Azure 媒体服务.NET SDK 扩展。 安装此软件包，也将安装介质服务.NET SDK 并添加其他所需的所有依赖项。
2. 添加配置文件，其中包含的帐户名称和密钥信息︰

    
        <?xml version="1.0" encoding="utf-8"?>
        <configuration>
            <startup> 
                <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
            </startup>
              <appSettings>
            
                <add key="MediaServicesAccountName" value="AccountName"/>
                <add key="MediaServicesAccountKey" value="AccountKey"/>
            
                <add key="Issuer" value="http://testacs.com"/>
                <add key="Audience" value="urn:test"/>
              </appSettings>
        </configuration>

1. 在本部分中所示的代码覆盖 Program.cs 文件中的代码。
    
    请确保更新变量指向输入的文件所在的文件夹。

        using System;
        using System.Collections.Generic;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using System.Text;
        using System.Threading;
        using System.Threading.Tasks;
        using System.Xml.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
        using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
        
        namespace PlayReadyDynamicEncryptAndKeyDeliverySvc
        {
            class Program
            {
                // Read values from the App.config file.
                private static readonly string _mediaServicesAccountName =
                    ConfigurationManager.AppSettings["MediaServicesAccountName"];
                private static readonly string _mediaServicesAccountKey =
                    ConfigurationManager.AppSettings["MediaServicesAccountKey"];
        
                private static readonly Uri _sampleIssuer =
                    new Uri(ConfigurationManager.AppSettings["Issuer"]);
                private static readonly Uri _sampleAudience =
                    new Uri(ConfigurationManager.AppSettings["Audience"]);
        
                // Field for service context.
                private static CloudMediaContext _context = null;
                private static MediaServicesCredentials _cachedCredentials = null;
        
                private static readonly string _mediaFiles =
                    Path.GetFullPath(@"../..\Media");
        
                private static readonly string _singleMP4File =
                    Path.Combine(_mediaFiles, @"BigBuckBunny.mp4");
        
                static void Main(string[] args)
                {
                    // Create and cache the Media Services credentials in a static class variable.
                    _cachedCredentials = new MediaServicesCredentials(
                                    _mediaServicesAccountName,
                                    _mediaServicesAccountKey);
                    // Used the cached credentials to create CloudMediaContext.
                    _context = new CloudMediaContext(_cachedCredentials);
        
                    bool tokenRestriction = false;
                    string tokenTemplateString = null;
        
                    IAsset asset = UploadFileAndCreateAsset(_singleMP4File);
                    Console.WriteLine("Uploaded asset: {0}", asset.Id);
        
                    IAsset encodedAsset = EncodeToAdaptiveBitrateMP4Set(asset);
                    Console.WriteLine("Encoded asset: {0}", encodedAsset.Id);
        
                    IContentKey key = CreateCommonTypeContentKey(encodedAsset);
                    Console.WriteLine("Created key {0} for the asset {1} ", key.Id, encodedAsset.Id);
                    Console.WriteLine("PlayReady License Key delivery URL: {0}", key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense));
                    Console.WriteLine();
            
                    if (tokenRestriction)
                        tokenTemplateString = AddTokenRestrictedAuthorizationPolicy(key);
                    else
                        AddOpenAuthorizationPolicy(key);
        
                    Console.WriteLine("Added authorization policy: {0}", key.AuthorizationPolicyId);
                    Console.WriteLine();
        
                    CreateAssetDeliveryPolicy(encodedAsset, key);
                    Console.WriteLine("Created asset delivery policy. \n");
                    Console.WriteLine();
        
                    if (tokenRestriction && !String.IsNullOrEmpty(tokenTemplateString))
                    {
                        // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
                        // back into a TokenRestrictionTemplate class instance.
                        TokenRestrictionTemplate tokenTemplate =
                            TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);
        
                        // Generate a test token based on the the data in the given TokenRestrictionTemplate.
                        // Note, you need to pass the key id Guid because we specified 
                        // TokenClaim.ContentKeyIdentifierClaim in during the creation of TokenRestrictionTemplate.
                        Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);
        
                        //The GenerateTestToken method returns the token without the word “Bearer” in front
                        //so you have to add it in front of the token string. 
                        string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey, DateTime.UtcNow.AddDays(365));
                        Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
                        Console.WriteLine();
                    }
        
                    // You can use the http://smf.cloudapp.net/healthmonitor player 
                    // to test the smoothStreamURL URL.
                    //
                    string url = GetStreamingOriginLocator(encodedAsset);
                    Console.WriteLine("Encrypted Smooth Streaming URL: {0}/manifest", url);
        
        
                    Console.ReadLine();
                }
        
                static public IAsset UploadFileAndCreateAsset(string singleFilePath)
                {
                    if (!File.Exists(singleFilePath))
                    {
                        Console.WriteLine("File does not exist.");
                        return null;
                    }
        
                    var assetName = Path.GetFileNameWithoutExtension(singleFilePath);
                    IAsset inputAsset = _context.Assets.Create(assetName, AssetCreationOptions.StorageEncrypted);
        
                    var assetFile = inputAsset.AssetFiles.Create(Path.GetFileName(singleFilePath));
        
                    Console.WriteLine("Created assetFile {0}", assetFile.Name);
        
                    var policy = _context.AccessPolicies.Create(
                                            assetName,
                                            TimeSpan.FromDays(30),
                                            AccessPermissions.Write | AccessPermissions.List);
        
                    var locator = _context.Locators.CreateLocator(LocatorType.Sas, inputAsset, policy);
        
                    Console.WriteLine("Upload {0}", assetFile.Name);
        
                    assetFile.Upload(singleFilePath);
                    Console.WriteLine("Done uploading {0}", assetFile.Name);
        
                    locator.Delete();
                    policy.Delete();
        
                    return inputAsset;
                }
        
        
                static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset inputAsset)
                {
                    var encodingPreset = "H264 Adaptive Bitrate MP4 Set 720p";
        
                    IJob job = _context.Jobs.Create(String.Format("Encoding into Mp4 {0} to {1}",
                                            inputAsset.Name,
                                            encodingPreset));
        
                    var mediaProcessors =
                        _context.MediaProcessors.Where(p => p.Name.Contains("Media Encoder")).ToList();
        
                    var latestMediaProcessor =
                        mediaProcessors.OrderBy(mp => new Version(mp.Version)).LastOrDefault();
        
        
        
                    ITask encodeTask = job.Tasks.AddNew("Encoding", latestMediaProcessor, encodingPreset, TaskOptions.None);
                    encodeTask.InputAssets.Add(inputAsset);
                    encodeTask.OutputAssets.AddNew(String.Format("{0} as {1}", inputAsset.Name, encodingPreset), AssetCreationOptions.StorageEncrypted);
        
                    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
                    job.Submit();
                    job.GetExecutionProgressTask(CancellationToken.None).Wait();
        
                    return job.OutputMediaAssets[0];
                }
        
        
                static public IContentKey CreateCommonTypeContentKey(IAsset asset)
                {
                    // Create envelope encryption content key
                    Guid keyId = Guid.NewGuid();
                    byte[] contentKey = GetRandomBuffer(16);
        
                    IContentKey key = _context.ContentKeys.Create(
                                            keyId,
                                            contentKey,
                                            "ContentKey",
                                            ContentKeyType.CommonEncryption);
        
                    // Associate the key with the asset.
                    asset.ContentKeys.Add(key);
        
                    return key;
                }
        
            static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
            {
        
                // Create ContentKeyAuthorizationPolicy with Open restrictions 
                // and create authorization policy          
        
                List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
                {
                    new ContentKeyAuthorizationPolicyRestriction 
                    { 
                        Name = "Open", 
                        KeyRestrictionType = (int)ContentKeyRestrictionType.Open, 
                        Requirements = null
                    }
                };
        
                // Configure PlayReady license template.
                string newLicenseTemplate = ConfigurePlayReadyLicenseTemplate();
        
                IContentKeyAuthorizationPolicyOption policyOption =
                    _context.ContentKeyAuthorizationPolicyOptions.Create("",
                        ContentKeyDeliveryType.PlayReadyLicense,
                            restrictions, newLicenseTemplate);
        
                IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                            ContentKeyAuthorizationPolicies.
                            CreateAsync("Deliver Common Content Key with no restrictions").
                            Result;
        
        
                contentKeyAuthorizationPolicy.Options.Add(policyOption);
        
                // Associate the content key authorization policy with the content key.
                contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
                contentKey = contentKey.UpdateAsync().Result;
            }
        
                public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
                {
                    string tokenTemplateString = GenerateTokenRequirements();
        
                    IContentKeyAuthorizationPolicy policy = _context.
                                            ContentKeyAuthorizationPolicies.
                                            CreateAsync("HLS token restricted authorization policy").Result;
        
                    List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
                    {
                        new ContentKeyAuthorizationPolicyRestriction 
                        { 
                            Name = "Token Authorization Policy", 
                            KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                            Requirements = tokenTemplateString, 
                        }
                    };
        
                    // Configure PlayReady license template.
                    string newLicenseTemplate = ConfigurePlayReadyLicenseTemplate();
        
                    IContentKeyAuthorizationPolicyOption policyOption =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                            ContentKeyDeliveryType.PlayReadyLicense,
                                restrictions, newLicenseTemplate);
        
                    IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("Deliver Common Content Key with no restrictions").
                                Result;
                    
                    policy.Options.Add(policyOption);
        
                    // Add ContentKeyAutorizationPolicy to ContentKey
                    contentKeyAuthorizationPolicy.Options.Add(policyOption);
        
                    // Associate the content key authorization policy with the content key
                    contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
                    contentKey = contentKey.UpdateAsync().Result;
        
                    return tokenTemplateString;
                }
        
                static private string GenerateTokenRequirements()
                {
                    TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);
        
                    template.PrimaryVerificationKey = new SymmetricVerificationKey();
                    template.AlternateVerificationKeys.Add(new SymmetricVerificationKey());
                    template.Audience = _sampleAudience;
                    template.Issuer = _sampleIssuer;
                    template.RequiredClaims.Add(TokenClaim.ContentKeyIdentifierClaim);
        
                    return TokenRestrictionTemplateSerializer.Serialize(template);
                } 
        
                static private string ConfigurePlayReadyLicenseTemplate()
                {
                    // The following code configures PlayReady License Template using .NET classes
                    // and returns the XML string.
                     
                    PlayReadyLicenseResponseTemplate responseTemplate = new PlayReadyLicenseResponseTemplate();
                    PlayReadyLicenseTemplate licenseTemplate = new PlayReadyLicenseTemplate();
        
                    responseTemplate.LicenseTemplates.Add(licenseTemplate);
        
                    return MediaServicesLicenseTemplateSerializer.Serialize(responseTemplate);
                }
        
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
        
        
                /// <summary>
                /// Gets the streaming origin locator.
                /// </summary>
                /// <param name="assets"></param>
                /// <returns></returns>
                static public string GetStreamingOriginLocator(IAsset asset)
                {
        
                    // Get a reference to the streaming manifest file from the  
                    // collection of files in the asset. 
        
                    var assetFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                                                 EndsWith(".ism")).
                                                 FirstOrDefault();
        
                    // Create a 30-day readonly access policy. 
                    IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
                        TimeSpan.FromDays(30),
                        AccessPermissions.Read);
        
                    // Create a locator to the streaming content on an origin. 
                    ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
                        policy,
                        DateTime.UtcNow.AddMinutes(-5));
        
                    // Create a URL to the manifest file. 
                    return originLocator.Path + assetFile.Name;
                }
        
                static private void JobStateChanged(object sender, JobStateChangedEventArgs e)
                {
                    Console.WriteLine(string.Format("{0}\n  State: {1}\n  Time: {2}\n\n",
                        ((IJob)sender).Name,
                        e.CurrentState,
                        DateTime.UtcNow.ToString(@"yyyy_M_d__hh_mm_ss")));
                }
        
                static private byte[] GetRandomBuffer(int length)
                {
                    var returnValue = new byte[length];
        
                    using (var rng =
                        new System.Security.Cryptography.RNGCryptoServiceProvider())
                    {
                        rng.GetBytes(returnValue);
                    }
        
                    return returnValue;
                }
            }
        }


测试

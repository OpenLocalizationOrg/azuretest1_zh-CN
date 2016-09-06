---
ms.openlocfilehash: d54d91e2af8449781a3cbb9c4da83d7738495911
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="动态加密︰ 配置使用.NET 的内容密钥授权策略" 
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



#动态加密︰ 配置内容主要授权策略 
[AZURE.INCLUDE [media-services-selector-content-key-auth-policy](../../includes/media-services-selector-content-key-auth-policy.md)] 


##Overivew

Microsoft Azure 媒体服务使您能够提供您动态地使用高级加密标准 (AES) （使用 128 位加密密钥） 和/或 PlayReady DRM 加密的内容。 介质服务还提供了用于提供键和 PlayReady 许可证授权客户端的服务。 

目前，您可以加密下面流格式︰ HLS、 MPEG 划线和平滑流式处理。 无法加密流格式，HDS 或渐进式下载。

如果要对资产进行加密的介质服务，您需要加密密钥 （**CommonEncryption**或**EnvelopeEncryption**） 相关联的资产 （如所描述在[此处](media-services-dotnet-create-contentkey.md)） 还配置密钥的授权策略 （如本文中所述）。 

当玩家通过请求流时，介质服务使用指定的键来动态加密使用 AES 或 PlayReady 加密内容。 要解密流，播放机将请求密钥传递服务密钥。 若要确定用户有权获取密钥，服务评估您为密钥指定的授权策略。

媒体服务支持多种方法进行关键请求的用户进行身份验证。 内容密钥授权策略可以有一个或多个授权限制︰**打开**、**标记**限制或**IP**限制。 受限制的令牌策略必须伴随颁发的安全令牌服务 (STS) 的标记。 媒体服务支持的**简单 Web 标记**([SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)) 格式和**JSON Web 标记**(JWT) 的格式标记。  

媒体服务不提供安全令牌服务。 您可以创建自定义的 STS 或利用 Microsoft Azure ACS 对问题标记。 必须配置 STS 创建使用指定的键和问题索赔 （如本文中所述） 标记限制配置中指定的签名的标记。 如果令牌无效并且令牌中的声明匹配那些配置为内容的密钥，介质服务密钥传递服务将向客户端返回加密密钥。

有关详细信息，请参见 

[JWT 标记 authenitcation](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)

[将集成 Azure 媒体服务 OWIN MVC 基于 Azure 活动目录的应用程序和限制基于 JWT 索赔的内容密钥传递](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/)。

[使用 Azure ACS 对问题标记](http://mingfeiy.com/acs-with-key-services)。

###一些注意事项︰

- 若要能够使用动态打包和动态加密，您必须确保至少有一个流保留的单位。 有关详细信息，请参阅[如何调整介质服务](media-services-manage-origins.md#scale_streaming_endpoints)。 
- 您的资产必须包含一组自适应比特率 MP4s 或自适应的比特率平滑流式处理文件。 有关详细信息，请参阅[编码资产](media-services-encode-asset.md)。  
- 上载和编码您使用**AssetCreationOptions.StorageEncrypted**选项的资产。
- 如果您计划让需要相同的策略配置的多个内容项，它是强烈建议创建单个授权策略和内容的多个键重新使用它。
- 键传递服务缓存 15 分钟 ContentKeyAuthorizationPolicy 和其相关的对象 （策略选项和限制）。  如果您创建 ContentKeyAuthorizationPolicy，并指定要使用"的令牌"的限制，然后测试它，然后更新的策略，"开放"的限制，需要大约 15 分钟前策略切换到"开放"的策略版本。
- 如果您添加或更新资产的交付策略，必须删除现有的定位器 （如果有的话） 并创建新的定位程序。


##AES 128 动态加密 

###打开限制

打开限制意味着系统会将密钥传递给密钥申请的人。 这种限制可能适用于测试目的。

下面的示例创建一个打开的授权策略，并将其添加到内容的密钥。
    
    static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
    {
        // Create ContentKeyAuthorizationPolicy with Open restrictions 
        // and create authorization policy             
        IContentKeyAuthorizationPolicy policy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("Open Authorization Policy").Result;
    
        List<ContentKeyAuthorizationPolicyRestriction> restrictions =
            new List<ContentKeyAuthorizationPolicyRestriction>();
    
        ContentKeyAuthorizationPolicyRestriction restriction =
            new ContentKeyAuthorizationPolicyRestriction
            {
                Name = "HLS Open Authorization Policy",
                KeyRestrictionType = (int)ContentKeyRestrictionType.Open,
                Requirements = null // no requirements needed for HLS
            };
    
        restrictions.Add(restriction);
    
        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create(
            "policy", 
            ContentKeyDeliveryType.BaselineHttp, 
            restrictions, 
            "");
    
        policy.Options.Add(policyOption);
    
        // Add ContentKeyAutorizationPolicy to ContentKey
        contentKey.AuthorizationPolicyId = policy.Id;
        IContentKey updatedKey = contentKey.UpdateAsync().Result;
        Console.WriteLine("Adding Key to Asset: Key ID is " + updatedKey.Id);
    }


###限制标记

本部分介绍如何创建内容的主要授权策略并将其与内容的密钥。 授权策略描述必须满足哪些授权要求来确定用户有权收到的密钥 （例如，"验证密钥"列表中包含与签名令牌的密钥）。

若要配置标记限制选项，您需要使用 XML 来描述标记的授权要求。 XML 的标记限制配置必须符合下面的 XML 架构。

####<a id="schema"></a>限制标记架构
    
    <?xml version="1.0" encoding="utf-8"?>
    <xs:schema xmlns:tns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/TokenRestrictionTemplate/v1" elementFormDefault="qualified" targetNamespace="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/TokenRestrictionTemplate/v1" xmlns:xs="http://www.w3.org/2001/XMLSchema">
      <xs:complexType name="TokenClaim">
        <xs:sequence>
          <xs:element name="ClaimType" nillable="true" type="xs:string" />
          <xs:element minOccurs="0" name="ClaimValue" nillable="true" type="xs:string" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="TokenClaim" nillable="true" type="tns:TokenClaim" />
      <xs:complexType name="TokenRestrictionTemplate">
        <xs:sequence>
          <xs:element minOccurs="0" name="AlternateVerificationKeys" nillable="true" type="tns:ArrayOfTokenVerificationKey" />
          <xs:element name="Audience" nillable="true" type="xs:anyURI" />
          <xs:element name="Issuer" nillable="true" type="xs:anyURI" />
          <xs:element name="PrimaryVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
          <xs:element minOccurs="0" name="RequiredClaims" nillable="true" type="tns:ArrayOfTokenClaim" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="TokenRestrictionTemplate" nillable="true" type="tns:TokenRestrictionTemplate" />
      <xs:complexType name="ArrayOfTokenVerificationKey">
        <xs:sequence>
          <xs:element minOccurs="0" maxOccurs="unbounded" name="TokenVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ArrayOfTokenVerificationKey" nillable="true" type="tns:ArrayOfTokenVerificationKey" />
      <xs:complexType name="TokenVerificationKey">
        <xs:sequence />
      </xs:complexType>
      <xs:element name="TokenVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
      <xs:complexType name="ArrayOfTokenClaim">
        <xs:sequence>
          <xs:element minOccurs="0" maxOccurs="unbounded" name="TokenClaim" nillable="true" type="tns:TokenClaim" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ArrayOfTokenClaim" nillable="true" type="tns:ArrayOfTokenClaim" />
      <xs:complexType name="SymmetricVerificationKey">
        <xs:complexContent mixed="false">
          <xs:extension base="tns:TokenVerificationKey">
            <xs:sequence>
              <xs:element name="KeyValue" nillable="true" type="xs:base64Binary" />
            </xs:sequence>
          </xs:extension>
        </xs:complexContent>
      </xs:complexType>
      <xs:element name="SymmetricVerificationKey" nillable="true" type="tns:SymmetricVerificationKey" />
    </xs:schema>

配置**标记**限制时策略，则必须指定主**验证键**、**颁发者**和**观众**参数。 **主要验证密钥**包含与签名令牌的密钥、**颁发者**已颁发令牌的安全令牌服务。 **观众**（有时称为**作用域**） 介绍了该标记的目的或资源标记授予的访问权限。 媒体服务密钥传递服务验证令牌中的这些值与该模板中的值相匹配。 

使用**用于.NET 的媒体服务 SDK**，可以使用**TokenRestrictionTemplate**类来生成限制标记。
下面的示例创建具有标记限制授权策略。 在此示例中，客户端必须提供包含标记︰ 签名密钥 (VerificationKey)、 令牌颁发者，并要求的索赔。
    
    public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
    {
        string tokenTemplateString = GenerateTokenRequirements();
    
        IContentKeyAuthorizationPolicy policy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("HLS token restricted authorization policy").Result;
    
        List<ContentKeyAuthorizationPolicyRestriction> restrictions =
                new List<ContentKeyAuthorizationPolicyRestriction>();
    
        ContentKeyAuthorizationPolicyRestriction restriction =
                new ContentKeyAuthorizationPolicyRestriction
                {
                    Name = "Token Authorization Policy",
                    KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                    Requirements = tokenTemplateString
                };
    
        restrictions.Add(restriction);
    
        //You could have multiple options 
        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create(
                "Token option for HLS",
                ContentKeyDeliveryType.BaselineHttp,
                restrictions,
                null  // no key delivery data is needed for HLS
                );
    
        policy.Options.Add(policyOption);
    
        // Add ContentKeyAutorizationPolicy to ContentKey
        contentKey.AuthorizationPolicyId = policy.Id;
        IContentKey updatedKey = contentKey.UpdateAsync().Result;
        Console.WriteLine("Adding Key to Asset: Key ID is " + updatedKey.Id);
    
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

####<a id="test"></a>测试标记

要获取测试标记基于令牌用于密钥的授权策略的限制，请执行以下操作。
    
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
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey);
    Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
    Console.WriteLine();


##PlayReady 动态加密 

媒体服务使您能够配置的权利和所需 PlayReady DRM 运行时强制实施用户尝试播放受保护的内容时的限制。 

保护您的 PlayReady 的内容，您需要授权策略中指定的任务之一时， [PlayReady 许可证模板](https://msdn.microsoft.com/library/azure/dn783459.aspx)定义一个 XML 字符串。 在.net 的媒体服务 SDK 的**PlayReadyLicenseResponseTemplate**和**PlayReadyLicenseTemplate**类将帮助您定义 PlayReady 许可证模板。 

###打开限制
    
打开限制意味着系统会将密钥传递给密钥申请的人。 这种限制可能适用于测试目的。

下面的示例创建一个打开的授权策略，并将其添加到内容的密钥。

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

###限制标记

若要配置标记限制选项，您需要使用 XML 来描述标记的授权要求。 XML 的标记限制配置必须符合[本](#schema)部分中所示的 XML 架构。
    
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

要获取测试标记基于令牌的限制，所使用的密钥的授权策略，请参阅[本](#test)部分。 

##<a id="types"></a>类型定义 ContentKeyAuthorizationPolicy 时使用

###<a id="ContentKeyRestrictionType"></a>ContentKeyRestrictionType
    public enum ContentKeyRestrictionType
    {
        Open = 0,
        TokenRestricted = 1,
        IPRestricted = 2,
    }

###<a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType

    public enum ContentKeyDeliveryType
    {
        None = 0,
        PlayReadyLicense = 1,
        BaselineHttp = 2,
    }

###<a id="TokenType"></a>TokenType

    public enum TokenType
    {
        Undefined = 0,
        SWT = 1,
        JWT = 2,
    }



##下一步行动
现在，您已经配置内容密钥授权策略，转到[如何配置资产交付策略](media-services-dotnet-configure-asset-delivery-policy.md)主题。
 
测试

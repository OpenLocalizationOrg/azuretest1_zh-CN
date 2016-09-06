---
ms.openlocfilehash: 194d53b312e34b1330aafcc678870c056f725d9a
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="使用.NET 创建 ContentKeys" 
    description="了解如何创建内容资产提供安全访问权限的注册表项。" 
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


#使用.NET 创建 ContentKeys

> [AZURE.SELECTOR]
- [REST](media-services-rest-create-contentkey.md)
- [.NET](media-services-dotnet-create-contentkey.md)

媒体服务使您能够创建和提供加密的资产。 **ContentKey**提供了安全访问**资产**s。 

当您创建新的资产 （例如之前您[上载的文件](media-services-dotnet-upload-files.md)）, 时，您可以指定下面的加密选项︰ **StorageEncrypted**、 **CommonEncryptionProtected**或**EnvelopeEncryptionProtected**。 

当您为客户提供资产时，可以用以下两种加密[配置资产动态加密](media-services-dotnet-configure-asset-delivery-policy.md)︰ **DynamicEnvelopeEncryption**或**DynamicCommonEncryption**。

加密的资产必须与**ContentKey**s。 本文介绍如何创建一个内容项。

>[AZURE.NOTE] 在创建一个新的**StorageEncrypted**资产使用介质服务.NET SDK 时， **ContentKey**会自动创建并链接与资产。

##ContentKeyType

一个值，您必须设置时创建内容关键在于内容的密钥类型。 选择以下值之一。 

    /// <summary>
    /// Specifies the type of a content key.
    /// </summary>
    public enum ContentKeyType
    {
        /// <summary>
        /// Specifies a content key for common encryption.
        /// </summary>
        /// <remarks>This is the default value.</remarks>
        CommonEncryption = 0,

        /// <summary>
        /// Specifies a content key for storage encryption.
        /// </summary>
        StorageEncryption = 1,

        /// <summary>
        /// Specifies a content key for encrypting encoding configuration data that may contain sensitive preset information. 
        /// </summary>
        ConfigurationEncryption = 2,
    }

##<a id="envelope_contentkey"></a>创建信封类型 ContentKey

下面的代码段创建封套加密类型的内容项。 然后将与指定资产关联的键。

    static public IContentKey CreateEnvelopeTypeContentKey(IAsset asset)
    {
        // Create envelope encryption content key
        Guid keyId = Guid.NewGuid();
        byte[] contentKey = GetRandomBuffer(16);

        IContentKey key = _context.ContentKeys.Create(
                                keyId,
                                contentKey,
                                "ContentKey",
                                ContentKeyType.EnvelopeEncryption);

        asset.ContentKeys.Add(key);

        return key;
    }

    static private byte[] GetRandomBuffer(int size)
    {
        byte[] randomBytes = new byte[size];
        using (RNGCryptoServiceProvider rng = new RNGCryptoServiceProvider())
        {
            rng.GetBytes(randomBytes);
        }

        return randomBytes;
    }

调用

    IContentKey key = CreateEnvelopeTypeContentKey(encryptedsset);



##<a id="common_contentkey"></a>创建公共类型 ContentKey    

下面的代码段创建常见的加密类型的内容项。 然后将与指定资产关联的键。

    static public IContentKey CreateCommonTypeContentKey(IAsset asset)
    {
        // Create common encryption content key
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
调用

    IContentKey key = CreateCommonTypeContentKey(encryptedsset); 
测试

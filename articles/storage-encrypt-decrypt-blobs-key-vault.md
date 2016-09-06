---
ms.openlocfilehash: cd22b2be4fb052a70add6921b493b8fc064dab1d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="加密和解密使用 Azure 密钥存储库的 Microsoft Azure 存储 Blob"
   description="本教程将指导您完成如何加密和解密使用客户端加密来使用 Azure 密钥存储库的 Microsoft Azure 存储 blob"
   services="storage"
   documentationCenter=""
   authors="adhurwit"
   manager=""
   editor=""/>

<tags
   ms.service="storage"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="06/17/2015"
   ms.author="adhurwit"/>

# 加密和解密使用 Azure 密钥存储库的 Microsoft Azure 存储中的 blob

## 简介
 
本教程介绍了如何使也使用客户端的存储加密-正在预览的 Azure 密钥存储库的当前在预览中。 它将引导您完成如何加密和解密在控制台应用程序中使用这些技术的 blob。 

**估计完成时间︰** 20 分钟

关于 Azure 密钥存储库的概述信息，请参阅[Azure 密钥存储库是什么？](key-vault/key-vault-whatis.md)

有关客户端加密的 Azure 存储概述信息，请参阅[Microsoft Azure 存储--预览的客户端的加密](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/04/28/client-side-encryption-for-microsoft-azure-storage-preview.aspx)


## 先决条件

若要完成本教程，您必须︰

- Azure 存储帐户
- Visual Studio 2013年或更高版本
- Azure PowerShell 


## 客户端-端加密过程概述

Microsoft Azure 存储的客户端加密的概述，请参阅[http://blogs.msdn.com/b/windowsazurestorage/archive/2015/04/29/getting-started-with-client-side-encryption-for-microsoft-azure-storage.aspx](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/04/29/getting-started-with-client-side-encryption-for-microsoft-azure-storage.aspx "开始使用 Microsoft Azure 存储的客户端的加密")

该博客文章所述，以下是该过程︰

1. Azure 存储客户端 SDK 将生成内容加密密钥 (CEK) 这是一次使用对称密钥。
2. 使用此 CEK 加密用户数据。
3. CEK 然后包装使用 KEK 的密钥加密密钥 （加密）。 KEK 由密钥标识符进行标识和可以是一个非对称密钥对或对称密钥并可以管理本地或 Azure 密钥存储库中存储。 存储客户端本身永远不会有权 KEK。 它只需调用键换行算法所提供的密钥存储库。 用户可以选择密钥包装/取消包装中使用自定义提供程序，如果需要。
4. 加密的数据然后上载到 Azure 存储服务中。


## 设置您 Azure 的密钥存储区
为了继续本教程，您需要执行以下操作此教程中介绍︰[开始使用 Azure 密钥存储库](key-vault/key-vault-get-started.md) 

- 创建密钥存储库
- 将密钥添加到密钥存储库
- Azure Active Directory 中注册应用程序
- 授权应用程序要使用的密钥或密码

请记下客户机 Id 和 ClientSecret Azure Active Directory 中注册应用程序时生成。 

在密钥存储库中创建两个密钥。 我们将假定为本教程的其余部分使用了下面的名称︰ ContosoKeyVault 和 TestRSAKey1。 


## 创建一个控制台应用程序使用软件包和 AppSettings

在 Visual Studio 中，将创建新的控制台应用程序。

在程序包管理器控制台上添加必要的 nuget 程序包︰

    // Note that this is the preview version for Azure Storage
    Install-Package WindowsAzure.Storage -Pre

    // This is the latest stable release for ADAL
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.16.204221202

    // These are currently only available in preview
    Install-Package Microsoft.Azure.KeyVault -Pre
    Install-Package Microsoft.Azure.KeyVault.Extensions -Pre


添加到 App.Config AppSettings。 

    <appSettings>
        <add key="accountName" value="myaccount"/>
        <add key="accountKey" value="theaccountkey"/>
        <add key="clientId" value="theclientid"/>
        <add key="clientSecret" value="theclientsecret"/>
        <add key="container" value="stuff"/>
    </appSettings>

添加以下语句使用，并确保将对 System.Configuration 的引用添加到项目。 

    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System.Configuration;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    using Microsoft.Azure.KeyVault;
    using System.Threading;     
    using System.IO;


## 添加方法以获取标记到您的控制台应用程序

需要验证您的密钥存储库访问密钥存储库类使用下面的方法。 

    private async static Task<string> GetToken(string authority, string resource, string scope)
    {
        var authContext = new AuthenticationContext(authority);
        ClientCredential clientCred = new ClientCredential(
            ConfigurationManager.AppSettings["clientId"], 
            ConfigurationManager.AppSettings["clientSecret"]);
        AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);
    
        if (result == null)
            throw new InvalidOperationException("Failed to obtain the JWT token");
    
        return result.AccessToken;
    }

## 访问存储和程序中的密钥存储库 

在 Main 函数中，添加以下代码︰

    // This is standard code to interact with Blob Storage
    StorageCredentials creds = new StorageCredentials(
        ConfigurationManager.AppSettings["accountName"],
        ConfigurationManager.AppSettings["accountKey"]);
    CloudStorageAccount account = new CloudStorageAccount(creds, useHttps: true);
    CloudBlobClient client = account.CreateCloudBlobClient();
    CloudBlobContainer contain = client.GetContainerReference(ConfigurationManager.AppSettings["container"]);
    contain.CreateIfNotExists();

    // The Resolver object is used to interact with Key Vault for Azure Storage
    // This is where the GetToken method from above is used
    KeyVaultKeyResolver cloudResolver = new KeyVaultKeyResolver(GetToken);


> [AZURE.NOTE] 密钥存储库对象模型
>
>务必要了解实际上两个密钥存储库对象模型需要注意的︰ 一基于 REST API （KeyVault 命名空间） 和另一种是客户端-端加密的扩展。

> 密钥存储库客户端与 REST API 进行交互，并了解 JSON Web 项和机密信息的两种类型的密钥存储库中包含的内容。 

> 密钥存储库扩展是似乎专为客户端加密在 Azure 存储中创建的类。 它们包含的键-IKey-和键冲突解决程序的概念所基于的类的接口。 有两种实现您需要知道的 IKey: RSAKey 和 SymmetricKey。 现在他们碰巧重合的内容包含在密钥存储库中，但此时它们独立类 （这样的密钥和密钥的密钥存储库客户端检索未实现 IKey）。 


## 对 blob 进行加密，并将上载
添加以下代码，以对 Blob 进行加密并将其上载到 Azure 存储帐户。 使用 ResolveKeyAsync 方法返回 IKey。 

    
    // Retrieve the key that you created previously
    // The IKey that is returned here is an RsaKey
    // Remember that we used the names contosokeyvault and testrsakey1
    var rsa = cloudResolver.ResolveKeyAsync("https://contosokeyvault.vault.azure.net/keys/TestRSAKey1", CancellationToken.None).GetAwaiter().GetResult();


    // Now you simply use the RSA key to encrypt by setting it in the BlobEncryptionPolicy. 
    BlobEncryptionPolicy policy = new BlobEncryptionPolicy(rsa, null);
    BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

    // Reference a block blob
    CloudBlockBlob blob = contain.GetBlockBlobReference("MyFile.txt");

    // Upload using the UploadFromStream method
    using (var stream = System.IO.File.OpenRead(@"C:\data\MyFile.txt"))
        blob.UploadFromStream(stream, stream.Length, null, options, null);


以下是从当前的 Azure 管理门户为 blob 已使用客户端加密密钥存储库中存储的密钥加密的一个屏幕快照。 该密钥 id 为属性是作为加密密钥 (KEK) 的密钥存储库中的密钥的 URI。 地属性包含 (CEK) 的内容加密密钥的加密的版本。 

![显示包含加密的元数据的 Blob 元数据的屏幕截图][1]

> [AZURE.NOTE] 如果您看一下 BlobEncryptionPolicy 构造函数，您将看到，它可以接受键和/或冲突解决程序。 请注意，现在不能进行加密使用的解析器，因为它当前并不支持默认密钥。



## 解密 blob 和下载
解密是真正的解析器类的意义。 用于加密的密钥 ID 是与它的元数据中的 Blob 关联，因此没有任何理由可以检索该密钥和记忆键和斑点之间的关联。 您只需确保密钥在密钥存储库中。   

因此对于由块进行加密键的解密包含 CEC （内容加密密钥） 的元数据被发送到密钥存储库进行解密，RSA 密钥的私钥将保留在密钥存储库。 

添加以下项来解密刚上载的 blob。 

    // In this case we will not pass a key and only pass the resolver because 
    //  this policy will only be used for downloading / decrypting
    BlobEncryptionPolicy policy = new BlobEncryptionPolicy(null, cloudResolver);
    BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

    using (var np = File.Open(@"C:\data\MyFileDecrypted.txt", FileMode.Create))
        blob.DownloadToStream(np, null, options, null);


> [AZURE.NOTE] 有几种其他类型的冲突解决程序以简化密钥管理，包括︰ AggregateKeyResolver 和 CachingKeyResolver。


## 使用的机密密钥存储库
使用客户端加密使用一个秘密的方法是通过 SymmetricKey 类，因为机密是在本质上是一个对称密钥。 但是，如上文所述，一个秘密密钥存储库中的不准确映射到 SymmetricKey。 若要了解以下几点︰


- SymmetricKey 中的键必须是固定的长度︰ 128、 192、 256、 384 或 512 位
- 在 SymmetricKey 中的项应是 Base64 编码
- 将使用 SymmetricKey 作为一个密钥存储库秘密需要密钥存储库中具有"应用程序/八位字节流"的内容类型

下面是一个示例，可用作 SymmetricKey 的密钥存储库中创建一个秘密的继续︰

    // Here we are making a 128-bit key so we have 16 characters. 
    //  The characters are in the ASCII range of UTF8 so they are
    //  each 1 byte. 16 x 8 = 128
    $key = "qwertyuiopasdfgh"
    $b = [System.Text.Encoding]::UTF8.GetBytes($key)
    $enc = [System.Convert]::ToBase64String($b)
    $secretvalue = ConvertTo-SecureString $enc -AsPlainText -Force

    // substitute the VaultName and Name in this command
    $secret = Set-AzureKeyVaultSecret -VaultName 'ContoseKeyVault' -Name 'TestSecret2' -SecretValue $secretvalue -ContentType "application/octet-stream"

在控制台应用程序，可以使用前的为同一调用来检索该密码为 SymmetricKey。

    SymmetricKey sec = (SymmetricKey) cloudResolver.ResolveKeyAsync(
        "https://contosokeyvault.vault.azure.net/secrets/TestSecret2/", 
        CancellationToken.None).GetAwaiter().GetResult();

就是这样。 尽情享受 ！

## 下一步行动

有关 Microsoft Azure 存储中使用的 C# 的详细信息，请参阅[Microsoft Azure 存储.NET 客户端库](https://msdn.microsoft.com/library/azure/dn261237.aspx)

有关 Blob REST API 的详细信息，请参阅[REST API，Blob 服务](https://msdn.microsoft.com/library/azure/dd135733.aspx)

有关 Microsoft Azure 存储最新信息，请转到[Microsoft Azure 存储团队博客](http://blogs.msdn.com/b/windowsazurestorage/)


<!--Image references-->
[1]: ./media/storage-encrypt-decrypt-blobs-key-vault/blobmetadata.png

测试

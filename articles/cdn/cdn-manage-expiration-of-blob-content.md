---
ms.openlocfilehash: 3fb2e63aff853cb8db29dd82f25913b2e81e1502
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
 pageTitle="如何管理 Azure 内容传递网络 (CDN) 中的 Blob 内容过期" 
 description="" 
 services="cdn" 
 documentationCenter=".NET" 
 authors="zhangmanling" 
 manager="dwrede" 
 editor=""/>
<tags 
 ms.service="cdn" 
 ms.workload="media" 
 ms.tgt_pltfrm="na" 
 ms.devlang="dotnet" 
 ms.topic="article" 
 ms.date="09/01/2015" 
 ms.author="mazha"/>


#如何管理 Azure 内容传递网络 (CDN) 中的 Blob 内容过期  

最大受益 Azure CDN 缓存中的 blob 是指那些经常在其生存时间 (TTL) 期间访问。 Blob TTL 时间保留在缓存中，然后通过刷新 blob 服务经过该时间后。 然后重复该过程。  

有两个选项，用于控制 TTL。  

1.  未设置缓存值，因此使用默认 TTL 设置为 7 天。 
2.  显式**将 Blob**、**将阻止列表**中或**设置 Blob 属性**请求上设置*x-ms-blob 的缓存控制*属性或使用 Azure 托管库设置[BlobProperties.CacheControl](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.blobproperties.cachecontrol.aspx)属性。 设置此属性将设置为 blob*缓存控制*标头的值。 标头或属性的值应按秒指定适当的值。 例如，若要设置缓存时间为一年的最大值，可以指定请求标头为`x-ms-blob-cache-control: public, max-age=31556926`。 设置缓存标头的详细信息，请参阅[HTTP/1.1 规范](http://www.w3.org/Protocols/rfc2616/rfc2616-sec13.html)。  

希望通过 CDN 缓存任何内容必须为可公开访问的 blob 存储在 Azure 存储帐户。 Azure Blob 服务的详细信息，请参阅**Blob 服务概念**。  

有多种不同的方式，您可以使用中的 Blob 服务的内容︰  

-   通过使用托管的 API 提供的**Azure 托管类库参考**。
-   通过使用一个第三方存储管理工具。
-   通过使用 Azure 存储服务 REST API。  

下面的代码示例是一个控制台应用程序，它使用 Azure 托管库创建一个容器，设置其权限的公共访问权限，并创建 blob 容器内。 通过设置在该 blob 缓存控制标头，它还明确指定所需的刷新间隔。   

假设您已经启用了 CDN，如上所示，将由 CDN 缓存创建 blob。 请务必指定您使用自己的存储帐户和访问密钥的帐户凭据︰  

    using System;
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.StorageClient;
    
    namespace BlobsInCDN
    {
        class Program
        {
            static void Main(string[] args)
            {
                //Specify storage credentials.
                StorageCredentialsAccountAndKey credentials = new StorageCredentialsAccountAndKey("storagesample",
                    "m4AHAkXjfhlt2rE2BN/hcUR4U2lkGdCmj2/1ISutZKl+OqlrZN98Mhzq/U2AHYJT992tLmrkFW+mQgw9loIVCg==");
                
                //Create a reference to your storage account, passing in your credentials.
                CloudStorageAccount storageAccount = new CloudStorageAccount(credentials, true);
                
                //Create a new client object, which will provide access to Blob service resources.
                CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    
                //Create a new container.
                CloudBlobContainer container = blobClient.GetContainerReference("cdncontent");
                container.CreateIfNotExist();
    
                //Specify that the container is publicly accessible.
                BlobContainerPermissions containerAccess = new BlobContainerPermissions();
                containerAccess.PublicAccess = BlobContainerPublicAccessType.Container;
                container.SetPermissions(containerAccess);
    
                //Create a new blob and write some text to it.
                CloudBlob blob = blobClient.GetBlobReference("cdncontent/testblob.txt");
                blob.UploadText("This is a test blob.");
    
                //Set the Cache-Control header on the blob to specify your desired refresh interval.
                blob.SetCacheControl("public, max-age=31536000");
            }
        }
    
        public static class BlobExtensions
        {
            //A convenience method to set the Cache-Control header.
            public static void SetCacheControl(this CloudBlob blob, string value)
            {
                blob.Properties.CacheControl = value;
                blob.SetProperties();
            }
        }
    }

测试您的斑点是通过 CDN 特定 URL。 该 blob，如上所示，将类似于以下 URL:  

    http://az1234.vo.msecnd.net/cdncontent/testblob.txt  

如果需要，可以使用**wget**或 Fiddler 等工具来检查请求和响应的详细信息。

##请参见

[如何管理 Azure 内容传递网络 (CDN) 中的云服务内容过期](./cdn-manage-expiration-of-cloud-service-content.md
) 

测试

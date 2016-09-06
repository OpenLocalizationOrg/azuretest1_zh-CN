---
ms.openlocfilehash: 4fbe5263df2345b7b9daf0b0347b1d25c2cfa26d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="访问 Azure 存储资源管理 |Microsoft Azure" 
    description="了解如何管理用户访问您的 Azure 存储资源的方式。" 
    services="storage" 
    documentationCenter="" 
    authors="tamram" 
    manager="jdial" 
    editor=""/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/04/2015" 
    ms.author="micurd;tamram"/>

# 管理对 Azure 存储资源的访问

## 概述

默认情况下，只有存储帐户的所有者可以访问该帐户内的存储资源。 如果您的服务或应用程序需要使这些资源可为其他客户端不共享您的访问密钥的情况下，您可以为允许访问以下选项︰

- 您可以设置允许匿名读取访问容器和其 blob 容器的权限。 匿名的读取访问权限是仅适用于容器和 blob。 

- 您可以公开通过共享的访问签名，这使您可以通过指定的时间间隔，有足够的资源和客户端都给它的权限委派到容器、 斑点、 表、 队列、 文件共享或文件限制的访问的资源。

- 可用的存储的访问策略来管理容器或其 blob、 队列、 表或文件共享或其文件共享的访问签名。 存储的访问策略为您提供了控制共享的访问签名的附加度量，并还提供一种简单的方式，废除它们。

## 限制访问容器和 Blob

默认情况下，可能只能通过存储帐户的所有者访问容器，其内部任何 blob。 若要授予匿名用户对容器和其 blob 的读取权限，可以设置容器权限，以允许公共访问权限。 匿名用户可以读取 blob 可公开访问的容器中而对请求进行身份验证。

容器提供了用于管理容器访问以下选项︰

- **完全公共的读访问权限︰**可以通过匿名请求读取容器和 blob 数据。 客户端可以枚举通过匿名请求时，容器中的 blob，但无法枚举内存储帐户的容器。

- **公共读取 blob 仅访问︰**这个容器中的 blob 数据可以读取通过匿名请求，但容器数据不可用。 客户端无法枚举通过匿名请求容器中的 blob。

- **没有公共的读访问权限︰**可以按帐户所有者只读取容器和 blob 数据。

>[AZURE.NOTE]如果您的服务需要您练习更精细地控制 blob 的资源，或者如果您想要提供读取操作以外的操作的权限，您可以使用共享访问签名以使用户可访问的资源。 

### 匿名用户可访问的功能
下表显示了哪些操作可能由匿名用户，当容器的 ACL 设置为允许公共访问权限。

| 其他操作                                         | 具有完整公钥的读取访问权限 | 与公钥 blob 仅读访问权限的权限 |
|--------------------------------------------------------|-----------------------------------------|---------------------------------------------------|
| 列表的容器                                        | 只有所有者                              | 只有所有者                                        |
| 创建容器                                       | 只有所有者                              | 只有所有者                                        |
| 获取容器属性                               | 所有                                     | 只有所有者                                        |
| 获取容器的元数据                                 | 所有                                     | 只有所有者                                        |
| 设置容器元数据                                 | 只有所有者                              | 只有所有者                                        |
| 获取容器 ACL                                      | 只有所有者                              | 只有所有者                                        |
| 设置容器 ACL                                      | 只有所有者                              | 只有所有者                                        |
| 删除容器                                       | 只有所有者                              | 只有所有者                                        |
| 列表中的 Blob                                             | 所有                                     | 只有所有者                                        |
| 将 Blob                                               | 只有所有者                              | 只有所有者                                        |
| 获取 Blob                                               | 所有                                     | 所有                                               |
| 获取 Blob 属性                                    | 所有                                     | 所有                                               |
| Blob 设置属性                                    | 只有所有者                              | 只有所有者                                        |
| 获取元数据 Blob                                      | 所有                                     | 所有                                               |
| 设置元数据 Blob                                      | 只有所有者                              | 只有所有者                                        |
| 放块                                              | 只有所有者                              | 只有所有者                                        |
| 获取块列表 （仅提交块）                 | 所有                                     | 所有                                               |
| 获取块列表 （仅在未提交的块或所有块） | 只有所有者                              | 只有所有者                                        |
| 放块列表                                         | 只有所有者                              | 只有所有者                                        |
| 删除 Blob                                            | 只有所有者                              | 只有所有者                                        |
| 复制 Blob                                              | 只有所有者                              | 只有所有者                                        |
| 快照的斑点                                          | 只有所有者                              | 只有所有者                                        |
| 租约 Blob                                             | 只有所有者                              | 只有所有者                                        |
| 放入页面                                               | 只有所有者                              | 只有所有者                                        |
| 获取页面范围                                        | 所有                                     | 所有                                                  |

## 创建和使用共享的访问签名
共享的访问签名 (SA) 是授予受指定的时间间隔限制对存储资源的访问权限的 URI。 您可以在这些存储资源上创建 SA:

- 容器和 blob
- 队列
- 表
- 文件共享和文件 

通过提供一个客户端使用共享的访问签名，可以使这些不与他们共享您的帐户密钥访问您的存储帐户中的资源。

>[AZURE.NOTE] 深层的概念性概述和有关共享的访问签名的教程，请参阅[共享访问签名](storage-dotnet-shared-access-signature-part-1.md)。

共享的访问签名 URI 查询参数将纳入所有受控制的授权访问存储资源所需的信息。 在 SAS 的 URI 查询参数包括通过它的共享的访问签名有效的时间间隔、 它授予的权限，可供资源、 版本用于执行请求，并存储服务将用于对请求进行身份验证的签名。

此外，共享的访问签名 URI 可以引用提供额外的一套签名，其中包括的功能，以修改或废除对资源的访问，如有必要控制存储的访问策略。 

共享的访问签名的 URI 格式的信息，请参阅[委派访问共享的访问权限签名](https://msdn.microsoft.com/library/ee395415.aspx)。

### 安全使用共享访问签名
共享的访问签名授予访问权限的 URI 授予的权限所指定的资源。 应始终使用 HTTPS 来构造 URI 的共享的访问签名。 具有共享的访问签名使用 HTTP 可以使您的存储帐户容易受到恶意使用。

如果共享的访问签名授予不是公众的访问，然后它应使用来构造最少可能的权限。 此外，共享的访问签名应该会安全地分发到客户端通过安全连接、 应吊销，用于存储的访问策略相关联，应指定签名的最短可能生存时间。

>[AZURE.NOTE] 共享的访问签名 URI 相关联使用帐户密钥用于创建该签名，并且关联存储访问策略 （如果有的话）。 如果未不指定任何存储的访问策略，撤消共享的访问签名的唯一办法是更改帐户密码。 

### 创建共享的访问签名
下面的代码示例创建的容器上的访问策略，然后生成容器的共享的访问签名。 然后，此共享的访问签名可以提供给客户端︰

    // The connection string for the storage account.  Modify for your account.
    string storageConnectionString =
       "DefaultEndpointsProtocol=https;" +
       "AccountName=myaccount;" +
       "AccountKey=<account-key>";
    
    // As an alternative, you can retrieve storage account information from an app.config file. 
    // This is one way to store and retrieve a connection string if you are 
    // writing an application that will run locally, rather than in Microsoft Azure.
    
    // string storageConnectionString = ConfigurationManager.AppSettings["StorageAccountConnectionString"];
    
    // Create the storage account with the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageConnectionString);
       
    // Create the blob client object.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    
    // Get a reference to the container for which shared access signature will be created.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");
    container.CreateIfNotExists();
    
    // Create blob container permissions, consisting of a shared access policy 
    // and a public access setting. 
    BlobContainerPermissions blobPermissions = new BlobContainerPermissions();
    
    // The shared access policy provides 
    // read/write access to the container for 10 hours.
    blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
    {
       // To ensure SAS is valid immediately, don’t set start time.
       // This way, you can avoid failures caused by small clock differences.
       SharedAccessExpiryTime = DateTime.UtcNow.AddHours(10),
       Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
    });
    
    // The public access setting explicitly specifies that 
    // the container is private, so that it can't be accessed anonymously.
    blobPermissions.PublicAccess = BlobContainerPublicAccessType.Off;
    
    // Set the permission policy on the container.
    container.SetPermissions(blobPermissions);
    
    // Get the shared access signature to share with users.
    string sasToken =
       container.GetSharedAccessSignature(new SharedAccessBlobPolicy(), "mypolicy");

### 使用共享的访问签名
接收的共享的访问签名的客户端可以使用它从他们的代码构造类型[StorageCredentials](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.auth.storagecredentials.aspx)的对象。 这些凭据可以再用于构造[CloudStorageAccount](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.cloudstorageaccount.aspx)或[CloudBlobClient](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobclient.aspx)对象使用的资源，如本示例所示︰

    Uri blobUri = new Uri("https://myaccount.blob.core.windows.net/mycontainer/myblob.txt");
    
    // Create credentials with the SAS token. The SAS token was created in previous example.
    StorageCredentials credentials = new StorageCredentials(sasToken);
    
    // Create a new blob.
    CloudBlockBlob blob = new CloudBlockBlob(blobUri, credentials);
    
    // Upload the blob. 
    // If the blob does not yet exist, it will be created. 
    // If the blob does exist, its existing content will be overwritten.
    using (var fileStream = System.IO.File.OpenRead(@"c:\Test\myblob.txt"))
    {
    blob.UploadFromStream(fileStream);
    }

## 使用存储的访问策略
存储的访问策略服务器端提供附加控制共享的访问签名的级别。 建立存储的访问策略是以分组共享的访问签名并提供对受该策略的签名的附加限制。 可以使用存储的访问策略更改开始时间、 到期时间或权限的一个签名，或其后撤消其已发出。

存储的访问策略使您可以更好地控制共享的访问已释放的签名。 而不是 url 指定签名的生存期和权限，可以对存储在容器、 文件共享、 队列或表包含共享的资源存储的访问策略来指定这些参数。 若要更改这些参数的一个或多个签名，您可以修改存储的访问策略，而不是重新签名。 通过修改存储的访问策略，可以快速废除签名。

例如，假设已发出存储的访问策略相关联的共享的访问签名。 如果您指定的到期时间内存储的访问策略，可以修改扩展的签名，而不必重新创建新签名的访问策略。

建议的最佳做法是，指定您颁发一个共享的访问签名，所有签名资源的存储的访问策略为存储的策略可以用于修改或撤消签名后已发出。 如果没有指定存储的策略，建议您为您的存储帐户资源到任何风险降至最低限制的生存期的签名。 

### 共享的访问签名关联的存储的访问策略
存储的访问策略包括最多 64 个字符长，它是唯一容器、 文件共享、 队列或表内的名称。 要访问存储的策略相关联的共享的访问签名，请指定此标识符创建共享的访问签名时。 共享的访问签名 URI 中， *signedidentifier*字段中指定的存储的访问策略标识符。

容器、 文件共享、 队列或表可以包括最多 5 存储的访问策略。 每个策略可由任意数量的共享的访问签名。

>[AZURE.NOTE]当建立在容器、 文件共享、 队列或表的存储的访问策略时，它可能需要 30 秒才能生效。 在此期间，与存储的访问策略相关联的共享的访问签名将失败并状态代码 403 （禁止），直到访问策略将变为活动状态。

### 共享的访问策略指定访问策略参数
存储的访问策略可以指定与它相关联的签名下面访问策略参数︰

- 开始时间

- 到期时间

- 权限

具体取决于您希望如何控制对存储资源的访问，可以指定所有这些参数在存储的访问策略中，并忽略这些参数的 url 的共享的访问签名。 这样做使您来修改关联的签名行为，在任何时间，以及要将其撤消。 或者，您可以指定一个或多个访问策略参数中存储的访问策略，和其他的 url。 最后，您可以指定所有参数在 URL 上。 在这种情况下，可以使用存储的访问策略来撤消该签名，但不是能修改其行为。

在一起共享的访问签名和存储的访问策略必须包括对签名进行身份验证所需的所有字段。 如果缺少任何必需的字段，则请求将失败，状态代码 403 （禁止）。 同样，如果在共享的访问签名 URL 和存储的访问策略中指定字段，则请求将失败，状态代码 403 （错误请求）。 请参阅创建和使用共享访问签名有关字段组成特征码的详细信息。

### 修改或撤消存储的访问策略

若要撤销访问权限共享的访问签名，使用相同的存储的访问策略，删除存储的策略存储资源通过用新的列表不包含策略名称覆盖存储的策略列表中。 若要更改存储的访问策略访问权限设置，覆盖新的列表，其中包含有新的访问控制的详细信息的名称相同的策略存储的策略列表。

## 请参见

- [Azure 存储服务的身份验证](https://msdn.microsoft.com/library/azure/dd179428.aspx)
- [共享访问权限签名︰ 了解 SAS 模型](storage-dotnet-shared-access-signature-part-1.md)
- [委派访问权限共享的访问签名](https://msdn.microsoft.com/library/azure/ee395415.aspx) 
---
ms.openlocfilehash: a450d40d443c1481935b06a13dfc6dc130164ee9
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="Azure Blob 存储和 Visual Studio 入门连接服务"
    description="如何开始使用 Azure Blob 存储在 Visual Studio 的 ASP.NET 5 项目"
    services="storage"
    documentationCenter=""
    authors="patshea123"
    manager="douge"
    editor="tglee"/>

<tags
    ms.service="storage"
    ms.workload="web"
    ms.tgt_pltfrm="vs-getting-started"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/22/2015"
    ms.author="patshea123"/>

# Azure Blob 存储和 Visual Studio 入门连接服务

> [AZURE.SELECTOR]
> - [入门教程](vs-storage-aspnet5-getting-started-blobs.md)
> - [发生了什么事](vs-storage-aspnet5-what-happened.md)

> [AZURE.SELECTOR]
> - [Blob](vs-storage-aspnet5-getting-started-blobs.md)
> - [队列](vs-storage-aspnet5-getting-started-queues.md)
> - [表](vs-storage-aspnet5-getting-started-tables.md)

##概述

本文介绍了如何开始使用 Azure Blob 存储在 Visual Studio 中，您创建或通过使用 Visual Studio 中添加连接服务对话框引用 ASP.NET 5 项目在 Azure 存储帐户后。 

Azure Blob 存储是一种服务来存储大量的非结构化数据可以从任何位置访问在通过 HTTP 或 HTTPS 世界。 单个 blob 可以为任意大小。 Blob 可以是诸如图像、 音频和视频文件，原始数据和文档文件。 本文介绍如何使用 ASP.NET 5 项目中的 Visual Studio**添加连接服务**对话框中创建的 Azure 存储帐户之后开始使用 blob 存储。

正如文件居住在文件夹中，存储 blob 生存在容器中。 您已经创建了一个存储后，在存储区中创建一个或多个容器。 例如，在存储称为"剪贴簿"中，可以创建名为"映像"来存储图片的存储的容器，另一个名为"音频"来存储声音文件。 创建容器后，您可以将单个 blob 文件上载到它们。 有关以编程方式操作 blob，请参阅[如何使用 blob 存储从.NET](storage-dotnet-how-to-use-blobs.md "如何使用.NET 中的 blob 存储")的详细信息。



##在代码中访问 blob 容器

若要以编程方式访问 blob ASP.NET 5 项目中的，您需要添加下面的项，如果它们目前尚不存在。

1. 要以编程方式访问 Azure 存储任何 C# 文件的顶部添加以下代码的命名空间声明。

        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;

2. 获得`CloudStorageAccount`对象，它表示您存储的帐户信息。 下面的代码用于获取您的存储连接字符串和从 Azure 服务配置的存储帐户信息。

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

    **注意︰**在下面的部分会使用所有上面的代码，该代码的前面。


3. 使用`CloudBlobClient`对象来获取`CloudBlobContainer`到现有的容器中存储帐户的引用。

        // Create a blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        // Get a reference to a container named “mycontainer.”
        CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");



##在代码中创建一个容器

您还可以使用`CloudBlobClient`在您的存储帐户中创建容器。 所有您需要做是对的调用中添加`CreateIfNotExistsAsync`如以下代码所示︰

    // Create a blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Get a reference to a container named “my-new-container.”
    CloudBlobContainer container = blobClient.GetContainerReference("my-new-container");

    // If “mycontainer” doesn’t exist, create it.
    await container.CreateIfNotExistsAsync();


**注意︰**在 ASP.NET 5 执行对 Azure 存储调用的 Api 是异步的。 有关更多信息，请参见[异步编程与异步和等待](http://msdn.microsoft.com/library/hh191443.aspx)。 下面的代码假定正在使用异步编程方法。

要使容器内的文件可供每个人使用，您可以设置容器，以通过使用下面的代码是公共的。

    await container.SetPermissionsAsync(new BlobContainerPermissions
    {
        PublicAccess = BlobContainerPublicAccessType.Blob
    });

##上载到的容器的 blob

以斑点文件上载到一个容器中，获取容器引用并使用它来获取 blob 引用。 Blob 引用之后，可以上载数据任意流到它通过调用`UploadFromStreamAsync`方法。 如果它已不存在，或将覆盖它，如果它不存在，此操作将创建该 blob。 下面的示例演示如何将 blob 传到一个容器，并假定该容器已创建。

    // Get a reference to a blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite the "myblob" blob with the contents of a local file
    // named “myfile”.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        await blockBlob.UploadFromStreamAsync(fileStream);
    }

##列出容器中的 blob
若要列出容器中的 blob，先获取容器引用。 然后，您可以调用容器的`ListBlobsSegmentedAsync`方法来检索 blob 和/或目录中的。 若要访问的属性和方法返回的丰富`IListBlobItem`，您必须将其转换为`CloudBlockBlob`， `CloudPageBlob`，或`CloudBlobDirectory`对象。 如果您不知道 blob 类型，您可以使用执行类型检查来确定要将其转换为。 下面的代码演示如何检索和输出容器中的每一项的 URI。

    BlobContinuationToken token = null;
        do
        {
            BlobResultSegment resultSegment = await container.ListBlobsSegmentedAsync(token);
            token = resultSegment.ContinuationToken;

            foreach (IListBlobItem item in resultSegment.Results)
            {
                if (item.GetType() == typeof(CloudBlockBlob))
                {
                    CloudBlockBlob blob = (CloudBlockBlob)item;
                    Console.WriteLine("Block blob of length {0}: {1}", blob.Properties.Length, blob.Uri);
                }

                else if (item.GetType() == typeof(CloudPageBlob))
                {
                    CloudPageBlob pageBlob = (CloudPageBlob)item;

                    Console.WriteLine("Page blob of length {0}: {1}", pageBlob.Properties.Length, pageBlob.Uri);
                }

                else if (item.GetType() == typeof(CloudBlobDirectory))
                {
                    CloudBlobDirectory directory = (CloudBlobDirectory)item;

                    Console.WriteLine("Directory: {0}", directory.Uri);
                }
            }
        } while (token != null);

他人没有办法列出一个 blob 容器的内容。 详细信息，请参阅[如何使用 blob 存储从.NET](storage-dotnet-how-to-use-blobs.md#list-the-blobs-in-a-container) 。

##下载一个 blob
想下载一个 blob，首先获取 blob，引用，然后调用`DownloadToStreamAsync`方法。 下面的示例使用`DownloadToStreamAsync`方法将 blob 内容传输到一个流对象，然后您可以将其保存为本地文件。

    // Get a reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save the blob contents to a file named “myfile”.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        await blockBlob.DownloadToStreamAsync(fileStream);
    }

有其他方法将 blob 另存为文件。 详细信息，请参阅[如何使用 blob 存储从.NET](storage-dotnet-how-to-use-blobs.md/#download-blobs) 。

##删除一个 blob
若要删除某个 blob，先获取对该 blob，引用，然后调用`DeleteAsync`在它的方法。

    // Get a reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    await blockBlob.DeleteAsync();

## 下一步行动

[AZURE.INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]



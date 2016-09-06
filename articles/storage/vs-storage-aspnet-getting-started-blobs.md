---
ms.openlocfilehash: 1d985e26dd2b28361528ae7fb54c85f20b237344
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="Azure Blob 存储和 Visual Studio 入门连接服务"
    description="如何开始使用 ASP.NET 项目在 Visual Studio 中的 Azure Blob 存储"
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
    ms.date="08/04/2015"
    ms.author="patshea123"/>

# Azure Blob 存储和 Visual Studio 入门连接服务

> [AZURE.SELECTOR]
> - [入门教程](vs-storage-aspnet-getting-started-blobs.md)
> - [发生了什么事](vs-storage-aspnet-what-happened.md)

> [AZURE.SELECTOR]
> - [Blob](vs-storage-aspnet-getting-started-blobs.md)
> - [队列](vs-storage-aspnet-getting-started-queues.md)
> - [表](vs-storage-aspnet-getting-started-tables.md)

## 概述

本文介绍如何开始使用 Azure Blob 存储后已创建，或通过使用 Visual Studio**中添加连接服务**对话框中引用 ASP.NET 应用程序中的 Azure 存储帐户。 文章介绍了如何创建 blob 容器，并执行其他常见任务，例如上传、 列表、 下载和删除的 blob。 用 c 语言编写的示例\#并使用[Azure 存储.NET 客户端库](https://msdn.microsoft.com/library/azure/dn261237.aspx)。 

 - 有关使用 Azure Blob 存储的详细信息，请参阅[如何使用 Blob 存储从.NET](storage-dotnet-how-to-use-blobs.md)。 
 - 有关 ASP.NET 项目的详细信息，请参见[ASP.NET](http://www.asp.net)。


Azure Blob 存储是一种服务来存储大量的非结构化数据可以从任何位置访问在通过 HTTP 或 HTTPS 世界。 单个 blob 可以为任意大小。 Blob 可以是诸如图像、 音频和视频文件，原始数据和文档文件。

正如文件居住在文件夹中，存储 blob 生存在容器中。 创建存储帐户后，您可以在存储中创建一个或多个容器。 例如，在存储称为"剪贴簿"中，您可以创建名为"映像"来存储图片的存储 blob 容器，另一个名为"音频"来存储声音文件。 创建容器后，您可以将单个 blob 文件上载到它们。




##在代码中访问 blob 容器

若要以编程方式访问 blob ASP.NET 项目中的，您需要添加下面的项，如果它们目前尚不存在。

1. 您希望以编程方式访问 Azure 存储任何 C# 文件的顶部添加以下代码的命名空间声明。

        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Auth;
        using Microsoft.WindowsAzure.Storage.Blob;


2. 获得`CloudStorageAccount`对象，它表示您存储的帐户信息。 下面的代码用于获取您的存储连接字符串和从 Azure 服务配置的存储帐户信息。

        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

    > [AZURE.NOTE] 以下各节中使用上面的代码在代码前面的所有。

3. 获得`CloudBlobClient`对象引用现有容器中存储帐户。

        // Create a blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        // Get a reference to a container named “mycontainer.”
        CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

> [AZURE.NOTE] 一些 ASP.NET 第 5 执行出到 Azure 存储调用的 Api 是异步的。 有关更多信息，请参见[异步和等待异步编程](http://msdn.microsoft.com/library/hh191443.aspx)。


## 在代码中创建 blob 容器

您还可以使用`CloudBlobClient`要在您的存储帐户中创建容器对象。 所有您需要做的只是添加对的调用`CreateIfNotExistsAsync`为上面的代码，如下面的示例所示。

    // If “mycontainer” doesn’t exist, create it.
    await container.CreateIfNotExistsAsync();

## 上载到的容器的 blob

Azure Blob 存储支持阻止 blob 和页面 blob。  在大多数情况下，块斑点是推荐使用的类型。

若要将文件上载到块斑点，获取容器引用，并使用它来获取块斑点引用。 Blob 引用之后，可以上载数据的任意流到它通过调用`UploadFromStream`方法。 如果它以前不存在，或者覆盖它，如果它不存在，则此操作将创建该 blob。 下面的示例演示如何将 blob 传到一个容器，并假定该容器已创建。

    // Get a CloudBlobContainer named 'container' as described in "Access blob containers in code."

    // Create or overwrite the "myblob" blob with contents from a local file.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        blockBlob.UploadFromStream(fileStream);
    }

## 列出容器中的 blob

若要列出容器中的 blob，使用`ListBlobs`方法来检索 blob 和/或目录中的。 若要访问的属性和方法返回的丰富`IListBlobItem`，您必须将其转换为`CloudBlockBlob`， `CloudPageBlob`，或`CloudBlobDirectory`对象。  如果该类型是未知的您可以使用执行类型检查来确定要将其转换为。  下面的代码演示如何检索和输出中每个项的 URI`photos`容器。

    // Get a CloudBlobContainer named 'container' as described in "Access blob containers in code."

    // Loop over items within the container and output the length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, false))
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

前面的示例中所示，blob 服务都有内的容器，以及目录的概念。 这是这样，您可以整理您更类似于文件夹结构中的 blob。 例如，请考虑以下一组名为容器中的块 blob `photos`。

    photo1.jpg
    2010/architecture/description.txt
    2010/architecture/photo3.jpg
    2010/architecture/photo4.jpg
    2011/architecture/photo5.jpg
    2011/architecture/photo6.jpg
    2011/architecture/description.txt
    2011/photo7.jpg

当您调用`ListBlobs`返回的集合将包含在照片容器 （如前面的示例中所示）， `CloudBlobDirectory` ，`CloudBlockBlob`对象的目录和包含在顶级的 blob 表示。 下面的示例显示所生成的输出。

    Directory: https://<accountname>.blob.core.windows.net/photos/2010/
    Directory: https://<accountname>.blob.core.windows.net/photos/2011/
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


或者，您可以设置`UseFlatBlobListing`参数的`ListBlobs`方法对`true`。 这会导致每个 blob 返回为`CloudBlockBlob`，无论目录。  下面的示例演示在调用`ListBlobs`。

    // Loop over items within the container and output the length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, true))
    {
       ...
    }

然后下一个示例中显示的结果。

    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2010/architecture/description.txt
    Block blob of length 314618: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo3.jpg
    Block blob of length 522713: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo4.jpg
    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2011/architecture/description.txt
    Block blob of length 419048: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo5.jpg
    Block blob of length 506388: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo6.jpg
    Block blob of length 399751: https://<accountname>.blob.core.windows.net/photos/2011/photo7.jpg
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg



## 下载 blob

若要下载 blob，使用`DownloadToStream`方法。 下面的示例使用`DownloadToStream`方法将 blob 内容传输到再持久保存到本地文件的流对象。

    // Get a CloudBlobContainer named 'container' as described in "Access blob containers in code"

    // Retrieve a reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save blob contents to a file.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        blockBlob.DownloadToStream(fileStream);
    }

您还可以使用`DownloadToStream`方法下载的内容作为文本字符串的 blob。

    // Get a CloudBlobContainer named 'container' as described in "Access blob containers in code"

    // Retrieve a reference to a blob named "myblob.txt"
    CloudBlockBlob blockBlob2 = container.GetBlockBlobReference("myblob.txt");

    string text;
    using (var memoryStream = new MemoryStream())
    {
        blockBlob2.DownloadToStream(memoryStream);
        text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
    }

## 删除 blob

若要删除某个 blob，使用`Delete`方法。

    // Get a CloudBlobContainer named 'container' as described in "Access blob containers in code"

    // Retrieve reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    blockBlob.Delete();


## 以异步方式列出网页中的 blob

如果列出了大量的 blob，或想要控制在一个列表操作返回的结果数，您可以列出在结果页中的 blob。 下面的示例演示如何异步，返回的结果页中，以便执行未被阻塞在等待返回大型结果集时。

本示例显示平面 blob 列出，但您还可以通过设置执行层次结构列表，`useFlatBlobListing`的参数`ListBlobsSegmentedAsync`方法对`false`。

该示例方法调用的异步方法，因为它必须开头`async`关键字，并且它必须返回`Task`对象。 为指定的 await 关键字`ListBlobsSegmentedAsync`方法列表任务完成之前暂停执行示例方法。

    async public static Task ListBlobsSegmentedInFlatListing(CloudBlobContainer container)
    {
        //List blobs to the console window, with paging.
        Console.WriteLine("List blobs in pages:");

        int i = 0;
        BlobContinuationToken continuationToken = null;
        BlobResultSegment resultSegment = null;

        //Call ListBlobsSegmentedAsync and enumerate the result segment returned, while the continuation token is non-null.
        //When the continuation token is null, the last page has been returned and execution can exit the loop.
        do
        {
            //This overload allows control of the page size. You can return all remaining results by passing null for the maxResults parameter,
            //or by calling a different overload.
            resultSegment = await container.ListBlobsSegmentedAsync("", true, BlobListingDetails.All, 10, continuationToken, null, null);
            if (resultSegment.Results.Count<IListBlobItem>() > 0) { Console.WriteLine("Page {0}:", ++i); }
            foreach (var blobItem in resultSegment.Results)
            {
                Console.WriteLine("\t{0}", blobItem.StorageUri.PrimaryUri);
            }
            Console.WriteLine();

            //Get the continuation token.
            continuationToken = resultSegment.ContinuationToken;
        }
        while (continuationToken != null);
    }

## 下一步行动

[AZURE.INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]


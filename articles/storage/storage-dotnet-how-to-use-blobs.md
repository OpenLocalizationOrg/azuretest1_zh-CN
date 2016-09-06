---
ms.openlocfilehash: 03bfc5a45a485fffe2ccf62b8a80bb0be649a3dd
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="如何使用.NET 中的 Blob 存储 |Microsoft Azure"
    description="了解有关 Azure Blob 存储，并了解如何创建一个容器和要上载，列表，下载并删除 blob 内容。"
    services="storage"
    documentationCenter=".net"
    authors="tamram"
    manager="adinah"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article" 
    ms.date="08/04/2015"
    ms.author="tamram"/>


# 如何使用.NET 中的 Blob 存储

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

## 概述

本指南将说明如何执行常见使用 Azure Blob 存储服务的方案。 用 c 语言编写的示例是\#，并且针对.NET 使用 Azure 存储客户端库。 所包含的方案包括**上传**、**列表**、**下载**和**删除**blob。

[AZURE.INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-configure-connection-string-include](../../includes/storage-configure-connection-string-include.md)]

## 以编程方式访问 Blob 存储

[AZURE.INCLUDE [storage-dotnet-obtain-assembly](../../includes/storage-dotnet-obtain-assembly.md)]

### Namespace 声明
将下面的命名空间声明添加到顶部的任何 C\#想以编程方式访问 Azure 存储文件︰

    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Blob;

请确保您引用`Microsoft.WindowsAzure.Storage.dll`程序集。

[AZURE.INCLUDE [storage-dotnet-retrieve-conn-string](../../includes/storage-dotnet-retrieve-conn-string.md)]

**CloudBlobClient**类型允许您检索表示容器和 blob 存储 Blob 存储服务中的对象。 下面的代码创建一个**CloudBlobClient**对象，该对象使用上面我们检索存储帐户对象︰

    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

## 创建容器

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

此示例演示如何创建一个容器，如果不存在︰

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve a reference to a container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Create the container if it doesn't already exist.
    container.CreateIfNotExists();

默认情况下，新的容器是私有的您必须指定您的存储访问密钥下载此容器中的 blob。 如果您想要向所有人提供容器内的文件，您可以设置容器是公共使用以下代码︰

    container.SetPermissions(
        new BlobContainerPermissions { PublicAccess =
        BlobContainerPublicAccessType.Blob });

在 Internet 上的任何人都可以看到 blob 在公共容器中，但您可以修改或删除它们，只有具有适当的访问键。

## 上载到的容器的 blob

Azure Blob 存储支持块 blob 和页面 blob。  在大多数情况下，块斑点是推荐使用的类型。

若要将文件上载到块斑点，获取容器引用，并使用它来获取块斑点引用。 Blob 引用之后，可以通过调用**UploadFromStream**方法来上载所有的数据的流。 如果它以前不存在，或者覆盖它，如果它不存在，则此操作将创建该 blob。 

下面的示例演示如何将 blob 传到一个容器，并假定该容器已创建。

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite the "myblob" blob with contents from a local file.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        blockBlob.UploadFromStream(fileStream);
    }

## 列出容器中的 blob

若要列出容器中的 blob，先获取容器引用。 然后可以使用容器的**ListBlobs**方法来检索 blob 和/或目录中。 若要访问丰富的属性和方法的返回**IListBlobItem**，必须将其转换为**CloudBlockBlob**、 **CloudPageBlob**或**CloudBlobDirectory**对象。  如果该类型是未知的您可以使用执行类型检查来确定要将其转换为。  下面的代码演示如何检索和输出中每个项的 URI`photos`容器︰

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("photos");

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

如上所示，您可以命名 blob 在其名称中的路径信息。 这将创建一个虚拟目录结构，您可以组织和遍历就像传统的文件系统。 请注意，只是虚拟目录结构，即在 Blob 存储中可用的唯一资源是容器和 blob。 但是，存储客户端库提供了一个**CloudBlobDirectory**对象引用一个虚拟目录并简化了使用这种方式组织的 blob 的过程。

例如，请考虑以下一组名为容器中的块 blob `photos`:

    photo1.jpg
    2010/architecture/description.txt
    2010/architecture/photo3.jpg
    2010/architecture/photo4.jpg
    2011/architecture/photo5.jpg
    2011/architecture/photo6.jpg
    2011/architecture/description.txt
    2011/photo7.jpg

在照片容器 （如上面的示例中） 上调用**ListBlobs**时，将返回层次结构列表。 它包含**CloudBlobDirectory**和**CloudBlockBlob**对象，分别代表目录和 blob 的容器中。 输出结果如下所示︰

    Directory: https://<accountname>.blob.core.windows.net/photos/2010/
    Directory: https://<accountname>.blob.core.windows.net/photos/2011/
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


（可选） 您可以**ListBlobs**方法的**UseFlatBlobListing**参数设置为**true**。 在这种情况下，每个容器中的 blob 被返回一个**CloudBlockBlob**对象。 **ListBlobs**调用以返回平面列表如下所示︰

    // Loop over items within the container and output the length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, true))
    {
       ...
    }

和结果如下所示︰

    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2010/architecture/description.txt
    Block blob of length 314618: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo3.jpg
    Block blob of length 522713: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo4.jpg
    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2011/architecture/description.txt
    Block blob of length 419048: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo5.jpg
    Block blob of length 506388: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo6.jpg
    Block blob of length 399751: https://<accountname>.blob.core.windows.net/photos/2011/photo7.jpg
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


## 下载 blob

若要下载 blob，首先检索 blob 引用，然后调用**DownloadToStream**方法。 下面的示例使用**DownloadToStream**方法将 blob 内容转移到一个流对象，然后可以保存到一个本地文件。

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save blob contents to a file.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        blockBlob.DownloadToStream(fileStream);
    }

您可以使用**DownloadToStream**方法要下载的内容作为文本字符串的 blob。

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "myblob.txt"
    CloudBlockBlob blockBlob2 = container.GetBlockBlobReference("myblob.txt");

    string text;
    using (var memoryStream = new MemoryStream())
    {
        blockBlob2.DownloadToStream(memoryStream);
        text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
    }

## 删除 blob

若要删除某个 blob，先获取 blob 引用，然后在其上调用**Delete**方法。

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    blockBlob.Delete();


## 以异步方式列出网页中的 blob

如果列出了大量的 blob，或想要控制在一个列表操作返回的结果数，您可以列出在结果页中的 blob。 此示例演示如何异步，返回的结果页中，以便执行未被阻塞在等待返回大型结果集时。

本示例显示平面 blob 列出，但您还可以通过设置执行层次结构列表，`useFlatBlobListing`与**ListBlobsSegmentedAsync**方法的参数`false`。

该示例方法调用的异步方法，因为它必须开头`async`关键字，并且它必须返回的**Task**对象。 为**ListBlobsSegmentedAsync**方法指定了 await 关键字挂起执行示例方法直到列表任务完成。

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

## 追加 blob 写入

追加 blob 是 blob，版本中引入的一种新型 5.x 的.NET Azure 存储客户端库。 对于附加操作，如登录优化追加 blob。 像块 blob，追加 blob 组成的块，但至追加 blob 添加新块时，总是附加到 blob 的末尾。 您无法更新或删除现有块中追加 blob。 因为它们是为阻止 blob 不公开追加 blob 的模块 Id。 
 
在追加 blob 中的每个数据块可以是不同的大小，最多为 4 MB，并追加 blob 可以包含最多为 50000 块。 因此，追加 blob 的最大大小是略大于 195 GB （4 MB X 50000 块）。

下面的示例创建一个新的追加 blob，将一些数据追加到它，模拟一个简单的日志记录操作。

    //Parse the connection string for the storage account.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

    //Create service client for credentialed access to the Blob service.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    //Get a reference to a container.
    CloudBlobContainer container = blobClient.GetContainerReference("my-append-blobs");

    //Create the container if it does not already exist. 
    container.CreateIfNotExists();

    //Get a reference to an append blob.
    CloudAppendBlob appendBlob = container.GetAppendBlobReference("append-blob.log");

    //Create the append blob. Note that if the blob already exists, the CreateOrReplace() method will overwrite it.
    //You can check whether the blob exists to avoid overwriting it by using CloudAppendBlob.Exists().
    appendBlob.CreateOrReplace();

    int numBlocks = 10;

    //Generate an array of random bytes.
    Random rnd = new Random();
    byte[] bytes = new byte[numBlocks];
    rnd.NextBytes(bytes);
        
    //Simulate a logging operation by writing text data and byte data to the end of the append blob.
    for (int i = 0; i < numBlocks; i++)
    {
        appendBlob.AppendText(String.Format("Timestamp: {0} \tLog Entry: {1}{2}",
            DateTime.Now.ToUniversalTime().ToString(), bytes[i], Environment.NewLine));
    }

    //Read the append blob to the console window.
    Console.WriteLine(appendBlob.DownloadText());

有关 blob 的三种类型之间的差异的详细信息，请参阅[了解块 Blob、 页面 Blob 和追加的 Blob](https://msdn.microsoft.com/library/azure/ee691964.aspx) 。

## 下一步行动

现在，您已经学习了 blob 存储的基本知识，按照这些链接以了解更复杂的存储任务。
<ul>
<li>查看有关 Api 的完整详细信息的 Blob 服务参考文档︰ <ul>
    <li><a href="http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409">对于.NET 引用存储客户端库</a>
    </li>
    <li><a href="http://msdn.microsoft.com/library/azure/dd179355">REST API 参考</a></li>
  </ul>
</li>
<li>了解有关使用 Azure 存储在可执行更多高级任务<a href="http://msdn.microsoft.com/library/azure/gg433040.aspx">存储和访问数据 Azure</a>。</li>
<li>了解如何简化您编写能够通过使用 Azure 存储代码<a href="../websites-dotnet-webjobs-sdk/">Azure WebJobs SDK。</li>
<li>查看更多的功能指南以了解如何将数据存储在 Azure 中的其他选项。
  <ul>
    <li>使用<a href="/documentation/articles/storage-dotnet-how-to-use-tables/">表存储</a>来存储结构化的数据。</li>
    <li>使用<a href="/documentation/articles/storage-dotnet-how-to-use-queues/">队列存储</a>来存储非结构化的数据。</li>
    <li>使用<a href="/documentation/articles/sql-database-dotnet-how-to-use/">SQL 数据库</a>来存储关系数据。</li>
  </ul>
</li>
</ul>

  [Blob5]: ./media/storage-dotnet-how-to-use-blobs/blob5.png
  [Blob6]: ./media/storage-dotnet-how-to-use-blobs/blob6.png
  [Blob7]: ./media/storage-dotnet-how-to-use-blobs/blob7.png
  [Blob8]: ./media/storage-dotnet-how-to-use-blobs/blob8.png
  [Blob9]: ./media/storage-dotnet-how-to-use-blobs/blob9.png

  [Azure 存储]: http://msdn.microsoft.com/library/azure/gg433040.aspx
  [Azure 存储团队博客]: http://blogs.msdn.com/b/windowsazurestorage/
  [配置连接字符串]: http://msdn.microsoft.com/library/azure/ee758697.aspx
  [.NET 客户端库参考]: http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
  [REST API 参考]: http://msdn.microsoft.com/library/azure/dd179355
 
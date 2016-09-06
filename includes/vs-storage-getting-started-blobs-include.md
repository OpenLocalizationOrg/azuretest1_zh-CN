---
ms.openlocfilehash: 16d74ae85f8b3eeaa1abd124b7f18fe11c19202a
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
#####创建容器
正如文件居住在文件夹中，存储 blob 生存在容器中。 您可以使用**CloudBlobClient**对象引用现有容器，或者可以调用 CreateCloudBlobClient() 方法来创建一个新的容器。

下面的代码演示如何创建一个新的 blob 存储容器。 此代码首先创建一个**BlobClient**对象，以便您可以访问该对象的功能，如创建存储容器。 然后，代码将尝试引用存储容器，名为"mycontainer"。 如果它找不到具有该名称的容器，它会创建一个。

    // Create a blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Get a reference to a container named “mycontainer.”
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // If “mycontainer” doesn’t exist, create it.
    container.CreateIfNotExists();

默认情况下，新的容器是私有的您必须指定您的存储访问密钥下载此容器中的 blob。 如果您想要向所有人提供容器内的文件，您可以设置容器，以通过使用下面的代码是公共的。

    container.SetPermissions(
        new BlobContainerPermissions { PublicAccess = 
        BlobContainerPublicAccessType.Blob }); 


**注意︰**以下各节中使用此代码块代码的前面。

#####上载到的容器的 blob
以斑点文件上载到一个容器中，获取容器引用并使用它来获取 blob 引用。 Blob 引用之后，可以通过调用**UploadFromStream()**方法来上载所有的数据的流。 此操作将创建该 blob，如果尚不存在，或覆盖它，如果它不存在。 下面的示例演示如何将 blob 传到一个容器，并假定该容器已创建。

    // Get a reference to a blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");
    
    // Create or overwrite the "myblob" blob with the contents of a local file
    // named “myfile”.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        blockBlob.UploadFromStream(fileStream);
    }

#####列出容器中的 blob
若要列出容器中的 blob，先获取容器引用。 然后可以调用容器的**ListBlobs()**方法来检索 blob 和/或目录中。 若要访问丰富的属性和方法的返回**IListBlobItem**，必须将其转换为**CloudBlockBlob**、 **CloudPageBlob**或**CloudBlobDirectory**对象。 如果您不知道 blob 类型，您可以使用执行类型检查来确定要将其转换为。 下面的代码演示如何检索和输出的每一项名为"照片"容器中的 URI。

    // Get a reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("photos");

    // Loop through items in the container and output the length and URI for each 
    // item.
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

他人没有办法列出一个 blob 容器的内容。 详细信息，请参阅[如何使用 Blob 存储从.NET](../articles/storage/storage-dotnet-how-to-use-blobs.md/#list-blob) 。

#####下载一个 blob
若要下载一个 blob，先获取对该 blob 的引用，然后调用 DownloadToStream() 方法。 下面的示例使用 DownloadToStream() 方法将 blob 内容转移到一个流对象，然后您可以将其保存为本地文件。

    // Get a reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save the blob contents to a file named “myfile”.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        blockBlob.DownloadToStream(fileStream);
    }

有其他方法将 blob 另存为文件。 详细信息，请参阅[如何使用 Blob 存储从.NET](../articles/storage/storage-dotnet-how-to-use-blobs.md/#download-blobs) 。

#####删除一个 blob
若要删除某个 blob，先获取对该 blob 的引用，然后在其上调用 Delete() 方法。

    // Get a reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    blockBlob.Delete();

[了解更多关于 Azure 存储](http://azure.microsoft.com/documentation/services/storage/)请参阅[浏览服务器资源管理器中的存储资源](http://msdn.microsoft.com/library/azure/ff683677.aspx)。
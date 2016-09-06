---
ms.openlocfilehash: 34df629fe44d4476d4048559118fabc4f4baea4e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="如何使用 Java 从 Azure Blob 存储 |Microsoft Azure"
    description="了解如何使用 Azure Blob 存储上载、 下载、 列表，并删除 blob 内容。 用 Java 编写的示例。"
    services="storage"
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor="jimbe"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/31/2015" 
    ms.author="robmcm"/>

# 如何使用 Blob 存储从 Java

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

## 概述

这篇文章将说明如何执行常见使用 Microsoft Azure Blob 存储的方案。 这些示例用 Java 编写的并使用[Azure 存储 Java SDK][]。 所包含的方案包括**上传**、**列表**、**下载**和**删除**blob。 Blob 的详细信息，请参阅[后续步骤](#NextSteps)部分。

> [AZURE.NOTE] SDK 仅供开发人员使用 Azure 存储在 Android 设备上。 有关详细信息，请参阅[为 Android 的 Azure 存储 SDK][]。

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## 创建一个 Java 应用程序

在本文中，您将使用存储功能，可以在本地或在 web 角色或在 Azure 中的辅助角色中运行的代码中运行 Java 应用程序中。

若要执行此操作，您将需要安装 Java 开发工具箱 (JDK) 并在 Azure 订阅创建 Azure 存储帐户。 一旦您这样做了，您需要验证您开发的系统满足最低要求和相关性在 GitHub [Azure 存储 Java SDK][]存储库中列出了这些。 如果您的系统符合这些要求，您可以按照说明下载和安装 Java Azure 存储库，该存储库从您的系统上。 完成这些任务之后，您将能够创建一个 Java 应用程序，它使用本文中的示例。

## 配置应用程序以访问 Blob 存储

想要使用 Azure 存储 Api 来访问 blob 的 Java 文件的顶部添加以下的导入语句。

    // Include the following imports to use blob APIs.
    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.blob.*;

## 设置 Azure 存储连接字符串

Azure 存储客户端使用存储的连接字符串将存储终结点和凭据以访问数据管理服务。 客户端应用程序在运行时，您必须提供存储连接字符串以下面的格式，列出在 Azure 门户中的*帐户名*和*AccountKey*值的存储帐户使用您的存储帐户和主访问键的名称。 下面的示例演示如何声明一个静态字段来保存连接字符串。

    // Define the connection-string with your values
    public static final String storageConnectionString =
        "DefaultEndpointsProtocol=http;" +
        "AccountName=your_storage_account;" +
        "AccountKey=your_storage_account_key";

在 Microsoft Azure 角色中运行的应用程序，此字符串将存储在服务配置文件中， *ServiceConfiguration.cscfg*，并可以通过调用**RoleEnvironment.getConfigurationSettings**方法。 Followng 示例获取从**设置**元素名为*StorageConnectionString*的服务配置文件中的连接字符串。

    // Retrieve storage account from connection-string.
    String storageConnectionString =
        RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");

下面的示例假定您已经使用这两种方法之一来获取存储连接字符串。

## 创建容器

**CloudBlobClient**对象可以引用对象获取容器和 blob。 下面的代码创建一个**CloudBlobClient**对象。

> [AZURE.NOTE] 还有其他的方式来创建**CloudStorageAccount**对象;有关详细信息，请参阅**CloudStorageAccount** [Azure 存储客户端 SDK 参考]中。

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

使用**CloudBlobClient**对象来获取对您想要使用的容器的引用。 如果不存在与**createIfNotExists**方法，否则将返回现有的容器，您可以创建容器。 默认情况下，新的容器是私有的所以您必须指定您的存储访问密钥 （就像前面） 下载此容器中的 blob。

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

        // Create the blob client.
       CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

       // Get a reference to a container.
       // The container name must be lower case
       CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

       // Create the container if it does not exist.
        container.createIfNotExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

### 可选︰ 配置公共访问权限的容器

默认情况下，配置为专用访问容器的权限，但您可以轻松地配置容器的权限，以允许 Internet 上的所有用户的公共的只读访问︰

    // Create a permissions object.
    BlobContainerPermissions containerPermissions = new BlobContainerPermissions();

    // Include public access in the permissions object.
    containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);

    // Set the permissions on the container.
    container.uploadPermissions(containerPermissions);

## 上载到的容器的 blob

若要将文件上载到 blob，获取容器引用，并使用它来获取 blob 引用。 Blob 引用之后，可以上载任何流通过斑点引用上调用上载。 如果它不存在，或覆盖它，如果它存在，则此操作将创建该 blob。 下面的代码示例演示此操作，并假定已创建容器。

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

        // Create the blob client.
        CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

       // Retrieve reference to a previously created container.
        CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

        // Define the path to a local file.
        final String filePath = "C:\\myimages\\myimage.jpg";

        // Create or overwrite the "myimage.jpg" blob with contents from a local file.
        CloudBlockBlob blob = container.getBlockBlobReference("myimage.jpg");
        File source = new File(filePath);
        blob.upload(new FileInputStream(source), source.length());
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## 列出容器中的 blob

若要列出容器中的 blob，先获取容器引用一样要上载一个 blob。 您可以使用**for**循环使用容器的**listBlobs**方法。 下面的代码输出到控制台的容器中的每个 blob 的 Uri。

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

        // Create the blob client.
        CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

        // Retrieve reference to a previously created container.
        CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

        // Loop over blobs within the container and output the URI to each of them.
        for (ListBlobItem blobItem : container.listBlobs()) {
           System.out.println(blobItem.getUri());
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

Blob 服务具有容器，以及内目录的概念。 这样，您可以整理您更类似于文件夹结构中的 blob。

例如，可能有一个名为"照片"，可能在其中上载 blob，名为"rootphoto1"、"2010年/photo1"、"2010年/photo2"和"2011年/photo1"的容器。 这将创建的虚拟目录"2010"和"照片"容器内"2011"。 在"照片"容器调用**listBlobs**时，返回的集合将包含目录和包含在顶级的 blob 表示的**CloudBlobDirectory**和**CloudBlob**对象。 在这种情况下，将返回目录"2010"和"2011"，以及"rootphoto1"的照片。 您可以使用**instanceof**运算符区分这些对象。

（可选） 您可以将参数传递到**useFlatBlobListing**参数设置为 true 的**listBlobs**方法。 这将导致每个 blob 返回，而不考虑目录。 有关详细信息，请参阅**CloudBlobContainer.listBlobs** [Azure 存储客户端 SDK 参考]中。

## 下载一个 blob

若要下载 blob，按照同样的步骤，就像为了获取 blob 引用上载一个 blob。 在上载示例中，在 blob 对象上调用上载。 在以下示例中，调用下载将 blob 内容传输到 stream 对象，如**FileOutputStream** ，您可以使用来持久保存到本地文件 blob。

    try
    {
        // Retrieve storage account from connection-string.
       CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

       // Create the blob client.
       CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

       // Retrieve reference to a previously created container.
       CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

       // Loop through each blob item in the container.
       for (ListBlobItem blobItem : container.listBlobs()) {
           // If the item is a blob, not a virtual directory.
           if (blobItem instanceof CloudBlob) {
               // Download the item and save it to a file with the same name.
                CloudBlob blob = (CloudBlob) blobItem;
                blob.download(new FileOutputStream("C:\\mydownloads\\" + blob.getName()));
            }
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## 删除一个 blob

若要删除某个 blob，获取的 blob 引用，并调用**deleteIfExists**。

    try
    {
       // Retrieve storage account from connection-string.
       CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

       // Create the blob client.
       CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

       // Retrieve reference to a previously created container.
       CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

       // Retrieve reference to a blob named "myimage.jpg".
       CloudBlockBlob blob = container.getBlockBlobReference("myimage.jpg");

       // Delete the blob.
       blob.deleteIfExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## 删除一个 blob 容器

最后，若要删除一个 blob 容器，获取 blob 容器引用，并调用**deleteIfExists**。

    try
    {
       // Retrieve storage account from connection-string.
       CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

       // Create the blob client.
       CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

       // Retrieve reference to a previously created container.
       CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

       // Delete the blob container.
       container.deleteIfExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## 下一步行动

现在，您已经学习了 Blob 存储的基本知识，按照这些链接以了解更复杂的存储任务。

- [Azure 存储 Java SDK][]
- [Azure 存储客户端 SDK 参考][]
- [REST API，azure 存储][]
- [Azure 存储团队博客][]

[对于 Java 的 azure SDK]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure 存储 Java SDK]: https://github.com/azure/azure-storage-java
[Azure 存储为 Android SDK]: https://github.com/azure/azure-storage-android
[Azure 存储客户端 SDK 参考]: http://dl.windowsazure.com/storage/javadoc/
[REST API，azure 存储]: http://msdn.microsoft.com/library/azure/gg433040.aspx
[Azure 存储团队博客]: http://blogs.msdn.com/b/windowsazurestorage/

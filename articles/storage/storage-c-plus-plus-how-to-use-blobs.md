---
ms.openlocfilehash: d90302df909c32a0c3c55ec58d1461809c14f731
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何使用 blob 存储 （c + +） |Microsoft Azure" 
    description="了解如何在 Azure 中使用 blob 存储服务。 示例是用 c + + 编写的。" 
    services="storage" 
    documentationCenter=".net" 
    authors="tamram" 
    manager="adinah" 
    editor=""/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/19/2015" 
    ms.author="tamram"/>

# 如何使用 c + + 中的 Blob 存储  

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

## 概述
本指南将说明如何执行常见使用 Azure Blob 存储服务的方案。 这些示例用 c + + 编写和使用[c + + 的 Azure 存储客户端库](https://github.com/Azure/azure-storage-cpp/blob/v1.0.0/README.md)。 所包含的方案包括**上传**、**列表**、**下载**和**删除**blob。  

>[AZURE.NOTE] 本指南面向 Azure 存储客户端库，c + + 版本 1.0.0 和上面。 建议使用的版本是存储客户端库 1.0.0 版，可通过[NuGet](http://www.nuget.org/packages/wastorage)或[GitHub](https://github.com/Azure/azure-storage-cpp)。 

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]
[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## 创建一个 c + + 应用程序
在本指南中，您将使用存储功能，可以在 c + + 应用程序中运行。  

为此，将需要安装 c + + 的 Azure 存储客户端库和在 Azure 订阅创建 Azure 存储帐户。   

为 c + + 安装 Azure 存储客户端库，您可以使用以下方法︰

-   **Linux:**按照在[Azure 存储客户端库的 c + + 自述文件](https://github.com/Azure/azure-storage-cpp/blob/master/README.md)页中的说明进行操作。  
-   **窗口︰**在 Visual Studio 中，单击**工具 > NuGet 程序包管理器 > 程序包管理器控制台**。 [NuGet 程序包管理器控制台](http://docs.nuget.org/docs/start-here/using-the-package-manager-console)中键入以下命令，然后按**enter 键**。  

        Install-Package wastorage

## 配置应用程序以访问 Blob 存储  
添加以下语句，您希望使用 Azure 存储 Api 来访问 blob 的 c + + 文件的顶部︰  

    #include "was/storage_account.h"
    #include "was/blob.h"

## 安装程序将 Azure 存储连接字符串
Azure 存储客户端使用存储的连接字符串将存储终结点和凭据以访问数据管理服务。 在客户端应用程序运行时，您必须提供存储连接字符串以下面的格式，用于存储帐户的*帐户名*和*AccountKey*值管理门户中列出您的存储帐户和存储访问键的名称。 有关存储帐户和访问密钥的信息，请参阅[关于 Azure 存储帐户](storage-create-storage-account.md)。 本示例显示了如何声明一个静态字段来保存连接字符串︰  

    // Define the connection-string with your values.
    const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));

若要在您的本地 Windows 计算机中测试您的应用程序，您可以使用[Azure SDK](http://azure.microsoft.com/downloads/)安装 Microsoft Azure[存储仿真器](https://msdn.microsoft.com/library/azure/hh403989.aspx)。 存储仿真器是一个实用程序，模拟在 Azure 中本地开发计算机上可用的 Blob、 队列和表服务。 下面的示例演示如何声明一个静态字段来保存到本地存储仿真程序的连接字符串︰

    // Define the connection-string with Azure Storage Emulator.
    const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  

要启动 Azure 存储仿真程序，请选择**开始**按钮或按**Windows**键。 开始键入**Azure 存储仿真程序**，并从应用程序的列表中选择**Microsoft Azure 存储仿真器**。  

下面的示例假定您已经使用这两种方法之一来获取存储连接字符串。  

## 检索连接字符串
您可以使用**cloud_storage_account**类来表示您的存储帐户信息。 若要检索您的存储帐户信息从存储连接字符串，可以使用**parse**方法。  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

下一步，获得对**cloud_blob_client**类的引用，因为它允许您检索表示容器和 blob 存储 Blob 存储服务中的对象。 下面的代码创建一个**cloud_blob_client**对象，该对象使用上面我们检索存储帐户对象︰  

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();  

## 如何︰ 创建容器

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

此示例演示如何创建一个容器，如果不存在︰  

    try 
    {
        // Retrieve storage account from connection string.
        azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

        // Create the blob client.
        azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

        // Retrieve a reference to a container.
        azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

        // Create the container if it doesn't already exist.
        container.create_if_not_exists();
    }
    catch (const std::exception& e)
    {
        std::wcout << U("Error: ") << e.what() << std::endl;
    }  

默认情况下，新的容器是私有的您必须指定您的存储访问密钥下载此容器中的 blob。 如果您想要使容器内 (blob) 文件可供每个人，您可以设置容器是公共使用以下代码︰  

    // Make the blob container publicly accessible.
    azure::storage::blob_container_permissions permissions;
    permissions.set_public_access(azure::storage::blob_container_public_access_type::blob);
    container.upload_permissions(permissions);  

在 Internet 上的任何人都可以看到 blob 在公共容器中，但您可以修改或删除它们，只有具有适当的访问键。  

## 如何︰ 上载到的容器的 blob
Azure Blob 存储支持块 blob 和页面 blob。 在大多数情况下，块斑点是推荐使用的类型。  

若要将文件上载到块斑点，获取容器引用，并使用它来获取块斑点引用。 Blob 引用之后，可以通过调用**upload_from_stream**方法来上载所有的数据的流。 如果它以前不存在，或者覆盖它，如果它不存在，则此操作将创建该 blob。 下面的示例演示如何将 blob 传到一个容器，并假定该容器已创建。  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Retrieve reference to a blob named "my-blob-1".
    azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

    // Create or overwrite the "my-blob-1" blob with contents from a local file.
    concurrency::streams::istream input_stream = concurrency::streams::file_stream<uint8_t>::open_istream(U("DataFile.txt")).get();
    blockBlob.upload_from_stream(input_stream);
    input_stream.close().wait();

    // Create or overwrite the "my-blob-2" and "my-blob-3" blobs with contents from text.
    // Retrieve a reference to a blob named "my-blob-2".
    azure::storage::cloud_block_blob blob2 = container.get_block_blob_reference(U("my-blob-2"));
    blob2.upload_text(U("more text"));

    // Retrieve a reference to a blob named "my-blob-3".
    azure::storage::cloud_block_blob blob3 = container.get_block_blob_reference(U("my-directory/my-sub-directory/my-blob-3"));
    blob3.upload_text(U("other text"));  

或者，可以使用**upload_from_file**方法将文件上载到块斑点。

## 如何︰ 列出容器中的 blob
若要列出容器中的 blob，先获取容器引用。 然后可以使用容器的**list_blobs**方法来检索 blob 和/或目录中。 若要访问的属性和方法返回**list_blob_item**的丰富，则必须调用**list_blob_item.as_blob**方法来获取一个**cloud_blob**或**list_blob.as_directory**方法来获取一个 cloud_blob_directory 对象。 下面的代码演示如何检索和输出**我样本容器**容器中的每一项的 URI:

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Output URI of each item.
    azure::storage::list_blob_item_iterator end_of_results;
    for (auto it = container.list_blobs(); it != end_of_results; ++it)
    {
        if (it->is_blob())
        {
            std::wcout << U("Blob: ") << it->as_blob().uri().primary_uri().to_string() << std::endl;
        }
        else
        {
            std::wcout << U("Directory: ") << it->as_directory().uri().primary_uri().to_string() << std::endl;
        }
    }

其中列出操作的详细信息，请参阅[列表 c + + 中的 Azure 存储资源](storage-c-plus-plus-enumeration.md)。

## 如何︰ 下载 blob
若要下载 blob，首先检索 blob 引用，然后调用**download_to_stream**方法。 下面的示例使用**download_to_stream**方法将 blob 内容转移到一个流对象，然后可以保存到一个本地文件。  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Retrieve reference to a blob named "my-blob-1".
    azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

    // Save blob contents to a file.
    concurrency::streams::container_buffer<std::vector<uint8_t>> buffer;
    concurrency::streams::ostream output_stream(buffer);
    blockBlob.download_to_stream(output_stream);

    std::ofstream outfile("DownloadBlobFile.txt", std::ofstream::binary);
    std::vector<unsigned char>& data = buffer.collection();
        
    outfile.write((char *)&data[0], buffer.size());
    outfile.close();  

或者，可以使用**download_to_file**方法来 blob 的内容下载到文件。
此外，您还可以使用**download_text**方法要下载的内容作为文本字符串的 blob。  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Retrieve reference to a blob named "my-blob-2".
    azure::storage::cloud_block_blob text_blob = container.get_block_blob_reference(U("my-blob-2"));

    // Download the contents of a blog as a text string.
    utility::string_t text = text_blob.download_text();

## 如何︰ 删除 blob
若要删除某个 blob，先获取 blob 引用，然后在其上调用**delete_blob**方法。  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Retrieve reference to a blob named "my-blob-1".
    azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

    // Delete the blob.
    blockBlob.delete_blob();

## 下一步行动
现在，您已经学习了 blob 存储的基本知识，按照这些链接以了解更多关于 Azure 存储。  

-   [如何使用 c + + 中的队列存储](storage-c-plus-plus-how-to-use-queues.md)
-   [如何使用 c + + 中的表存储](storage-c-plus-plus-how-to-use-tables.md)
-   [在 c + + 的列表 Azure 存储资源](storage-c-plus-plus-enumeration.md)
-   [对于 c + + 引用存储客户端库](http://azure.github.io/azure-storage-cpp)
-   [Azure 存储文档](http://azure.microsoft.com/documentation/services/storage/)




 

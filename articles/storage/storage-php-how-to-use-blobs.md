---
ms.openlocfilehash: 0ea1c0f510c3611ccd513da894fee0d74353a20e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="如何使用 PHP 中的 blob 存储 |Microsoft Azure"
    description="了解如何使用 Azure blob 服务上载、 列表、 下载和删除 blob。 代码示例是用 PHP 编写的。"
    documentationCenter="php"
    services="storage"
    authors="tfitzmac"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="09/01/2015"
    ms.author="tomfitz"/>

# 如何使用 PHP 中的 blob 存储

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

## 概述

本指南介绍了如何执行使用 Azure blob 服务的常见方案。 示例用 PHP 编写的并使用[php 的 Azure SDK] [下载]。 所包含的方案包括**上传**、**列表**、**下载**和**删除**blob。 Blob 的详细信息，请参阅[后续步骤](#next-steps)部分。

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## 创建一个 PHP 应用程序

创建 PHP 应用程序访问 Azure blob 服务唯一的要求是引用 Azure SDK 中的类从 php 代码中。 任何开发工具可用于创建您的应用程序，其中包括记事本。

在本指南中，您将使用服务功能，可以在本地或在 Azure web 角色、 工作人员角色或网站中运行的代码中调用 PHP 应用程序中。

## 获取 Azure 的客户端库

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## 配置应用程序以访问 blob 服务

若要使用 Azure blob 服务 Api，您需要︰

1. 引用使用[require_once][require_once]语句的自动磁带加载机文件和
2. 参考您可以使用任何类。

下面的示例演示如何引用**ServicesBuilder**类包含自动磁带加载机文件。

> [AZURE.NOTE] 本示例 （和本文中的其他示例） 假定安装了 Azure 作曲家通过 PHP 客户端库。 如果您已安装的手动或梨树打包库，您需要引用`WindowsAzure.php`自动磁带加载机文件。

    require_once 'vendor\autoload.php';
    use WindowsAzure\Common\ServicesBuilder;


在下面示例中，`require_once`语句将显示始终，但仅执行该示例所需的类引用。

## 设置 Azure 存储连接

要实例化的 Azure blob 服务客户端，首先必须具有有效的连接字符串。 Blob 服务连接字符串的格式是︰

用于访问实时服务︰

    DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]

用于访问存储仿真程序︰

    UseDevelopmentStorage=true


若要创建任何 Azure 服务客户端，您需要使用**ServicesBuilder**类。 您可以：

* 直接给它传递连接字符串或
* 使用**CloudConfigurationManager (CCM)**检查多个外部源的连接字符串︰
    * 默认情况下，它配备有一个外部来源的环境变量中的支持。
    * 通过扩展的**ConnectionStringSource**类，您可以添加新的源。

对于此处列出的示例中，将直接传递连接字符串。

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;

    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);

## 创建容器

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

**BlobRestProxy**对象，可以使用**createContainer**方法创建一个 blob 容器。 在创建容器时，可以设置选项容器，但这样做很不必要。 （下面的示例显示如何设置容器的访问控制列表 (ACL) 和容器的元数据）。

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use WindowsAzure\Blob\Models\CreateContainerOptions;
    use WindowsAzure\Blob\Models\PublicAccessType;
    use WindowsAzure\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    // OPTIONAL: Set public access policy and metadata.
    // Create container options object.
    $createContainerOptions = new CreateContainerOptions();

    // Set public access policy. Possible values are
    // PublicAccessType::CONTAINER_AND_BLOBS and PublicAccessType::BLOBS_ONLY.
    // CONTAINER_AND_BLOBS:
    // Specifies full public read access for container and blob data.
    // proxys can enumerate blobs within the container via anonymous
    // request, but cannot enumerate containers within the storage account.
    //
    // BLOBS_ONLY:
    // Specifies public read access for blobs. Blob data within this
    // container can be read via anonymous request, but container data is not
    // available. proxys cannot enumerate blobs within the container via
    // anonymous request.
    // If this value is not specified in the request, container data is
    // private to the account owner.
    $createContainerOptions->setPublicAccess(PublicAccessType::CONTAINER_AND_BLOBS);

    // Set container metadata.
    $createContainerOptions->addMetaData("key1", "value1");
    $createContainerOptions->addMetaData("key2", "value2");

    try {
        // Create container.
        $blobRestProxy->createContainer("mycontainer", $createContainerOptions);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

调用**setPublicAccess (PublicAccessType::CONTAINER\_AND\_BLOB)**使容器和 blob 数据可通过匿名请求访问。 调用**setPublicAccess(PublicAccessType::BLOBS_ONLY)**使仅 blob 数据可通过匿名请求访问。 有关容器 Acl 的详细信息，请参阅[设置容器 ACL (REST API)][容器 acl]。

有关 Blob 服务错误代码的详细信息，请参阅[Blob 服务错误代码][的错误代码]。

## 上载到的容器的 blob

若要上载一个文件作为一个 blob，使用的**BlobRestProxy-> createBlockBlob**方法。 此操作将创建 blob，如果它不存在，或者如果它将覆盖它。 下面的代码示例假定该容器已创建，并使用[fopen][fopen]打开该文件以流的形式。

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use WindowsAzure\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    $content = fopen("c:\myfile.txt", "r");
    $blob_name = "myblob";

    try {
        //Upload blob
        $blobRestProxy->createBlockBlob("mycontainer", $blob_name, $content);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

请注意上面的示例上载流的形式的 blob。 但是，一个 blob 可以还上载作为字符串的使用，例如，[文件\_获得\_内容][file_get_contents]函数。 若要执行此操作使用上面的示例中，将更改`$content = fopen("c:\myfile.txt", "r");`到`$content = file_get_contents("c:\myfile.txt");`。

## 列出容器中的 blob

若要列出容器中的 blob，使用**foreach**循环来遍历结果的**BlobRestProxy-> listBlobs**方法。 下面的代码作为输出容器中显示的每个 blob 名称，并在浏览器中显示它的 URI。

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use WindowsAzure\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    try {
        // List blobs.
        $blob_list = $blobRestProxy->listBlobs("mycontainer");
        $blobs = $blob_list->getBlobs();

        foreach($blobs as $blob)
        {
            echo $blob->getName().": ".$blob->getUrl()."<br />";
        }
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }


## 下载一个 blob

若要下载一个 blob，调用**BlobRestProxy-> getBlob**方法中，然后生成的**GetBlobResult**对象上调用**getContentStream**方法。

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use WindowsAzure\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    try {
        // Get blob.
        $blob = $blobRestProxy->getBlob("mycontainer", "myblob");
        fpassthru($blob->getContentStream());
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

请注意，上面的示例中获取 blob 作为流资源 （默认行为）。 但是，您可以使用[流\_获得\_内容][流获取内容]函数将返回的数据流转换为字符串。

## 删除一个 blob

若要删除某个 blob，请向**BlobRestProxy-> deleteBlob**传递容器名称和 blob 名称。

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use WindowsAzure\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    try {
        // Delete container.
        $blobRestProxy->deleteBlob("mycontainer", "myblob");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## 删除一个 blob 容器

最后，若要删除一个 blob 容器，容器名称传递到**BlobRestProxy-> deleteContainer**。

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use WindowsAzure\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    try {
        // Delete container.
        $blobRestProxy->deleteContainer("mycontainer");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## 下一步行动

现在，学 Azure blob 服务的基本知识，按照这些链接以了解更复杂的存储任务。

- 请参阅 MSDN 参考︰ [Azure 存储](http://msdn.microsoft.com/library/azure/gg433040.aspx)
- 访问[Azure 存储团队博客](http://blogs.msdn.com/b/windowsazurestorage/)
- <Https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/BlockBlobExample.php>的 PHP 块 blob 示例，请参阅。
- <Https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/PageBlobExample.php>的 PHP 页面 blob 示例，请参阅

[下载]: http://go.microsoft.com/fwlink/?LinkID=252473
[存储和访问 Azure 中的数据]: http://msdn.microsoft.com/library/azure/gg433040.aspx
[容器 acl]: http://msdn.microsoft.com/library/azure/dd179391.aspx
[错误代码]: http://msdn.microsoft.com/library/azure/dd179439.aspx
[file_get_contents]: http://php.net/file_get_contents
[require_once]: http://php.net/require_once
[fopen]: http://www.php.net/fopen
[流获取内容]: http://www.php.net/stream_get_contents

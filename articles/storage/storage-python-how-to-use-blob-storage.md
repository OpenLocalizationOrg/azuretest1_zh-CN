---
ms.openlocfilehash: 3d24cdd81bdfce7a264c71ad21168f87f6022506
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="如何使用 Python 从 Azure Blob 存储 |Microsoft Azure"
    description="了解如何使用 Python 的 Azure Blob 存储上载、 列表、 下载，并删除 blob。"
    services="storage"
    documentationCenter="python"
    authors="emgerner-msft"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="08/25/2015"
    ms.author="emgerner"/>

# 如何使用 Python 从 Azure Blob 存储

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

## 概述

这篇文章将说明如何执行常见使用 Blob 存储的方案。 这些示例用 Python 编写和使用[Python Azure 存储软件包][]。 所包含的方案包括上传、 列表、 下载和删除 blob。

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## 创建容器

> [AZURE.NOTE] 如果您需要安装 Python 或者[Python Azure 包][]，请参阅[Python 安装指南 》](../python-how-to-install.md)。

**BlobService**对象使您可以使用容器和 blob。 下面的代码创建一个**BlobService**对象。 添加您希望以编程方式访问 Azure 存储任何 Python 文件的顶部附近以下内容。

    from azure.storage.blob import BlobService

下面的代码创建一个**BlobService**对象，该对象使用存储帐户的用户名和帐户密码。  真正的帐户和密钥替换我的帐户和 mykey 派生而来。

    blob_service = BlobService(account_name='myaccount', account_key='mykey')

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

在下面的代码示例中，可以使用**BlobService**对象来创建的容器，如果它不存在。

    blob_service.create_container('mycontainer')

默认情况下，新的容器是私有的所以您必须指定您的存储访问密钥 （就像前面） 下载此容器中的 blob。 如果您想要向所有人提供容器内的文件，您可以创建的容器并通过公共访问级别，使用下面的代码。

    blob_service.create_container('mycontainer', x_ms_blob_public_access='container')

或者，您可以修改容器创建使用下面的代码之后。

    blob_service.set_container_acl('mycontainer', x_ms_blob_public_access='container')

这种更改之后, 在 Internet 上的任何人都可以看到公共容器中的 blob 但只有您可以修改或删除它们。

## 上载到的容器的 blob

若要将数据上载到 blob，使用**将\_块\_blob\_从\_路径**，**放\_块\_blob\_从\_文件**，**放\_块\_blob\_从\_字节**或**放入\_块\_blob\_从\_文本**方法。 它们是执行必要的分块的数据的大小超过 64 MB 时的高级方法。

**放入\_块\_blob\_从\_路径**上载的内容从指定的路径、 文件和**放入\_块\_blob\_从\_文件**上载从已打开的文件/流的内容。 **放入\_块\_blob\_从\_字节**上载的字节数组和**放入\_块\_blob\_从\_文本**上载指定的文本值使用指定的编码 （默认为 utf-8）。

下面的示例将**sunset.png**文件的内容上载到**myblob**斑点。

    blob_service.put_block_blob_from_path(
        'mycontainer',
        'myblob',
        'sunset.png',
        x_ms_blob_content_type='image/png'
    )

## 列出容器中的 blob

若要列出容器中的 blob，使用**列表\_blob**方法。 每次调用**列表\_blob**将返回结果的一段。 若要获取所有结果，请检查**下一步\_标记**结果和调用**列表\_blob**再次需要。 下面的代码输出到控制台的容器中的每个 blob**名称**。

    blobs = []
    marker = None
    while True:
        batch = blob_service.list_blobs('mycontainer', marker=marker)
        blobs.extend(batch)
        if not batch.next_marker:
            break
        marker = batch.next_marker
    for blob in blobs:
        print(blob.name)

## 下载 blob

结果每一段可以包含可变数目的最多 5000 个 blob。 如果**下\_标记**存在的特定片段，可能有多个 blob 容器中。

若要将数据从一个 blob 下载，使用**获得\_blob\_到\_路径**，**获得\_blob\_到\_文件**，**获得\_blob\_到\_字节**，或**获得\_blob\_到\_文本**。 它们是执行必要的分块的数据的大小超过 64 MB 时的高级方法。

下面的示例演示如何使用**获得\_blob\_到\_路径**下载**myblob** blob 的内容并将其存储到**出 sunset.png**文件。

    blob_service.get_blob_to_path('mycontainer', 'myblob', 'out-sunset.png')

## 删除一个 blob

最后，若要删除某个 blob，调用**delete_blob**。

    blob_service.delete_blob('mycontainer', 'myblob')

## 下一步行动

现在，您已经学习了 Blob 存储的基本知识，按照这些链接以了解更复杂的存储任务。

-   请参阅 MSDN 参考︰[在 Azure 存储和访问数据][]
-   访问[Azure 存储团队博客][]

[存储和访问 Azure 中的数据]: http://msdn.microsoft.com/library/azure/gg433040.aspx
[Azure 存储团队博客]: http://blogs.msdn.com/b/windowsazurestorage/
[Python Azure 包]: https://pypi.python.org/pypi/azure
[Python Azure 存储软件包]: https://pypi.python.org/pypi/azure-storage  

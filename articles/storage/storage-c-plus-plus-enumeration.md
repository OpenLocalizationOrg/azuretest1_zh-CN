---
ms.openlocfilehash: ee5a1607c61e122998851ddef95d9f8b6e8a0f9e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="列出 c + + 的 Azure 存储资源与 Microsoft Azure 存储客户端库 |Microsoft Azure" 
    description="了解如何使用 c + + 的 Microsoft Azure 存储客户端库中的列表 Api 枚举容器、 blob、 队列、 表和实体。" 
    documentationCenter=".net" 
    services="storage"
    authors="tamram" 
    manager="carolz" 
    editor=""/>
<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/19/2015" 
    ms.author="zhimingyuan;tamram"/>

# 在 c + + 的列表 Azure 存储资源

列表操作是使用 Azure 存储许多开发方案的关键。 本文介绍如何最有效地使用该列表在 Microsoft Azure 存储客户端库提供 c + + Api 的 Azure 存储中的对象。

>[AZURE.NOTE] 本指南针对 c + + 版本 Azure 存储客户端库 1.x、 通过[NuGet](http://www.nuget.org/packages/wastorage)或[GitHub](https://github.com/Azure/azure-storage-cpp)可用。

存储客户端库提供了多种方法对 Azure 存储中的列表或查询对象。 本文介绍了以下情况︰

-   在帐户列表容器
-   容器或 blob 虚拟目录中的列表 blob
-   在帐户列表队列
-   在帐户列表中表
-   表中的查询实体

每一种方法是显示为不同的方案中使用不同的重载。

## 同步与异步

存储客户端库的 c + + 基于[其他 c + + 库 （项目卡萨布兰卡）](http://casablanca.codeplex.com/)，因为我们本质上是通过使用[pplx::task](http://microsoft.github.io/cpprestsdk/classpplx_1_1task.html)支持异步操作。 例如︰

    pplx::task<list_blob_item_segment> list_blobs_segmented_async(continuation_token& token) const;

同步操作包装相应的异步操作︰

    list_blob_item_segment list_blobs_segmented(const continuation_token& token) const
    {
        return list_blobs_segmented_async(token).get();
    }

如果您正在使用多个线程的应用程序或服务，我们建议您使用异步 Api 直接而不是创建一个线程来调用同步 Api，这将显著影响您的性能。

## 分段的列表

云存储的规模需要分段的列表。 例如，可以有一百万个 blob Azure blob 容器中或十亿美元 Azure 表中的图元。 这些不是理论的数字，但真正的客户使用情况。

因此是不切实际列出单个响应中的所有对象。 相反，您可以列出使用分页的对象。 每个列表 Api 有*分段*重载。

分段的列表操作的响应包括︰

-   <i>_segment</i>，其中包含的单个列表 API 调用的返回结果集。 
-   *continuation_token*，它将被传递给下一个调用以获得结果的下一页。 当没有更多要返回的结果时，继续标记为空。

例如，若要列出所有 blob 容器中的典型调用可能类似于下面的代码段。 代码是在我们[的示例](https://github.com/Azure/azure-storage-cpp/blob/master/Microsoft.WindowsAzure.Storage/samples/BlobsGettingStarted/Application.cpp)中可用︰

    // List blobs in the blob container
    azure::storage::continuation_token token;
    do
    {
        azure::storage::list_blob_item_segment segment = container.list_blobs_segmented(token);
        for (auto it = segment.results().cbegin(); it != segment.results().cend(); ++it)
    {
        if (it->is_blob())
        {
            process_blob(it->as_blob());
        }
        else
        {
            process_diretory(it->as_directory());
        }
    }
    
        token = segment.continuation_token();
    }
    while (!token.empty());

请注意，在页中返回的结果数可由控制参数*max_results*中的每个 API，例如︰
    
    list_blob_item_segment list_blobs_segmented(const utility::string_t& prefix, bool use_flat_blob_listing, 
        blob_listing_details::values includes, int max_results, const continuation_token& token, 
        const blob_request_options& options, operation_context context)

如果不指定*max_results*参数，达 5000 结果的默认最大值则返回在单个页面中。

此外请注意对 Azure 表存储的查询可能会返回任何记录或较少比*max_results*参数指定的值的记录，即使继续标记不是空。 一个原因可能是该查询无法在 5 秒钟内完成。 只要继续标记不是空的应该继续查询，并且您的代码不应假定段结果的大小。

大多数情况下建议的编码模式被分段列出，它提供明确进度列表或查询，请与该服务对每个请求的响应方式。 对于 c + + 应用程序或服务，尤其是较低级别控制的列表进行有助于控制内存和性能。

## 贪婪的列表

早期版本的 c + + 存储客户端库 (预览版本 0.5.0 或更早) 包含的表和队列，如下面的示例中所示的非分段列表 Api:

    std::vector<cloud_table> list_tables(const utility::string_t& prefix) const;
    std::vector<table_entity> execute_query(const table_query& query) const;
    std::vector<cloud_queue> list_queues() const;

这些方法实现为分段的 Api 的包装。 分段列出每个响应，代码附加到一个向量的结果和完整的容器被扫描后返回的所有结果。

当存储帐户或表包含少量的对象时，此方法可能起作用。 但是，随着对象的数目增加，所需的内存可能会增加无限制，因为所有结果都保留在内存中。 一个列表操作可能需要很长时间，在此期间调用方具有任何有关其进度信息。 

这些贪婪的列表 Api 在 SDK 中并不存在于 C#、 Java 或 JavaScript Node.js 环境。 若要避免使用这些贪婪的 Api 的潜在的问题，我们这些版本中移除 0.6.0 预览。

如果您的代码正在调用这些贪婪的 Api:

    std::vector<azure::storage::table_entity> entities = table.execute_query(query);
    for (auto it = entities.cbegin(); it != entities.cend(); ++it)
    {
        process_entity(*it);
    }

然后您应该修改代码以使用该分段的列表 Api:

    azure::storage::continuation_token token;
    do
    {
        azure::storage::table_query_segment segment = table.execute_query_segmented(query, token);
        for (auto it = segment.results().cbegin(); it != segment.results().cend(); ++it)
        {
            process_entity(*it);
        }
    
        token = segment.continuation_token();
    } while (!token.empty());

通过指定段的*max_results*参数，您可以请求数和内存使用量，以满足您的应用程序的性能注意事项之间进行平衡。

此外，如果您正在使用分段的列表 Api，但本地的"贪婪"的样式集合中存储的数据，我们还强烈建议您重构您的代码以处理将数据存储在本地集合仔细在规模较大。

## 惰性的列表

尽管贪婪清单引发潜在的问题，这样就方便如果在容器中没有太多的对象。

如果您还使用 C# 或 Oracle Java Sdk，您应该熟悉的可枚举的编程模型，提供惰性的样式列表，在其中某些偏移量仅提取数据是否需要。 C + + 中的基于迭代器的模板还提供了一种类似的方法。

典型的懒惰列表 API，以**list_blobs**为例，如下所示︰

    list_blob_item_iterator list_blobs() const;

使用惰性列表模式的典型代码段如下所示︰

    // List blobs in the blob container
    azure::storage::list_blob_item_iterator end_of_results;
    for (auto it = container.list_blobs(); it != end_of_results; ++it)
    {
        if (it->is_blob())
        {
            process_blob(it->as_blob());
        }
        else
        {
            process_directory(it->as_directory());
        }
    }

请注意，惰性列表仅在同步模式下可用。

与贪婪的列表相比，惰性列表提取数据仅在必要时。 在内部，它提取数据从 Azure 存储只有在下一次迭代器移到下一段时。 因此，内存使用量控制有限的大小，而且操作非常快。

惰性列表 Api 包含在存储客户端库 c + + 版本 1.0.0 版中。 

## 结论

在本文中，我们讨论了不同的重载的 c + + 客户端存储库中的各种对象列表 Api。 总结︰

-   在多线程方案中，强烈建议异步 Api。
-   在大多数情况下建议分段的列表。
-   惰性列表同步方案中方便的包装库中提供。
-   贪婪的列表不推荐使用，并已从库中删除。

##下一步行动

C + + Azure 存储和客户端库的详细信息，请参阅以下资源。

-   [如何使用 c + + 中的 Blob 存储](storage-c-plus-plus-how-to-use-blobs.md)
-   [如何使用 c + + 中的表存储](storage-c-plus-plus-how-to-use-tables.md)
-   [如何使用 c + + 中的队列存储](storage-c-plus-plus-how-to-use-queues.md)
-   [C + + API 文档 azure 存储的客户端库。](http://azure.github.io/azure-storage-cpp/)
-   [Azure 存储团队博客](http://blogs.msdn.com/b/windowsazurestorage/)
-   [Azure 存储文档](http://azure.microsoft.com/documentation/services/storage/)

---
ms.openlocfilehash: 3b3a537654fbe2ca5fe230f73ad803463ea39894
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="开始使用 Azure 表存储和 Visual Studio 连接服务 |Microsoft Azure"
    description="如何开始使用 Azure 表存储在 ASP.NET 项目在 Visual Studio 中"
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

# 开始使用 Azure 表存储和 Visual Studio 连接服务

> [AZURE.SELECTOR]
> - [入门教程](vs-storage-aspnet-getting-started-tables.md)
> - [发生了什么事](vs-storage-aspnet-what-happened.md)

> [AZURE.SELECTOR]
> - [Blob](vs-storage-aspnet-getting-started-blobs.md)
> - [队列](vs-storage-aspnet-getting-started-queues.md)
> - [表](vs-storage-aspnet-getting-started-tables.md)

## 概述
本文介绍了如何开始使用 Azure 表存储在 Visual Studio 中，您创建或通过使用 Visual Studio**中添加连接服务**对话框引用 ASP.NET 项目的 Azure 存储帐户后。 本文介绍了如何在 Azure 表，包括创建和删除表，以及使用表实体执行的常见任务。 用 c 语言编写的示例\#的代码并使用[Azure 存储.NET 客户端库](https://msdn.microsoft.com/library/azure/dn261237.aspx)。 有关使用 Azure 的更多常规信息表存储中，请参阅[如何使用.NET 中的表存储](storage-dotnet-how-to-use-tables.md)。

Azure 表存储使您能够存储大量的结构化数据。 该服务是接受来自通过身份验证的调用的内部和外部 Azure 云 NoSQL 数据存储。 Azure 表非常适合于结构化、 非关系型数据存储。


## 在代码中访问表

1. 请确保 C# 文件顶部的命名空间声明包含这些`using`语句。

         using Microsoft.Azure;
         using Microsoft.WindowsAzure.Storage;
         using Microsoft.WindowsAzure.Storage.Auth;
         using Microsoft.WindowsAzure.Storage.Table;

2. 获得`CloudStorageAccount`对象，它表示您存储的帐户信息。 下面的代码用于获取您的存储连接字符串和从 Azure 服务配置的存储帐户信息。

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

    **注意**--在下面的示例使用所有上面的代码，该代码的前面。

3. 获得`CloudTableClient`来引用您的存储帐户中的表对象的对象。  

        // Create the table client.
        CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

4. 获得`CloudTable`来引用特定的表并使用实体的引用对象。

        // Get a reference to a table named "peopleTable"
        CloudTable table = tableClient.GetTableReference("peopleTable");

## 在代码中创建一个表

若要创建 Azure 表，只需添加对的调用`CreateIfNotExistsAsync()`与上面的代码。

    // Create the CloudTable if it does not exist
    await table.CreateIfNotExistsAsync();


## 插入一批的实体

入的表中的一个写操作，您可以插入多个实体。 下面的代码示例创建两个实体对象 （Jeff Smith 和 Ben Smith），将它们添加到`TableBatchOperation`对象使用 Insert 方法，，然后开始操作，通过调用`CloudTable.ExecuteBatchAsync`。

    // Get a reference to a CloudTable object named 'peopleTable' as described in "Access a table in code"

    // Create the batch operation.
    TableBatchOperation batchOperation = new TableBatchOperation();

    // Create a customer entity and add it to the table.
    CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
    customer1.Email = "Jeff@contoso.com";
    customer1.PhoneNumber = "425-555-0104";

    // Create another customer entity and add it to the table.
    CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
    customer2.Email = "Ben@contoso.com";
    customer2.PhoneNumber = "425-555-0102";

    // Add both customer entities to the batch insert operation.
    batchOperation.Insert(customer1);
    batchOperation.Insert(customer2);

    // Execute the batch operation.
    await peopleTable.ExecuteBatchAsync(batchOperation);

## 获取在一个分区中的所有实体
要查询的表的所有分区中的实体，请使用`TableQuery`对象。 下面的代码示例指定为 Smith 所在的分区键的实体的筛选器。 本示例打印到控制台查询结果中的每个实体的字段。

    // Get a reference to a CloudTable object named 'peopleTable' as described in "Access a table in code"

    // Construct the query operation for all customer entities where PartitionKey="Smith".
    TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>().Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

    // Print the fields for each customer.
    TableContinuationToken token = null;
    do
    {
        TableQuerySegment<CustomerEntity>
        resultSegment = await peopleTable.ExecuteQuerySegmentedAsync(query, token);
        token = resultSegment.ContinuationToken;

        foreach (CustomerEntity entity in resultSegment.Results)
        {
        Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
        entity.Email, entity.PhoneNumber);
        }
        } while (token != null);

        return View();


## 获取单个实体
您可以编写一个查询，以获取特定的一个独立的实体。 下面的代码使用`TableOperation`对象以指定的名为 Ben Smith 客户。 此方法返回的只是一个实体，而不是集合，并且在将返回的值`TableResult.Result`是`CustomerEntity`对象。 在查询中指定分区和行键是从表服务中检索单个实体的最快方法。

        // Get a reference to a CloudTable object named 'peopleTable' as described in "Access a table in code"

        // Create a retrieve operation that takes a customer entity.
        TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the retrieve operation.
    TableResult retrievedResult = await peopleTable.ExecuteAsync(retrieveOperation);

    // Print the phone number of the result.
    if (retrievedResult.Result != null)
       Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
    else
       Console.WriteLine("The phone number could not be retrieved.");

## 删除实体
找到它之后，您可以删除某个实体。 下面的代码中寻找名为"Ben Smith 的客户实体，如果它发现它，它会删除它。

    // Get a reference to a CloudTable object named 'peopleTable' as described in "Access a table in code"

    // Create a retrieve operation that expects a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = peopleTable.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity object.
    CustomerEntity deleteEntity = (CustomerEntity)retrievedResult.Result;

    // Create the Delete TableOperation and then execute it.
    if (deleteEntity != null)
    {
       TableOperation deleteOperation = TableOperation.Delete(deleteEntity);

       // Execute the operation.
       await peopleTable.ExecuteAsync(deleteOperation);

       Console.WriteLine("Entity deleted.");
    }

    else
       Console.WriteLine("Couldn't delete the entity.");

## 下一步行动

[AZURE.INCLUDE [vs-storage-dotnet-tables-next-steps](../../includes/vs-storage-dotnet-tables-next-steps.md)]

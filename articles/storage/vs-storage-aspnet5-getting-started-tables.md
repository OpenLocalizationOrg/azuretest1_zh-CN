---
ms.openlocfilehash: a2452b61af0be6009126ddeac2cd93055c13439f
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="如何开始使用 Azure 表存储和 Visual Studio 连接服务 |Microsoft Azure"
    description="如何开始使用 Azure 表存储在 ASP.NET 5 项目在 Visual Studio 中"
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

# 如何开始使用 Azure 表存储和 Visual Studio 连接服务

> [AZURE.SELECTOR]
> - [入门教程](vs-storage-aspnet5-getting-started-tables.md)
> - [发生了什么事](vs-storage-aspnet5-what-happened.md)

> [AZURE.SELECTOR]
> - [Blob](vs-storage-aspnet5-getting-started-blobs.md)
> - [队列](vs-storage-aspnet5-getting-started-queues.md)
> - [表](vs-storage-aspnet5-getting-started-tables.md)

## 概述

本文介绍了如何开始使用 Azure 表存储在 Visual Studio 中，您创建或通过使用 Visual Studio**中添加连接服务**对话框引用 ASP.NET 5 项目在 Azure 存储帐户后。

Azure 表存储服务使您能够存储大量的结构化数据。 该服务是接受来自通过身份验证的调用的内部和外部 Azure 云 NoSQL 数据存储。 Azure 表非常适合于结构化、 非关系型数据存储。

**添加连接服务**操作安装适当的 NuGet 程序包，以访问您的项目在 Azure 存储并将存储帐户连接字符串添加到项目配置文件。

有关使用 Azure 表存储的详细信息，请参阅[如何使用.NET 中的表存储](storage-dotnet-how-to-use-tables.md)。

若要开始，首先需要在您的存储帐户中创建表。 我们将向您展示如何在代码中创建一个 Azure 表。 我们将还显示如何执行基本表和实体的操作，例如添加、 修改、 读取和读表的实体。 用 c 语言编写的示例\#针对.NET 使用 Azure 存储客户端库和代码。

**注意**-一些执行出到 Azure 存储在 ASP.NET 5 是异步调用的 Api。 有关更多信息，请参见[异步和等待异步编程](http://msdn.microsoft.com/library/hh191443.aspx)。 下面的代码假定正在使用异步编程方法。

## 在代码中访问表

若要访问 ASP.NET 5 项目中的表，您需要包括以下各项的访问 Azure 表存储任何 C# 源代码文件。

1. 请确保 C# 文件顶部的命名空间声明包含这些`using`语句。

        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Table;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;

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

若要创建 Azure 表，只需添加对的调用`CreateIfNotExistsAsync()`。

    // Create the CloudTable if it does not exist
    await table.CreateIfNotExistsAsync();

## 向表中添加实体

要向表中添加实体创建定义您的实体的属性的类。 下面的代码定义了一个名为**CustomerEntity**的行键和姓氏的客户的名字用作分区键的实体类。

    public class CustomerEntity : TableEntity
    {
        public CustomerEntity(string lastName, string firstName)
        {
            this.PartitionKey = lastName;
            this.RowKey = firstName;
        }

        public CustomerEntity() { }

        public string Email { get; set; }

        public string PhoneNumber { get; set; }
    }

完成表操作涉及实体使用`CloudTable`"在代码中访问表"。 在前面部分中创建的对象 `TableOperation`对象表示要执行的操作。 下面的代码示例演示如何创建`CloudTable`对象和一个`CustomerEntity`对象。 准备操作，`TableOperation`创建要插入到表中的客户实体。 最后，通过调用 CloudTable.ExecuteAsync 来执行操作。

    // Get a reference to the CloudTable object named 'peopleTable' as described in "Access a table in code"

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";

    // Create the TableOperation that inserts the customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);

    // Execute the insert operation.
    await peopleTable.ExecuteAsync(insertOperation);

## 插入一批的实体

入的表中的一个写操作，您可以插入多个实体。 下面的代码示例创建两个实体对象 （Jeff Smith 和 Ben Smith），将它们添加到`TableBatchOperation`对象使用的**Insert**方法，，然后通过调用 CloudTable.ExecuteBatchAsync 开始操作。

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
        TableQuerySegment<CustomerEntity> resultSegment = await peopleTable.ExecuteQuerySegmentedAsync(query, token);
        token = resultSegment.ContinuationToken;

        foreach (CustomerEntity entity in resultSegment.Results)
        {
            Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
            entity.Email, entity.PhoneNumber);
        }
    } while (token != null);

    return View();


## 获取单个实体
您可以编写一个查询，以获取特定的一个独立的实体。 下面的代码使用`TableOperation`对象以指定的名为 Ben Smith 客户。 此方法返回的只是一个实体，而不是集合，并且在将返回的值`TableResult.Result`是`CustomerEntity`对象。 在查询中指定分区和行键是从**表**服务中检索单个实体的最快方法。

    // Get a reference to a CloudTableobject named 'peopleTable' as described in "Access a table in code"

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

    // Get a reference to a CloudTableobject named 'peopleTable' as described in "Access a table in code"

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

[AZURE.INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]

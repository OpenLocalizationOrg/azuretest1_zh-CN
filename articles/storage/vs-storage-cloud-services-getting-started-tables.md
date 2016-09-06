---
ms.openlocfilehash: bb980d40f1cef66b0c4bda9594bd87afff82b0ca
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="开始使用 Azure 表存储和 Visual Studio 连接服务"
    description="如何开始使用 Azure 表存储在 Visual Studio 中一个云服务项目。"
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

# 开始使用 Azure 表存储和 Visual Studio 连接服务

> [AZURE.SELECTOR]
> - [入门教程](vs-storage-cloud-services-getting-started-tables.md)
> - [发生了什么事](vs-storage-cloud-services-what-happened.md)

> [AZURE.SELECTOR]
> - [Blob](vs-storage-cloud-services-getting-started-blobs.md)
> - [队列](vs-storage-cloud-services-getting-started-queues.md)
> - [表](vs-storage-cloud-services-getting-started-tables.md)

##概述

本文介绍如何开始使用 Azure 表存储在 Visual Studio 中创建或通过使用 Visual Studio**中添加连接服务**对话框引用 Azure 存储帐户在一个云服务项目中。 **添加连接服务**操作安装适当的 NuGet 程序包，以访问您的项目在 Azure 存储并将存储帐户连接字符串添加到项目配置文件。

Azure 表存储服务使您能够存储大量的结构化数据。 该服务是接受来自通过身份验证的调用的内部和外部 Azure 云 NoSQL 数据存储。 Azure 表非常适合于结构化、 非关系型数据存储。

若要开始，首先需要在您的存储帐户中创建表。 我们将向您展示如何在代码中，创建一个 Azure 表以及如何执行基本表和实体的操作，例如添加、 修改、 读取和读表的实体。 用 c 语言编写的示例\#的代码并使用[Azure 存储.NET 客户端库](https://msdn.microsoft.com/library/azure/dn261237.aspx)。

**注意︰**一些异步执行出到 Azure 存储调用的 Api。 有关更多信息，请参见[异步编程与异步和等待](http://msdn.microsoft.com/library/hh191443.aspx)。 下面的代码假定正在使用异步编程方法。

- 以编程方式操作表的详细信息，请参阅[如何使用.NET 中的表存储](storage-dotnet-how-to-use-tables.md)。
- Azure 存储有关的一般信息，请参阅[存储文档](https://azure.microsoft.com/documentation/services/storage/)。
- Azure 的云服务的常规信息，请参阅[云服务文档](http://azure.microsoft.com/documentation/services/cloud-services/)。
- 有关编程 ASP.NET 应用程序的详细信息，请参见[ASP.NET](http://www.asp.net) 。

##在代码中访问表

若要访问云服务项目中的表，您需要包括以下各项的访问 Azure 表存储任何 C# 源代码文件。

1. 请确保 C# 文件顶部的命名空间声明包含这些`using`语句。

        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Table;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;

2. 得到一个**CloudStorageAccount**对象，该对象表示您存储的帐户信息。 下面的代码用于获取从 Azure 服务配置的存储连接字符串和存储的帐户信息。

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage account name>
         _AzureStorageConnectionString"));
> [AZURE.NOTE]  在下面的示例使用所有上面的代码，该代码的前面。

3. 得到一个**CloudTableClient**来引用您的存储帐户中的表对象。

         // Create the table client.
         CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

4. 获得**CloudTable**引用对象引用特定的表和图元。

        // Get a reference to a table named "peopleTable".
        CloudTable table = tableClient.GetTableReference("peopleTable");

##在代码中创建一个表

若要创建 Azure 表，只需添加对的调用`CreateIfNotExistsAsync`为获得后`CloudTable`对象"访问代码中的表"一节中所述。

    // Create the CloudTable if it does not exist.
    await table.CreateIfNotExistsAsync();

##向表中添加实体

将实体添加到表中，创建定义您的实体的属性的类。 下面的代码定义了一个名为**CustomerEntity**的客户的名字作为行键和最后一个名称用作分区键的实体类。

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

完成表操作涉及实体使用您以前创建的**CloudTable**对象"访问代码中的表"。 **TableOperation**对象表示要执行的操作。 下面的代码示例演示如何创建一个**CloudTable**对象和一个**CustomerEntity**对象。 准备操作， **TableOperation**将创建要插入到表中的客户实体。 最后，通过调用**CloudTable.ExecuteAsync**来执行操作。

    // Get a reference to the **CloudTable** object named 'peopleTable' as described in "Access a table in code".

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";

    // Create the TableOperation that inserts the customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);

    // Execute the insert operation.
    await peopleTable.ExecuteAsync(insertOperation);

##插入一批的实体

入的表中的一个写操作，您可以插入多个实体。 下面的代码示例创建两个实体对象 （Jeff Smith 和 Ben Smith），将它们添加到一个**TableBatchOperation**对象，该对象使用 Insert 方法，然后通过调用**CloudTable.ExecuteBatchAsync**开始操作。

    // Get a reference to a **CloudTable** object named 'peopleTable' as described in "Access a table in code".

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

    // Create the CloudTable if it does not exist
    await table.CreateIfNotExistsAsync();

##向表中添加实体

要向表中添加实体创建定义您的实体的属性的类。 下面的代码定义一个实体类，称为`CustomerEntity`与分区键的行键和姓氏使用客户的名字。

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

    // Get a reference to the CloudTable object named 'peopleTable' as described in "Access a table in code".

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";

    // Create the TableOperation that inserts the customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);

    // Execute the insert operation.
    await peopleTable.ExecuteAsync(insertOperation);

##插入一批的实体

入的表中的一个写操作，您可以插入多个实体。 下面的代码示例创建两个实体对象 （Jeff Smith 和 Ben Smith），将它们添加到`TableBatchOperation`对象使用 Insert 方法，，然后开始操作，通过调用`CloudTable.ExecuteBatchAsync`。

    // Get a reference to a CloudTable object named 'peopleTable' as described in "Access a table in code".

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

##获取在一个分区中的所有实体

要查询的表的所有分区中的实体，请使用`TableQuery`对象。 下面的代码示例指定为 Smith 所在的分区键的实体的筛选器。 本示例打印到控制台查询结果中的每个实体的字段。

    // Get a reference to a CloudTable object named 'peopleTable' as described in "Access a table in code".

    // Construct the query operation for all customer entities where PartitionKey="Smith".
    TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>()
        .Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

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


##获取单个实体

您可以编写一个查询，以获取特定的一个独立的实体。 下面的代码使用**TableOperation**对象来指定一个名为 Ben Smith 的客户。 只是一个实体，则此方法返回，而不是集合，并且在**TableResult.Result**中返回的值是一个**CustomerEntity**对象。 在查询中指定分区和行键是从**表**服务中检索单个实体的最快方法。

    // Get a reference to a **CloudTable** object named 'peopleTable' as described in "Access a table in code".

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the retrieve operation.
    TableResult retrievedResult = await peopleTable.ExecuteAsync(retrieveOperation);

    // Print the phone number of the result.
    if (retrievedResult.Result != null)
       Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
    else
       Console.WriteLine("The phone number could not be retrieved.");

##删除实体
找到它之后，您可以删除某个实体。 下面的代码中寻找名为"Ben Smith"，一个客户实体，如果它发现它，它会删除它。

    // Get a reference to a **CloudTable** object named 'peopleTable' as described in "Access a table in code".

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

##下一步行动

[AZURE.INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]

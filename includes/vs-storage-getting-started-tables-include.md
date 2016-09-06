---
ms.openlocfilehash: 11b172648130d5204956b48367f032ad300cbbf5
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
#####创建表
**CloudTableClient**对象可获取表和实体引用的对象。 下面的代码创建一个**CloudTableClient**对象并使用它来创建一个新表。 该代码尝试引用的表名为"人"。 如果它找不到具有该名称的表，则会创建一个。

**注意︰**本指南中的所有代码都假定正在生成的应用程序是 Azure 云服务项目，并使用 Azure 应用程序服务配置中存储的存储连接字符串。

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    
    // Create the table if it doesn't exist.
    CloudTable table = tableClient.GetTableReference("people");
    table.CreateIfNotExists();

#####向表中添加实体
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

完成表操作涉及实体使用您以前创建的**CloudTable**对象"创建表"。 **TableOperation**对象表示要执行的操作。 下面的代码示例演示如何创建一个**CloudTable**对象和一个**CustomerEntity**对象。 准备操作， **TableOperation**将创建要插入到表中的客户实体。 最后，通过调用 CloudTable.Execute 来执行操作。

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");
    
    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";
    
    // Create the TableOperation that inserts the customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);
    
    // Execute the insert operation.
    table.Execute(insertOperation);

#####插入一批的实体
入的表中的一个写操作，您可以插入多个实体。 下面的代码示例创建两个实体对象 （Jeff Smith 和 Ben Smith），将它们添加到一个**TableBatchOperation**对象，该对象使用 Insert 方法，然后通过调用 CloudTable.Execute 开始操作。

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");
    
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
    table.ExecuteBatch(batchOperation);

#####获取在一个分区中的所有实体
要查询的表的所有分区中的实体，请使用**TableQuery**对象。 下面的代码示例指定为 Smith 所在的分区键的实体的筛选器。 本示例打印到控制台查询结果中的每个实体的字段。

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");
    
    // Construct the query operation for all customer entities where PartitionKey="Smith".
    TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>().Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));
    
    // Print the fields for each customer.
    foreach (CustomerEntity entity in table.ExecuteQuery(query))
    {
        Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
            entity.Email, entity.PhoneNumber);
    }

#####获取单个实体
您可以编写一个查询，以获取特定的一个独立的实体。 下面的代码使用**TableOperation**对象来指定一个名为 Ben Smith 的客户。 只是一个实体，则此方法返回，而不是集合，并且在 TableResult.Result 中返回的值是一个**CustomerEntity**对象。 在查询中指定分区和行键是从**表**服务中检索单个实体的最快方法。

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");
    
    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");
    
    // Execute the retrieve operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);
    
    // Print the phone number of the result.
    if (retrievedResult.Result != null)
       Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
    else
       Console.WriteLine("The phone number could not be retrieved.");

#####删除实体
找到它之后，您可以删除某个实体。 下面的代码中寻找名为"Ben Smith 的客户实体，如果它发现它，它会删除它。

    // Create the CloudTable that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");
    
    // Create a retrieve operation that expects a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");
    
    // Execute the operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);
    
    // Assign the result to a CustomerEntity object.
    CustomerEntity deleteEntity = (CustomerEntity)retrievedResult.Result;
    
    // Create the Delete TableOperation and then execute it.
    if (deleteEntity != null)
    {
       TableOperation deleteOperation = TableOperation.Delete(deleteEntity);
    
       // Execute the operation.
       table.Execute(deleteOperation);
    
       Console.WriteLine("Entity deleted.");
    }
    
    else
       Console.WriteLine("Couldn't delete the entity.");

[了解更多关于 Azure 存储](http://azure.microsoft.com/documentation/services/storage/)请参阅[浏览服务器资源管理器中的存储资源](http://msdn.microsoft.com/library/azure/ff683677.aspx)。
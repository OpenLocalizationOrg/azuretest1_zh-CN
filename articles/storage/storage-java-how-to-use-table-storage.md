---
ms.openlocfilehash: 440aff3f649c412f2188a6bd07dab722e5df90c5
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何使用 Java 存储表 |Microsoft Azure" 
    description="了解如何使用在 Azure 表存储服务。 在 Java 代码中编写的代码样本。" 
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


# 如何使用 Java 表存储

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]

## 概述

本指南将向您介绍如何执行使用 Azure 表存储服务的常见方案。 这些示例用 Java 编写的并使用[Azure 存储 Java SDK][]。 所包含的方案包括在表中**创建**、**列出**和**删除**表，以及**插入**、**查询**、**修改**和**删除**实体。 有关表的详细信息，请参阅[后续步骤](#NextSteps)部分。

注︰ 对于开发人员来说在 Android 设备上使用 Azure 存储提供了 SDK。 有关详细信息，请参阅[为 Android 的 Azure 存储 SDK][]。 

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## 创建一个 Java 应用程序

在本指南中，您将使用存储功能，可以在本地或在 web 角色或在 Azure 中的辅助角色中运行的代码中运行 Java 应用程序中。

若要执行此操作，您将需要安装 Java 开发工具箱 (JDK) 并在 Azure 订阅创建 Azure 存储帐户。 一旦您这样做了，您需要验证您开发的系统满足最低要求和相关性在 GitHub [Azure 存储 Java SDK][]存储库中列出了这些。 如果您的系统符合这些要求，您可以按照说明下载和安装 Java Azure 存储库，该存储库从您的系统上。 完成这些任务之后，您将能够创建一个 Java 应用程序，它使用本文中的示例。

## 配置应用程序来访问表的存储

将下面的导入语句添加到想要使用 Microsoft Azure 存储 Api 来访问表的 Java 文件的顶部︰

    // Include the following imports to use table APIs
    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.table.*;
    import com.microsoft.azure.storage.table.TableQuery.*;

## 安装程序将 Azure 存储连接字符串

Azure 存储客户端使用存储的连接字符串将存储终结点和凭据以访问数据管理服务。 在客户端应用程序运行时，您必须提供存储连接字符串以下面的格式，用于存储帐户的*帐户名*和*AccountKey*值管理门户中列出您的存储帐户和主访问键的名称。 本示例显示了如何声明一个静态字段来保存连接字符串︰

    // Define the connection-string with your values.
    public static final String storageConnectionString = 
        "DefaultEndpointsProtocol=http;" + 
        "AccountName=your_storage_account;" + 
        "AccountKey=your_storage_account_key";

在 Microsoft Azure 角色中运行的应用程序，此字符串将存储在服务配置文件中， *ServiceConfiguration.cscfg*，并可以通过调用**RoleEnvironment.getConfigurationSettings**方法。 这里是从名为*StorageConnectionString*的服务配置文件中的**设置**元素获取连接字符串的示例︰

    // Retrieve storage account from connection-string.
    String storageConnectionString = 
        RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");

下面的示例假定您已经使用这两种方法之一来获取存储连接字符串。

## 如何︰ 创建表

**CloudTableClient**对象可获取表和实体引用的对象。 下面的代码创建一个**CloudTableClient**对象并使用它来创建一个新的**CloudTable**对象表示表名为"人"。 (注︰ 还有其他的方式来创建**CloudStorageAccount**对象; 有关详细信息，请参阅**CloudStorageAccount** [Azure 存储客户端 SDK 参考]中。)

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

       // Create the table client.
       CloudTableClient tableClient = storageAccount.createCloudTableClient();

       // Create the table if it doesn't exist.
       String tableName = "people";
       CloudTable cloudTable = new CloudTable(tableName,tableClient);
       cloudTable.createIfNotExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## 如何︰ 列出的表

若要获取表的列表，请调用**CloudTableClient.listTables()**方法来检索 iterable 的表名列表。

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Loop through the collection of table names.
        for (String table : tableClient.listTables())
        {
          // Output each table name.
          System.out.println(table);
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## 如何︰ 向表中添加实体

实体映射到使用实现**TableEntity**的自定义类的 Java 对象。 为方便起见， **TableServiceEntity**类实现**TableEntity** ，并使用反射将属性映射到名为的属性 getter 和 setter 方法。 将实体添加到表中，首先创建定义您的实体的属性的类。 下面的代码定义一个实体类，它使用作为行键并选择最后一名的客户的名字，作为分区键。 在一起，实体的分区行键唯一标识表中的实体。 可比那些具有不同的分区键查询具有相同的分区键的实体。

    public class CustomerEntity extends TableServiceEntity {
        public CustomerEntity(String lastName, String firstName) {
            this.partitionKey = lastName;
            this.rowKey = firstName;
        }

        public CustomerEntity() { }

        String email;
        String phoneNumber;
        
        public String getEmail() {
            return this.email;
        }
        
        public void setEmail(String email) {
            this.email = email;
        }
        
        public String getPhoneNumber() {
            return this.phoneNumber;
        }
        
        public void setPhoneNumber(String phoneNumber) {
            this.phoneNumber = phoneNumber;
        }
    }

表操作涉及实体需要一个**TableOperation**对象。 此对象定义要执行的实体，可以使用**CloudTable**对象执行的操作。 下面的代码用客户数据存储创建**CustomerEntity**类的一个新实例。 该代码接下来调用**TableOperation.insertOrReplace**来创建一个**TableOperation**对象，以将实体插入到表中，然后将新**CustomerEntity**与它相关联。 最后，该代码调用上的**CloudTable**对象，指定"人"表和新**TableOperation**，然后将请求发送到存储服务来将新的客户实体插入到"人"表中，或如果已经存在，则替换该实体的**执行**方法。

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();
            
        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");
            
        // Create a new customer entity.
        CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
        customer1.setEmail("Walter@contoso.com");
        customer1.setPhoneNumber("425-555-0101");
            
        // Create an operation to add the new customer to the people table.
        TableOperation insertCustomer1 = TableOperation.insertOrReplace(customer1);

        // Submit the operation to the table service.
        cloudTable.execute(insertCustomer1);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## 如何︰ 插入一批的实体

在一个写操作，可以插入到表服务实体一批。 下面的代码创建一个**TableBatchOperation**对象，然后添加三个插入到该操作。 通过创建新的实体对象，设置其值，然后再要新的插入操作相关联的实体的**TableBatchOperation**对象上调用**insert**方法添加每个插入操作。 然后该代码调用**执行**上的**CloudTable**对象，指定"人"表和**TableBatchOperation**对象，将表操作的批处理发送给单个请求中的存储服务。

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Define a batch operation.
        TableBatchOperation batchOperation = new TableBatchOperation();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a customer entity to add to the table.
        CustomerEntity customer = new CustomerEntity("Smith", "Jeff");
        customer.setEmail("Jeff@contoso.com");
        customer.setPhoneNumber("425-555-0104");
        batchOperation.insertOrReplace(customer);

       // Create another customer entity to add to the table.
       CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
       customer2.setEmail("Ben@contoso.com");
       customer2.setPhoneNumber("425-555-0102");
       batchOperation.insertOrReplace(customer2);

       // Create a third customer entity to add to the table.
       CustomerEntity customer3 = new CustomerEntity("Smith", "Denise");
       customer3.setEmail("Denise@contoso.com");
       customer3.setPhoneNumber("425-555-0103");
       batchOperation.insertOrReplace(customer3);

       // Execute the batch of operations on the "people" table.
       cloudTable.execute(batchOperation);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

要在批处理操作上注意的一些事项︰

- 可以执行最多 100 个插入、 删除、 合并、 替换、 插入或合并，以及插入或替换操作的任意组合在单个批处理中。
- 批处理操作可以检索操作中，如果没有它批次中的唯一操作。
- 在一个批操作中的所有实体都必须都有相同的分区键。
- 批处理操作被限制为 4 MB 的数据有效负载。

## 如何︰ 检索一个分区中的所有实体

要查询的表的分区中的实体，您可以使用**TableQuery**。 调用**TableQuery.from**属性返回指定的结果类型的特定表创建一个查询。 下面的代码指定为 Smith 所在的分区键的实体的筛选器。 **TableQuery.generateFilterCondition**是一个帮助器方法来为查询创建筛选器。 **在其中**调用**TableQuery.from**方法将筛选器应用到该查询所返回的引用。 当执行查询时为**CloudTable**对象上**执行**的调用时，它返回**迭代器**指定的**CustomerEntity**结果类型。 然后，您可以使用**迭代器**返回的 for each 循环来使用结果。 此代码将打印到控制台查询结果中的每个实体的字段。

    try
    {
        // Define constants for filters.
        final String PARTITION_KEY = "PartitionKey";
        final String ROW_KEY = "RowKey";
        final String TIMESTAMP = "Timestamp";

        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();
            
       // Create a cloud table object for the table.
       CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a filter condition where the partition key is "Smith".
        String partitionFilter = TableQuery.generateFilterCondition(
           PARTITION_KEY, 
           QueryComparisons.EQUAL,
           "Smith");

       // Specify a partition query, using "Smith" as the partition key filter.
       TableQuery<CustomerEntity> partitionQuery =
           TableQuery.from(CustomerEntity.class)
           .where(partitionFilter);

        // Loop through the results, displaying information about the entity.
        for (CustomerEntity entity : cloudTable.execute(partitionQuery)) {
            System.out.println(entity.getPartitionKey() +
                " " + entity.getRowKey() + 
                "\t" + entity.getEmail() +
                "\t" + entity.getPhoneNumber());
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## 如何︰ 检索实体在一个分区中的区域

如果您希望查询在一个分区中的所有实体，您可以在筛选中使用比较运算符来指定范围。 下面的代码将字母表中获取在分区中的所有实体"Smith"行键 （名字） 其中以到 E 字母开头的两个筛选器组合在一起。 然后打印查询结果。 如果您使用的实体添加到批处理中的表插入本指南的部分，将返回 （Ben 和史丹）; 这次只有两个实体，李明不包括在内。

    try
    {
        // Define constants for filters.
        final String PARTITION_KEY = "PartitionKey";
        final String ROW_KEY = "RowKey";
        final String TIMESTAMP = "Timestamp";
            
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

       // Create the table client.
       CloudTableClient tableClient = storageAccount.createCloudTableClient();

       // Create a cloud table object for the table.
       CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a filter condition where the partition key is "Smith".
        String partitionFilter = TableQuery.generateFilterCondition(
           PARTITION_KEY, 
           QueryComparisons.EQUAL,
           "Smith");

        // Create a filter condition where the row key is less than the letter "E".
        String rowFilter = TableQuery.generateFilterCondition(
           ROW_KEY, 
           QueryComparisons.LESS_THAN,
           "E");

        // Combine the two conditions into a filter expression.
        String combinedFilter = TableQuery.combineFilters(partitionFilter, 
            Operators.AND, rowFilter);

        // Specify a range query, using "Smith" as the partition key,
        // with the row key being up to the letter "E".
        TableQuery<CustomerEntity> rangeQuery =
           TableQuery.from(CustomerEntity.class)
           .where(combinedFilter);

        // Loop through the results, displaying information about the entity
        for (CustomerEntity entity : cloudTable.execute(rangeQuery)) {
            System.out.println(entity.getPartitionKey() +
                " " + entity.getRowKey() +
                "\t" + entity.getEmail() +
                "\t" + entity.getPhoneNumber());
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## 如何︰ 检索单个实体

您可以编写一个查询来检索单一的特定实体。 下面的代码调用**TableOperation.retrieve**分区键和行关键参数来指定客户"小红"，而不是创建**TableQuery**并使用筛选器来执行相同的操作。 执行时检索操作返回只是一个实体，而不是集合。 **GetResultAsType**方法将强制转换为赋值目标， **CustomerEntity**对象的类型的结果。 如果该类型不兼容与指定的查询类型，将引发异常。 如果没有实体具有完全相同的分区和行键匹配，则返回空值。 在查询中指定分区和行键是从表服务中检索单个实体的最快方法。

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Retrieve the entity with partition key of "Smith" and row key of "Jeff"
        TableOperation retrieveSmithJeff = 
           TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

       // Submit the operation to the table service and get the specific entity.
       CustomerEntity specificEntity =
            cloudTable.execute(retrieveSmithJeff).getResultAsType();
            
        // Output the entity.
        if (specificEntity != null)
        {
            System.out.println(specificEntity.getPartitionKey() +
                " " + specificEntity.getRowKey() +
                "\t" + specificEntity.getEmail() +
                "\t" + specificEntity.getPhoneNumber());
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## 如何︰ 修改实体

修改实体，从表服务中检索、 更改的实体对象，并将更改保存回表服务与替换或合并操作。 下面的代码更改现有客户的电话号码。 而不是像我们所做的插入调用**TableOperation.insert** ，此代码调用**TableOperation.replace**。 **CloudTable.execute**方法调用表服务中，并替换该实体，除非另一应用程序已将其更改的时间因为此应用程序中检索它。 在这种情况下，将引发异常，和实体必须进行检索、 修改，然后再次保存。 这种开放式并发重试模式是常见的分布式的存储系统中。

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Retrieve the entity with partition key of "Smith" and row key of "Jeff".
        TableOperation retrieveSmithJeff = 
           TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

        // Submit the operation to the table service and get the specific entity.
        CustomerEntity specificEntity =
          cloudTable.execute(retrieveSmithJeff).getResultAsType();

        // Specify a new phone number.
        specificEntity.setPhoneNumber("425-555-0105");

        // Create an operation to replace the entity.
        TableOperation replaceEntity = TableOperation.replace(specificEntity);

        // Submit the operation to the table service.
        cloudTable.execute(replaceEntity);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## 如何︰ 查询实体属性的子集

对一个表的查询可以检索几个属性从一个实体。 这种技术，称为投影，减少了带宽，并可以提高查询性能，尤其是对于大型实体。 下面的代码中的查询使用**select**方法以返回表中的实体的电子邮件地址。 结果被投影到**EntityResolver**，后者执行从服务器返回的实体的类型转换的帮助**字符串**的集合。 您可以了解有关此[博客文章][]中的投影。 请注意，投影不支持本地存储仿真器，因此只在上表服务使用的帐户时，才运行此代码。

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Define a projection query that retrieves only the Email property
        TableQuery<CustomerEntity> projectionQuery = 
           TableQuery.from(CustomerEntity.class)
           .select(new String[] {"Email"});

        // Define a Entity resolver to project the entity to the Email value.
        EntityResolver<String> emailResolver = new EntityResolver<String>() {
            @Override
            public String resolve(String PartitionKey, String RowKey, Date timeStamp, HashMap<String, EntityProperty> properties, String etag) {
                return properties.get("Email").getValueAsString();
            }
        };

        // Loop through the results, displaying the Email values.
        for (String projectedString : 
            cloudTable.execute(projectionQuery, emailResolver)) {
                System.out.println(projectedString);
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## 如何︰ 插入或替换图元

通常，您想要向表中添加实体，而无需知道如果表中已经存在。 插入或替换操作允许您将插入实体，如果它不存在或者如果它不替换现有的单个请求。 构建在前面的示例，下面的代码插入或替换该实体的"Walter 竖琴"。 创建一个新实体后, 此代码调用**TableOperation.insertOrReplace**方法。 然后，此代码表和插入的**CloudTable**对象上调用**执行**或替换为参数的表操作。 若要更新实体的一部分，可以改为使用**TableOperation.insertOrMerge**方法。 请注意，插入或-替换不支持本地存储仿真器，因此只在上表服务使用的帐户时，才运行此代码。 您可以了解有关插入或替换，并在此[博客文章][]中插入或合并的详细信息。

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a new customer entity.
        CustomerEntity customer5 = new CustomerEntity("Harp", "Walter");
        customer5.setEmail("Walter@contoso.com");
        customer5.setPhoneNumber("425-555-0106");

        // Create an operation to add the new customer to the people table.
        TableOperation insertCustomer5 = TableOperation.insertOrReplace(customer5);

        // Submit the operation to the table service.
        cloudTable.execute(insertCustomer5);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## 如何︰ 删除实体

已检索到它之后，可以轻松地删除实体。 一旦检索到该实体，调用**TableOperation.delete**与要删除的实体。 然后**执行**对象上调用**CloudTable** 。 下面的代码检索和删除一个客户实体。

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Create an operation to retrieve the entity with partition key of "Smith" and row key of "Jeff".
        TableOperation retrieveSmithJeff = TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

        // Retrieve the entity with partition key of "Smith" and row key of "Jeff".
        CustomerEntity entitySmithJeff =
            cloudTable.execute(retrieveSmithJeff).getResultAsType();

        // Create an operation to delete the entity.
        TableOperation deleteSmithJeff = TableOperation.delete(entitySmithJeff);

        // Submit the delete operation to the table service.
        cloudTable.execute(deleteSmithJeff);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## 如何︰ 删除表

最后，下面的代码删除表中存储帐户。 已删除的表将无法执行删除操作，通常不超过四十秒的时间内重新创建。

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Delete the table and all its data if it exists.
        CloudTable cloudTable = new CloudTable("people",tableClient);
        cloudTable.deleteIfExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## 下一步行动

现在，您已经了解了表存储的基本知识，按照这些链接以了解如何执行更复杂的存储任务。

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
[博客张贴内容]: http://blogs.msdn.com/b/windowsazurestorage/archive/2011/09/15/windows-azure-tables-introducing-upsert-and-query-projection.aspx
 
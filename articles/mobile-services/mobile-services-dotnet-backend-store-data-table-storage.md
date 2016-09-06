---
ms.openlocfilehash: 05bd55c0fbd19c52683c777052b97a4577b6676f
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="生成的服务，而不是 SQL 数据库使用表格存储 |Microsoft Azure" 
    description="了解如何使用 Azure 表存储与.NET 后端移动服务。" 
    services="mobile-services" 
    documentationCenter="" 
    authors="ggailey777" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="06/09/2015" 
    ms.author="glenga"/>

# 生成.NET 后端使用表格存储，而不是 SQL 数据库的移动服务

本主题演示如何使用.NET 后端移动服务的非关系型数据存储。 在本教程中，您将修改 Azure 移动服务快速启动项目，而不是默认的 SQL Azure 数据库数据存储使用 Azure 表存储。

本教程需要[开始使用移动服务]或[添加到现有应用程序的移动服务]指南的完成。 您还需要 Azure 存储帐户。 

##配置 Azure 表存储在.NET 后端移动服务

首先，您需要配置您的移动服务和.NET 后端代码项目连接到 Azure 存储。

1. 在**解决方案资源管理器**在 Visual Studio 中.NET 后端项目中，用鼠标右键单击，然后选择**管理 NuGet 程序包**。

2. 在左窗格中，选择**联机**类别，选择**仅 Stabile**、 **MobileServices**搜索，单击**安装** **Microsoft Azure 移动服务.NET 后端 Azure 存储扩展**包，然后接受许可协议。 

    ![](./media/mobile-services-dotnet-backend-store-data-table-storage/mobile-add-storage-nuget-package-dotnet.png)

    这将对 Azure 存储服务的支持添加到.NET 后端的移动服务项目。

3. 如果您没有创建存储帐户，请参阅[如何创建存储帐户](../storage-create-storage-account.md)。

4. 在管理门户中，单击**存储**，请单击存储帐户，然后单击**管理密钥**。 

5. 记下**存储帐户名称**和**访问键**。
 
6. 在您的移动服务，单击**配置**选项卡，向下滚动到**连接字符串**，然后输入新的连接字符串的**名称** `StorageConnectionString` ，并为存储帐户连接字符串采用以下格式的**值**。 

        DefaultEndpointsProtocol=https;AccountName=<ACCOUNT_NAME>;AccountKey=<ACCESS_KEY>;

    ![](./media/mobile-services-dotnet-backend-store-data-table-storage/mobile-blob-storage-app-settings.png)

7. 上面字符串中的替换值的`<ACCOUNT_NAME>`，`<ACCESS_KEY>`门户中的值，然后单击**保存**。 

    存储帐户连接字符串存储在应用程序设置中加密。 您可以访问此字符串在运行时的任何表控制器中。 

8. 在解决方案资源管理器在 Visual Studio 中，打开手机信息服务项目的 Web.config 文件，添加以下新的连接字符串︰

        <add name="StorageConnectionString" connectionString="<STORAGE_CONNECTION_STRING>" />

9. 更换`<STORAGE_CONNECTION_STRING>`与步骤 6 中的连接字符串的占位符。

    您可以测试代码，然后将它发布的本地计算机上运行时，移动服务将使用此连接字符串。 Azure 中运行时，移动服务将改为使用在门户网站中设置的连接字符串值，并忽略项目中的连接字符串。 

## <a name="modify-service"></a>修改数据类型和表控制器

因为任务列表快速入门项目旨在使用 SQL 数据库使用实体框架，您需要在项目中使用表格存储进行一些更新。 

1. 修改，如下所示从**StorageData**而不是**EntityData**， **TodoItem**数据类型。

        public class TodoItem : StorageData
        {
            public string Text { get; set; }
            public bool Complete { get; set; }
        }

    >[AZURE.NOTE]该**StorageData**类型具有要求复合键格式*partitionId*，*rowValue*中的字符串 Id 属性。

2. 在**TodoItemController**中，添加以下 using 语句。

        using System.Web.Http.OData.Query;
        using System.Collections.Generic;

3. 用以下内容替换**TodoItemController**的**初始化**方法。

        protected override void Initialize(HttpControllerContext controllerContext)
        {
            base.Initialize(controllerContext);

            // Create a new Azure Storage domain manager using the stored 
            // connection string and the name of the table exposed by the controller.
            string connectionStringName = "StorageConnectionString";
            var tableName = controllerContext.ControllerDescriptor.ControllerName.ToLowerInvariant();
            DomainManager = new StorageDomainManager<TodoItem>(connectionStringName, 
                tableName, Request, Services);          
        }

    这将创建新的存储域管理器使用存储帐户连接字符串的请求控制器。

3. 下面的代码替换现有的**GetAllTodoItems**方法。

        public Task<IEnumerable<TodoItem>> GetAllTodoItems(ODataQueryOptions options)
        {
            // Call QueryAsync, passing the supplied query options.
            return DomainManager.QueryAsync(options);
        } 

    此版本不与 SQL 数据库，不同返回 IQueryable<TEntity>，因此结果也可以绑定到但不是进一步包括在查询中。 

## 更新的客户端应用程序

您需要在客户端进行快速入门应用程序使用.NET 后端使用表格存储进行一次更改。 这是因为表存储提供程序所需的复合键。 

1. 打开包含数据访问代码的客户端代码文件，并找到方法执行插入操作的位置。

2. 更新 TodoItem 实例添加显式设置 Id 字段，以字符串格式`<partitionID>,<rowValue>`。

    这是举例说明可能如何在 C# 应用程序，其中的分区部分固定的行部分是基于 GUID 的设置此 ID。

         todoItem.Id = string.Format("partition,{0}", Guid.NewGuid());

现在您可以测试该应用程序。

## <a name="test-application"></a>测试应用程序

1. （可选）重新发布您的移动服务.NET 后端的项目。 
    
    您还可以测试您的移动服务本地才能发布.NET 后端项目到 Azure。 是否在本地或在 Azure 中测试时，移动服务将使用 Azure 表存储。 

4. 运行快速启动客户端应用程序连接到您的移动服务。

    请注意看不到以前使用快速入门教程添加的项。 这是因为表存储当前为空。

5. 添加新的项目以生成数据库的更改。  
 
    应用程序和移动服务应该会像之前，除了现在您的数据存储在 SQL 数据库中非关系而不是存储。

##下一步行动

现在，您已经看到表存储使用.NET 后端是多么容易，请考虑探索一些其他后端存储选项︰

+ [把 MongoDB 用作与您移动 Services.NET 后端数据存储区](mobile-services-dotnet-backend-use-non-relational-data-store.md)</br>像刚刚完成本教程，本主题演示您如何使用您的移动服务的非关系型数据存储。 在本教程中，您将修改移动服务快速启动项目，而不是 SQL 数据库的 MongoDB 用作数据存储区。
 
+ [连接到本地 SQL Server 使用混合连接](mobile-services-dotnet-backend-hybrid-connections-get-started.md)</br>混合连接可以让您安全地连接到您的内部资产的移动服务。 这种方式，您可以使内部数据移动客户端可以访问使用 Azure。 支持的资产包括运行在一个静态的 TCP 端口，包括 Microsoft SQL Server，MySQL，HTTP Web Api 和大多数自定义 web 服务的任何资源。

+ [将图像传到使用移动服务的 Azure 存储](mobile-services-dotnet-backend-windows-store-dotnet-upload-data-blob-storage.md)</br>演示如何扩展任务列表示例项目，以使您可以将图像从您的应用程序到 Azure Blob 存储上载。

<!-- Anchors. -->
[创建非关系型存储]: #create-store
[修改数据和控制器]: #modify-service
[测试应用程序]: #test-application


<!-- Images. -->


<!-- URLs. -->
[开始使用移动服务]: mobile-services-dotnet-backend-windows-store-dotnet-get-started.md
[添加到现有应用程序的移动服务]: ../mobile-services-dotnet-backend-windows-store-dotnet-get-started-data.md
[Azure 的管理门户]: https://manage.windowsazure.com/
[表服务是什么]: ../storage-dotnet-how-to-use-tables.md#what-is
[MongoLab 附加页]: /gallery/store/mongolab/mongolab
 
测试

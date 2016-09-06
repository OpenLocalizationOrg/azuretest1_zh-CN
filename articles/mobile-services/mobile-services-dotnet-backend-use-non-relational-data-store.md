---
ms.openlocfilehash: a83b04a330aaf262d4cf0f0878a26e9bf68f23ee
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="生成使用非关系型数据存储服务 |Microsoft Azure" 
    description="了解如何使用 MongoDB 如非关系型数据存储区或使用您的.NET Azure 表存储基于移动服务" 
    services="mobile-services" 
    documentationCenter="" 
    authors="mattchenderson" 
    manager="dwrede" 
    editor="mollybos"/>

<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="08/08/2015" 
    ms.author="mahender"/>

# 生成.NET 后端存储而不是 SQL 数据库使用 MongoDB 的移动服务

本主题演示如何使用.NET 后端移动服务的非关系型数据存储。 在本教程中，您将修改移动服务快速启动项目，而不是默认的 SQL Azure 数据库数据存储使用 MongoDB。

本教程需要[开始使用移动服务]或[添加到现有应用程序的移动服务]指南的完成。 您还需要添加到您的订购的 MongoLab 服务。 

## <a name="create-store"></a>创建 MongoLab 非关系型存储

1. 在[Azure 管理门户]中，单击**新建**，然后单击**市场**。

2. 单击**MongoLab**加载项，并完成该向导以注册 MongoLab 帐户。 

    有关 MongoLab 的详细信息，请参阅[MongoLab 附加页]。

2. 该帐户被设置后，单击**连接信息**和复制连接字符串。

3. 在您的移动服务，单击**配置**选项卡，向下滚动到**连接字符串**，然后输入新的连接字符串的**名称**与`MongoConnectionString`**的值**是您 MongoDB 的连接，然后单击**保存**。 

    ![添加 MongoDB 连接字符串](./media/mobile-services-dotnet-backend-use-non-relational-data-store/mongo-connection-string.png)

    存储帐户连接字符串存储在应用程序设置中加密。 您可以访问此字符串在运行时的任何表控制器中。 

8. 在解决方案资源管理器在 Visual Studio 中，打开手机信息服务项目的 Web.config 文件，添加以下新的连接字符串︰

        <add name="MongoConnectionString" connectionString="<MONGODB_CONNECTION_STRING>" />

9. 更换`<MONGODB_CONNECTION_STRING>`使用 MongoDB 的连接字符串的占位符。

    您可以测试代码，然后将它发布的本地计算机上运行时，移动服务将使用此连接字符串。 Azure 中运行时，移动服务将改为使用在门户网站中设置的连接字符串值，并忽略项目中的连接字符串。  

## <a name="modify-service"></a>修改数据类型和表控制器

1. 安装**WindowsAzure.MobileServices.Backend.Mongo** NuGet 程序包。

2. 修改**TodoItem**从**DocumentData**而不是**EntityData**。

        public class TodoItem : DocumentData
        {
            public string Text { get; set; }

            public bool Complete { get; set; }
        }

3. 在**TodoItemController**中，用以下内容替换该**初始化**方法︰

        protected override async void Initialize(HttpControllerContext controllerContext)
        {
            base.Initialize(controllerContext);
            string connectionStringName = "MongoConnectionString";
            string databaseName = "<YOUR-DATABASE-NAME>";
            string collectionName = "todoItems";
            DomainManager = new MongoDomainManager<TodoItem>(connectionStringName, databaseName, 
                collectionName, Request, Services);
        }

4. 在上面的**初始化**方法的代码，将`<YOUR-DATABASE-NAME>`具有名称中选择 MongoLab 加载项的设置时。

现在您可以测试该应用程序。

## <a name="test-application"></a>测试应用程序

1. （可选）重新发布您的移动服务.NET 后端的项目。

    您还可以测试您的移动服务本地才能发布.NET 后端项目到 Azure。 是否在本地或在 Azure 中测试时，移动服务要使用您 MongoDB 进行存储。 

4. 位于开始页之前使用任一**立即尝试**按钮，或使用查询数据库中的项连接到您的移动应用程序，客户端应用程序。  
 
    请注意，您将看不到任何以前存储在 SQL 数据库中快速入门教程中的项目。

    >[AZURE.NOTE]当您使用**立即尝试**按钮启动帮助 API 页面时，请务必提供您应用程序的密钥的密码 （使用空的用户名）。

3. 创建一个新项。 

    应用程序和移动服务应该会像之前，除了现在您的数据存储在 SQL 数据库中非关系而不是存储。

##下一步行动

现在，您已经看到表存储使用.NET 后端是多么容易，请考虑探索一些其他后端存储选项︰

+ [生成.NET 后端使用表格存储，而不是 SQL 数据库的移动服务](mobile-services-dotnet-backend-store-data-table-storage.md)</br>像刚刚完成本教程，本主题演示您如何使用您的移动服务的非关系型数据存储。 在本教程中，您将修改作为数据存储区而不是 SQL 数据库使用 Azure 存储移动服务快速启动项目。
 
+ [连接到本地 SQL Server 使用混合连接](mobile-services-dotnet-backend-hybrid-connections-get-started.md)</br>混合连接可以让您安全地连接到您的内部资产的移动服务。 这种方式，您可以使内部数据移动客户端可以访问使用 Azure。 支持的资产包括运行在一个静态的 TCP 端口，包括 Microsoft SQL Server，MySQL，HTTP Web Api 和大多数自定义 web 服务的任何资源。

+ [将图像传到使用移动服务的 Azure 存储](mobile-services-dotnet-backend-windows-store-dotnet-upload-data-blob-storage.md)</br>演示如何扩展任务列表示例项目，以使您可以将图像从您的应用程序到 Azure Blob 存储上载。


<!-- Anchors. -->
[创建非关系型存储]: #create-store
[修改数据和控制器]: #modify-service
[测试应用程序]: #test-application


<!-- Images. -->
[0]: ./media/mobile-services-dotnet-backend-use-non-relational-data-store/create-mongo-lab.png
[1]: ./media/mobile-services-dotnet-backend-use-non-relational-data-store/mongo-connection-string.png


<!-- URLs. -->
[开始使用移动服务]: mobile-services-dotnet-backend-windows-store-dotnet-get-started.md
[添加到现有应用程序的移动服务]: ../mobile-services-dotnet-backend-windows-store-dotnet-get-started-data.md
[Azure 的管理门户]: https://manage.windowsazure.com/
[表服务是什么]: ../storage-dotnet-how-to-use-tables.md#what-is
[MongoLab 附加页]: /gallery/store/mongolab/mongolab
 
测试

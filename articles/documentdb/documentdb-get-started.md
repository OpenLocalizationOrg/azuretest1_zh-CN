---
ms.openlocfilehash: ae88b022207018ac38821a5587da7661e963defb
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="开始使用 DocumentDB.NET SDK |Microsoft Azure"
    description="了解如何创建和配置一个 Azure DocumentDB 帐户、 创建数据库、 创建集合，和存储文档 NoSQL 数据库帐户内部的 JSON 文档。"
    services="documentdb"
    documentationCenter=".net"
    authors="AndrewHoh"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article" 
    ms.date="09/03/2015"
    ms.author="anhoh"/>

#开始使用 DocumentDB.NET SDK  

本教程展示了如何开始使用[Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/)和[DocumentDB.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/)。 您将构建的控制台应用程序创建和查询 DocumentDB 资源，并将输出写入到控制台窗口。

DocumentDB 是 NoSQL 文档数据库服务，有[的 Api 和 Sdk 可数](https://msdn.microsoft.com/library/dn781482.aspx)。 这篇文章中的代码用 C# 编写的并使用 DocumentDB.NET SDK，打包并作为 NuGet 程序包分发。

本文包括以下方案︰

- 创建和连接到一个 DocumentDB 帐户
- 将 DocumentDB 添加到 Visual Studio 解决方案
- 创建数据库
- 创建集合
- 创建 JSON 文档
- 查询的资源
- 删除数据库

不要有时间来完成本教程，只是想要获得的工作解决方案？ 没有方面的担忧。 在[GitHub](https://github.com/Azure/azure-documentdb-net/tree/master/tutorials/get-started)上提供了完整的解决方案。 快速的说明，请参阅[获得完整的解决方案](#GetSolution)。 

一旦您完成了本教程，请在的开头或结尾的主题使用投票按钮，让我们知道我们做的那样。 本主题是主动更新，因此我们需要您的反馈意见改进它。 如果您希望我们与您联系，随时跟进的注释中包含您的电子邮件地址。   

## 先决条件

在这篇文章中的说明进行操作之前，应确保您具备以下︰

- 活动的 Azure 帐户。 如果您没有帐户，您可以在几分钟创建免费的试用帐户。 有关详细信息，请参阅[Azure 免费试用版](http://azure.microsoft.com/pricing/free-trial/)。
- [Visual Studio 2013 年](http://www.visualstudio.com/)4 或更高版本的更新。

## 步骤 1︰ 创建一个 DocumentDB 帐户

用于创建 DocumentDB 帐户入手。 如果您已经有一个帐户，您可以跳到[安装 Visual Studio 解决方案](#SetupVS)。

[AZURE.INCLUDE [documentdb-create-dbaccount](../../includes/documentdb-create-dbaccount.md)]

##<a id="SetupVS"></a> 步骤 2︰ 设置 Visual Studio 解决方案

1. 在您的计算机上打开**Visual Studio** 。
2. 从**文件**菜单中选择**新建**，然后选择**项目**。
3. 在**新建项目对话框**中，选择**模板** / **C#** / **控制台应用程序**，项目，命名，然后再单击**添加**。
4. 在**解决方案资源管理器**中右键单击新控制台应用程序，即在 Visual Studio 解决方案。
5. 再无需离开菜单上，单击上**管理 NuGet 程序包...**
6. 在左边的大多数面板**管理 NuGet 程序包**窗口中，单击**在线** / **nuget.org**。
7. 在**联机搜索**输入框，搜索**DocumentDB 客户端库**。
8. 在结果中，查找**Microsoft Azure DocumentDB 客户端库**，然后单击**安装**。  
   DocumentDB 客户端库的包 ID 为[Microsoft.Azure.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB)

很好 ！ 现在您可以开始使用 DocumentDB。

##<a id="Connect"></a> 第 3 步︰ 连接到一个 DocumentDB 帐户

我们首先要建立与我们的 DocumentDB 帐户创建[DocumentClient](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.aspx)类的一个新实例。   在我们的 C# 应用程序开始，我们都需要以下参考资料︰  

    using Microsoft.Azure.Documents;
    using Microsoft.Azure.Documents.Client;
    using Microsoft.Azure.Documents.Linq;
    using Newtonsoft.Json;

接下来，可以立即**DocumentClient**使用 DocumentDB 帐户终结点和用来与该帐户相关联的主要或次要的访问键。 将这些属性添加到您的类中。

    private static string EndpointUrl = "<your endpoint URI>";
    private static string AuthorizationKey = "<your key>";

现在让我们创建一个新的异步任务，在您的类中调用**GetStartedDemo** 。 在此新任务创建并设置您的**DocumentClient**。

    private static async Task GetStartedDemo()
    {
        // Create a new instance of the DocumentClient.
        var client = new DocumentClient(new Uri(EndpointUrl), AuthorizationKey);
    }

调用的 Main 方法的异步任务类似于下面的代码。

    public static void Main(string[] args)
    {
        try
        {
            GetStartedDemo().Wait();
        }
        catch (Exception e)
        {
            Exception baseException = e.GetBaseException();
            Console.WriteLine("Error: {0}, Message: {1}", e.Message, baseException.Message);
        }
    }

> [AZURE.WARNING] 永远不会将凭据存储在源代码中。 若要使此示例尽量简单，凭据所示的源代码。 请参阅[Azure 网站︰ 如何应用字符串和连接字符串的工作](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/)有关如何在生产环境中存储凭据的信息。 存储凭据的源代码之外例如， [GitHub](https://github.com/Azure/azure-documentdb-net/blob/master/tutorials/get-started/src/Program.cs)上看一看我们的示例应用程序。

EndpointUrl 和 AuthorizationKey 的值是 URI 和为您的 DocumentDB 帐户，可为您的 DocumentDB 帐户[密钥](https://portal.azure.com)刀片式服务器中获取主键。

![显示突出显示活动中心、 键按钮突出显示在 DocumentDB 帐户刀片式服务器，和 URI、 为主键和辅助键值突出显示键刀片式服务器上的 DocumentDB 帐户，Azure 预览门户的屏幕抓图][keys]

这些键将授予对您的 DocumentDB 帐户和资源的管理访问权限。 DocumentDB 还支持使用允许客户端读取、 写入和删除根据您已授予的而不需要帐户密钥的权限的 DocumentDB 帐户中的资源的资源键。 资源键的详细信息，请参阅[权限](documentdb-resources.md#permissions)和[查看、 复制和再生的访问键](documentdb-manage-account.md#keys)。

您已经知道了如何连接到一个 DocumentDB 帐户，并创建**DocumentClient**类的一个实例，让我们看看如何使用 DocumentDB 资源。  

## 步骤 4︰ 创建数据库
通过使用[CreateDatabaseAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx)方法的**DocumentClient**类可以创建[数据库](documentdb-resources.md#databases)。 数据库是跨集合划分文档存储的逻辑容器。 **DocumentClient**创建后在**GetStartedDemo**方法中创建新的数据库。

    // Create a database.
    Database database = await client.CreateDatabaseAsync(
        new Database
            {
                Id = "FamilyRegistry"
            });

##<a id="CreateColl"></a>步骤 5︰ 创建集合  

> [AZURE.WARNING] **CreateDocumentCollectionAsync**将创建新的 S1 集合，它具有定价的影响。 有关详细信息，请访问我们的[定价页](https://azure.microsoft.com/pricing/details/documentdb/)。

可以通过使用**DocumentClient**类的[CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx)方法创建一个[集合](documentdb-resources.md#collections)。 集合是 JSON 文档和关联的 JavaScript 应用程序逻辑的容器。 新创建的集合将被映射到[S1 的性能级别](documentdb-performance-levels.md)。 在上一步中创建的数据库具有的大量的属性，其中之一是[CollectionsLink](https://msdn.microsoft.com/library/microsoft.azure.documents.database.collectionslink.aspx)属性。  使用该信息，我们现在可以在我们的数据库创建之后创建一个集合。

    // Create a document collection.
    DocumentCollection documentCollection = await client.CreateDocumentCollectionAsync(database.CollectionsLink,
        new DocumentCollection
            {
                Id = "FamilyCollection"
            });

##<a id="CreateDoc"></a>第 6 步︰ 创建文档
通过使用[CreateDocumentAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx)方法的**DocumentClient**类，可以创建[文档](documentdb-resources.md#documents)。 文档是用户定义的 （任意） JSON 内容。 在上一步中创建的集合具有的大量的属性，其中之一是[DocumentsLink](https://msdn.microsoft.com/library/microsoft.azure.documents.documentcollection.documentslink.aspx)属性。  这些信息，我们可以将一个或多个文档。 如果您已经有了想要在数据库中存储的数据，您可以使用 DocumentDB 的[数据迁移工具](documentdb-import-data.md)。

首先，我们需要创建**父**、**子**、**宠物**、**地址**和**家族**类。 通过添加以下内部子类创建这些类。

    internal sealed class Parent
    {
        public string FamilyName { get; set; }
        public string FirstName { get; set; }
    }

    internal sealed class Child
    {
        public string FamilyName { get; set; }
        public string FirstName { get; set; }
        public string Gender { get; set; }
        public int Grade { get; set; }
        public Pet[] Pets { get; set; }
    }

    internal sealed class Pet
    {
        public string GivenName { get; set; }
    }

    internal sealed class Address
    {
        public string State { get; set; }
        public string County { get; set; }
        public string City { get; set; }
    }

    internal sealed class Family
    {
        [JsonProperty(PropertyName = "id")]
        public string Id { get; set; }
        public string LastName { get; set; }
        public Parent[] Parents { get; set; }
        public Child[] Children { get; set; }
        public Address Address { get; set; }
        public bool IsRegistered { get; set; }
    }

接下来，创建您的文档中**GetStartedDemo**异步方法。

    // Create the Andersen family document.
    Family AndersenFamily = new Family
    {
        Id = "AndersenFamily",
        LastName = "Andersen",
        Parents =  new Parent[] {
            new Parent { FirstName = "Thomas" },
            new Parent { FirstName = "Mary Kay"}
        },
        Children = new Child[] {
            new Child {
                FirstName = "Henriette Thaulow",
                Gender = "female",
                Grade = 5,
                Pets = new Pet[] {
                    new Pet { GivenName = "Fluffy" }
                }
            }
        },
        Address = new Address { State = "WA", County = "King", City = "Seattle" },
        IsRegistered = true
    };

    await client.CreateDocumentAsync(documentCollection.DocumentsLink, AndersenFamily);

    // Create the WakeField family document.
    Family WakefieldFamily = new Family
    {
        Id = "WakefieldFamily",
        Parents = new Parent[] {
            new Parent { FamilyName= "Wakefield", FirstName= "Robin" },
            new Parent { FamilyName= "Miller", FirstName= "Ben" }
        },
        Children = new Child[] {
            new Child {
                FamilyName= "Merriam",
                FirstName= "Jesse",
                Gender= "female",
                Grade= 8,
                Pets= new Pet[] {
                    new Pet { GivenName= "Goofy" },
                    new Pet { GivenName= "Shadow" }
                }
            },
            new Child {
                FamilyName= "Miller",
                FirstName= "Lisa",
                Gender= "female",
                Grade= 1
            }
        },
        Address = new Address { State = "NY", County = "Manhattan", City = "NY" },
        IsRegistered = false
    };

    await client.CreateDocumentAsync(documentCollection.DocumentsLink, WakefieldFamily);

现在已经在您的 DocumentDB 帐户中创建下面的数据库、 收集和文档。

![用图表演示帐户、 数据库、 集合和文档之间的层次关系](./media/documentdb-get-started/account-database.png)

##<a id="Query"></a>第 7 步︰ 查询 DocumentDB 资源

DocumentDB 支持丰富[查询](documentdb-sql-query.md)存储在每个集合中的 JSON 文档。  下面的代码示例显示了各种查询-使用两种 DocumentDB SQL 语法上一步中，我们可以对运行文档我们插入以及 LINQ 的。 将这些查询添加到**GetStartedDemo**异步方法。

    // Query the documents using DocumentDB SQL for the Andersen family.
    var families = client.CreateDocumentQuery(documentCollection.DocumentsLink,
        "SELECT * " +
        "FROM Families f " +
        "WHERE f.id = \"AndersenFamily\"");

    foreach (var family in families)
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }

    // Query the documents using LINQ for the Andersen family.
    families =
        from f in client.CreateDocumentQuery(documentCollection.DocumentsLink)
        where f.Id == "AndersenFamily"
        select f;

    foreach (var family in families)
    {
        Console.WriteLine("\tRead {0} from LINQ", family);
    }

    // Query the documents using LINQ lambdas for the Andersen family.
    families = client.CreateDocumentQuery(documentCollection.DocumentsLink)
        .Where(f => f.Id == "AndersenFamily")
        .Select(f => f);

    foreach (var family in families)
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }

    // Query the documents using DocumentSQL with one join.
    var items = client.CreateDocumentQuery<dynamic>(documentCollection.DocumentsLink,
        "SELECT f.id, c.FirstName AS child " +
        "FROM Families f " +
        "JOIN c IN f.Children");

    foreach (var item in items.ToList())
    {
        Console.WriteLine(item);
    }

    // Query the documents using LINQ with one join.
    items = client.CreateDocumentQuery<Family>(documentCollection.DocumentsLink)
        .SelectMany(family => family.Children
            .Select(children => new
            {
                family = family.Id,
                child = children.FirstName
            }));

    foreach (var item in items.ToList())
    {
        Console.WriteLine(item);
    }

下图演示对 LINQ 查询以及如何应用 DocumentDB SQL 查询语法调用针对集合的创建和相同的逻辑。

![图中展示范围和含义的查询](./media/documentdb-get-started/collection-documents.png)

[FROM](documentdb-sql-query.md/#from-clause)关键字是可选的查询，因为 DocumentDB 查询已作用于单个集合。 因此，"从家族 f"可以使用更换"从根 r"或任何其他变量命名为您选择。 DocumentDB 将推断该系列、 根或您选择的变量名称，默认情况下引用的当前集合。

##<a id="DeleteDatabase"></a>步骤 8︰ 删除数据库

删除创建的数据库将删除数据库及其所有的子资源 （集合、 文档等）。 您可以通过将下面的代码段添加到**GetStartedDemo**异步方法的末尾删除数据库和文档客户端。

    // Clean up/delete the database
    await client.DeleteDatabaseAsync(database.SelfLink);
    client.Dispose();

##<a id="Run"></a>步骤 9︰ 运行您的应用程序 ！

现在您可以运行您的应用程序。 在**Main**方法的末尾，添加下面的代码，这会让您读取控制台输出应用程序完成运行之前行。

    Console.ReadLine();

现在按 F5 在 Visual Studio 生成该应用程序在调试模式下。

您现在应该看到您获取已启动的应用程序的输出。 输出将显示查询我们添加并应与下面的示例文本相匹配的结果。

    Read {
      "id": "AndersenFamily",
      "LastName": "Andersen",
      "Parents": [
        {
          "FamilyName": null,
          "FirstName": "Thomas"
        },
        {
          "FamilyName": null,
          "FirstName": "Mary Kay"
        }
      ],
      "Children": [
        {
          "FamilyName": null,
          "FirstName": "Henriette Thaulow",
          "Gender": "female",
          "Grade": 5,
          "Pets": [
            {
              "GivenName": "Fluffy"
            }
          ]
        }
      ],
      "Address": {
        "State": "WA",
        "County": "King",
        "City": "Seattle"
      },
      "IsRegistered": true,
      "_rid": "ybVlALUoqAEBAAAAAAAAAA==",
      "_ts": 1428372205,
      "_self": "dbs/ybVlAA==/colls/ybVlALUoqAE=/docs/ybVlALUoqAEBAAAAAAAAAA==/",
      "_etag": "\"0000400c-0000-0000-0000-55233aed0000\"",
      "_attachments": "attachments/"
    } from SQL
    Read {
      "id": "AndersenFamily",
      "LastName": "Andersen",
      "Parents": [
        {
          "FamilyName": null,
          "FirstName": "Thomas"
        },
        {
          "FamilyName": null,
          "FirstName": "Mary Kay"
        }
      ],
      "Children": [
        {
          "FamilyName": null,
          "FirstName": "Henriette Thaulow",
          "Gender": "female",
          "Grade": 5,
          "Pets": [
            {
              "GivenName": "Fluffy"
            }
          ]
        }
      ],
      "Address": {
        "State": "WA",
        "County": "King",
        "City": "Seattle"
      },
      "IsRegistered": true,
      "_rid": "ybVlALUoqAEBAAAAAAAAAA==",
      "_ts": 1428372205,
      "_self": "dbs/ybVlAA==/colls/ybVlALUoqAE=/docs/ybVlALUoqAEBAAAAAAAAAA==/",
      "_etag": "\"0000400c-0000-0000-0000-55233aed0000\"",
      "_attachments": "attachments/"
    } from LINQ
    Read {
      "id": "AndersenFamily",
      "LastName": "Andersen",
      "Parents": [
        {
          "FamilyName": null,
          "FirstName": "Thomas"
        },
        {
          "FamilyName": null,
          "FirstName": "Mary Kay"
        }
      ],
      "Children": [
        {
          "FamilyName": null,
          "FirstName": "Henriette Thaulow",
          "Gender": "female",
          "Grade": 5,
          "Pets": [
            {
              "GivenName": "Fluffy"
            }
          ]
        }
      ],
      "Address": {
        "State": "WA",
        "County": "King",
        "City": "Seattle"
      },
      "IsRegistered": true,
      "_rid": "ybVlALUoqAEBAAAAAAAAAA==",
      "_ts": 1428372205,
      "_self": "dbs/ybVlAA==/colls/ybVlALUoqAE=/docs/ybVlALUoqAEBAAAAAAAAAA==/",
      "_etag": "\"0000400c-0000-0000-0000-55233aed0000\"",
      "_attachments": "attachments/"
    } from LINQ query
    {
      "id": "AndersenFamily",
      "child": "Henriette Thaulow"
    }
    {
      "id": "WakefieldFamily",
      "child": "Jesse"
    }
    {
      "id": "WakefieldFamily",
      "child": "Lisa"
    }
    { family = AndersenFamily, child = Henriette Thaulow }
    { family = WakefieldFamily, child = Jesse }
    { family = WakefieldFamily, child = Lisa }


> [AZURE.NOTE] 如果不删除该数据库的情况下多次运行该应用程序，可能会遇到创建新的数据库 id 已在使用中的问题。 若要避免此问题，您可以检查数据库，集合，或者已经存在具有相同 id 的文档。 在如何实现这一个引用，请访问我们[GitHub 页](https://github.com/Azure/azure-documentdb-net/tree/master/tutorials/get-started)。

Contratulations ！ 您已经创建了您的第一个 DocumentDB 应用程序 ！ 

##<a id="GetSolution"></a> 获取完整的解决方案
若要生成包含本文中的所有示例的 GetStarted 解决方案，您将需要︰

-   [DocumentDB 帐户][documentdb 创建帐户]。
-   在 GitHub 上可用的[GetStarted](https://github.com/Azure/azure-documentdb-net/tree/master/tutorials/get-started)解决方案。

要恢复在 Visual Studio 2013年 DocumentDB.NET SDK 参考， **GetStarted**解决方案中的解决方案资源管理器中，用鼠标右键单击，然后单击**启用还原 NuGet 程序包**。 接下来，在 App.config 文件中，更新的 EndpointUrl 和 AuthorizationKey 值[连接到 DocumentDB 帐户](#Connect)中所述。

## 下一步行动

-   想要一个更复杂的 ASP.NET MVC 样本？ 请参阅[生成和使用 DocumentDB 的 ASP.NET MVC web 应用程序](documentdb-dotnet-application.md)。
-   了解如何[监视 DocumentDB 帐户](documentdb-monitor-accounts.md)。
-   对[查询运动场](https://www.documentdb.com/sql/demo)在我们的示例数据集运行查询。
-   了解有关在[文档页中找到 DocumentDB](../../services/documentdb/)的开发部分的编程模型。

[doc 登录页]: ../../services/documentdb/
[documentdb 创建帐户]: documentdb-create-account.md
[documentdb-管理]: documentdb-manage.md

[密钥]: media/documentdb-get-started/keys.png
 

测试

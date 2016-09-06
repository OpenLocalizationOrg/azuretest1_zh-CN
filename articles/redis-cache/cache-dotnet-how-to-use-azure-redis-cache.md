---
ms.openlocfilehash: 0143bdbdddacbd5b1715bec17c017ce9843037ea
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何使用 Azure 的 Redis 高速缓存" 
    description="了解如何提高 Azure Redis 缓存了 Azure 应用程序的性能" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="dotnet" 
    ms.topic="hero-article" 
    ms.date="08/25/2015" 
    ms.author="sdanie"/>

# 如何使用 Azure 的 Redis 高速缓存

本指南介绍了如何开始使用**Azure Redis 高速缓存**。 用 c 语言编写的示例\#的代码，并使用[StackExchange.Redis][]客户端。 所包含的方案包括**创建和配置缓存**、**配置缓存客户端**、**添加和移除缓存中的对象**和**存储 ASP.NET 缓存中的会话状态**。 有关使用 Azure Redis 高速缓存的详细信息，请参阅[后续步骤][]部分。

<a name="what-is"></a>
## 什么是 Azure Redis 高速缓存？

Microsoft Azure Redis 缓存基于流行的开源 Redis 高速缓存。 到一个安全的、 专用 Redis 高速缓存，由 Microsoft 使您可以访问它。 创建使用 Azure Redis 缓存缓存是从 Microsoft Azure 中的任何应用程序访问。

Microsoft Azure Redis 高速缓存有两层︰

-   **基本**-单个节点。 多个调整为 53 GB。
-   **标准**— 双节点主复制副本。 多个调整为 53 GB。 99.9%的 SLA。

每一层不同的功能和价格方面。 功能包括在本指南的后面部分和有关定价的详细信息，请参阅[缓存定价详细信息][]。

本指南提供入门 Azure Redis 缓存的概述。 此入门指南的范围之内的这些功能的更多详细信息指导，请参阅[Azure 的 Redis 缓存概述][]。

<a name="getting-started-cache-service"></a>
## 学习如何使用 Azure 的 Redis 高速缓存

开始使用 Azure Redis 高速缓存可以很容易。 若要开始，调配和配置缓存。 下一步，这样他们才可以访问缓存配置缓存客户端。 缓存客户端配置，您可以开始使用它们。

-   [创建高速缓存][]
-   [配置缓存客户端][]

<a name="create-cache"></a>
## 创建一个缓存

若要创建一个缓存，首次登录到[Azure 预览门户][]，并单击**新建****数据 + 存储**、 **Redis 高速缓存**。

![新的缓存][NewCacheMenu]

>[AZURE.NOTE] 如果您没有 Azure 帐户，您可以在几分钟创建免费试用帐户。 有关详细信息，请参阅[Azure 免费试用版][]。

在**新的 Redis 高速缓存**刀片式服务器，指定高速缓存所需的配置。

![创建高速缓存][CacheCreate]

在**Dns 名称**中，输入要使用缓存端点的子域名称。 终结点必须是一个字符串六个和 20 个字符之间，只包含小写字母数字和字母，并且必须以字母开头。

使用**定价层**来选择所需的高速缓存大小和功能。 **基本**高速缓存具有单个节点具有多个调整为 53 GB。 **标准**的缓存有两个节点主/副本配置有 99.9%的 SLA 中，和多个大小为 53 GB。

在**资源组**中，选择或创建您的缓存的资源组。

>[AZURE.NOTE] 有关详细信息，请参阅[管理 Azure 资源使用的资源组][]。 

对于**订阅**，选择要用于缓存的 Azure 订阅。 如果您的帐户有只有一个预订，将自动选择，并将不会显示**订阅**下拉列表。

使用**位置**指定的地理位置，位于您的缓存。 为了获得最佳性能，Microsoft 强烈建议您在同一区域作为缓存客户端应用程序中创建缓存。

一旦配置了新的高速缓存选项，请单击**创建**。 可能需要几分钟时间来创建缓存。 若要检查状态，您可以监视在 startboard 上的进度。 在创建缓存之后，新的缓存**运行**状态和使用默认设置即可使用。

![创建高速缓存][CacheCreated]

缓存创建后，您可以从**浏览**刀片式服务器来访问它。 

![浏览刀片式服务器][BrowseCaches]

单击**Redis 缓存**以查看您的缓存。

![高速缓存][Caches]

<a name="NuGet"></a>
## 配置缓存客户端

创建使用 Azure Redis 缓存缓存是可以从任何 Azure 应用程序访问。 在 Visual Studio 开发.NET 应用程序可以使用**StackExchange.Redis**缓存客户端，可以使用 NuGet 程序包，可以简化配置缓存客户端应用程序的配置。 

>[AZURE.NOTE] 有关详细信息，请参阅[StackExchange.Redis][] github 页和[StackExchange.Redis 缓存客户端文档][]。

要配置客户端应用程序使用 StackExchange.Redis NuGet 程序包的 Visual Studio 中，用鼠标右键单击**解决方案资源管理器**中的项目，然后选择**管理 NuGet 程序包**。 

![管理 NuGet 程序包][NuGetMenu]

**联机搜索**文本框中键入**StackExchange.Redis**或**StackExchange.Redis.StrongName** ，从结果中，选择所需的版本并单击**安装**。

>[AZURE.NOTE] 如果您希望使用具有强名称的**StackExchange.Redis**客户端库的版本，选择**StackExchange.Redis.StrongName**;否则，请选择**StackExchange.Redis**。

![StackExchange.Redis NuGet 程序包][StackExchangeNuget]

NuGet 程序包下载，并将为客户端应用程序与 StackExchange.Redis 缓存客户端访问 Redis Azure 的高速缓存所需程序集引用添加。

一旦客户端项目配置为缓存，您可以使用缓存中使用以下各节中介绍的技术。

<a name="working-with-caches"></a>
## 使用高速缓存

本节中的步骤介绍如何使用缓存执行常规任务。

-   [连接到高速缓存][]
-   [添加和从缓存中检索对象][]
-   [将 ASP.NET 会话状态存储在缓存中][]

<a name="connect-to-cache"></a>
## 连接到高速缓存

为了以编程方式使用缓存，您需要对缓存的引用。 您想要使用 StackExchange.Redis 客户端来访问 Redis Azure 缓存的任何文件的顶部添加以下项︰

    using StackExchange.Redis;

>[AZURE.NOTE] StackExchange.Redis 客户端需要.NET Framework 4 或更高版本。

到 Azure Redis 高速缓存的连接由`ConnectionMultiplexer`类。 此类旨在共享和重用整个客户端应用程序，并不需要在每个操作的基础上创建。 

连接到 Azure Redis 缓存并将返回的连接实例`ConnectionMultiplexer`，调用静态`Connect`方法并传入缓存端点和密钥的步骤如下面的示例。 使用 Azure 从预览门户作为密码参数生成的密钥。

    ConnectionMultiplexer connection = ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");

>[AZURE.IMPORTANT] 警告︰ 永远不会存储凭据在源代码中。 若要使此示例尽量简单，我在源代码中显示它们。 如何存储凭据的信息，请参阅[如何应用字符串和连接字符串的工作][]。

如果您不想使用 SSL，请将设置`ssl=false`或省略`ssl`参数。

>[AZURE.NOTE] 默认情况下，新的高速缓存禁用非 SSL 端口。 启用非 SSL 端口的说明，请参阅[Redis Azure 的高速缓存中的缓存配置][]主题中的访问端口部分。

高级的连接配置选项的详细信息，请参阅[StackExchange.Redis 配置模型][]。

高速缓存端点和键可以从**Redis 缓存**刀片式服务器的缓存实例。

![缓存属性][CacheProperties]

![管理密钥][ManageKeys]

一旦建立连接，则通过调用返回 redis 缓存数据库引用`ConnectionMultiplexer.GetDatabase`方法。

    // connection refers to a previously configured ConnectionMultiplexer
    IDatabase cache = connection.GetDatabase();

>[AZURE.NOTE] 从返回的对象`GetDatabase`方法是轻量的传递对象，并不需要存储。

    ConnectionMultiplexer connection = ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");

    IDatabase cache = connection.GetDatabase();

    // Perform cache operations using the cache object...
    // Simple put of integral data types into the cache
    cache.StringSet("key1", "value");
    cache.StringSet("key2", 25);

    // Simple get of data types from the cache
    string key1 = cache.StringGet("key1");
    int key2 = (int)cache.StringGet("key2");

您已经知道了如何连接到 Azure Redis 缓存实例并返回缓存数据库的参考，让我们看看使用缓存。

<a name="add-object"></a>
## 添加和从缓存中检索对象

可以存储在中并通过从缓存中检索项`StringSet`和`StringGet`的方法。

    // If key1 exists, it is overwritten.
    cache.StringSet("key1", "value1");

    string value = cache.StringGet("key1");

>[AZURE.NOTE] Redis 大多数数据为 Redis 的字符串，但这些字符串可以包含多种类型的数据，包括序列化时，可以使用存储.NET 对象在缓存中的二进制数据的存储区。

当调用`StringGet`，如果该对象存在，则返回它，并且如果不包含，则返回 null。 在这种情况下可以从所需的数据源中检索值，并将其存储在缓存中以便将来使用。 这就是缓存模式。

    string value = cache.StringGet("key1");
    if (value == null)
    {
        // The item keyed by "key1" is not in the cache. Obtain
        // it from the desired data source and add it to the cache.
        value = GetValueFromDataSource();

        cache.StringSet("key1", value);
    }

>[AZURE.NOTE] Azure Redis 高速缓存可以缓存.NET 对象以及基元数据类型，但可以缓存一个.NET 对象之前它必须进行序列化。 这是应用程序开发人员的责任，让开发人员能够灵活选择的序列化程序。 有关详细信息，请参阅[使用.NET 对象在缓存中][]。

<a name="specify-expiration"></a>
## 指定缓存中的项的过期日期

若要指定在缓存中某项的过期日期，请使用`TimeSpan`参数的`StringSet`。

    cache.StringSet("key1", "value1", TimeSpan.FromMinutes(90));


<a name="store-session"></a>
## 将 ASP.NET 会话状态存储在缓存中

Azure Redis 缓存提供了一个会话状态提供程序，可用于存储会话状态，而不是内存中的缓存或 SQL Server 数据库中。 要使用缓存的会话状态提供程序，请首先配置高速缓存，然后配置 ASP.NET 应用程序使用 Redis 缓存会话状态 NuGet 程序包的缓存。

要配置客户端应用程序使用 Redis 缓存会话状态 NuGet 程序包的 Visual Studio 中，用鼠标右键单击**解决方案资源管理器**中的项目，然后选择**管理 NuGet 程序包**。 

![管理 NuGet 程序包][NuGetMenu]

在**联机搜索**文本框中键入**RedisSessionStateProvider** ，从结果中，选择它，单击**安装**。

![Redis 高速缓存会话状态 NuGet 程序包][SessionStateNuGet]

NuGet 程序包下载并添加所需的程序集引用并添加下列添加到 web.config 文件，其中包含有关 ASP.NET 应用程序使用 Redis 的高速缓存会话状态提供程序所需的配置下一节。

    <sessionState mode="Custom" customProvider="MySessionStateStore">
      <providers>
        <!--
          <add name="MySessionStateStore" 
            host = "127.0.0.1" [String]
            port = "" [number]
            accessKey = "" [String]
            ssl = "false" [true|false]
            throwOnError = "true" [true|false]
            retryTimeoutInMilliseconds = "0" [number]
            databaseId = "0" [number]
            applicationName = "" [String]
            connectionTimeoutInMilliseconds = "5000" [number]
            operationTimeoutInMilliseconds = "5000" [number]
          />
        -->
        <add name="MySessionStateStore" type="Microsoft.Web.Redis.RedisSessionStateProvider" host="127.0.0.1" accessKey="" ssl="false" />
      </providers>
    </sessionState>

注释部分中提供属性和它们的示例设置的示例。

配置在预览门户，您缓存刀片式服务器中的值的属性，并根据需要配置其他值。

    <sessionState mode="Custom" customProvider="MySessionStateStore">
      <providers>
        <!--
          <add name="MySessionStateStore" 
            host = "127.0.0.1" [String]
            port = "" [number]
            accessKey = "" [String]
            ssl = "false" [true|false]
            throwOnError = "true" [true|false]
            retryTimeoutInMilliseconds = "0" [number]
            databaseId = "0" [number]
            applicationName = "" [String]
            connectionTimeoutInMilliseconds = "5000" [number]
            operationTimeoutInMilliseconds = "5000" [number]
          />
        -->
        <add name="MySessionStateStore" type="Microsoft.Web.Redis.RedisSessionStateProvider" host="contoso5.redis.cache.windows.net" 
        accessKey="..." ssl="true" />
      </providers>
    </sessionState>

一定要注释掉标准**进程内**会话状态提供程序。

    <!-- <sessionState mode="InProc" customProvider="DefaultSessionProvider">
      <providers>
        <add name="DefaultSessionProvider" type="System.Web.Providers.DefaultSessionStateProvider, System.Web.Providers, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35" connectionStringName="DefaultConnection" />
      </providers>
    </sessionState> -->

有关配置这些设置和使用 Azure Redis 会话状态提供程序的详细信息，请参阅[Azure Redis 会话状态提供程序][]。

<a name="next-steps"></a>
## 下一步行动

既然已经学习了 Azure Redis 缓存的基本知识，按照这些链接以了解如何执行更复杂的缓存任务。

-   [启用缓存诊断](cache-how-to-monitor.md#enable-cache-diagnostics)以便 [高速缓存 monitor.md 如何) 缓存的运行状况。 您可以预览门户中查看规格，您还可以[下载并查看](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring)其使用您选择的工具。
-   检出[StackExchange.Redis 缓存客户端文档][]。
    -   可以从许多 Redis 客户端和开发语言访问 azure Redis 高速缓存。 有关详细信息，请参阅[http://redis.io/clients][]和[Redis Azure 的高速缓存的其他语言的发展][]。
    -   也可使用服务，如 Redsmin azure Redis 高速缓存。 有关详细信息，请参阅[如何检索 Azure Redis 连接字符串并将其与 Redsmin][]。
-   请参阅[redis][]文档并阅读有关[redis 数据类型][]和[十五分钟简介 Redis 数据类型][]。
-   [Azure Redis 高速缓存][]请参阅 MSDN 参考。 


<!-- INTRA-TOPIC LINKS -->
[下一步行动]: #next-steps
[Azure 简介 Redis 缓存 （视频）]: #video
[什么是 Azure Redis 高速缓存？]: #what-is
[创建一个 Azure 的缓存]: #create-cache
[哪种类型的缓存最适合我？]: #choosing-cache
[准备您的 Visual Studio 项目以使用 Azure 缓存]: #prepare-vs
[配置应用程序使用缓存]: #configure-app
[学习如何使用 Azure 的 Redis 高速缓存]: #getting-started-cache-service
[创建高速缓存]: #create-cache
[配置缓存]: #enable-caching
[配置缓存客户端]: #NuGet
[使用高速缓存]: #working-with-caches
[连接到高速缓存]: #connect-to-cache
[添加和从缓存中检索对象]: #add-object
[指定缓存中的对象的过期日期]: #specify-expiration
[将 ASP.NET 会话状态存储在缓存中]: #store-session

  
<!-- IMAGES -->
[NewCacheMenu]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-new-cache-menu.png

[CacheCreate]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-cache-create.png

[StackExchangeNuget]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-stackexchange-redis.png

[NuGetMenu]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-manage-nuget-menu.png

[CacheProperties]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-properties.png

[ManageKeys]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-manage-keys.png

[SessionStateNuGet]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-session-state-provider.png

[BrowseCaches]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-browse-caches.png

[高速缓存]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-caches.png

[CacheCreated]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-cache-created.png




   
<!-- LINKS -->
[http://redis.io/clients]: http://redis.io/clients
[在其他语言中制定的 Azure Redis 高速缓存]: http://msdn.microsoft.com/library/azure/dn690470.aspx
[如何检索 Azure Redis 连接字符串并将其与 Redsmin]: https://redsmin.uservoice.com/knowledgebase/articles/485711-how-to-connect-redsmin-to-azure-redis-cache
[Azure Redis 会话状态提供程序]: http://go.microsoft.com/fwlink/?LinkId=398249
[如何︰ 以编程方式配置缓存客户端]: http://msdn.microsoft.com/library/windowsazure/gg618003.aspx
[Azure 缓存的会话状态提供程序]: http://go.microsoft.com/fwlink/?LinkId=320835
[Azure AppFabric 缓存︰ 缓存会话状态]: http://www.microsoft.com/showcase/details.aspx?uuid=87c833e9-97a9-42b2-8bb1-7601f9b5ca20
[Azure 缓存的输出缓存提供程序]: http://go.microsoft.com/fwlink/?LinkId=320837
[Azure 的共享缓存]: http://msdn.microsoft.com/library/windowsazure/gg278356.aspx
[团队博客]: http://blogs.msdn.com/b/windowsazure/
[Azure 缓存]: http://www.microsoft.com/showcase/Search.aspx?phrase=azure+caching
[如何配置虚拟机大小]: http://go.microsoft.com/fwlink/?LinkId=164387
[Azure 缓存容量规划的考虑因素]: http://go.microsoft.com/fwlink/?LinkId=320167
[Azure 缓存]: http://go.microsoft.com/fwlink/?LinkId=252658
[方法︰ 以声明方式设置的 ASP.NET 页的可缓存性]: http://msdn.microsoft.com/library/zd1ysf1y.aspx
[如何︰ 以编程方式设置页的可缓存性]: http://msdn.microsoft.com/library/z852zf6b.aspx
[在 Azure Redis 缓存配置缓存]: http://msdn.microsoft.com/library/azure/dn793612.aspx

[StackExchange.Redis 配置模型]: http://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Configuration.md

[使用.NET 对象缓存中]: http://msdn.microsoft.com/library/dn690521.aspx#Objects


[NuGet 程序包管理器安装]: http://go.microsoft.com/fwlink/?LinkId=240311
[定价详细信息的缓存]: http://www.windowsazure.com/pricing/details/cache/
[Azure 预览门户]: https://portal.azure.com/

[Azure Redis 缓存的概述]: http://go.microsoft.com/fwlink/?LinkId=320830
[Azure 的 Redis 高速缓存]: http://go.microsoft.com/fwlink/?LinkId=398247

[将迁移到 Azure 的 Redis 高速缓存]: http://go.microsoft.com/fwlink/?LinkId=317347
[Azure Redis 缓存示例]: http://go.microsoft.com/fwlink/?LinkId=320840
[使用资源组来管理 Azure 资源]: http://azure.microsoft.com/documentation/articles/resource-group-overview/

[StackExchange.Redis]: http://github.com/StackExchange/StackExchange.Redis
[StackExchange.Redis 高速缓存客户端文档]: http://github.com/StackExchange/StackExchange.Redis#documentation

[Redis]: http://redis.io/documentation
[Redis 数据类型]: http://redis.io/topics/data-types
[Redis 数据类型十五分钟简介]: http://redis.io/topics/data-types-intro

[应用程序的字符串和连接字符串的工作方式]: http://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/

[Azure 的免费试用版]: http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero

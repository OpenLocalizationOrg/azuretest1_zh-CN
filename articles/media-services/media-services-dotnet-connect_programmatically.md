---
ms.openlocfilehash: df97f0c15af149f42b47f79ce3c4e04542d5118e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="连接到使用.NET 的媒体服务帐户" 
    description="本主题演示如何连接到媒体服务 uisng.NET。" 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/11/2015"  
    ms.author="juliako"/>


# 连接到使用.NET 媒体服务 SDK 的媒体服务帐户

> [AZURE.SELECTOR]
- [REST](media-services-rest-connect_programmatically.md)
- [.NET](media-services-dotnet-connect_programmatically.md)


本主题介绍如何使用媒体服务 SDK 编程针对.NET 时获得以编程方式连接到 Microsoft Azure 媒体服务。


## 连接到媒体服务

若要以编程方式连接到媒体服务时，您必须有以前设置 Azure 帐户帐户，配置介质服务，然后设置介质服务 SDK 的 Visual Studio 项目开发.NET。 有关详细信息，请参阅安装程序开发与媒体服务 SDK.net。

在媒体服务帐户安装过程结束时，您将获得以下所需的连接值。 使用这些控件以编程方式连接到介质服务。

- 媒体服务帐户名。

- 媒体服务帐户密钥。

要查找这些值，转到 Azure 管理门户，选择介质服务帐户，请单击的门户窗口底部的"**管理密钥**"图标。 单击每个文本框旁边的图标将值复制到系统剪贴板。


## 创建一个 CloudMediaContext 实例

开始对介质服务进行编程，您需要创建**CloudMediaContext**实例，它表示服务器上下文。 **CloudMediaContext**包括对重要包括作业、 资产、 文件、 访问策略和定位器的集合的引用。

>[AZURE.NOTE] **CloudMediaContext**类不是线程安全的。 您应该创建新的 CloudMediaContext 每个线程或每一组操作。


CloudMediaContext 有五个构造函数重载。 建议使用采用**MediaServicesCredentials**作为参数的构造函数。 有关详细信息，请参阅后面**重用访问控制服务令牌**。 

下面的示例使用 CloudMediaContext(MediaServicesCredentials credentials) 的公共构造函数︰

    // _cachedCredentials and _context are class member variables. 
    _cachedCredentials = new MediaServicesCredentials(
                    _mediaServicesAccountName,
                    _mediaServicesAccountKey);
    
    _context = new CloudMediaContext(_cachedCredentials);


## 重复使用访问控制服务标记

本节演示如何通过使用 CloudMediaContext 采用 MediaServicesCredentials 作为参数的构造函数中重复使用访问控制服务标记。


[Azure 的 Active Directory 访问控制](https://msdn.microsoft.com/library/hh147631.aspx)也称为访问控制服务 （ACS） 是提供一种简单方法进行身份验证和授权的用户才能访问他们的 web 应用程序的基于云的服务。 Microsoft Azure 媒体服务控制访问其服务尽管 OAuth 协议，要求在 ACS 令牌。 媒体服务接收来自授权服务器的 ACS 标记。

当使用介质服务 SDK 进行开发，您可以选择因为 SDK 代码管理器未处理标记它们为您。 但是，让完全管理 ACS 标记 SDK 会导致不必要的令牌请求。 请求令牌需要时间，并且会占用客户端和服务器资源。 此外，ACS 服务器限制请求，如果太高。 30 每秒请求数限制，有关更多详细信息，请参阅[ACS 服务限制](https://msdn.microsoft.com/library/gg185909.aspx)。

从介质服务 SDK 版本 3.0.0.0 开始，您可以重复使用 ACS 标记。 采用**MediaServicesCredentials**作为参数的**CloudMediaContext**的构造函数启用共享多个上下文之间的 ACS 标记。 MediaServicesCredentials 类封装的介质服务凭据。 如果 ACS 的标记并且已知其过期时间，可以使用标记中创建一个新的 MediaServicesCredentials 实例并将其传递给 CloudMediaContext 的构造函数。 请注意媒体服务 SDK 自动刷新令牌，只要它们过期。 有两种方法来重新使用 ACS 的标记，如下面的示例中所示。

- 您可以缓存在内存中 （例如，在一个静态的类变量） 的**MediaServicesCredentials**对象。 然后，将缓存的对象传递给 CloudMediaContext 的构造函数。 MediaServicesCredentials 对象包含一个可以重复使用是否仍然有效的 ACS 标记。 如果标记是无效的它将刷新介质服务 sdk 使用 MediaServicesCredentials 构造函数为指定的凭据。

    请注意**MediaServicesCredentials**对象获取有效的令牌后被称为 RefreshToken。 **CloudMediaContext**的构造函数中调用**RefreshToken**方法。 如果您计划将保存到外部存储的标记值，请务必检查保存标记的数据之前的 TokenExpiration 值是否有效。 如果它是无效的在缓存之前调用 RefreshToken。

        // Create and cache the Media Services credentials in a static class variable.
        _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey);

        
        // Use the cached credentials to create a new CloudMediaContext object.
        if(_cachedCredentials == null)
        {
            _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey);
        }
        
        CloudMediaContext context = new CloudMediaContext(_cachedCredentials);

- 您也可以缓存的 AccessToken 字符串和 TokenExpiration 值。 这些值以后可用于缓存的标记数据创建一个新的 MediaServicesCredentials 对象。  这是在令牌可以安全地共享多个进程或计算机之间的方案尤其有用。

    下面的代码段调用此示例中未定义的 SaveTokenDataToExternalStorage、 GetTokenDataFromExternalStorage 和 UpdateTokenDataInExternalStorageIfNeeded 方法。 您可以定义这些方法来存储、 检索和更新标记外部存储中的数据。 

        CloudMediaContext context1 = new CloudMediaContext(_mediaServicesAccountName, _mediaServicesAccountKey);
        
        // Get token values from the context.
        var accessToken = context1.Credentials.AccessToken;
        var tokenExpiration = context1.Credentials.TokenExpiration;
        
        // Save token values for later use. 
        // The SaveTokenDataToExternalStorage method should check 
        // whether the TokenExpiration value is valid before saving the token data. 
        // If it is not valid, call MediaServicesCredentials’s RefreshToken before caching.
        SaveTokenDataToExternalStorage(accessToken, tokenExpiration);
        
    保存标记值用于创建 MediaServicesCredentials。


        var accessToken = "";
        var tokenExpiration = DateTime.UtcNow;
        
        // Retrieve saved token values.
        GetTokenDataFromExternalStorage(out accessToken, out tokenExpiration);
        
        // Create a new MediaServicesCredentials object using saved token values.
        MediaServicesCredentials credentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey)
        {
            AccessToken = accessToken,
            TokenExpiration = tokenExpiration
        };
        
        CloudMediaContext context2 = new CloudMediaContext(credentials);

    在媒体服务 SDK 更新标记的情况下更新标记的副本。 
    
        if(tokenExpiration != context2.Credentials.TokenExpiration)
        {
            UpdateTokenDataInExternalStorageIfNeeded(accessToken, context2.Credentials.TokenExpiration);
        }
        

- 如果您有多个介质服务帐户 （例如，用于负载共享目的或地理分布） 可以缓存使用 System.Collections.Concurrent.ConcurrentDictionary 集合 （ConcurrentDictionary 集合表示可由多个线程同时访问的键/值对的线程安全集合） 的 MediaServicesCredentials 对象。 然后，可以使用 GetOrAdd 方法来获取缓存的凭据。 

        // Declare a static class variable of the ConcurrentDictionary type in which the Media Services credentials will be cached.  
        private static readonly ConcurrentDictionary<string, MediaServicesCredentials> mediaServicesCredentialsCache = 
            new ConcurrentDictionary<string, MediaServicesCredentials>();
        

        // Cache (or get already cached) Media Services credentials. Use these credentials to create a new CloudMediaContext object.
        static public CloudMediaContext CreateMediaServicesContext(string accountName, string accountKey)
        {
            CloudMediaContext cloudMediaContext;
            MediaServicesCredentials mediaServicesCredentials;
        
            mediaServicesCredentials = mediaServicesCredentialsCache.GetOrAdd(
                accountName,
                valueFactory => new MediaServicesCredentials(accountName, accountKey));
        
            cloudMediaContext = new CloudMediaContext(mediaServicesCredentials);
        
            return cloudMediaContext;
        }
        
## 连接到位于北中国地区的媒体服务帐户

如果您的帐户位于中国北部地区，使用下面的构造函数︰

    public CloudMediaContext(Uri apiServer, string accountName, string accountKey, string scope, string acsBaseAddress)

例如︰


    _context = new CloudMediaContext(
        new Uri("https://wamsbjbclus001rest-hs.chinacloudapp.cn/API/"),
        _mediaServicesAccountName,
        _mediaServicesAccountKey,
        "urn:WindowsAzureMediaServices",
        "https://wamsprodglobal001acs.accesscontrol.chinacloudapi.cn");


## 在配置存储连接值

它是将连接值，如您的帐户名和密码，特别是敏感值存储在配置强烈推荐的做法。 另外，就是建议的做法，对敏感的配置数据进行加密。 您可以通过使用 Windows 加密文件系统 (EFS) 来加密整个配置文件。 要启用 EFS 文件上的，用鼠标右键单击该文件，选择**属性**并启用**高级**设置选项卡中的加密。 或者，您可以创建自定义解决方案的使用受保护的配置加密配置文件中的选定的部分。 请参阅[加密配置信息使用受保护的配置](https://msdn.microsoft.com/library/53tyfkaw.aspx)。

下面的 App.config 文件包含所需的连接值。 中的值<appSettings>元素是你从媒体服务帐户安装过程所需的值。


<pre>
&lt;配置&gt;
    &lt;appSettings&gt;
    &lt;添加键 ="MediaServicesAccountName"值 ="媒体-服务的帐户的名称"/&gt;
        &lt;添加键 ="MediaServicesAccountKey"值 ="媒体服务的帐户-键"/&gt;
    &lt;/appSettings&gt;
&lt;/configuration&gt;
</pre>

若要检索配置连接值，可以使用**ConfigurationManager**类，然后在代码中将值分配给字段︰
    
    private static readonly string _accountName = ConfigurationManager.AppSettings["MediaServicesAccountName"];
    private static readonly string _accountKey = ConfigurationManager.AppSettings["MediaServicesAccountKey"];


<!-- Anchors. -->


<!-- URLs. -->
 

测试

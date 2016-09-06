---
ms.openlocfilehash: b3026c2baee02e9f98ab51f53aabf99f690bd88a
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
 pageTitle="如何管理 Azure 内容传递网络 (CDN) 中的云服务内容过期" 
 description="" 
 services="cdn" 
 documentationCenter=".NET" 
 authors="zhangmanling" 
 manager="dwrede" 
 editor=""/>
<tags 
 ms.service="cdn" 
 ms.workload="media" 
 ms.tgt_pltfrm="na" 
 ms.devlang="dotnet" 
 ms.topic="article" 
 ms.date="09/01/2015" 
 ms.author="mazha"/>

#如何管理 Azure 内容传递网络 (CDN) 中的云服务内容过期

Azure CDN 缓存最受益的对象是指那些经常在其生存时间 (TTL) 期间访问。 对象 TTL 时间保留在缓存中，则是从云服务刷新后的时间。 然后重复该过程。  

如果您没有提供缓存值，对象的 TTL 为 7 天。   

对于静态内容，如图像和样式表您可以控制更新频率，通过包含 web.config CDN 文件夹包含的内容和修改的**clientCache**设置，以控制您的内容的缓存控制标头中。 Web.config 设置将影响所有的文件夹和所有子文件夹中，除非另一个子文件夹中的设置覆盖进一步向下。  例如，可以设置默认生存时间有 3 天，缓存的所有静态内容的根部，但包含子文件夹具有多变量内容缓存设置为 6 小时。  

以下 XML 显示和设置**clientCache**以指定 3 天的最长期限的示例︰  

    <configuration> 
      <system.webServer> 
            <staticContent> 
                <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00" /> 
            </staticContent> 
      </system.webServer> 
    </configuration>

指定**UseMaxAge**会增加缓存控制︰ 最大年龄 =<nnn>在**CacheControlMaxAge**属性中指定的值基于响应标头。 时间跨度的格式是**cacheControlMaxAge**属性为<days>。<hours>:<min>:<sec>. **ClientCache**节点的详细信息，请参阅[客户端缓存<clientCache>](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache)。  

对于返回的应用程序，例如.aspx 页的内容，可以通过设置**HttpResponse.Cache**属性以编程方式设置 CDN 缓存行为。 **HttpResponse.Cache**属性的详细信息，请参阅[HttpResponse.Cache 属性](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx)和[HttpCachePolicy 类](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx)。  

如果您希望以编程方式缓存应用程序内容，请确保内容将被标记为可缓存通过将 HttpCacheability 设置为*公共*。 另外，请确保缓存验证程序的设置。 高速缓存验证程序可以是最后一次修改时间戳设置通过调用 SetLastModified 或一个 etag 值设置通过调用 SetETag。 或者，您还可以通过调用 SetExpires，指定缓存过期时间或您可以依靠本文档前面所述的默认缓存试探法。  

例如，到缓存一小时的内容，添加以下代码︰  

            // Set the caching parameters.
            Response.Cache.SetExpires(DateTime.Now.AddHours(1));
            Response.Cache.SetCacheability(HttpCacheability.Public);
            Response.Cache.SetLastModified(DateTime.Now);

##请参见

[如何管理到期的 Blob 内容在 Azure 内容传递网络 (CDN)](./cdn-manage-expiration-of-blob-content.md
)测试

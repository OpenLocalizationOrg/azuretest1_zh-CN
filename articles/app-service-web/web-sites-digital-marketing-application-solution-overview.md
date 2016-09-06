---
ms.openlocfilehash: 749b72bc9c000a4b5b708245508b39565d43f76a
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="在 Azure 应用程序服务 Web 应用程序上创建数字市场营销活动" 
    description="本指南提供了如何使用 Azure 应用程序服务 Web 应用程序来创建数字市场营销活动的技术概述。 这包括部署，社交媒体集成、 扩展策略，和监视。" 
    editor="jimbe" 
    manager="wpickett" 
    authors="cephalin" 
    services="app-service\web" 
    documentationCenter=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/06/2015" 
    ms.author="cephalin"/>

# 在 Azure 应用程序服务 Web 应用程序上创建数字市场营销活动
[Azure 应用程序服务](http://go.microsoft.com/fwlink/?LinkId=529714)Web 应用程序是个不错的选择，为数字市场营销活动。 数字营销活动都通常较短，和旨在推动短期的市场营销目标。 有两个主要的方案可以考虑。 在第一个方案中，第三方市场营销公司创建并升级的持续时间内将为他们的客户管理市场活动。 第二种方案涉及到市场营销的公司创建，然后将数字营销的市场活动资源的所有权转移给客户。 然后，客户运行并管理自己的数字市场营销活动。 是非常匹配的这两种方案。 

>[AZURE.NOTE] 如果您想要怎样的 Azure 帐户之前开始使用 Azure 应用程序服务，请转到[尝试应用程序服务](http://go.microsoft.com/fwlink/?LinkId=523751)，立即可以在此创建短期的初学者 web 应用程序在应用程序服务。 没有信用卡，所需;没有承诺。

以下全局、 多通道数字的市场营销活动的一个示例使用应用程序服务 Web 应用程序。 它演示如何通过组合应用程序服务 Web 应用程序和其他服务，以最少的技术投资。 **在了解地形中的某个元素上单击它。** 

<object type="image/svg+xml" data="https://sidneyhcontent.blob.core.windows.net/documentation/digital-marketing-notitle.svg" width="100%" height="100%"></object>

> [AZURE.NOTE]
> 本指南提供了一些最常见的区域和对准在 Azure 应用程序服务 Web 应用程序中运行数字市场营销活动的任务。 不过，有其他可以实现应用程序服务 Web 应用程序中的通用解决方案。 若要查看这些解决方案，看到[全局 Web 平台](web-sites-global-web-presence-solution-overview.md)和[业务应用程序](web-sites-business-application-solution-overview.md)的其他指南。

## 从头开始创建或使现有的资产

快速从流行 CMS 在库中创建新的 web 应用程序或应用程序服务 Web 应用程序从不同的语言和框架使您现有的 web 资源。

Azure 市场上提供了从流行网站内容管理系统 (CMS)，如[果园]、 [Umbraco]、 Drupal 和[WordPress]模板。 您可以创建 web 应用程序使用您最喜爱的 CMS 风格。 您可以从各种数据库 backends 以满足您的需要，包括[SQL Azure 数据库]和[MySQL]。

您现有的 web 资源可以在 Web 应用程序上运行，无论它们是.NET、 PHP、 Java、 Node.js 或 Python。 您可以将它们移动到 Web 应用程序使用您熟悉的[FTP]工具。 如果您经常创建数字市场营销活动，则可能您在源代码控制管理系统中具有现有的 web 资源。 可以部署到 Web 应用程序常用的源代码管理选项，例如[Visual Studio]、 [Visual Studio 在线]，以及[Git]的本地，直接从 GitHub、 BitBucket、 收存、 Mercurial 等.

## 灵活应变

保持灵活通过直接从您现有的源代码管理连续发布和运行 A / B 测试的应用程序服务 Web 应用程序中。 

在规划过程中创建原型和早期开发的 web 应用程序中，您和您的客户可以查看市场活动应用程序的实际工作版本之前由[临时插槽部署]web 应用程序开始运行。 通过将源代码管理集成与应用程序服务 Web 应用程序，您可以[连续发布]临时插槽中，然后将其切换到生产环境，而不需要停机，准备就绪时。 

此外，在规划时对实时 web 应用程序的更改，您可以轻松地[运行 A / B 测试]中功能的建议更新在生产中使用的测试和分析真实的用户行为，以帮助您在应用程序设计上做出明智的决策。


## 转到社会

数字市场营销中的应用程序服务 Web 应用程序可以通过与 Facebook 和 Twitter 之类的流行提供验证集成与社交媒体。 这种方法与 ASP.NET 应用程序的示例，请参阅[创建 ASP.NET MVC 应用程序使用身份验证和 SQL 数据库并将其部署到 Azure 应用程序服务]。 

此外，每个社交媒体网站通常提供信息与它从.NET 和许多其他框架集成的其他方法。

## 使用富媒体和到达的所有设备

如浓缩与其他 Azure 服务，数字营销活动︰

-  上载和[Azure 媒体服务]全局流视频
-  向[SendGrid]服务在 Azure 市场中的用户发送电子邮件
-  存在于 Windows、 iOS 和 Android 设备与[移动服务]上
-  给数百万个[通知]集线器的设备发送推式通知

## 转到全局

通过提供区域站点使用 Azure 流量管理器和提供内容的闪电般快速使用 Azure CDN 转全局。

为了满足全球客户在其各自的区域中，路由站点访问者到网站提供最佳性能的区域使用[Azure 流量管理器]。 或者，您可以跨站点负载均匀您承载多个区域中的 web 应用程序的多个副本。

您的静态内容闪电般快速向用户提供全局集成[您的 web 应用程序使用 Azure CDN]。 Azure CDN 缓存[CDN 节点]最接近用户，这样就会减少延迟和连接到您的 web 应用程序中的静态内容。

## 优化

通过自动缩放比例与自动缩放、 Azure Redis 缓存缓存、 使用 WebJobs，运行后台任务和维护高可用性使用 Azure 流量管理器来优化您的 web 应用程序。

[向上扩展]到应用程序服务 Web 应用程序的能力是完全不可预测的工作负载，这与数字市场营销活动的情况。 扩展您的 web 应用程序手动通过[Azure 预览门户](http://go.microsoft.com/fwlink/?LinkId=529715)，以编程方式通过[服务管理 API]或[PowerShell 脚本]，或自动通过自动缩放功能。 在**标准**层中，自动缩放可以扩展 web 应用程序会自动根据 CPU 利用率。 此功能可帮助您实现最大的灵活性和最小化成本同时按比例的不足使 web 应用程序只在需要时根据用户活动。 有关最佳做法、[特洛伊查寻]的[10 件事，我学习了如何快速缩放与 Azure 的 web 应用程序]。

使您的 web 应用程序与[Redis Azure 的高速缓存]的响应能力更强。 使用该后端数据库和其他事物的[ASP.NET 会话状态]和[输出缓存]中缓存数据。

维护您使用[Azure 流量管理器]的 web 应用程序的高可用性。 使用**故障转移**的方法，流量管理器自动传递通讯到辅助站点如果在主站点上存在问题。

## 监视和分析

保持最新的 web 应用程序的性能与 Azure 或第三方工具。 接收到通知关键的 web 应用程序的事件。 深入了解用户很容易地使用应用程序分析或 web 日志分析从 HDInsight。 

获取在[Azure 预览门户](http://go.microsoft.com/fwlink/?LinkId=529715)的 web 应用程序的刀片式服务器中的[快速浏览]的 web 应用程序的当前性能指标和资源配额。 应用程序在可用性、 性能和使用情况的 360 ° 视图，使用[Azure 应用程序理解]为您提供快速且功能强大的故障诊断，诊断和使用情况的见解。 或者，使用[新的 Relic]类第三方工具来提供高级监视 web 应用程序的数据。

在**标准**层中，监视器应用程序快速响应接收电子邮件通知，当您的 web 应用程序变得无法响应。 有关详细信息，请参阅[如何︰ 接收警报通知和 Azure 中管理预警规则]。

## 更多的资源

- [应用程序服务 Web 应用程序的文档](/services/app-service/web/)
- [学习的 Azure 应用程序服务 Web 应用程序映射](websites-learning-map.md)
- [Azure 网站博客](/blog/topics/web/)

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[Azure 应用程序服务]: /services/app-service/web/

[果园]:web-sites-dotnet-orchard-cms-gallery.md
[Umbraco]:web-sites-gallery-umbraco.md
[WordPress]:web-sites-php-web-site-gallery.md
  
[MySQL]:web-sites-php-mysql-deploy-use-git.md
[SQL azure 数据库]:web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md
[FTP]:web-sites-deploy.md#ftp
[Visual Studio]:web-sites-dotnet-get-started.md
[Visual Studio 联机]:../cloud-services-continuous-delivery-use-vso.md
[Git]:web-sites-publish-source-control.md

[部署到临时插槽]:web-sites-staged-publishing.md 
[连续发布]:http://rickrainey.com/2014/01/21/continuous-deployment-github-with-azure-web-sites-and-staged-publishing/
[运行 A / B 测试]:http://blogs.msdn.com/b/tomholl/archive/2014/11/10/a-b-testing-with-azure-websites.aspx

[创建一个 ASP.NET MVC 应用程序使用身份验证和 SQL 数据库并将其部署到 Azure 应用程序服务]:web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md

[Azure 媒体服务]:http://blogs.technet.com/b/cbernier/archive/2013/09/03/windows-azure-media-services-and-web-sites.aspx
[在 Azure 市场上 SendGrid 服务]:sendgrid-dotnet-how-to-send-email.md
[移动服务]:../mobile-services-dotnet-backend-windows-store-dotnet-push-notifications-app-users.md
[通知中心]:../mobile-services-dotnet-backend-windows-store-dotnet-push-notifications-app-users.md

[Azure 的流量管理器]:http://www.hanselman.com/blog/CloudPowerHowToScaleAzureWebsitesGloballyWithTrafficManager.aspx
[与 Azure CDN 集成您的 web 应用程序]:cdn-websites-with-cdn.md 
[CDN 节点]:https://msdn.microsoft.com/library/azure/gg680302.aspx

[向上扩展]:/manage/services/web-sites/how-to-scale-websites/
[Azure 的管理门户]:http://manage.windowsazure.com/
[服务管理 API]:http://msdn.microsoft.com/library/windowsazure/ee460799.aspx
[PowerShell 脚本]:http://msdn.microsoft.com/library/windowsazure/jj152841.aspx
[金衡制查寻]:https://twitter.com/troyhunt
[10 件事，我学习了快速缩放与 Azure 的 web 应用程序]:http://www.troyhunt.com/2014/09/10-things-i-learned-about-rapidly.html
[Azure 的 Redis 高速缓存]:/blog/2014/06/05/mvc-movie-app-with-azure-redis-cache-in-15-minutes/
[ASP.NET 会话状态]:https://msdn.microsoft.com/library/azure/dn690522.aspx
[输出缓存]:https://msdn.microsoft.com/library/azure/dn798898.aspx

[快速浏览]:/manage/services/web-sites/how-to-monitor-websites/
[Azure 应用程序的见解]:http://blogs.msdn.com/b/visualstudioalm/archive/2015/01/07/application-insights-and-azure-websites.aspx
[新 Relic]:/develop/net/how-to-guides/new-relic/
[如何︰ 接收警报通知和管理 Azure 中的预警规则]:http://msdn.microsoft.com/library/windowsazure/dn306638.aspx

  
  [gitstaging]:http://www.bradygaster.com/post/multiple-environments-with-windows-azure-web-sites  
 

测试

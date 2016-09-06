---
ms.openlocfilehash: 402adc548bd7f39ab48b86cd5b66b502904cabac
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="在 Azure 应用程序服务 Web 应用程序上创建 web 的全局呈现" 
    description="本指南提供了对技术概述如何承载您组织的 (.COM) 网站在 Azure 应用程序服务 Web 应用程序上。 这包括部署、 自定义的域、 SSL 和监视。" 
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


# 在 Azure 应用程序服务 Web 应用程序上创建 web 的全局呈现

[Azure 应用程序服务](http://go.microsoft.com/fwlink/?LinkId=529714)Web 应用程序都有所有您需要建立.COM 网站的全球 web 内容的功能。 无论您的组织规模大小，您需要一个可靠、 安全，且可伸缩的平台驱动您的业务、 您的品牌意识和您的客户通信。 应用程序服务 Web 应用程序可以帮助保持您的公司的品牌和标识与 Microsoft 支持业务连续性。

>[AZURE.NOTE] 如果您想要怎样的 Azure 帐户之前开始使用 Azure 应用程序服务，请转到[尝试应用程序服务](http://go.microsoft.com/fwlink/?LinkId=523751)，立即可以在此创建短期的初学者 web 应用程序在应用程序服务。 没有信用卡，所需;没有承诺。

下面是.COM 网站应用程序服务 Web 应用程序上运行的示例。 它演示只是通过和其他服务，以最少的技术投资组合的 Web 应用程序可以执行哪些操作。 **在了解地形中的某个元素上单击它。** 

<object type="image/svg+xml" data="https://sidneyhcontent.blob.core.windows.net/documentation/corp-website-visio.svg" width="100%" height="100%"></object>

> [AZURE.NOTE]
> 本指南提供了一些最常见的区域和对准在 Azure 应用程序服务 Web 应用程序中运行的面向公众的.COM 网站的任务。 不过，有其他可以在 Azure 应用程序服务 Web 应用程序中实现的通用解决方案。 若要查看这些解决方案，看到[数字市场营销活动](web-sites-digital-marketing-application-solution-overview.md)和[业务应用程序](web-sites-business-application-solution-overview.md)的其他指南。

## 从头开始创建或使现有的资产

快速从流行 CMS 在库中创建新的网站或应用程序服务 Web 应用程序从不同的语言和框架使您现有的 web 资源。

Azure 市场上提供了从流行网站内容管理系统 (CMS)，如[果园]、 [Umbraco]、 Drupal 和[WordPress]模板。 您可以创建 web 应用程序使用您最喜爱的 CMS 风格。 您可以从各种数据库 backends 以满足您的需要，包括[SQL Azure 数据库]和[MySQL]。

您现有的 web 资源可以在应用程序服务 Web 应用程序，运行，无论是.NET、 PHP、 Java、 Node.js 或 Python。 您可以将它们移动到 Web 应用程序使用您熟悉的[FTP]工具或源代码控制管理系统。 Web 应用程序支持直接从发布流行源代码管理选项，例如[Visual Studio]、 [Visual Studio 在线]，以及[Git]的本地，GitHub、 BitBucket、 收存、 Mercurial 等.

## 发布可靠

通过连续发布直接从您现有的版本控制系统和实时测试内容可靠地发布您的网站。 

在规划、 设计原型、 和早期开发的网站，可以查看网站的真实工作版本之前它投入由您的站点[部署到临时插槽]上的应用程序服务 Web 应用程序。 通过与 Web 应用程序中集成源代码管理，您可以[连续发布]临时插槽，并投入生产，而不需要停机进行交换，当准备好这样做。 如果出现任何错误在生产站点上，您还可以交换出来为以前版本的网站立即。 

此外，在规划活动网站的更改时，您可以轻松地[运行 A / B 测试]中功能的建议更新在生产中使用的测试和分析真实的用户行为，以帮助您在网站设计上做出明智的决策。

## 品牌和安全

免费使用应用程序服务 Web 应用程序域或映射到您注册的域名，然后保护自己的品牌与 CA 签名的 SSL 证书。

** \*。 Azurewebsites.net** Web 应用程序上运行您的网站时，域是免费赠送。 或者，您可以向[自定义的域]-如 contoso.com-从任何 DNS 注册表，如 GoDaddy 获取映射您的网站。

如果您收集任何用户信息、 执行电子商务，或管理任何其他敏感数据，您可以保护您的品牌声誉和您的客户使用[HTTPS]。 ** \*。 Azurewebsites.net**域名已经都附带了 SSL 证书，并且如果您使用您自定义的域，可以将 SSL 证书为其 Web 应用程序。 没有与每个 SSL 证书相关联的每月费用 （每小时按比例）。 有关详细信息，请参阅[应用程序服务定价详细信息]。

## 转到全局

通过提供区域站点使用 Azure 流量管理器和提供内容的闪电般快速使用 Azure CDN 转全局。

为了满足全球客户在其各自的区域中，路由站点访问者到网站提供最佳性能的区域使用[Azure 流量管理器]。 或者，您可以跨站点负载均匀您承载多个区域中的网站的多个副本。

您的静态内容闪电般快速向用户提供全局集成[您的 web 应用程序使用 Azure CDN]。 Azure CDN 缓存接近用户，缩短滞后时间和连接到您的网站的[CDN 节点]中的静态内容。

## 优化

通过使用自动缩放自动缩放、 Azure Redis 缓存缓存、 使用 WebJobs，运行后台任务和维护高可用性使用 Azure 流量管理器优化.COM 网站。

[向上扩展]的应用程序服务 Web 应用程序能够满足.COM 网站，无论规模大小您的工作负载的需要。 扩展您的网站手动通过[Azure 预览门户](http://go.microsoft.com/fwlink/?LinkId=529715)，以编程方式通过[服务管理 API]或[PowerShell 脚本]，或自动通过自动缩放功能。 在**标准**托管计划，自动缩放可以扩张网站自动基于 CPU 的利用率。 有关最佳做法、[特洛伊查寻]的[10 件事，我学习了如何快速缩放与 Azure 的 web 应用程序]。

使您的网站使用[Azure Redis 缓存]的响应能力更强。 使用该后端数据库和其他事物的[ASP.NET 会话状态]和[输出缓存]中缓存数据。

维护您的网站使用[Azure 流量管理器]的高可用性。 使用**故障转移**的方法，流量管理器自动传递通讯到辅助站点如果在主站点上存在问题。

## 监视和分析

掌握最新信息与 Azure 或第三方工具的网站的性能。 接收有关网站关键事件的通知。 深入了解用户很容易地使用应用程序分析或 web 日志分析从 HDInsight。 

在[Azure 预览门户](http://go.microsoft.com/fwlink/?LinkId=529715)的 web 应用程序的刀片式服务器中获得[快速浏览]该网站的当前性能指标和资源配额。 应用程序在可用性、 性能和使用情况的 360 ° 视图，使用[Azure 应用程序理解]为您提供快速且功能强大的故障诊断，诊断和使用情况的见解。 或者，使用[新的 Relic]类第三方工具来提供高级监控您的网站的数据。

在**标准**托管计划中，监视站点的响应速度站点变得无法响应时收到电子邮件通知。 有关详细信息，请参阅[如何︰ 接收警报通知和 Azure 中管理预警规则]。

## 使用富媒体和到达的所有设备

使.COM 网站吸引力与富媒体，如使用︰

-  上载和[Azure 媒体服务]全局流视频
-  向[SendGrid]服务在 Azure 市场中的用户发送电子邮件

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

[自定义的域]:web-sites-custom-domain-name.md
[HTTPS]:web-sites-configure-ssl-certificate.md
[应用程序服务定价详细信息]: /pricing/details/app-service/#ssl-connections

[Azure 的流量管理器]:http://www.hanselman.com/blog/CloudPowerHowToScaleAzureWebsitesGloballyWithTrafficManager.aspx
[与 Azure CDN 集成您的 web 应用程序]:cdn-websites-with-cdn.md 
[CDN 节点]:https://msdn.microsoft.com/library/azure/gg680302.aspx

[向上扩展]:web-sites-scale.md
[Azure 的管理门户]:http://manage.windowsazure.com/
[服务管理 API]:https://msdn.microsoft.com/library/azure/ee460799.aspx
[PowerShell 脚本]:https://msdn.microsoft.com/library/azure/jj152841.aspx
[金衡制查寻]:https://twitter.com/troyhunt
[10 件事，我学习了快速缩放与 Azure 的 web 应用程序]:http://www.troyhunt.com/2014/09/10-things-i-learned-about-rapidly.html
[Azure 的 Redis 高速缓存]:/blog/2014/06/05/mvc-movie-app-with-azure-redis-cache-in-15-minutes/
[ASP.NET 会话状态]:https://msdn.microsoft.com/library/azure/dn690522.aspx
[输出缓存]:https://msdn.microsoft.com/library/azure/dn798898.aspx

[快速浏览]:web-sites-monitor.md
[Azure 应用程序的见解]:http://blogs.msdn.com/b/visualstudioalm/archive/2015/01/07/application-insights-and-azure-websites.aspx
[新 Relic]:../store-new-relic-cloud-services-dotnet-application-performance-management.md
[如何︰ 接收警报通知和管理 Azure 中的预警规则]:http://msdn.microsoft.com/library/windowsazure/dn306638.aspx

[Azure 媒体服务]:http://blogs.technet.com/b/cbernier/archive/2013/09/03/windows-azure-media-services-and-web-sites.aspx
[在 Azure 市场上 SendGrid 服务]:sendgrid-dotnet-how-to-send-email.md

 

测试

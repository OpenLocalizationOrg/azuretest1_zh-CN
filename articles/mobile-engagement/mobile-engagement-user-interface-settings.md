---
ms.openlocfilehash: 2f9d035fcc1348c1549f5161c7eb9acc89d66c87
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="Azure 的移动服务用户界面的设置" 
   description="了解如何管理应用程序使用 Azure 移动服务的全局设置" 
   services="mobile-engagement" 
   documentationCenter="" 
   authors="piyushjo" 
   manager="dwrede" 
   editor=""/>

<tags
   ms.service="mobile-engagement"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="mobile-multiple"
   ms.workload="mobile" 
   ms.date="08/10/2015"
   ms.author="piyushjo"/>

# 如何管理您的应用程序的全局设置
设置菜单选项可用于应用程序的变化，根据所用平台的应用程序和您已被授予该应用程序的权限。 设置包括以下︰ 详细信息、 项目、 本机推，推速度、 SDK、 跟踪、 应用信息、 商业压力和权限。 仅应用程序信息菜单选项的用户界面设置部分包含元素可被管理设备 API 的"标记"功能。 "概念"中的"术语表"包括定义的术语和缩写，如下所示︰ APNS、 GCM、 IDFA、 API、 SDK、 API 密钥、 SDK 键、 应用程序 ID (应用程序 ID)、 AppStore ID、 标签计划、 用户 ID、 设备 ID、 应用程序委托、 堆栈跟踪和深层链接。

### 请参见
- [API 文档的 API 设备][链接 4]，[概念的术语表][链接 6]，[服务疑难解答指南-][链接 24]

  ![settings1][46]

## 详细信息
允许您更改名称和说明应用程序视图所有者应用程序和您的角色的权限。 分析配置︰ 允许您查看或更改，保留时间在天的周开始的天
 
  ![settings2][47]
 
## Projects
允许您选择希望应用程序中显示的所有项目。 此外可以为项目搜索并查看的名称、 说明、 所有者和您的应用程序属于任何项目的角色权限。

### 请参见
-    [用户界面文档 – 主页][链接 13]
 
  ![settings3][48]

## 本机的推
使您可以用本机推入注册新证书或删除并使用现有的证书。 本机的推使 Azure 移动合作推到您的应用程序在任何时候，即使在未运行。 在至少一个本机推送服务提供凭据或证书之后，您可以选择"任何时间"推 API 中创建到达市场，同时也使用了"通告"参数时。

### 请参见
- [API 文档-到达 API] [链接 4]，[API 文档-推 API] [链路 4]， [UI 文档所不及的新推活动][链接 27]

### 苹果推送通知服务 (APNS)
若要启用本机推使用苹果推送通知服务将需要注册您的证书。 需要的证书类型指定为开发 （开发人员） 或生产 （生产）。 然后您将需要上传您的证书和密码。

### 请参见
- [SDK 文档的 iOS-如何准备为苹果推送通知您应用程序][链接 5]
 
![settings4][49]
 
### Windows 推送通知服务 (WPNS)
若要启用本机推使用 Windows 通知服务，您必须提供您的应用程序的凭据。 您将需要您的包安全标识符 (SID) 和您的机密密钥。
 
![settings5][50]
 
### Google 的 Android (GCM) 消息的云
若要启用使用 GCM 本机推，您需要从 Google 按照说明操作。 然后您必须粘贴一个服务器简单的 API 键，没有 IP 限制配置。 对于 Android v1.12.0 + 要求与 SDK 的集成。

### 请参见
- [SDK 文档-Android-如何集成 GCM][链接 5]， [Google 开发的 GCM 指南](http://developer.android.com/guide/google/gcm/gs.html)， [Google 开发的 GCM 文档](http://developer.android.com/google/gcm/index.html)
 
### Amazon 设备的 Android (ADM) 消息
若要启用使用 ADM 本机推，必须提供 Amazon<OAuth credentials>客户机 ID 和客户端密钥 （需要与 SDK 集成 Android v2.1.0 +） 组成。

### 请参见
- [SDK 文档-Android-如何集成 ADM][链接 5]， [Amazon 开发人员 — ADM 文档](https://developer.amazon.com/sdk/adm/credentials.html#Getting)

![settings6][51]

## 下压速度
显示应用程序的当前推速度并使您可以定义您的应用程序的下压速度。 下压速度定义每秒达到模块将执行的最大数目。 这可以有助于在市场激活后，每 second(/s) 的太多请求通过重载您自己的服务器的情况下。
 
  ![settings7][52]

## 跟踪
跟踪功能允许您跟踪 iOS 和 Android 应用程序安装的来源。 它可以让您知道，您的用户下载您的应用程序 （即从哪个应用程序存储） 和哪些源了它们 (如广告市场活动、 博客、 网站、 电子邮件、 SMS，等等。)。 Azure 移动服务的跟踪功能必须集成到从 SDK 应用程序作为一个单独的步骤。 

### 请参见
- [SDK 文档-Android-如何集成][链接 5]， [SDK 文档的 iOS-如何集成][链接 5]
 
### 存储区
允许您注册所有商店可以从中下载应用程序基于它们的名称和它们的相关的下载 Url。 此操作允许您创建到主机跟踪 Url 位置。 可以创建或从该页面删除存储区。 使用图标来创建新的跟踪 URL 转到的 Url 页面涵盖以下的跟踪。 有多种方法可以跟踪您的用户下载您的应用程序的位置︰

-    使用第三方广告服务器 （Azure 的移动服务目前支持[SmartAd](http://smartadserver.fr/)和[Surikate](http://www.surikate.com/)。
-    使用第三方归属服务 （Azure 移动服务目前也可能在不久的将来支持[移动应用程序跟踪]( http://www.mobileapptracking.com/)和[Trademob](http://www.trademob.com/)支持。）
-    使用第三方安装引用站点 （Azure 移动合作目前[Google 播放安装引用](https://developers.google.com/app-conversion-tracking/docs/third-party-trackers/)的 Android 仅支持）。
-    使用手动报告。
 
  ![settings8][53]
 
### 广告活动
允许您创建 Ad 服务器名称、 市场活动 ID 和您的应用程序可以下载到其中的源组成的新广告活动。 
 
  ![settings9][54]
 
### 跟踪 Url
使您可以生成跟踪 Url 用作目标在您的源的 Url (广告市场活动、 博客、 网站、 电子邮件、 SMS，等等) 它可以将用户重定向到他们可以在哪里下载您的应用程序商店。 此外使您可以显示所有跟踪您已经创建的 Url。 跟踪 URL 可以用作广告市场或到达通知的操作 URL 来邀请用户下载一个应用程序从另一个应用程序。 跟踪下载的 URL 重定向 URL 与所选商店，同时允许我们跟踪系统来记录从其安装时下载该应用程序的存储。 如果提供一个源，我们跟踪系统也记录，允许您为应用程序创建的各种广告市场活动之间进行区分。

创建新跟踪 URL 要求您指定一个存储区和来源的无、 客户或添加服务器。 

-    无源创建默认跟踪 URL。
-    自定义源代码可以下载应用程序外部服务器上指定一个 URL。
-    Ad 服务器的源创建默认跟踪中名为 Ad 服务器的默认 URL。
 
### 请参见
- [用户界面文档所不及的新推的市场活动][链接 27] 
 
### 生成跟踪 URL
您还可以生成跟踪 URL，从而允许用户从应用程序内新到达市场的内容部分中的一个。

### 请参见
- [用户界面文档-到达-按内容][链接 29]

![settings10][55]

## 应用程序信息
可以注册到应用程序的用户关联的附加信息。 您的应用程序 （使用 SDK） 或由您后端 （使用设备 API），此信息可以被注入。 

### 请参见
- [API 文档的 API 设备][链接 4]

注册应用程序的信息标记使您可以通过创建基于特定到达观众条件段到达市场。 您可以查看的名称和现有的应用程序信息标记的类型或添加新的应用程序信息，基于名称和类型的字符串、 日期、 整数或布尔值。

### 请参见
- [用户界面文档所不及的新推的市场活动][链接 27]
 
![settings11][56]
 
## 商业压力
强制配额允许您定义一个设备可以在一段内推送到的最大次数。 强制配额仅使用已启用"应用推配额"选项的访问活动。 按细分市场或默认情况下，可以管理强制配额。 可以设置配额，只在最后一个小时、 天、 周或月一滑动段内发送入栈的最大数目。

### 请参见
- [用户界面文档所不及的新推的市场活动][链接 27]， [UI 文档-段][链接 18]

### 配额︰
- 默认配额︰ 此配额应用于由下面的配额由段一节中的任何段不匹配的用户。 默认情况下，无配额设置。
- 按细分市场的配额︰ 配额必须应用到一组用户，需要先创建一段来确定要应用到配额的用户。 它可能是大量用户段定义的会话，或任何您认为有趣来定义的用户组。 如果设备匹配定义配额具有多个段，则应用仅有入栈每小时最高的最大数目的配额。

![settings12][57]
 
## 权限
可以搜索和查看您的应用程序的用户的电子邮件、 名称、 组织和权限级别。 权限的概念是添加到的项目角色。 它允许您将一个对特定用户对应用程序具有访问权限的权限集相关联。

### 可用的权限级别当前为︰
-    "延伸市场活动的创建者"（用户可以创建到达市场活动） 
-    "延伸活动经理"（可以创建到达市场活动和激活、 暂停、 完成、 销毁它们的用户）

> 注意︰ 当用户具有项目角色和一组给定应用程序的权限时，将使用更宽松的概念。 因此，我们建议将权限时，您将项目的用户角色设置为"查看器"（要求最严格的角色），然后添加更大权限的权限在应用程序级别。

### 请参见
- [用户界面文档-主页][链接 13]  
 
![settings13][58]

<!--Image references-->
[1]: ./media/mobile-engagement-user-interface-navigation/navigation1.png
[2]: ./media/mobile-engagement-user-interface-home/home1.png
[3]: ./media/mobile-engagement-user-interface-home/home2.png
[4]: ./media/mobile-engagement-user-interface-home/home3.png
[5]: ./media/mobile-engagement-user-interface-home/home4.png
[6]: ./media/mobile-engagement-user-interface-home/home5.png
[7]: ./media/mobile-engagement-user-interface-my-account/myaccount1.png
[8]: ./media/mobile-engagement-user-interface-my-account/myaccount2.png
[9]: ./media/mobile-engagement-user-interface-my-account/myaccount3.png
[10]: ./media/mobile-engagement-user-interface-analytics/analytics1.png
[11]: ./media/mobile-engagement-user-interface-analytics/analytics2.png
[12]: ./media/mobile-engagement-user-interface-analytics/analytics3.png
[13]: ./media/mobile-engagement-user-interface-analytics/analytics4.png
[14]: ./media/mobile-engagement-user-interface-monitor/monitor1.png
[15]: ./media/mobile-engagement-user-interface-monitor/monitor2.png
[16]: ./media/mobile-engagement-user-interface-monitor/monitor3.png
[17]: ./media/mobile-engagement-user-interface-monitor/monitor4.png
[18]: ./media/mobile-engagement-user-interface-reach/reach1.png
[19]: ./media/mobile-engagement-user-interface-reach/reach2.png
[20]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign1.png
[21]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign2.png
[22]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign3.png
[23]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign4.png
[24]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign5.png
[25]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign6.png
[26]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign7.png
[27]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign8.png
[28]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign9.png
[29]: ./media/mobile-engagement-user-interface-reach-criterion/Reach-Criterion1.png
[30]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content1.png
[31]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content2.png
[32]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content3.png
[33]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content4.png
[34]: ./media/mobile-engagement-user-interface-dashboard/dashboard1.png
[35]: ./media/mobile-engagement-user-interface-segments/segments1.png
[36]: ./media/mobile-engagement-user-interface-segments/segments2.png
[37]: ./media/mobile-engagement-user-interface-segments/segments3.png
[38]: ./media/mobile-engagement-user-interface-segments/segments4.png
[39]: ./media/mobile-engagement-user-interface-segments/segments5.png
[40]: ./media/mobile-engagement-user-interface-segments/segments6.png
[41]: ./media/mobile-engagement-user-interface-segments/segments7.png
[42]: ./media/mobile-engagement-user-interface-segments/segments8.png
[43]: ./media/mobile-engagement-user-interface-segments/segments9.png
[44]: ./media/mobile-engagement-user-interface-segments/segments10.png
[45]: ./media/mobile-engagement-user-interface-segments/segments11.png
[46]: ./media/mobile-engagement-user-interface-settings/settings1.png
[47]: ./media/mobile-engagement-user-interface-settings/settings2.png
[48]: ./media/mobile-engagement-user-interface-settings/settings3.png
[49]: ./media/mobile-engagement-user-interface-settings/settings4.png
[50]: ./media/mobile-engagement-user-interface-settings/settings5.png
[51]: ./media/mobile-engagement-user-interface-settings/settings6.png
[52]: ./media/mobile-engagement-user-interface-settings/settings7.png
[53]: ./media/mobile-engagement-user-interface-settings/settings8.png
[54]: ./media/mobile-engagement-user-interface-settings/settings9.png
[55]: ./media/mobile-engagement-user-interface-settings/settings10.png
[56]: ./media/mobile-engagement-user-interface-settings/settings11.png
[57]: ./media/mobile-engagement-user-interface-settings/settings12.png
[58]: ./media/mobile-engagement-user-interface-settings/settings13.png

<!--Link references-->
[链接 1]: mobile-engagement-user-interface.md
[链接 2]: mobile-engagement-troubleshooting-guide.md
[链接 3]: mobile-engagement-how-tos.md
[链接 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[链接 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[6 链接]: http://go.microsoft.com/fwlink/?LinkId=525555
[链接 7]: https://account.windowsazure.com/PreviewFeatures
[链接 8]: https://social.msdn.microsoft.com/Forums/azure/home?forum=azuremobileengagement
[链接 9]: http://azure.microsoft.com/services/mobile-engagement/
[10 链接]: http://azure.microsoft.com/documentation/services/mobile-engagement/
[链接 11]: http://azure.microsoft.com/pricing/details/mobile-engagement/
[链接 12]: mobile-engagement-user-interface-navigation.md
[链接 13]: mobile-engagement-user-interface-home.md
[链接 14]: mobile-engagement-user-interface-my-account.md
[15 链接]: mobile-engagement-user-interface-analytics.md
[链接 16]: mobile-engagement-user-interface-monitor.md
[链接 17]: mobile-engagement-user-interface-reach.md
[链接 18]: mobile-engagement-user-interface-segments.md
[链接 19]: mobile-engagement-user-interface-dashboard.md
[链接 20]: mobile-engagement-user-interface-settings.md
[链接 21]: mobile-engagement-troubleshooting-guide-analytics.md
[链接 22]: mobile-engagement-troubleshooting-guide-apis.md
[链接 23]: mobile-engagement-troubleshooting-guide-push-reach.md
[链接 24]: mobile-engagement-troubleshooting-guide-service.md
[链接 25]: mobile-engagement-troubleshooting-guide-sdk.md
[链接 26]: mobile-engagement-troubleshooting-guide-sr-info.md
[链接 27]: ../mobile-engagement-how-tos-first-push.md
[链接 28]: ../mobile-engagement-how-tos-test-campaign.md
[链接 29]: ../mobile-engagement-how-tos-personalize-push.md
[链接 30]: ../mobile-engagement-how-tos-differentiate-push.md
[链接 31]: ../mobile-engagement-how-tos-schedule-campaign.md
[链接 32]: ../mobile-engagement-how-tos-text-view.md
[链接 33]: ../mobile-engagement-how-tos-web-view.md
 
测试

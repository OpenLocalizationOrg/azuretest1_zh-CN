---
ms.openlocfilehash: 08e0eca5657fce93d5442cfb744d25c5353c003c
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="Azure 的移动服务用户界面的范围" 
   description="了解如何使用推式通知使用 Azure 移动接洽动身去找您应用程序的用户" 
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


# 如何到达您的应用程序使用推式通知的用户
用户界面的范围部分是推的市场管理工具，您可以在其中创建/编辑/激活/完成/显示器并收到推式通知活动和功能，也可以通过访问 API （和低级别推 API 的某些元素） 的统计信息。 请记住，无论您使用 Api 或用户界面，您将需要集成 Azure 移动服务和进入您的应用程序为每个平台 sdk 在可以使用之前到达市场。

### 请参见
-  [API 文档-到达 API][链接 4]、 [API 文档的 API 推][链接 4]、[故障排除指南-推/到达][链接 23]
-  [到达-市场活动][链接 27]、[达到的标准][链接 28]、[到达的内容][链接 29]，[到达-如何][链接 3]
 
## 四种类型的推式通知
1.    通知-允许您将它们重定向到您的应用程序内的其他位置的用户发送广告邮件或将其发送到网页或在您的应用程序外部存储。 
2.    轮询-允许您从最终用户收集信息，通过询问他们问题。
3.    数据推送-允许您发送二进制或 base64 数据文件。 包含在数据推送的信息被发送到您的应用程序来修改您的应用程序在用户的当前体验。 应用程序必须能够处理中数据推送的数据。
4.    拼贴 (仅在 Windows Phone 中)-允许您使用 Microsoft 推送通知服务 (MPNS) 来发送包含 XML 数据的本机 Windows 推。 （SDK 版本 0.9.0 以来支持。 拼贴的最终有效负载不能超过 32k 字节。）

### 请参见
-  [概念的术语表][6 链接]

## 三个类别的每个市场活动显示的实时统计信息
1.    推送的发送多少推进基于指定市场活动中的条件。 
2.    多少用户答复了-对通知反应通过从外部应用程序打开或关闭应用程序中。 
3.    进行的多少用户单击通知被重定向到应用程序中的新位置、 商店、 或 web 浏览器中的链接。 

> 注意︰ 可以使用从详细市场统计通过访问 API 统计

### 请参见
-  [概念的术语表][6 链接]， [API 文档-达到统计的 API-][链接 4]


## 市场活动详细信息
可以编辑、 复制、 删除或激活通过悬停在它们的名称而尚未尚未激活的市场活动，或者您可以单击以打开它们。 可以克隆已经通过悬停在它们的名称激活的市场活动，或者您可以单击以打开它们。 但是，您无法更改市场活动一旦激活它。
 
![Reach1][18]

## 到达的反馈
可以从详细信息已经被激活开启的市场活动的统计视图切换和从简单切换到高级视图以查看更多详细的信息 （具体取决于您的权限） 的统计信息。 您还可以用作到达反馈信息，从以前的市场活动目标的条件，在新的市场活动。 从到达 API 到达的反馈还可以收集统计信息"统计数据"。 您还可以自定义基于以前的市场活动推市场活动的目标群体。


### 请参见 
-  [用户界面文档所不及的新推的市场活动][27]， [API 文档-达到统计的 API-][链接 4]

![Reach2][19]

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
[链接 27]: mobile-engagement-user-interface-reach-campaign.md
[链接 28]: mobile-engagement-user-interface-reach-criterion.md
[链接 29]: mobile-engagement-user-interface-reach-content.md
 
测试

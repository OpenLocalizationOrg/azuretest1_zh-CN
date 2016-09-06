---
ms.openlocfilehash: 5d85974d824c12a67a5ec73c92874075935ae090
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="Azure 的移动服务用户界面的监视器" 
   description="了解如何监视应用程序使用 Azure 移动服务有关的实时数据" 
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

# 如何监视应用程序的实时数据
用户界面的显示器部分提供实时分析信息，并允许您设置警报阈值达到大部分处于可从历史上看用户界面的"分析"部分相同的信息时。 "概念"中的"术语表"已定义的术语和缩写词中分析和监视，如下所示︰ 活动用户，新用户、 保留用户、 会话、 用户路径图、 用户映射、 跟踪 Url、 趋势、 活动、 事件、 作业、 错误、 额外信息、 崩溃和应用程序信息。

### 请参见
-  [概念的术语表][6 链接]，[故障排除指南-分析][链接 21]

## 监视器-会话、 作业、 事件、 错误和崩溃
您可以看到多少用户当前进程中以及在特定屏幕或执行特定的操作。 您可以查看用户的活动会话、 作业、 事件、 错误和崩溃的一半。 可以查看当前的信息和显示的最后一个小时、 工作日或周的信息。 您可以看到所有在每个类别或排序中通过特定会话、 作业、 事件、 错误和崩溃的信息。  实时监视非常有用推入市场等事件过程中使用来查看是否有反弹操作中右后发送推式通知。 
 
![Monitor1][14]  

## 使用监视的事件的详细信息进行故障排除
测试设备应用程序中生成一个事件，并查找其监视的事件的详细信息中是一个最简单的方法来找到测试设备的设备 ID，并确认该 Azure 移动服务集成的分析和监控段从您的应用程序工作。 一旦测试设备的设备 ID，您可以将其添加到测试设备中"我帐户-设备"。 如果无法生成事件时，请确保 Azure 移动服务正确集成 Android/iOS/Web/Windows/Windows Phone sdk 应用程序中。

### 请参见
-  [SDK 文档][链接 5]

![Monitor2][15]  

## 显示器故障诊断-崩溃-详细信息
您可以检查故障转储信息关于您的应用程序从监视器的崩溃-详细信息以帮助您确定您的应用程序崩溃的原因。 您应查找与每个版本的 SDK 发行说明，了解每个版本的 SDK，了解 Android/iOS/Web/Windows/Windows Phone 中的已知问题。 

### 请参见
-  [SDK 文档中的发行说明][链接 5]

![Monitor3][16] 

## 监视器-警报
您还可以指定将自动发送给您通过电子邮件或即时消息的警报的条件。 （等 Google 的 GTalk 的任何符合 XMPP 服务或支持苹果的 iChat）。警报根据预定义的检测阈值大于号 (>) 或小于 (<) 每秒、 分钟或小时特定数量的会话、 作业、 事件、 错误或系统崩溃。 警报可以监视所有活动的一个给定的类型，或仅都监视特定作业、 事件或错误的活动。 此外可以指定最小值的检测率，即最小分离两个相同的警报，以确保当触发警报时，您将永远不会收到多个通知每隔 X 分钟通知的时间量。
 
![Monitor4][17]

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

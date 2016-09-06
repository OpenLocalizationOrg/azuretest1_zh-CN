---
ms.openlocfilehash: fddb3f2392b2a2da0d0fa5fc76b66e55cde61f7c
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="Azure 的移动服务用户界面的段" 
   description="了解如何创建和管理领域的用户标识使用 Azure 移动服务的使用模式" 
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

# 如何创建和管理用户使用模式识别领域
用户界面的段部分允许您对您的用户根据不同的行为和分析可以从应用程序中获取，也可以通过段 API 访问进行分段处理。 段是第一次计算的创建，并且它们都重新计算基于最新的分析信息每隔 24 小时之后的 24 小时。 一旦一段计算，其显示"一天一天的历史记录"图表每一天。

### 请参见
- [API 文档-段 API][链接 4]，[故障排除指南-分析][链接 21]

## 创建线段
您可以创建基于最多 10 个条件在特定的时间段上向上到在过去的 60 天内从分析部分的一段。 例如，您可以创建基于已查看特定的页面或过去 10 天内搜索您的应用程序中的特定内容的人的一段。 在分析部分提供了此信息。 因此，您可以使用它来创建一个段，然后设置推式通知针对这部分用户来让他们回到应用程序。 
 
> 注意︰ 一旦计算的一段，它无法编辑;它可以只克隆 （复制） 或销毁 （已删除）。 一段可以克隆 （具有相同的 AppID)，相同的应用程序中，它还可以克隆到其他应用程序 （使用不同的应用程序标识）。 
 
 ![segments1][35] 

## 示例段
 ![segments2][36]

线段可以细分您的应用程序的最终用户。
您将您的用户是一个重要的市场营销策略。 Azure 移动服务允许您获取历史数据和创建自定义的线段。 此功能强大的工具使您能够了解在您的应用程序中的客户体验。 可以方便地分析对网段并用您段作为强制目标。
一个常见的用例是您想要发送推式通知鼓励最终用户来评价您的应用程序存储区中。 而不是将通知发送到所有的最终用户，您可以创建将指定的用户才具有应用程序每天用于上个月，有很好的用户体验一段。 当发送更少、 更具针对性的推式通知时，您会得到更好的投资回报率。
 
 ![segments3][37]

### 您可以创建的段基于 Azure 移动服务要素︰
- 事件︰ 创建段目标一个特定事件发生一周两次以上的应用程序。 
- 会话︰ 创建用户使用应用程序 5 倍以上的最后一个星期的一的段。
- 活动︰ 创建用户使用的一个页面或内容大于或小于 10 的最后一个月时间的段。
- 作业︰ 创建一段已完成作业一天两次以上的用户。
- 崩溃︰ 创建所有用户已崩溃 10 倍以上的最后一个星期的一段。 （无法推动这一段道歉或甚至优惠券 ！）
- 错误︰ 创建一段有错误超过 100 次过去 3 天内的所有用户。
- 应用程序信息︰ 创建面向自定义的应用程序信息，在过去的 25 天中发生的一段。
 
 ![segments4][38]

若要以最佳方式使用段，必须先完成 SDK 的自定义的集成在应用程序中使用"应用程序信息"标记一个标签计划。
然后，转到该接口的主页页面中，选择"段"节单击该应用程序。

1. 选择"段"一节。
2. 单击"新段"按钮来创建一个新段。

## 真实的生命示例︰ 创建简单段根据"会话"的信息
创建已在最近一周至少 50 次使用您的应用程序的所有最终用户的一段。 据此，发现花了至少 30 秒，应用程序每个会话中最终用户。 这将显示所有的最终用户在您的应用程序中具有积极的体验。 然后，创建段可用于将通知推送到这些最终用户，请他们评价您的应用程序存储区中。
 
 ![segments5][39]

1. 为了在"字段"列表中快速查找它为您所在部门提供一个名称。
2. 请单击"创建"按钮。
 
 ![segments6][40]

选择会话。
 
 ![segments7][41]

1. 选择"上一周"期间。
2. 单击下一步。
 
 ![segments8][42]

1. 选择相关列表中的运算符: =;≥, ≤.
2. 输入所需的次数。
3. 选择所需的匹配项。 
4. 单击下一步。
本示例设置，以便符合在最近一周，进行了至少 50 会话的用户。
 
 ![segments9][43]

对于会话细分中，您可以选择每个作为条件的会话的长度。

1. 从列表中选择运算符。
2. 提供每个会话的长度。
3. 单击下一步。
在此示例中，它说，对所有会话的出现部分分段后，选择只花费了超过 30 秒，每个会话的用户。
 
 ![segments10][44]

为了完成漏斗，对它进行检索命名您的条件，然后单击完成。
 
 ![segments11][45]

完成您的条件设置后，它将出现在段漏斗。
一段基于分析数据，因为段来计算每天进行一次。
在此示例中，47,7%的总的最终用户匹配条件。 他们应该有更好的体验，并将可能提供更高等级，如果将它们推送通知，要求它们进行评价的应用程序存储区中的用户。

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

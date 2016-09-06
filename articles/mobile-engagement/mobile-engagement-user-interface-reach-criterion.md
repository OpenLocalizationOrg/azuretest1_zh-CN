---
ms.openlocfilehash: 5e67b8f06a77fc9cb063ba5ccbbd5cf2e76afbe0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="Azure 的移动服务用户界面的到达条件" 
   description="了解如何使用目标条件来选择使用 Azure 移动签约用户的子集发送推式营销活动" 
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


# 如何使用目标条件来选择您的用户的子集发送推式营销活动

目标观众的"新条件"按钮的特定条件是可以帮助您发送相关推式通知的 Azure 移动接触最强大概念之一的客户将响应而不是发送垃圾邮件的每个人。 可以将限制观众根据标准条件和模拟推进以确定究竟有多少人会收到通知。

**另请参见︰**

- [用户界面文档所不及的新推的市场活动][链接 27]

## 访问群体条件可以包括︰
- **Technicals:**可以根据相同的技术信息的分析和监视部分可以看到。 **另请参见︰**[用户界面文档-分析][15 链接]，[用户界面文档-监视器][链接 16]
- **位置︰**使用"实时位置报告"Geo 围栏的应用程序可以使用作为条件的地理位置群体从 GPS 的位置。 "惰性区位置报告"调用也可用于从手机位置访问群体 （"实时位置报告"和"惰性区位置报告"必须激活从 SDK）。 **另请参见︰**[SDK 文档的 iOS 的集成][链接 5]， [SDK 文档-Android 的集成][链接 5]
- **到达反馈︰**您可以指定目标观众根据其以前的到达通知到达宣告、 轮询，以及将数据反馈到反馈。 这使您能够更好的目标受众后两个或三个比第一次到达市场。 它还可以用于筛选掉已通过设置一个市场活动，不会发送到用户已经收到特定的早期活动收到了类似的内容，通知用户。 您甚至可以排除用户谁是包含在接收新的推进仍处于活动状态的特定活动。 **另请参见︰**[用户界面文档-到达-按内容][链接 29]
- **安装跟踪︰**您可以跟踪基于您的用户安装您的应用程序的位置的信息。 **另请参见︰**[用户界面文档的设置][链接 20]
- **的用户配置文件︰**您可以将基于的标准用户信息，而且您可以根据您创建的自定义应用程序信息的目标。 这包括当前登录的用户以及用户回答特定的问题，已要求它们若要设置在应用程序本身而不是只是如何他们具有早期市场活动响应的。 您的应用程序信息的所有定义您应用程序显示在此列表上。
- 段︰ 您还可以基于您创建的段的目标基于特定的用户行为包含多个条件。 所有您为您的应用程序定义的段显示在此列表上。 **另请参见︰**[用户界面文档-段][链接 18]
- **应用程序信息︰**从"设置"来跟踪用户的行为，可以创建自定义的应用程序信息标记。 **另请参见︰**[用户界面文档的设置][链接 20]

## 示例： 
如果您希望仅对您所执行的应用程序中采购操作的用户的子集推送通知。

1. 转到您的应用程序设置页，选择"应用程序信息"菜单，然后选择"新建应用程序 ino"
2. 注册新的 boolean 类型的值的应用程序信息，名为"inAppPurchase"
3. 使应用程序在用户成功地执行应用程序中采购时设置为"true"此应用程序信息 (通过使用 sendAppInfo ("inAppPurchase"，...) 函数)
4. 如果您不想从您的应用程序执行此操作，您可以做到从您的后端使用 API 的设备）
5. 然后，只需使用条件限制设置为"inAppPurchase"的用户到观众创建您的公告，"真正的")
 
> 注意︰ 目标基于不同的应用程序信息标记条件要求 Azure 移动签约用户的设备中收集的信息强制发送之前，可能会导致延迟。 将复杂的推送配置选项 （如更新标记） 也可以延迟。 使用推式 API 从"一种拍摄"市场是 Azure 移动合约的绝对最快的 push 方法。 只有应用程序信息标签用作推条件达到市场活动 （无论从到达 API 或 UI） 是下一步的最快方法，因为在服务器端存储应用程序信息标记。 由于 Azure 移动合作以便发送市场活动查询设备推市场活动中使用其他目标的条件是最灵活的但最慢 push 方法。
 
![到达 Criterion1][29] 

## 标准选项适用于︰
- **Technicals**     
- 固件名称︰ 固件名称
- 固件版本︰ 固件版本
- 设备模型︰ 设备模型
- 设备制造商︰ 设备制造商
- 应用程序版本︰ 应用程序版本
- 承运人名称︰ 承运人名称，未定义
- 承运人国家︰ 承运人国家，未定义
- 网络类型︰ 网络类型
- 区域设置︰ 区域设置
- 屏幕大小︰ 屏幕大小
- **位置**      
- 最后一次正确的区域︰ 国家，区域，所在地
- 实时地理围墙︰ 列表的 POIs （名称、 操作）、 名称、 纬度、 经度 (以米为单位的半径） 的循环 POI
- **到达的反馈**     
- 公告反馈︰ 公告，反馈
- 轮询的反馈︰ 调查中，反馈
- 轮询答案反馈︰ 答案的反馈、 问题、 选择轮询
- 数据推反馈︰ 数据推送，反馈
- **跟踪安装**     
- 存储︰ 存储，未定义
- 来源︰ 来源，未定义
- **用户配置文件**     
- 性别︰ 男或女，未定义
- 出生日期︰ 运算符、 日期、 未定义的
- 自愿参加︰ 真或假、 未定义
- **应用程序信息**      
- 未定义的字符串︰ 字符串
- 日期︰ 运算符、 日期、 未定义的
- 整数︰ 运算符、 数字、 定义
- 布尔值︰ true 或 false，未定义
- **段**    
- 段的名称 （从下拉列表中），排除 （目标不是属于这一段的用户）。

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

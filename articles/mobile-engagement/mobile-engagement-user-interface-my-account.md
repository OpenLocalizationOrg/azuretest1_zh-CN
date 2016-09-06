---
ms.openlocfilehash: 886eaf1b1c40ae481fe57205d7fbc40fd5acc228
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="Azure 的移动服务用户界面的我的帐户" 
   description="了解如何管理使用 Azure 移动服务帐户配置文件和测试设备" 
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

# 如何管理您的帐户配置文件和测试设备 
用户界面的我的帐户部分是在其中可以查看和更改您的帐户，包括您的配置文件的设置与关联的设置和测试设备 Id。 这些设置包含也可以通过设备 API 访问的项。

### 请参见
-  [故障排除指南-服务][链接 24]

![MyAccount1][7]  

## 配置文件︰
您可以查看或更改您的帐户设置︰ 密码、 名字、 姓氏、 组织、 电话号码、 时间区域中或电子邮件选择加入或选择退出电子邮件更新。 您还可以授予其他用户的权限使用应用程序根据其电子邮件地址从"主页"。

### 请参见
-  [用户界面文档-主页][链接 13]

![MyAccount2][8]  

## 设备︰
您可以查看、 添加或移除测试设备 ID 可用于测试您到达或推的市场活动的测试设备。 有关如何查找每个平台的设备的设备 ID 的上下文说明 (iOS，Android、 Windows Phone 等) 当您单击"新设备"时显示。 
 
![MyAccount3][9]  
 
使用推 API 或设备的 API，您需要知道您的用户的唯一的设备标识符 （deviceid 参数）。 有几种方法来检索它︰
 
1. 从您的后端，可以使用设备 API 的"Get"功能以获取完整的设备标识符列表。
2. 从您的应用程序，您可以使用 SDK 来获取它。 （在 Android，调用 getDeviceID() 函数的代理类，然后在 iOS 中，阅读的代理类 deviceid 属性。）
3. 从到达公告，与公告关联的操作 URL 包含 {deviceid} 模式中，如果将自动代替它触发操作的设备标识符。
http://<example>.com/registeruser?deviceid = {deviceid} & otherparam = myparamdata 将替换为︰ http://<example>.com/registeruser?deviceid = XXXXXXXXXXXXXXXX & otherparam = myparamdata 
4. 从到达 web 发布，如果此次产品发布的 HTML 代码中包含 {deviceid} 模式中，将自动代替它显示 web 发布的设备标识符。
这里是我的设备标识符: {deviceid} 将替换为︰ 这里是我的设备标识符︰ XXXXXXXXXXXXXXXX
5.  打开您的设备上的应用程序并在您已标记的应用程序中执行事件。
从"用户界面-您的应用程序监视器的事件的详细信息"可以找到您在列表中执行事件。
单击监视器中的此事件。
您应该执行了此事件的设备的列表中发现的设备 ID。
然后，可以复制此设备 ID，并将其注册在"UI-我帐户-设备的新设备-选择您的设备平台"。
>（请注意，当 IDFA iOS 被禁用，设备 ID 可能会更改随着时间的推移如果您卸载并重新安装您的应用程序。）

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

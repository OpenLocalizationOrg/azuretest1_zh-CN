---
ms.openlocfilehash: a6951e2a146d8e79f5ee87f0bc9d56ffd011ad8b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="Azure 的移动服务用户界面-到达市场活动" 
   description="Laern 如何创建和管理推式通知市场活动使用 Azure 移动服务" 
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


# 如何创建和管理推送通知市场活动
可以使用用户界面的范围部分用复杂的公式创建新推入市场，通过提供发送推式通知所需的所有信息。 推入市场的选项略这四个市场活动类型取决于︰ 通告、 轮询、 数据推送，及拼贴 (仅适用于 Windows Phone)。

### 选项将应用于︰
- 语言︰ 所有 （公告，轮询，数据推送，拼贴）
- 市场活动︰ 所有 （公告，轮询，数据推送，拼贴）
- 通知︰ 公告轮询
- 内容︰ 唯一的每个市场活动类型
- 观众︰ 所有 （公告，轮询，数据推送，拼贴）
- 时间段︰ 公告，轮询，拼贴
- 测试︰ 所有 （公告，轮询，数据推送，拼贴）
 
![到达 Campaign1][20]

## 语言
语言下拉列表菜单可用于将您推的不同版本发送到已设置为使用不同语言的设备。 默认情况下，所有设备将都接收相同推送将它们设置为使用何种语言无关。 用他们的设备设置为另一种语言的用户将收到推式的默认语言版本。 许多推市场活动选项允许您指定的每种其他语言选择替换内容。 
 
![到达 Campaign2][21]

### 语言的差异都适用于︰
- 唯一的语言可能选择除默认语言的语言︰
- 市场活动︰ 相同的所有语言
- 通知︰ 针对每种语言，以及默认语言
- 内容︰ 针对每种语言，以及默认语言
- 观众︰ 可能由不同的语言条件筛选
- 时间段︰ 对所有语言相同
- 一次，可能被测试︰ 发送到每一种语言
 
### 支持的语言︰
- 阿拉伯语 (ar) 
- 保加利亚语 (bg) 
- 加泰罗尼亚语 (ca) 
- 中文 （中文） 
- 克罗地亚语 （小时） 
- Czech (cs) 
- 丹麦语 (da) 
- 荷兰语 (nl) 
- 英语 (en) 
- 芬兰语 (fi) 
- 法语 (fr) 
- 德语 (de) 
- 希腊语 (el) 
- 希伯来语 （他） 
- 印度语 (hi) 
- 匈牙利语 (hu) 
- 印度尼西亚语 (id) 
- 意大利语 （它） 
- 日语 (ja) 
- 朝鲜语 (ko) 
- 拉脱维亚语 (lv) 
- 立陶宛语 (lt) 
- 马来语 (macrolanguage) （毫秒） 
- 挪威博克马尔语 (nb) 
- 波兰语 (pl) 
- 葡萄牙语 (pt) 
- 罗马尼亚语 (ro) 
- 俄语 (ru) 
- 塞尔维亚语 (sr) 
- 斯洛伐克语 (sk) 
- 斯洛文尼亚 (sl) 
- 西班牙语 (es) 
- 瑞典语 (sv) 
- 塔加路文 (tl) 
- 泰语 (th) 
- 土耳其语 (tr) 
- 乌克兰语 （英国） 
- 越南语 (vi) 
 
## 市场活动
可以使用市场活动部分设置的名称和您的客户活动的类别，好像打算来忽略推入市场的观众部分，改为发送访问 API （和某些元素与低级别推 API） 通过此市场活动。 类别可与控件中应用程序通知根据预定义的设置自定义的通知模板。 您可以通过访问 API 您现有"类别"列表。

> 警告︰ 如果您使用"忽略观众推将被发送到用户通过 API"选项到达市场，"市场活动"一节中市场活动将不会自动发送，您将需要通过访问 API 对其进行手动发送。
 
![到达 Campaign3][22]
 
### 选项将应用于︰
- 名称︰ 所有
- 类别︰ 公告轮询
- 忽略观众，将通过 API 向用户发送推︰ 所有
 
## 通知
您可以使用通知部分设置用于推送包括基本设置︰ 标题的推、 消息、 应用程序中的图像，或者如果它防碍。 许多通知的设置是设备的特定于您的平台。 您可以选择是否将发送您推，"应用程序"或"从 app"或两者。 （请记住，用户可以"自愿参加"或"退出"的"从 app"将在操作系统级别上他们的设备，Azure 移动服务将不能重写此设置。 还要切记，到达 API 处理"应用程序"中将"出应用程序的"。 推式 API 可以用于处理"外 app"推进过。）带图片或 HTML 内容，包括深层链接链接您的应用程序的外部或向您的应用程序 （Android SDK 2.1.0 或所需的更高意图类别） 中的其他位置，可以自定义推送。 可以更改图标或 iOS 的徽章，并发送文本或 web 内容 （弹出带到另一个位置的内部或外部应用程序的 html 内容，URL 链接）。 此外可以使 Android 设备环或振动与推。 (请记住，您需要正确您 Android SDK 权限清单文件以环或振动设备。)目前尚没有行业标准的 Android 宏伟蓝图的大小，因为屏幕大小有不同的每个设备，但几乎所有的屏幕大小 400 x 100 图片处理。

### 交货类型︰
-    应用程序只从︰ 当用户不使用应用程序时，将发送通知。
- 唯一的应用程序的通知的要求来自苹果或 Google （APNS 或 GCM 证书） 的证书。
- 在应用程序只︰ 仅在运行应用程序时显示的通知。
- 通知使用 Capptain 传递系统，以达到用户。 完全可以自定义您推的可视布局/显示。
- 任何时间︰ 此选项确保您发送的通知与应用程序正在运行。

 
![到达 Campaign4][23]

### 选项将应用于︰
- 通知︰ 公告轮询
 
## 内容
可以使用内容部分修改内容通知、 轮询、 数据推送，和拼贴 (仅适用于 Windows Phone)。 推式营销活动的内容设置是特定于类型的市场活动。 

### 请参见
- [用户界面文档-到达-按内容][链接 29]
 
![到达 Campaign5][24]

## 受众
访问群体部分可用于定义标准列表中的项来限制您的市场活动或市场活动根据自定义条件的限制。 一组标准的选项来限制观众可以推送到新的或旧的用户或仅本机推用户。 您还可以设置配额来限制接收推的用户数量。 您可以手动编辑市场活动的筛选方式，包括对目标用户的一个或多个条件的表达式。 您可以手动键入表达式访问群体。 这种表达式，必须明确定义条件之间的关系。 条件被描述一个标识符，必须以大写字母开头，并且不能包含空格。 可以使用所述条件之间的关系 '和' 或，not 运算符以及 '（'，'）。 示例:"Criterion1 Criterion1 和不 Criterion2）"。

> 注意︰ 与众多的观众，包括在市场活动中，针对扫描的服务器端很慢，尤其是当您尝试同时启动多个市场活动。

- 如果可能，每次只能启动一个市场活动。
- 在最多、 一次只开始四个市场。
- 仅对当前用户推送 ("接洽只可以使用本机推到达的用户"复选框和"接洽活动只有")，以便只有您仍然必须安装该应用程序并使用它的用户才会需要进行扫描。
定义您的观众后，可用于模拟按钮查找出多少个用户将收到此推。 这将计算 （这是根据用户的随机样本估计） 此访问群体的潜在目标的已知用户的数量。 请注意已卸载该应用程序的用户也是该访问群体的一部分，但不能达到。

### 请参见
- [用户界面文档所不及的新强制标准][链接 28]

![到达 Campaign6][25]

### 编辑表达式
![到达 Campaign7][26]
 
### 限制观众选项适用于︰
- 吸引用户的子集︰ 所有公告、 轮询、 数据推送 (平铺）
- 与合作只有旧用户︰ 所有公告、 轮询、 数据推送 (平铺）
- 与合作只有新用户︰ 所有公告、 轮询、 数据推送 (平铺）
- 与合作只有空闲用户︰ 公告，轮询，拼贴
- 接洽活动只有︰ 所有公告、 轮询、 数据推送 (平铺）
- 与合作只有用户才可以使用本机推达到︰ 公告，轮询
 
## 时间范围
可以使用时间范围部分中设置将发送推送或可以空缺时间框架若要立即开始市场活动时。 请记住，使用最终用户的时区可能启动市场一天早于您希望最终用户在亚洲的并发送小批量一次直到所有世界上的时区匹配为您的活动设置时间范围推进。 使用最终用户的时区也可能导致延迟活动中因为它开始推前通过电话请求的时间。

> 注意︰ 市场活动结束日期可以缓存没有推进本地及仍显示它们之后手动完成市场活动。 若要避免这种现象，特定市场活动的结束时间。

### 请参见
- [到达-如何 Tos – 计划][链接 3] 
 
![到达 Campaign8][27]

### 设置应用于︰
- 时间段︰ 公告，轮询，拼贴
 
## 测试
测试部分可用于在保存市场活动之前将此推发送到您自己的测试设备。 如果您配置了此市场活动的任何自定义语言，您可以在每种语言测试推。 您可以设置从"我的帐户"测试设备。
> 注意︰ 当您使用按钮以"测试"，记录数据没有服务器端推送，真正推入市场活动只记录数据。

### 请参见
- [用户界面文档-我的帐户][链接 14]
 
![到达 Campaign9][28]

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

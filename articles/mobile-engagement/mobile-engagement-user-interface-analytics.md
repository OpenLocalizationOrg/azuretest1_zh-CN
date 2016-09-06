---
ms.openlocfilehash: ca2148aca1100356181449f78e43912bc64b0ed6
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="Azure 的移动服务用户界面的分析" 
   description="了解如何分析应用程序使用 Azure 移动接洽有关历史数据" 
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

# 如何分析有关您的应用程序的历史记录数据
用户界面的分析部分提供了有关应用程序基于每 24 小时更新的历史数据的聚合的信息。 在不同组成饼形线/条形图表、 网格和地图的仪表板上显示信息。 为.csv 文件，还可以下载数据。 此相同的信息大部分是用户界面的监视器中的实时提供，也可以从分析 API 访问。 "概念"中的"术语表"已定义的术语和缩写词中分析和监视，如下所示︰ 活动用户，新用户、 保留用户、 会话、 用户路径图、 用户映射、 跟踪 Url、 趋势、 活动、 事件、 作业、 错误、 额外信息、 崩溃和应用程序信息。

### 请参见
-  [概念的术语表][6 链接]，[故障排除指南-分析][链接 21]

## 标准和自定义分析
Azure 的移动服务提供一套基本的标准分析的应用程序可以用图形表示，只要您将您的应用程序集成使用 SDK 信息。 Azure 移动服务还提供了收集有关您的最终用户行为所需的附加自定义分析信息的能力。 您可以通过创建自定义"应用程序信息标记"，以便 Azure 移动合作收集此附加的数据，从"设置"创建标记计划来执行此操作。

### 请参见
-  [用户界面文档的设置][链接 20]
 
## 分析标头
- 项目名称︰ 标签的项计数
- 显示帮助︰ 显示有关这部分的上下文信息
- 版本︰ 使您可以显示有关每个版本的应用程序的不同分析信息或显示所有版本的信息。 (注︰ 筛选分析数据在用户界面中的将显示所有属于该类型，而不管您的应用程序的版本。 例如，按名称筛选"崩溃"将显示从版本 1 和版本 2 的应用程序。)
- 时间段︰ 过去 7 天内，最近 30 天内，所有的时间，自定义
- 速度︰ 每小时、 天、 周、 月
- 对下载的 CSV 文件的仪表板视图︰ 行图表、 网格、 发送
 
![Analytics1][10] 

## 分析
- 面板︰ 显示有关新的和活动用户和其发展趋势的一般信息。
- 用户︰ 用户由其设备标识符︰ 该标识符是唯一的每个设备 （一个新用户是实际上一个新的设备）。 用户都被视作新在某个给定的时间间隔内如果他已执行在此时间间隔过程中的他第一个会话。 因为如果他执行了在最近 7 天至少一个会话保留，则认为用户。 活动的用户是做在某一给定期间的至少一个会话的用户。 您可以按排序，每月、 每周、 每天或每小时的时间段。 所有图表类似，但允许您按不同功能，如您的应用程序的版本的筛选器，然后再通过一段时间内的排序。 标准集成 SDK 所收集的信息包括下面︰ 由客户使用的活动用户，新用户、 会话数，每个会话、 有关国家、 局部变量、 位置、 语言运营商、 设备、 固件、 网络 (WIFI)、 版本的应用程序和 SDK，技术的信息的长度。 实时监视部分中，可以查看此信息。 
- 跟踪源显示下载给定的促销活动的结果的应用程序的新用户数。 用户由其设备标识符︰ 该标识符是唯一的每个设备 （一个新用户是实际上一个新的设备）。 用户都被视作新在某个给定的时间间隔内如果他已执行在此时间间隔过程中的他第一个会话。
- 跟踪存储显示的数目已从给定存储下载应用程序的新用户。 用户由其设备标识符︰ 该标识符是唯一的每个设备 （一个新用户是实际上一个新的设备）。 用户都被视作新在某个给定的时间间隔内如果他已执行在此时间间隔过程中的他第一个会话。

> 注︰ 时间段基于日期从用户的设备设置，使的用户的电话有的日期设置不正确可能显示在错误的时间段。

- 保留︰ 由于保留在某个给定的时间间隔内，如果他已执行在此时间间隔过程中的他第一次会话，则认为用户。 您可以更改为小时、 天、 周或月的时间间隔，在此期间保留的用户 （和新用户） 也包括在内。 用户保留分析建立在群体。 队列是一套为给定期间检测到的所有新用户 （即执行在此期间他们第一次会话用户的设置）。 我们使用的 1 天、 2 天、 4 天、 7 天或 1 个月的群体。 由于群体，每天 1-2 天，4 天、 7 天或 1 个月，Azure 的移动服务计算的群体，并且属于所有用户集仍然活动 （亦即执行期间至少一个会话的用户组）。 此组的用户称为群体版本。 (Azure 移动合作可以显示您多少您的用户仍在使用您的应用程序，但仅平台特定商店可以告诉您多少用户卸载您的应用程序 — 例如，GooglePlay，iTunes，Windows 应用商店，等等。)。 
- 会话︰ 一个使用该应用程序的用户。 由用户执行的活动的序列中生成的会话 （活动是通常的一个应用程序时，屏幕使用相关联，但这可能因应用程序中已集成了 SDK 的方式）。 用户一次只能执行一个活动︰ 一旦用户开始他的第一个活动，并停止他完成他最后一次活动时启动会话。 如果用户不执行任何活动的情况下持续超过几秒钟，他的活动序列被分为两个不同的会话。
- 活动︰ 在您的应用程序中每个屏幕的名称和长度的时间用户花费在每个屏幕上。 活动是一个自定义的分析选项将对应于您已设置为您自己的应用程序的"应用程序信息"标记︰
- 用户路径︰ 将显示您的用户如何导航应用程序的活动 （屏幕）。 您可以移动滑块以调整的详细信息的级别。 蓝色的节点表示应用程序的活动。 它们的大小成正比时用户所用的它。 白色的节点表示会话开始和停止。 红色的节点表示崩溃。 链接表示应用程序的活动之间 （或活动和崩溃之间） 的转换。 单击某个节点或链接，以显示与数据有关的详细信息的工具提示︰ 计数过渡，以及从源活动过渡到目标活动的百分比，在特定的屏幕中，所用的时间。 (---60%---> 在活动 A 上的用户转到活动 B B 意味着 60%的时间。)可以重新组织图表，需要澄清每次进行更改时，将保存其位置。 您可以显示或隐藏系统崩溃要淡化图形。
- 事件︰ 用户在应用程序中采取特定的操作。 事件分发显示为每个用户每次会话的事件数。 事件表示即时操作，例如，单击某个按钮或通知的接收。 （事件的含义取决于如何将 SDK 集成在应用程序中。事件会话或作业过程中可以出现也可以独立。
- 作业︰ 类似于事件除了关注操作的长度。 例如，作业可以告诉您有关时间内容为负载或调用 web 服务的技术信息。 它还可以显示用户填写的表单，创建一个帐户，或购买花费多长时间。 作业表示任务的工期，则例如，下载任务或时间的持续时间横幅显示在屏幕上。 （作业的含义取决于如何将 SDK 集成在应用程序中。作业是通常与一个会话 （即，无需任何用户活动） 的范围之外执行的后台任务。
- Technicals︰ 技术有关的设备的信息您可以跟踪，如区域设置、 运营商、 网络、 设备、 固件和屏幕大小的用户的设备和您的应用程序的版本以及您的应用程序中使用 SDK 版本的应用程序的用户。
- 错误︰ 信息技术在应用程序内部错误不会导致应用程序崩溃。 错误表示一个即时的问题，例如，网络故障或不正确的操作。 （事件的含义取决于如何将 SDK 集成在应用程序中。错误的会话或作业过程中可能发生，也可以是独立。
- 崩溃︰ 信息导致应用程序崩溃的错误。 系统崩溃是意外的情况，其中应用程序停止执行其正常的工作，必须停止。 崩溃通常是由于应用程序中的 bug。
 
![Analytics2][11] 

## 访问固定概述
![Analytics3][12] 

固定概述分解成几个卡，中间每一段保留显示概述。 2 天的保留期间示例所示。 其他卡显示 4 天和 7 天保留期。 

## 了解固定概述卡
![Analytics4][13] 

### 每张卡片组成 3 个主要部分︰
1. 1︰ 考虑群体和期间
2. 2-4︰ 当前期间的保留
3. 5︰ 历史的迷你图

### 下面是有关每个元素的详细的信息︰
1.    群体和段︰ 此标题为该类型的群体。 这里"2 天期"意味着我们将着眼于用户的行为超过 2 天，2 天以下的块中连接用户数天，和是否到达 2 一段。 上面的示例中将考虑 21 和 22 11 月之间的用户的活动。
2.    这样在 19 和 20 11 月到达用户的保有率，通过 21 和 22 11 月。 这里我们有 1 个活动用户之间 21，22，通过了新用户在 19 和 20 之间的 3。
3.    此视觉指标给出与上面相同的信息以图形方式表示。 （第三个圆是从 33%数）。颜色将产生更多的信息︰ 绿色表示这个数字增长从以前的计算。 黄色表示减少的稳定，而红色表示。
4.    这表明用于计算的值。
5.    这是迷你的保留值的历史记录。 它允许您查看在过去具有总揽方式发展而来的值。

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

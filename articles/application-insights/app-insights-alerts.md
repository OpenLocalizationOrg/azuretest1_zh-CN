---
ms.openlocfilehash: 7972349f34c067583e6c30c191a175e2dead20f8
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="在应用程序的见解中设置警报" 
    description="获取有关系统崩溃、 异常、 指标更改的电子邮件。" 
    services="application-insights" 
    documentationCenter=""
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/12/2015" 
    ms.author="awills"/>
 
# 在应用程序的见解中设置警报

[Visual Studio 应用程序见解][开始]可以提醒您在性能或更改您的应用程序中的使用情况指标。 

应用程序的见解监视实时应用程序在[多种平台][的平台]可帮助您诊断性能问题，并了解使用模式上。

有两种类型的警报︰
 
* **Web 测试**可以告诉您时您的站点是在 internet 上，不可用或响应很慢。 [了解更多][可用性]。
* **公制的警报**会告诉您当任何指标跨越段-如失败计数、 内存或页面视图阈值。 

没有[单独页的 web 测试]的[可用性]，因此我们将重点介绍以下指标预警。

## 指标预警

如果尚未为您的应用程序，[请先不要][开始]设置应用程序的见解。

要得到一封电子邮件，跃点值超过阈值时，开始从测量数据资源管理器中，或者从概述刀片式服务器上的预警规则拼贴。

![在预警规则刀片式服务器，选择添加通知。 设置您的应用程序作为资源进行测量，为该警报，提供一个名称，然后选择指标。](./media/app-insights-alerts/01-set-metric.png)

设置其他属性之前的资源。 **选择"（组件）"资源**如果想要性能和使用情况指标设置警报。

请注意，您需要输入的临界值的单位。

*我看不到添加通知按钮。* -您使用组织帐户吗？ 如果您具有所有者或参与者对此应用程序资源的访问，您可以设置警报。 看一看设置-> 用户。 [关于访问控制][角色]。

## 请参阅您的通知

您收到一封电子邮件时通知更改状态处于非活动状态和活动之间。 

预警规则刀片式服务器显示了每个警报的当前状态。

历史记录状态更改是操作事件日志中︰

![在概述刀片，靠近底部，单击事件在过去一周](./media/app-insights-alerts/09-alerts.png)

*这些"事件"与遥测事件或自定义事件相关？*

* No. 这些操作的事件都记录到此应用程序资源发生了的事情。 

## 可用性的通知

您可以设置测试来自世界各地点的任何 web 站点的 web 测试。 [了解更多][可用性]。

## 好的警报设置有哪些？

这取决于您的应用程序。 启动时，则最好不要设置太多的度量标准。 花些时间来观察指标图表，在您的应用程序运行时，它如何可以正常工作的体会。 这将帮助您找到的方法来提高其性能。 然后设置警报通知您当指标超出正常的区域。 

常见的警报包括︰

* [Web 测试][可用性]是很重要的如果您的应用程序是可见，在公共 internet 的网站或 web 服务。 他们告诉您是否您的网站出现故障或响应速度很慢的即使它是承运人的问题，而不是您的应用程序。 但它们的综合测试，因此他们不衡量用户的实际体验。
* [浏览器标准][客户端]，特别是浏览器页面加载时间，非常适用于 web 应用程序。 如果您的页面具有大量脚本，您需要浏览器异常关注。 为了获得这些指标和警报，您必须设置[网页监控][客户端]。
* 服务器的响应时间和服务器端 web 应用程序的请求失败。 设置警报，以及留意查看如果不按比例变化具有高请求率这些指标︰ 指示您的应用程序已不足的资源。



<!--Link references-->

[可用性]: app-insights-monitor-web-app-availability.md
[客户端]: app-insights-javascript.md
[平台]: app-insights-platforms.md
[角色]: app-insights-resources-roles-access-control.md
[start]: app-insights-get-started.md

 
测试

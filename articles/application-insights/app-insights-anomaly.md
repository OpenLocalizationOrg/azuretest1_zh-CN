---
ms.openlocfilehash: 87f5b22e4b16849b9d610e590f9f635605f5eb49
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="应用程序的见解︰ 主动预防性的异常检测" 
    description="应用程序见解执行应用遥测的深入分析和潜在问题的警告。" 
    services="application-insights" 
    documentationCenter="windows"
    authors="alancameronwills" 
    manager="ronmart"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/18/2015" 
    ms.author="awills"/>

#  应用程序的见解︰ 主动预防性的异常检测

*在预览是应用程序的见解。*


应用程序深入执行深入分析您的应用程序遥测，并警告您关于潜在的性能问题。 可能正在阅读本文由于通过电子邮件收到我们异常警报。

## 关于异常警报

* *为什么收到过此警报？*
 * 应用程序的见解定期分析您的数据与模式识别规则。 它查找异常可能指示您的应用程序中的性能问题。
* *该通知并不意味着我肯定有问题吗？*
 * No. 它是只是建议有关一些您可能想要近距离看多。 
* *我应该做什么？*
 * [看看显示的数据](#responding-to-an-alert)，并考虑是否它可能表示出现了问题。 如果不是，这很好。
* *你们这些家伙，看我的数据吗？*
 * No. 该服务是完全自动的。 只有您将收到通知。 您的数据是[私有的](app-insights-data-retention-privacy.md)。


## 检测过程

* *检测到何种异常情况？*
 * 您会发现它非常耗时，要检查自己的模式。 例如，性能不佳的特定组合的位置、 时间和平台。
* *可以创建自己的异常检测规则？*
 * 不进行这种深入的分析。 （但您可以[设置通知](app-insights-alerts.md)，告诉您当指标跨越阈值）。
* *频率进行分析？*
 * 我们不在没有得到多大的遥测应用程序资源上执行分析。 你不可能收到警告，指出您的调试会话。


## 对警报的响应

从电子邮件或从异常列表，请打开异常情况报告。

![](./media/app-insights-anomaly/02.png)

注意︰

* 说明
* 影响陈述，告诉您多少或隔多久用户会受到影响。

单击以打开刀片式服务器的更多详细信息的图表。

修改时间范围和筛选器，以探究遥测。




## 通知电子邮件

* *是否有可以订阅此服务才能接收通知？*
 * No. 我们 bot 定期调查所有的应用程序理解用户的数据和发送通知，如果它检测到的问题。
* *可以取消订阅或获得而是发送给我的同事的通知？*
 * 单击一个或多个电子邮件中的链接。 打开异常设置。
 
    ![](./media/app-insights-anomaly/01.png)

    当前，它们被发送到那些具有[写访问权限的应用程序理解资源](app-insights-resources-roles-access-control.md)。
* *我不想将这些消息所淹没。*
 * 它们仅限于三个每日。 不会重复的任何消息。
* *如果我不做任何事情，我会提醒？*
 * 否，就每个问题有关的消息只有一次。
* *丢失的电子邮件。 在哪里找到的门户的通知？*
 * 在您的应用程序的应用程序的见解概述中，单击**异常**拼贴。 






 
测试

---
ms.openlocfilehash: cd3ec0072fd74da1378b3557167496a495f87264
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="检测和诊断中应用的见解与 Windows 应用商店和电话应用程序崩溃" 
    description="分析应用程序的见解与 Windows 设备应用程序中的性能问题。" 
    services="application-insights" 
    documentationCenter="windows"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/01/2015" 
    ms.author="awills"/>

# 检测和诊断中应用的见解与 Windows 应用商店和电话应用程序崩溃

*在预览是应用程序的见解。*

如果您的用户体验崩溃在您的应用程序，您想要知道关于它速度快，并且想出了什么问题有关的详细信息。 应用程序的见解，您可以监视频率崩溃发生时，当它们发生，并且调查个别事件报告获取获得通知。

"崩溃"意味着应用程序终止由于未捕获的异常。 如果您的应用程序捕捉到一个异常可以报告它与[TrackException API][apiexceptions] ，但仍可以继续运行。 在这种情况下，它将不会记录为故障。


## 显示器故障频率

如果尚未完成此操作，添加[windows]的[应用程序到应用程序项目的见解]，并将其重新发布。 

在[应用程序的见解门户][门户]应用程序的概述刀片式服务器上显示崩溃。

![](./media/app-insights-windows-crashes/appinsights-d018-oview.png)

您可以编辑的图表所示的时间范围。


## 设置警报来检测故障

![从崩溃图表，请单击警报规则，然后添加通知](./media/app-insights-windows-crashes/appinsights-d023-alert.png)

## 诊断故障

了解如果某些版本的应用程序崩溃崩溃图表通过超过其他人，请单击，然后再分段按应用程序版本︰

![](./media/app-insights-windows-crashes/appinsights-d26crashSegment.png)


要发现导致崩溃的异常，请打开诊断搜索。 您可能想要删除其他类型的遥测，将精力集中在例外情况︰

![](./media/app-insights-windows-crashes/appinsights-d26crashExceptions.png)

[了解有关筛选诊断搜索][诊断]。
 

单击要查看其详细信息，包括关联的属性和堆栈跟踪的任何异常。

![](./media/app-insights-windows-crashes/appinsights-d26crash.png)

请参阅异常和接近该异常发生的事件︰


![](./media/app-insights-windows-crashes/appinsights-d26crashRelated.png)

## 插入跟踪日志和事件

为了帮助诊断问题，您可以[插入跟踪调用和搜索应用程序的见解中的日志][诊断]。

## <a name="debug"></a>调试和发布模式

#### Debug

如果在调试模式下生成，只要它们生成发送事件。 如果您断开 internet 连接，然后在重新连接之前退出该应用程序脱机遥测将被丢弃。

#### 发行版

如果在发布配置生成时，事件会存储在该设备中，然后发送时应用程序将继续。 数据还发送应用程序的第一次使用。 如果在启动时没有 internet 连接，以前遥测以及遥测数据的当前生命周期存储和发送时间为下一步继续。

## <a name="next"></a>下一步行动

[检测，会审和诊断问题的应用程序的见解][检测]

[应用程序 API 的见解][api]

[诊断日志捕获][跟踪]

[疑难解答](app-insights-windows-troubleshoot.md)




<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiexceptions]: app-insights-api-custom-events-metrics.md#track-exception
[检测]: app-insights-detect-triage-diagnose.md
[诊断]: app-insights-diagnostic-search.md
[平台]: app-insights-platforms.md
[门户网站]: http://portal.azure.com/
[跟踪]: app-insights-search-diagnostic-logs.md
[窗口]: app-insights-windows-get-started.md

 
测试

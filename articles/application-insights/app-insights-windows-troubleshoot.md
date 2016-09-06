---
ms.openlocfilehash: e23833c7435f0a4e1d35a072df0f114e13e99b6b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="解决在 Windows 设备应用程序的见解" 
    description="故障排除指南和提示问题和答案。" 
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
    ms.date="06/17/2015" 
    ms.author="awills"/>
 
# 疑难解答和问答的 Windows 应用程序的见解设备

任何问题或与[Windows 中的 Visual Studio 应用程序理解][windows]的问题？ 以下是一些提示。



## 无数据 

*已成功添加应用程序的见解，并运行我的应用程序，但我从未见过在门户网站中的数据。*

* 等待一分钟，然后单击刷新。 目前，刷新不是自动的。
* 请检查您有检测键定义在 ApplicationInsights.config 文件中，它是应用程序的见解门户中的密钥相同。 若要查看该密钥，单击概述刀片式服务器概要。
* 请确保您的应用程序[请求出站网络访问权限](https://msdn.microsoft.com/library/windows/apps/hh452752.aspx)。
* 有仿真程序或测试设备和应用程序的见解门户之间的防火墙吗？ 您可能需要打开 TCP 端口 80 和 443 用于 dc.services.visualstudio.com 和 f5.services.visualstudio.com 的传出通信。
* 在 Microsoft Azure 中开始板，查看服务状态映射。 如果有一些警告的迹象，应该等到他们返回到确定然后关闭并重新打开您的应用程序理解应用程序刀片式服务器。


#### 我用来查看数据，但它已停止

* 检查[状态的博客](http://blogs.msdn.com/b/applicationinsights-status/)。
* 您点击您每月的配额的数据点了吗？ 打开设置/配额和价格来看一看。 如果是这样，可以升级您的计划，或支付额外的容量。 请参阅[定价方案](http://azure.microsoft.com/pricing/details/application-insights/)。


## 如何到通用的应用程序中添加应用程序的见解？

手动将 NuGet 程序包添加到解决方案中的每个设备项目。 请参阅[入门指南的通用应用程序][通用]。

## 禁用遥测

*如何禁用遥测集合？*

在代码中︰

    TelemetryConfiguration config = TelemetryConfiguration.Active;
    config.TrackingIsDisabled = true;

## 更改目标资源

*如何更改我的项目发送到数据的应用程序理解资源？*

在新应用程序的见解概述刀片式服务器，打开基础和复制的检测项。

将密钥粘贴到 ApplicationInsights.config 文件中`<InstrumentationKey>`节点。

或者，如果您想要在运行时更改目标，使用︰

     var telemetry = new TelemetryClient();
     telemetry.Context.InstrumentationKey = newKey;
    
## 如何监视客户端-服务器应用程序？

有两种方法来执行此操作︰

* 创建两个应用程序的见解资源 （不同检测键），客户端和服务器。 但在同一的 Azure 的资源组中创建它们。 这样，就可以轻松切换一个，另一个。
* 使用一个应用程序的见解资源并将其检测密钥放入客户端和服务器项目。 然后您可以关联指标和来自两个源的事件。

为了帮助在客户端和服务器的关联事件，生成每个请求操作 id。 在客户端和服务器之间传输，将其添加到两端的遥测数据︰

    telemetry.Context.OperationId = opid;


## Azure 的启动屏幕

*我查看[Azure 的门户](http://portal.azure.com)。 没有地图告诉我一些关于我的应用程序吗？*

不，它显示世界各地 Azure 服务器的运行状况。

*从 Azure 开始板 （主屏幕） 中，如何找到数据有关我的应用程序？*

假设您[已经设置了您的应用程序理解的应用程序][窗口]，单击浏览，选择应用程序的见解，并选择您为您的应用程序创建的资源。 获取那里更快地在将来，可以收回到开始委员会的资源。

## 数据保留 

*数据保留在门户网站中的多长时间？ 是否安全？*

请参阅[数据保留和隐私][数据]。

## 下一步行动

*设置应用程序理解的我的 Java 服务器应用程序。 我可以做些什么？*

* [将 web 页的监视器可用性][可用性]
* [监视 web 页使用情况][使用]
* [跟踪使用情况和诊断设备应用程序中的问题][平台]
* [编写代码以跟踪您的应用程序的使用情况][跟踪]
* [诊断日志捕获][javalogs]


## 获取帮助

* [堆栈溢出](http://stackoverflow.com/questions/tagged/ms-application-insights)

<!--Link references-->

[可用性]: app-insights-monitor-web-app-availability.md
[data]: app-insights-data-retention-privacy.md
[javalogs]: app-insights-java-trace-logs.md
[平台]: app-insights-platforms.md
[跟踪]: app-insights-api-custom-events-metrics.md
[通用]: app-insights-windows-get-started.md#universal
[使用]: app-insights-web-track-usage.md
[窗口]: app-insights-windows-get-started.md

 
测试

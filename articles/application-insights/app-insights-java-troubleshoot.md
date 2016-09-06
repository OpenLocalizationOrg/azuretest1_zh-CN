---
ms.openlocfilehash: 71c8a83529030ee2e4235a4e70f24ed91b494404
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="在 Java web 项目中解决应用程序的见解" 
    description="故障排除指南和提示问题和答案。" 
    services="application-insights" 
    documentationCenter="java"
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
 
# 故障排除和 Q 和 A 的 Java 应用程序理解的

问题还是[在 Java 中的 Visual Studio 应用程序理解][java]的问题？ 以下是一些提示。


## 生成错误

*在 Eclipse 中添加应用程序深入 SDK 通过 Maven 或 Gradle 时，得到生成或校验和验证错误。*

* 如果依赖项<version>元素与通配符一起使用的模式 (例如 (Maven) `<version>[0.9,)</version>` (Gradle) 或`version:'0.9.+'`)，请尝试指定特定版本而像`0.9.3`。

## 无数据 

*已成功添加应用程序的见解，并运行我的应用程序，但我从未见过在门户网站中的数据。*

* 等待一分钟，然后单击刷新。 图表会定期刷新自己，但是也可以手动刷新。 刷新间隔取决于图表的时间范围。
* 检查您具有检测键定义在 ApplicationInsights.xml 文件中 （在项目中的资源文件夹中）
* 验证是否有任何`<DisableTelemetry>true</DisableTelemetry>`在 xml 文件中的节点。
* 在您的防火墙，可能需要打开 TCP 端口 80 和 443 用于 dc.services.visualstudio.com 和 f5.services.visualstudio.com 的传出通信。
* 在 Microsoft Azure 中开始板，查看服务状态映射。 如果有一些警告的迹象，应该等到他们返回到确定然后关闭并重新打开您的应用程序理解应用程序刀片式服务器。
* 打开日志到 IDE 控制台窗口中，通过添加`<SDKLogger />`ApplicationInsights.xml 文件 （在资源文件夹在您的项目中），并检查项 [错误] 开头的根节点下的元素。
* 请确保，正确的 ApplicationInsights.xml 文件已成功加载 Java sdk，通过查看"配置文件已成功地找到"语句的控制台的输出消息。
* 如果找不到配置文件，检查输出消息可查看配置文件，搜索的位置，并确保 ApplicationInsights.xml 位于这些搜索位置中的一个。 根据经验，可以将配置文件放在应用程序的见解 SDK JARs 附近。 例如︰ 在 Tomcat，这意味着 WEB-INF/lib 文件夹。



#### 我用来查看数据，但它已停止

* 检查[状态的博客](http://blogs.msdn.com/b/applicationinsights-status/)。
* 您点击您每月的配额的数据点了吗？ 打开设置/配额和价格来看一看。 如果是这样，可以升级您的计划，或支付额外的容量。 请参阅[定价方案](http://azure.microsoft.com/pricing/details/application-insights/)。



## 没有使用率数据

*我看到请求和响应时间，但没有页面视图模式下，浏览器中，有关的数据或用户数据。*

您成功地设置您的应用程序从服务器发送遥测。 现在下一步是[设置 web 页发送遥测从 web 浏览器]的[用法]。

或者，如果您的客户端的[电话或其他设备][平台]的应用程序，您可以从那里发送遥测。 

使用相同的检测键设置您的客户端和服务器的遥测。 此数据将显示在相同的应用程序理解资源中，并能够将从客户端和服务器的事件相关联。



## 禁用遥测

*如何禁用遥测集合？*

在代码中︰

    TelemetryConfiguration config = TelemetryConfiguration.getActive();
    config.setTrackingIsDisabled(true);


**或** 

更新 ApplicationInsights.xml （在项目中的资源文件夹中）。 添加根节点下的下列︰

    <DisableTelemetry>true</DisableTelemetry>

使用 XML 的方法，您必须重新启动应用程序时更改的值。

## 更改目标

*如何更改我的项目将数据发送给哪个 Azure 资源？*

* [获取新资源的检测项。][java]
* 如果您的 web 项目添加到使用 Eclipse，Azure Toolkit 项目的应用程序的见解右击，选择**Azure**，**配置应用程序的见解**，并更改密钥。
* 否则，更新项目中的资源文件夹中的 ApplicationInsights.xml 中的键。


## Azure 的启动屏幕

*我查看[Azure 的门户](http://portal.azure.com)。 没有地图告诉我一些关于我的应用程序吗？*

不，它显示世界各地 Azure 服务器的运行状况。

*从 Azure 开始板 （主屏幕） 中，如何找到数据有关我的应用程序？*

假设您[设置您的应用程序的应用程序理解][java]，单击浏览，选择应用程序的见解，并选择您为您的应用程序创建的应用程序资源。 若要获取那里更快地在将来，可以收回开始板到您的应用程序。

## 内部网服务器

*我可以在我的 intranet 上监视服务器？*

是的请提供您的服务器可以发送到应用程序的见解通过公共互联网门户的遥测。 

在您的防火墙，可能需要打开 TCP 端口 80 和 443 用于 dc.services.visualstudio.com 和 f5.services.visualstudio.com 的传出通信。

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
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[平台]: app-insights-platforms.md
[跟踪]: app-insights-api-custom-events-metrics.md
[使用]: app-insights-web-track-usage.md

 
测试

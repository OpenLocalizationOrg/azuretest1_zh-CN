---
ms.openlocfilehash: 7f6939a71afb7cab9221ef475b6e7de77f83e033
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="浏览应用程序的见解中.NET 跟踪日志" 
    description="搜索与跟踪、 NLog 或 Log4Net 生成的日志。" 
    services="application-insights" 
    documentationCenter=".net"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/19/2015" 
    ms.author="awills"/>
 
# 浏览应用程序的见解中.NET 跟踪日志  

如果使用 NLog，log4Net 或 System.Diagnostics.Trace 诊断跟踪在 ASP.NET 应用程序，您可以您的日志发送到[Visual Studio 应用程序理解][开始]，您可以在这里浏览和搜索它们。 您的日志将会与来自您的应用程序，以便您能够识别跟踪记录相关联的服务处理的每个用户请求，并将它们与其他事件和异常报表相关联的其他遥测数据合并。

> [AZURE.NOTE] 您需要日志捕获模块吗？ 它是有用的第三方记录器适配器，但如果您还未使用 NLog、 log4Net 或 System.Diagnostics.Trace，请考虑只需直接调用[应用程序的见解 TrackTrace()](app-insights-api-custom-events-metrics.md#track-trace) 。

如果您还没有[设置为您的项目的应用程序理解][开始]，现在还。 您的项目应具有该文件`ApplicationInsights.config`和 NuGet 程序包`Microsoft.ApplicationInsights.Web`。


##  安装日志记录框架的适配器

如果您使用日志记录框架-log4Net，NLog 或 System.Diagnostics.Trace-您可以安装适配器将这些日志发送到应用程序的见解，以及其他遥测。 

1. 如果您计划使用 log4Net 或 NLog，则将其安装在您的项目。 
2. 在解决方案资源管理器中右击项目，选择**管理 NuGet 程序包**。
3. 搜索"应用程序信息"

    ![获取相应的适配器的预发布版本](./media/app-insights-asp-net-trace-logs/appinsights-36nuget.png)

4. 选择适当的包内的一种︰
  + Microsoft.ApplicationInsights.TraceListener （以捕获 System.Diagnostics.Trace 调用）
  + Microsoft.ApplicationInsights.NLogTarget
  + Microsoft.ApplicationInsights.Log4NetAppender

NuGet 程序包安装必需的程序集，并还修改 web.config 或 app.config。

#### 插入诊断日志调用

如果您使用 System.Diagnostics.Trace，将是一个典型的调用︰

    System.Diagnostics.Trace.TraceWarning("Slow response - database01");

如果您希望使用 log4net 或 NLog:

    logger.Warn("Slow response - database01");


## 直接使用 API 的跟踪

您可以直接调用应用程序深入跟踪 API。 日志记录适配器使用此 API。 

例如︰

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow response - database01");

TrackTrace 的优点是您可以将相对较长的数据放在消息中。 例如，无法编码那里发送数据。 


## 浏览您的日志

在您的应用程序在[应用程序的见解门户][门户]概述刀片式服务器，选择[搜索][诊断]。

![在应用程序的见解，选择搜索](./media/app-insights-asp-net-trace-logs/020-diagnostic-search.png)

![诊断搜索](./media/app-insights-asp-net-trace-logs/10-diagnostics.png)

您可以，例如︰

* 筛选日志跟踪信息，或具有特定属性的物料
* 检查详细信息中的特定项。
* 查找其他遥测与相同的用户请求 (即具有相同 OperationId) 
* 将此页的配置保存为收藏


## 下一步行动

[诊断故障和 ASP.NET 中的异常][例外情况]

[了解更多关于诊断搜索][诊断]。



## 疑难解答

### <a name="emptykey"></a>获取错误"检测键不能为空"

看起来您安装日志记录适配器 Nuget 程序包不安装应用程序的见解。

在解决方案资源管理器中，右击`ApplicationInsights.config`，然后选择**更新应用程序的见解**。 您将获得一个对话，邀请您登录到 Azure 并且必须要么创建应用程序建议的资源，或重新使用现有。 它能解决它。

### 我可以看到在诊断搜索，但没有其他事件的跟踪

有时可能需要一段时间的所有事件和请求以获取管道通过。

### <a name="limits"></a>保留的数据量？

每秒钟从每个应用程序的事件达 500 个。 事件保留七天。

## <a name="add"></a>下一步行动

* [设置的可用性和响应能力测试][可用性]
* [故障排除][通过]





<!--Link references-->

[可用性]: app-insights-monitor-web-app-availability.md
[诊断]: app-insights-diagnostic-search.md
[异常]: app-insights-web-failures-exceptions.md
[门户网站]: http://portal.azure.com/
[通过]: app-insights-troubleshoot-faq.md
[start]: app-insights-get-started.md

 
测试

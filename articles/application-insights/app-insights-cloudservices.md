---
ms.openlocfilehash: d615412e27fc73c8bfdde06ffde42d7569b65895
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="Azure 的云服务的应用程序理解"
   description="监视应用程序的见解与有效您 web 和工作人员的角色"
   services="application-insights"
   documentationCenter=""
   authors="soubhagyadash"
   manager="victormu"
   editor="alancameronwills"/>

<tags
   ms.service="application-insights"
   ms.devlang="na"
   ms.tgt_pltfrm="ibiza"
   ms.topic="article"
   ms.workload="tbd"
   ms.date="06/17/2015"
   ms.author="sdash"/>

# Azure 的云服务的应用程序理解


*在预览是应用程序的见解*

可以通过[Visual Studio 应用程序理解]的可用性、 性能、 故障和使用率[开始]监视[Microsoft Azure 云服务应用程序](http://azure.microsoft.com/services/cloud-services/)。 与处于放任状态获得关于性能和您的应用程序的有效性的反馈意见，您可以在每个开发周期中做出明智的选择，关于设计的方向。

![示例](./media/app-insights-cloudservices/sample.png)

您将需要与[Microsoft Azure](http://azure.com)的订阅。 使用 Microsoft 帐户，可能具有 Windows、 XBox Live，或其他 Microsoft 云服务的登录。 


#### 检测应用程序的见解与示例应用程序

请看一下此[示例应用程序](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService)的应用程序的见解添加到云服务与承载 Azure 中的两个工作者角色。 

以下内容可以告诉您如何以相同的方式调整自己的云服务项目。

## 创建的每个角色的见解应用程序资源

应用程序的见解资源是将分析并显示您的遥测数据。  

1.  在[Azure 门户网站][门户]中，创建新的应用程序理解资源。 对于应用程序类型，选择 ASP.NET 应用程序。 

    ![单击新的应用程序的见解](./media/app-insights-cloudservices/01-new.png)

2.  采用检测密钥的副本。 您将需要此不久要配置 SDK。

    ![单击属性，选择的注册表项，然后按 ctrl + C](./media/app-insights-cloudservices/02-props.png)


通常，最好从每个网站和辅助角色创建单独的资源数据。 

作为一种替代方法，可以将数据从所有角色发送到一个资源，但设置[默认属性][apidefaults] ，以便您可以筛选或分组中每个角色的结果。

## <a name="sdk"></a>每个项目中安装 SDK


1. 在 Visual Studio 中，编辑 NuGet 程序包的云应用程序项目。

    ![用鼠标右键单击该项目，然后选择管理 Nuget 程序包](./media/app-insights-cloudservices/03-nuget.png)

2. 添加 [应用程序网站的见解] (http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet 程序包。 本 SDK 版本添加服务器上下文信息，如角色信息的模块。

    ![搜索"应用程序信息"](./media/app-insights-cloudservices/04-ai-nuget.png)


3. 配置 SDK 将数据发送到应用程序的见解资源。

    打开`ApplicationInsights.config`并插入以下行︰

    `<InstrumentationKey>` *复制检测键* `</InstrumentationKey>`

    使用来自应用程序的见解资源的检测项。

4. 设置要始终复制到输出目录中的 ApplicationInsights.config 文件。 这只是所需的辅助角色。


或者，您可以在代码中设置检测键 (iKey)。 这很有用，例如，如果您想要使用 Azure 服务配置 (CSCFG) 设置来管理各自的环境的检测项。
[示例应用程序](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService)演示如何将 iKey 设置︰

* [Web 角色](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/Global.asax.cs#L27)
* [工作角色](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/WorkerRoleA.cs#L232)
* [为 web 页](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/Views/Shared/_Layout.cshtml#L13)

## 使用 SDK 进行报告遥测
### 报告请求
 * 在 web 角色请求模块自动收集有关 HTTP 请求的数据。 请参见有关如何重写默认集合行为的示例的[示例 MVCWebRole](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/MvcWebRole) 。 
 * 您可以通过 HTTP 请求相同的方式跟踪它们捕获对辅助角色的调用的性能。 在应用程序的见解，请求遥测类型测量进行计时，并且可以独立成功或失败的命名的服务器端工作的单位。 虽然 HTTP 请求会自动捕获通过 SDK，您可以插入您自己的代码来跟踪的辅助角色的请求。
 * 请参阅检测报告请求的两个示例工作者角色︰ [WorkerRoleA](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/WorkerRoleA)和[WorkerRoleB](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/WorkerRoleB)

### 报告依赖项
  * 应用程序的见解 SDK 可以报告您的应用程序可以对外部的依赖关系，如 REST api 和 SQL 服务器的调用。 这允许您查看是否慢响应或出现故障而导致特定的依赖项。
  * 跟踪依赖项，您必须设置 web/辅助角色具有[应用程序理解代理程序](app-insights-monitor-performance-live-website-now.md)也称为"状态监视器"。
  * 若要使用 web/工作人员角色的见解，应用程序代理︰
    * 将[AppInsightsAgent](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/WorkerRoleA/AppInsightsAgent)文件夹和这两个文件添加到 web/工作人员角色项目。 请务必设置它们的生成属性，以便它们总是复制到输出目录。 这些文件将安装代理。
    * 作为显示在[下面](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/AzureEmailService/ServiceDefinition.csdef#L18)的 CSDEF 文件添加任务启动。
    * 注意︰*辅助角色*都需要三个环境变量作为显示[在此处](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/AzureEmailService/ServiceDefinition.csdef#L44)。 这不是必需的 web 角色。

下面是您在应用程序的见解门户网站看到的内容的示例︰

* 请求自动关联和依赖关系的丰富诊断︰

    ![](./media/app-insights-cloudservices/SMxacy4.png)

* Web 角色，使用依赖项信息的性能︰

    ![](./media/app-insights-cloudservices/6yOBtKu.png)

* 这是屏幕截图上的请求和辅助角色的相关性信息︰

    ![](./media/app-insights-cloudservices/a5R0PBk.png)

### 报告异常

* 如何从其他 web 应用程序类型收集未经处理的异常的信息，请参阅[监视应用程序的见解中的例外情况](app-insights-asp-net-exceptions.md)。
* 示例 web 角色有 MVC5 和 Web API 2 控制器。 用以下捕获未经处理的异常，从 2:
    * [AiHandleErrorAttribute](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/Telemetry/AiHandleErrorAttribute.cs)设置了[这里](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/App_Start/FilterConfig.cs#L12)的 MVC5 控制器
    * [AiWebApiExceptionLogger](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/Telemetry/AiWebApiExceptionLogger.cs)设置了[以下](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/App_Start/WebApiConfig.cs#L25)Web API 2 控制器
* 为辅助的角色︰ 有两种方法能够跟踪例外日期。
    * TrackException(ex)
    * 如果您添加了应用程序深入跟踪侦听器 NuGet 程序包，可以使用 System.Diagnostics.Trace 来记录异常。 [代码示例。](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/WorkerRoleA.cs#L107)

### 性能计数器

默认情况下收集以下计数器︰

    * \Process(??APP_WIN32_PROC??)\% Processor Time
    * \Memory\Available Bytes
    * \.NET CLR Exceptions(??APP_CLR_PROC??)\# of Exceps Thrown / sec
    * \Process(??APP_WIN32_PROC??)\Private Bytes
    * \Process(??APP_WIN32_PROC??)\IO Data Bytes/sec
    * \Processor(_Total)\% Processor Time

此外，以下还为收集 web 角色︰

    * \ASP.NET Applications(??APP_W3SVC_PROC??)\Requests/Sec    
    * \ASP.NET Applications(??APP_W3SVC_PROC??)\Request Execution Time
    * \ASP.NET Applications(??APP_W3SVC_PROC??)\Requests In Application Queue

可以为显示[在此处](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/ApplicationInsights.config#L14)指定附加的自定义资源或其他 windows 性能计数器

  ![](./media/app-insights-cloudservices/OLfMo2f.png)

### 辅助角色相关遥测数据

它是什么导致了失败或高延迟请求，您可以看到丰富的诊断经验。 使用 web 角色，SDK 自动设置相关遥测之间的关联性。 为辅助的角色，可以使用自定义的遥测初始值设定项设置为实现此目标的所有遥测常见 Operation.Id 上下文属性。 这将允许您查看是否延迟/故障问题引起由于依赖项或代码，一眼 ！ 

下面是如何︰

* 将到调用上下文的关联 Id 设置为显示[在此处](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/WorkerRoleA.cs#L36)。 在这种情况下，我们正在使用请求 ID 作为相关性 id
* 添加自定义的 TelemetryInitializer 实现，程序会将 Operation.Id 设置为上面设置都会。 此处显示︰ [ItemCorrelationTelemetryInitializer](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/Telemetry/ItemCorrelationTelemetryInitializer.cs#L13)
* 添加自定义的遥测初始值设定项。 您可以这样做或代码中的 ApplicationInsights.config 文件，作为显示在[下面](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/WorkerRoleA.cs#L233)

就这么简单 ！ 门户体验已经连接上以帮助您查看所有相关的遥测一眼︰

![](./media/app-insights-cloudservices/bHxuUhd.png)

#### 没有数据？

* 打开[搜索][诊断]方块，查看单个事件。
* 使用该应用程序，以便它会生成一些遥测打开不同的页面。
* 等待几秒钟，然后单击刷新。
* 请参阅[疑难解答][qna]。


## 完成安装

若要获取完整的 360 度视图的应用程序，有一些更多应该做的事情︰


* [添加 JavaScript SDK web 页][客户端]以获取基于浏览器的页面视图数、 页面加载时间、 脚本异常的遥测并使您可以在页脚本中编写自定义的遥测。
* 添加依赖项跟踪诊断由数据库或由您的应用程序使用的其他组件的问题︰
 * [在 Azure 的 Web 应用程序或虚拟机][azure]
 * [内部部署 IIS 服务器中][redfield]
* [捕获日志跟踪][netlogs]从您最喜爱的日志记录框架
* [自定义跟踪的事件和标准]在客户端或服务器，以了解有关如何使用您的应用程序的[api] 。
* [设置 web 测试][可用性]，来确保您的应用程序保持实时和快速响应。



## 示例

[此示例](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService)监视具有 web 角色和两个辅助角色的服务。



[api]: app-insights-api-custom-events-metrics.md
[apidefaults]: app-insights-api-custom-events-metrics.md#default-properties
[apidynamicikey]: app-insights-api-custom-events-metrics.md#dynamic-ikey
[可用性]: app-insights-monitor-web-app-availability.md
[azure]: app-insights-azure.md
[客户端]: app-insights-javascript.md
[诊断]: app-insights-diagnostic-search.md
[netlogs]: app-insights-asp-net-trace-logs.md
[性能]: app-insights-web-monitor-performance.md
[门户网站]: http://portal.azure.com/
[通过]: app-insights-troubleshoot-faq.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-get-started.md 
测试t

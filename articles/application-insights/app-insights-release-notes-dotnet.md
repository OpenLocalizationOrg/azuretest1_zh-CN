---
ms.openlocfilehash: d17e5bf421f70ae0f4e9a22011503ab54a877266
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="发行说明，了解有关.NET 应用程序见解" 
    description="最新的更新。" 
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
    ms.date="08/06/2015" 
    ms.author="sergkanz"/>
 
# 应用程序的见解为.NET 的 SDK 的发行说明

[.Net 应用程序的见解 SDK](app-insights-start-monitoring-app-health-usage.md)将遥测有关实时应用程序发送到[应用程序的见解](http://azure.microsoft.com/services/application-insights/)，可以分析其使用情况和性能。


#### 若要安装 SDK 应用程序中

请参阅[开始使用.NET 的应用程序理解](app-insights-start-monitoring-app-health-usage.md)。

#### 若要升级到最新 SDK 

* 升级后，您将需要合并回对 ApplicationInsights.config 所做的任何自定义。 如果您不确定是否您自定义它，创建一个新项目，向其中添加应用程序的见解，比较新的项目中的一个带有.config 文件。 记下的任何差异。
* 在解决方案资源管理器中右击项目，选择**管理 NuGet 程序包**。
* 设置筛选器以显示已安装的软件包。 
* 选择**Microsoft.ApplicationInsights.Web** ，然后选择**升级**。 （这将升级所有的依赖包）。
* 将 ApplicationInsights.config 与旧的副本进行比较。 您将看到的更改的大多数都是因为我们移除某些模块和做其他人 parameterizable。 复旧文件所做的任何自定义。
* 重新生成解决方案。

## 版本 1.2

- 在 ASP.NET 库没有依赖项的遥测初始值设定项被移动从`Microsoft.ApplicationInsights.Web`到新依赖项 nuget `Microsoft.ApplicationInsights.WindowsServer`
- `Microsoft.ApplicationInsights.Web.dll` 在已重命名 `Microsoft.AI.Web.dll`
- `Microsoft.ApplicationInsights.Web.TelemetryChannel` nuget 已重命名的`Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel`。 `Microsoft.ApplicationInsights.Extensibility.Web.TelemetryChannel` 程序集已被重命名的`Microsoft.AI.ServerTelemetryChannel.dll`。 `Microsoft.ApplicationInsights.Extensibility.Web.TelemetryChannel` 在重命名类`Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel`。
- 是 Web SDK 的一部分的所有命名空间都已更改为排除`Extensibility`部分。 所有遥测初始值设定项都包含在 ApplicationInsights.config 和`ApplicationInsightsWebTracking`在 web.config 中的模块。
- 使用运行时检测代理 （通过扩展状态监视器或 Azure 网站启用） 收集的依赖关系将不被标记为如果有异步线程上的没有 HttpContext.Current。
- 属性`SamplingRatio`的`DependencyTrackingTelemetryModule`不执行任何操作，并标记为已过时。
- `Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector` 程序集已被重命名的 `Microsoft.AI.PerfCounterCollector`
- Web 和设备的 Sdk 中的几个小错误修复


## 1.1 版

- 新型遥测`DependencyTelemetry`添加了可用于从应用程序发送有关依赖项的调用的信息 （如 SQL，HTTP 调用 etc）。
- 新的重载方法`TelemetryClient.TrackDependency`，允许您发送有关依赖项的调用已添加。
- 在使用 TelemetryConfiguration.CreateDefault 时引发诊断模块固定的 NullReferenceException。

## 1.0 版

- 遥测初始值设定项和遥测模块从单独的子命名空间移动到根`Microsoft.ApplicationInsights.Extensibility.Web`命名空间。
- 从遥测初始值设定项和遥测模块的名称中删除"Web"前缀，因为它已包含在`Microsoft.ApplicationInsights.Extensibility.Web`命名空间名称。
- 移动`DeviceContextInitializer`从`Microsoft.ApplicationInsights`程序集到`Microsoft.ApplicationInsights.Extensibility.Web`程序集并将其转换为`ITelemetryInitializer`。
- 更改命名空间和程序集名称从`Microsoft.ApplicationInsights.Extensibility.RuntimeTelemetry`到`Microsoft.ApplicationInsights.Extensibility.DependencyCollector`为了与 NuGet 程序包的名称保持一致。
- 重命名`RemoteDependencyModule`到`DependencyTrackingTelemetryModule`。
- 重命名`CustomPerformanceCounterCollectionRequest`到`PerformanceCounterCollectionRequest`。

## 版本为 0.17
- 移除依赖项到 EventSource NuGet 4.5 框架应用程序。
- 匿名用户和会话 cookie 不会在服务器端生成。 若要实现用户和 web 应用程序的跟踪会话，JS sdk 工具现在是必需 – 从 JavaScript SDK 的 cookie 仍会考虑。 遥测模块```WebSessionTrackingTelemetryModule```和```WebUserTrackingTelemetryModule```不再受支持，并从 ApplicationInsights.config 文件中移除。 请注意，此更改可能导致用户巨大重述会话数为现在计仅用户发起会话。
- OSVersion 将不再默认情况下通过 SDK populuated。 当空时，OS 和 OSVersion 计算应用程序见解管道，根据用户代理。 
- 对于高负载方案优化的持久性通道用于 web SDK。 固定"的死亡螺旋"问题。 螺旋状的死亡是一个条件，大大超过了限制在终结点上的遥测项计数中的高峰会之后重试时某些时间，并将再次重试期间中止。
- 开发人员模式非常适合生产。 如果错误地保留它不会引起大开销为之前试图输出的附加信息。
- 默认情况下开发人员模式运行应用程序时在调试器下才会启用。 您可以覆盖它使用```DeveloperMode```属性```ITelemetryChannel```接口。

## 版 0.16 

2015-04-28 预览

- SDK 现在支持 DNX 来启用监视[核心.NET 框架](http://www.dotnetfoundation.org/NETCore5)应用程序的目标平台。
- 实例的```TelemetryClient```不缓存不再检测键。 现在，如果检测项中未设置```TelemetryClient```明确```InstrumentationKey```将返回 null。 它解决了的问题，当您设置```TelemetryConfiguration.Active.InstrumentationKey```后已收集了一些遥测、 遥测模块如依赖项收集器、 web 请求数据集和性能计数器收集器将使用新的检测项。

## 版 0.15

- 新的属性```Operation.SyntheticSource```现已在```TelemetryContext```。 现在可以将遥测项目标记为"不真实的用户通信流"，并指定如何生成该通信。 示例将此属性设置为可以从您的测试自动化与负载测试流量区分通信量。
- 通道逻辑移到单独的 NuGet 称为 Microsoft.ApplicationInsights.PersistenceChannel。 默认通道现在称为 InMemoryChannel
- 新方法```TelemetryClient.Flush```允许同步刷新缓冲区中的遥测项目

## 版本为 0.13

没有发行说明，了解可用的旧版本。

 

测试

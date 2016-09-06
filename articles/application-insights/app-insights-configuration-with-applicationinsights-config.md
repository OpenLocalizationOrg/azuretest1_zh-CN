---
ms.openlocfilehash: 9bf8f6b770f5920d3607e3e54f9d47a9be52e594
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="使用 ApplicationInsights.config 或.xml 配置应用程序深入 SDK" 
    description="启用或禁用数据收集模块，并添加性能计数器和其他参数" 
    services="application-insights"
    documentationCenter="" 
    authors="OlegAnaniev-MSFT"
    editor="alancameronwills" 
    manager="meravd"/>
 
<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/29/2015" 
    ms.author="awills"/>

# 使用 ApplicationInsights.config 或.xml 配置应用程序深入 SDK

应用程序的见解.NET SDK 包括大量的 NuGet 程序包。 [核心包](http://www.nuget.org/packages/Microsoft.ApplicationInsights)发送到应用程序理解的遥测提供 API。 [其他程序包](http://www.nuget.org/packages?q=Microsoft.ApplicationInsights)提供自动从您的应用程序和它的上下文跟踪遥测的遥测_模块_和_初始值设定项_。 通过调整配置文件，您可以启用或禁用遥测模块和初始值设定项，并为其中的某些设置参数。

配置文件命名为`ApplicationInsights.config`或`ApplicationInsights.xml`，具体取决于您的应用程序类型。 它会自动添加到您的项目时[安装 SDK 的某些版本][开始]。 它也添加到 web 应用程序的[IIS 服务器上的状态监视器][redfield]，或选择 Appplication 见解[Azure 网站或虚拟机的扩展][azure]。

没有等效的文件来控制[客户端] [SDK web 页中的]。

在配置文件中的每一个模块没有节点。 若要禁用某个模块，请删除节点或对它进行注释。

## [Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) NuGet 程序包

`Microsoft.ApplicationInsights` NuGet 程序包提供以下遥测模块中的`ApplicationInsights.config`。

```
<ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings">
  <TelemetryModules>
    <Add Type="Microsoft.ApplicationInsights.Extensibility.Implementation.Tracing.DiagnosticsTelemetryModule, Microsoft.ApplicationInsights" />
  </TelemetryModules>
</ApplicationInsights>
```
`DiagnosticsTelemetryModule`在应用程序的见解 instrumenation 代码本身中报告错误。 例如，如果代码不能访问性能计数器或`ITelemetryInitializer`将引发异常。 跟踪此模块的跟踪遥测显示在[诊断搜索][诊断]中。

## [Microsoft.ApplicationInsights.DependencyCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector) NuGet 程序包

`Microsoft.ApplicationInsights.DependencyCollector` NuGet 程序包提供以下遥测模块中的`ApplicationInsights.config`。

```
<ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings">
  <TelemetryModules>
    <Add Type="Microsoft.ApplicationInsights.Extensibility.DependencyCollector.DependencyTrackingTelemetryModule, Microsoft.ApplicationInsights.Extensibility.DependencyCollector" />
  </TelemetryModules>
</ApplicationInsights>
```
`DependencyTrackingTelemetryModule`跟踪遥测有关调用您的应用程序，例如 HTTP 请求和 SQL 查询所做的外部依赖项。 若要允许 IIS 服务器中使用此模块，您需要[安装状态监视器][redfield]。 在 Azure 的 web 应用程序或虚拟机，[选择应用程序的见解扩展][azure]中使用它。

您还可以编写您自己依赖项跟踪代码使用[TrackDependency API](app-insights-api-custom-events-metrics.md#track-dependency)。

## [Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet 程序包

`Microsoft.ApplicationInsights.Web` NuGet 程序包提供的以下遥测初始值设定项和在模块`ApplicationInsights.config`。

```
<ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings">
  <TelemetryInitializers>
    <Add Type="Microsoft.ApplicationInsights.Extensibility.Web.SyntheticTelemetryInitializer, Microsoft.ApplicationInsights.Extensibility.Web" />
    <Add Type="Microsoft.ApplicationInsights.Extensibility.Web.ClientIpHeaderTelemetryInitializer, Microsoft.ApplicationInsights.Extensibility.Web" />
    <Add Type="Microsoft.ApplicationInsights.Extensibility.Web.UserAgentTelemetryInitializer, Microsoft.ApplicationInsights.Extensibility.Web" />
    <Add Type="Microsoft.ApplicationInsights.Extensibility.Web.OperationNameTelemetryInitializer, Microsoft.ApplicationInsights.Extensibility.Web" />
    <Add Type="Microsoft.ApplicationInsights.Extensibility.Web.OperationIdTelemetryInitializer, Microsoft.ApplicationInsights.Extensibility.Web" />
    <Add Type="Microsoft.ApplicationInsights.Extensibility.Web.UserTelemetryInitializer, Microsoft.ApplicationInsights.Extensibility.Web" />
    <Add Type="Microsoft.ApplicationInsights.Extensibility.Web.SessionTelemetryInitializer, Microsoft.ApplicationInsights.Extensibility.Web" />
    <Add Type="Microsoft.ApplicationInsights.Extensibility.Web.AzureRoleEnvironmentTelemetryInitializer, Microsoft.ApplicationInsights.Extensibility.Web" />
    <Add Type="Microsoft.ApplicationInsights.Extensibility.Web.DomainNameRoleInstanceTelemetryInitializer, Microsoft.ApplicationInsights.Extensibility.Web" />
    <Add Type="Microsoft.ApplicationInsights.Extensibility.Web.BuildInfoConfigComponentVersionTelemetryInitializer, Microsoft.ApplicationInsights.Extensibility.Web" />
    <Add Type="Microsoft.ApplicationInsights.Extensibility.Web.DeviceTelemetryInitializer, Microsoft.ApplicationInsights.Extensibility.Web"/>
  </TelemetryInitializers>
  <TelemetryModules>
    <Add Type="Microsoft.ApplicationInsights.Extensibility.Web.RequestTrackingTelemetryModule, Microsoft.ApplicationInsights.Extensibility.Web" />
    <Add Type="Microsoft.ApplicationInsights.Extensibility.Web.ExceptionTrackingTelemetryModule, Microsoft.ApplicationInsights.Extensibility.Web" />
    <Add Type="Microsoft.ApplicationInsights.Extensibility.Web.DeveloperModeWithDebuggerAttachedTelemetryModule, Microsoft.ApplicationInsights.Extensibility.Web" />
  </TelemetryModules>
</ApplicationInsights>
```

**初始值设定项**

* `SyntheticTelemetryInitializer` 更新`User`， `Session` ，`Operation`的上下文项的属性的所有遥测跟踪处理综合源，例如，可用性测试中的请求时。 默认情况下，门户应用程序的见解不显示综合遥测。
* `ClientIpHeaderTelemetryInitializer` 更新`Ip`属性`Location`遥测的所有项的上下文基于`X-Forwarded-For`请求的 HTTP 标头。
* `UserAgentTelemetryInitializer` 更新`UserAgent`属性`User`遥测的所有项的上下文基于`User-Agent`请求的 HTTP 标头。
* `OperationNameTelemetryInitializer` 更新`Name`属性`RequestTelemetry`和`Name`的属性`Operation`遥测的所有项的上下文基于 HTTP 方法以及 ASP.NET MVC 控制器和调用来处理该请求的操作的名称。
* `OperationNameTelemetryInitializer` 更新 Operation.Id` context property of all telemetry items tracked while 
handling a request with the automatically generated `RequestTelemetry.Id。
* `UserTelemetryInitializer` 更新`Id`和`AcquisitionDate`的属性`User`上下文从提取的值的所有遥测项`ai_user`cookie 在用户的浏览器中运行的应用程序理解 JavaScript 检测代码生成。
* `SessionTelemetryInitializer` 更新`Id`属性`Session`上下文从提取的值的所有遥测项`ai_session`生成的 ApplicationInsights JavaScript 检测代码运行在用户的浏览器中的 cookie。 
* `AzureRoleEnvironmentTelemetryInitializer` 更新`RoleName`和`RoleInstance`的属性`Device`遥测流派为从 Azure 的运行时环境中提取信息的上下文。
* `DomainNameRoleInstanceTelemetryInitializer` 更新`RoleInstance`属性`Device`域名的 web 应用程序已在运行的计算机的所有遥测项目上下文。
* `BuildInfoConfigComponentVersionTelemetryInitializer` 更新`Version`属性`Component`上下文从提取的值的所有遥测项`BuildInfo.config`文件通过 TFS 版本生成的。
* `DeviceTelemetryInitializer` 更新下面的属性`Device`遥测的所有项的上下文。
 - `Type` 设置为"PC"
 - `Id` 设置为计算机的域名运行 web 应用程序的位置。
 - `OemName` 设置为从提取的值`Win32_ComputerSystem.Manufacturer`字段使用 WMI。
 - `Model` 设置为从提取的值`Win32_ComputerSystem.Model`字段使用 WMI。
 - `NetworkType` 设置从提取的值为`NetworkInterface`。
 - `Language` 设置的名称为`CurrentCulture`。

**模块**

* `RequestTrackingTelemetryModule` 跟踪由您的 web 应用程序接收请求并测量响应时间。
* `ExceptionTrackingTelemetryModule` 跟踪您的 web 应用程序中未处理的异常。 请参见[故障和异常][的异常]。
* `DeveloperModeWithDebuggerAttachedTelemetryModule` 强制应用程序理解`TelemetryChannel`将立即数据时将调试器附加到应用程序的过程，一次一个遥测项发送。 当您的应用程序跟踪遥测和大量 CPU 和网络带宽开销增加应用程序的见解门户上出现时，这减少了时刻之间的时间的量。

## [Microsoft.ApplicationInsights.PerfCounterCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.PerfCounterCollector) NuGet 程序包

`Microsoft.ApplicationInsights.PerfCounterCollector` NuGet 程序包添加到以下的遥测模块`ApplicationInsights.config`，默认情况。

```
<ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings">
  <TelemetryModules>
    <Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule, Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector" />
  </TelemetryModules>
</ApplicationInsights>
```

### PerformanceCollectorModule

`PerformanceCollectorModule` 跟踪大量的 Windows 性能计数器。 当您单击规格，资源管理器打开其细节刀片式服务器中的某个图表时，您可以看到这些计数器。

您可以监视的其他性能计数器的标准 Windows 计数器和已添加的任何其他。
      
使用下面的语法来收集更多性能计数器︰

```
<Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule, Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector">
  <Counters>
    <Add PerformanceCounter="\MyCategory\MyCounter" />
    <Add PerformanceCounter="\Process(??APP_WIN32_PROC??)\Handle Count" ReportAs="Process handle count" />
    <!-- ... -->
  </Counters>
</Add>
```      
      
`PerformanceCounter` 必须是`\CategoryName(InstanceName)\CounterName`或 `\CategoryName\CounterName`
      
如果您提供`ReportAs`特性，这将被用作应用程序的见解中显示的名称。

对于报告给应用程序的见解，必须只包含计数器名称︰ 倒圆角括号、 正斜杠、 连字符、 下划线的字母、 空格和点。

如果您想要监视的计数器包含任何无效字符，如 # 或数字，则必须使用 ReportAs。
      
以下占位符作为`InstanceName`:

    ?APP_WIN32_PROC?? - instance name of the application process  for Win32 counters.
    ??APP_W3SVC_PROC?? - instance name of the application IIS worker process for IIS/ASP.NET counters.
    ??APP_CLR_PROC?? - instance name of the application CLR process for .NET counters.

## 通道参数 (Java)

这些参数影响 Java SDK 应在如何存储和刷新它收集的遥测数据。

#### MaxTelemetryBufferCapacity

可以存储在内存中存储的 SDK 的遥测项的数目。 达到此数目后，刷新遥测-即遥测项发送给服务器应用程序的见解。

-   最小值︰ 1
-   最大值︰ 1000年
-   默认值︰ 500

```

  <ApplicationInsights>
      ...
      <Channel>
       <MaxTelemetryBufferCapacity>100</MaxTelemetryBufferCapacity>
      </Channel>
      ...
  </ApplicationInsights>
```

#### FlushIntervalInSeconds 

确定如何通常存储在内存中存储的数据应刷新 （发送到应用程序的见解）。

-   最小值︰ 1
-   最大值︰ 300
-   默认值︰ 5

```

    <ApplicationInsights>
      ...
      <Channel>
        <FlushIntervalInSeconds>100</FlushIntervalInSeconds>
      </Channel>
      ...
    </ApplicationInsights>
```

#### MaxTransmissionStorageCapacityInMB

决定的最大大小，以 mb 为单位分配到本地磁盘上的永久存储区。 此存储用于保持遥测项传输到应用程序的见解终结点失败。 当达到存储大小时，新的遥测项目将被放弃。

-   最小值︰ 1
-   最大值︰ 100
-   默认值︰ 10

```

   <ApplicationInsights>
      ...
      <Channel>
        <MaxTransmissionStorageCapacityInMB>50</MaxTransmissionStorageCapacityInMB>
      </Channel>
      ...
   </ApplicationInsights>
```

## 自定义初始值设定项


如果标准的初始值设定项不能适用于您的应用程序，您可以创建您自己。

使用上下文初始值设定项来设置将用于初始化每个遥测客户端的值。 例如，如果您已发布的应用程序的多个版本，无法确保可以单独通过筛选的自定义属性的数据︰

    plublic class MyContextInitializer: IContextInitializer
    {
        public void Initialize(TelemetryContext context)
        {
          context.Properties["AppVersion"] = "v2.1";
        }
    }

使用遥测初始值设定项添加到每个事件的处理。 例如，SDK 网站标记为失败任何请求与响应代码 > = 400。 您可以重写该行为︰  

    public class MyTelemetryInitializer : ITelemetryInitializer
    {
        public void Initialize(ITelemetry telemetry)
        {
            var requestTelemetry = telemetry as RequestTelemetry;
            if (requestTelemetry == null) return;
            int code;
            bool parsed = Int32.TryParse(requestTelemetry.ResponseCode, out code);
            if (!parsed) return;
            if (code >= 400 && code < 500)
            {
                requestTelemetry.Success = true;
                requestTelemetry.Context.Properties["Overridden400s"] = "true";
            }            
        }
    }
 
若要安装您的初始值设定项，请在 ApplicationInsights.config 中添加行︰

    <TelemetryInitializers> <!-- or ContextInitializers -->
    <Add Type="MyNamespace.MyTelemetryInitializer, MyAssemblyName" />


或者您可以编写代码以执行应用程序安装在早期时候的初始值设定项。 例如︰ 


    // In the app initializer such as Global.asax.cs:

    protected void Application_Start()
    {
      TelemetryConfiguration.Active.TelemetryInitializers.Add(
                new MyTelemetryInitializer());
            ...




## InstrumentationKey

这将确定您的数据将显示应用程序的见解资源。 通常您创建单独的资源，与使用单独的密钥，为每个应用程序。

如果您想要设置密钥动态-例如如果要将结果发送给不同的资源的应用程序中可以省略参数从配置文件中，并在代码中设置它。

将键设置为 TelemetryClient 的所有实例，包括标准的遥测模块，在 TelemetryConfiguration.Active 中设置项。 在初始化方法中，如 global.aspx.cs ASP.NET 服务中执行此操作︰

```C#

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey = 
          // - for example -
          WebConfigurationManager.Settings["ikey"];
      //...
```

如果您只想将一组特定的事件发送给不同的资源，可以为特定的 TelemetryClient 设置密钥︰

```C#

    var tc = new TelemetryClient();
    tc.Context.InstrumentationKey = "----- my key ----";
    tc.TrackEvent("myEvent");
    // ...

```

[了解更多关于 API][api]。

获取新密钥，[创建新的资源在应用程序的见解门户][新]。

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[azure]: ../insights-perf-analytics.md
[客户端]: app-insights-javascript.md
[诊断]: app-insights-diagnostic-search.md
[异常]: app-insights-web-failures-exceptions.md
[netlogs]: app-insights-asp-net-trace-logs.md
[新建]: app-insights-create-new-resource.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-get-started.md


测试

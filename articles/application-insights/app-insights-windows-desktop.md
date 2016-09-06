---
ms.openlocfilehash: 9307713caebfae39904d286489e54a117ccb59df
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="应用程序的见解为 Windows 桌面应用程序和服务" 
    description="分析使用情况和性能的 Windows 桌面应用程序与应用程序的见解。" 
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
    ms.date="08/19/2015" 
    ms.author="awills"/>

# 应用程序在 Windows 桌面应用程序、 服务和辅助角色的见解

*在预览是应用程序的见解。*

[AZURE.INCLUDE [app-insights-selector-get-started](../../includes/app-insights-selector-get-started.md)]

应用程序的见解让您监视部署的应用程序的使用情况和性能。

包括桌面应用程序和后台服务工作者角色的-所有 Windows 应用程序可以都使用应用程序的见解核心 SDK 将遥测发送到应用程序的见解。 核心 SDK 只是提供一个 API︰ 与不同的网站或设备的 Sdk，它不包括自动收集数据的任何模块因此您必须编写代码来发送自己的遥测。


## <a name="add"></a> 创建应用程序的见解资源


1.  在[Azure 门户网站][门户]中，创建新的应用程序理解资源。 对于应用程序类型，请选择 Windows 应用商店应用程序。 

    ![单击新的应用程序的见解](./media/app-insights-windows-desktop/01-new.png)

    （您选择的应用程序类型设置概述刀片和[公制的资源管理器][测量数据]中的可用属性的内容）。

2.  采用检测密钥的副本。

    ![单击属性，选择的注册表项，然后按 ctrl + C](./media/app-insights-windows-desktop/02-props.png)

## <a name="sdk"></a>在应用程序安装 SDK


1. 在 Visual Studio 中，编辑的 NuGet 程序包的桌面应用程序项目。

    ![用鼠标右键单击该项目，然后选择管理 Nuget 程序包](./media/app-insights-windows-desktop/03-nuget.png)

2. 安装应用程序的见解核心 API 软件包。

    ![搜索"应用程序信息"](./media/app-insights-windows-desktop/04-core-nuget.png)

    可以安装其他程序包如性能计数器或日志捕获的包，如果您想要使用他们的设施。

3. 在代码中，例如在 main （） 中设置您的 InstrumentationKey。 

    `TelemetryConfiguration.Active.InstrumentationKey = "your key";`

*为什么没有 ApplicationInsights.config？*

* .Config 文件不被安装的核心 API 包，只用来配置遥测采集器。 因此您可以编写您自己的代码来设置检测项和发送遥测。
* 如果您安装的其他程序包，则必须.config 文件。 您可以插入检测键存在而不是在代码中设置它。

*可以使用不同的 NuGet 程序包？*

* 是的您可以使用 web 服务器软件包 (Microsoft.ApplicationInsights.Web)，这将安装收集器的各种如性能计数器收集模块。 它将安装检测键放的.config 文件中。 使用[ApplicationInsights.config 来禁用不需要的模块](app-insights-configuration-with-applicationinsights-config.md)，如 HTTP 请求收集器。 
* 如果您想要使用的[日志或跟踪收集器包](app-insights-asp-net-trace-logs.md)，开始与 web 服务器包。 

## <a name="telemetry"></a>插入遥测调用

创建`TelemetryClient`实例，然后[使用它来发送遥测][api]。


例如，在 Windows 窗体应用程序中，您可以编写︰

```C#

    public partial class Form1 : Form
    {
        private TelemetryClient tc = new TelemetryClient();
        ...
        private void Form1_Load(object sender, EventArgs e)
        {
            // Alternative to setting ikey in config file:
            tc.InstrumentationKey = "key copied from portal";

            // Set session data:
            tc.Context.User.Id = Environment.GetUserName();
            tc.Context.Session.Id = Guid.NewGuid().ToString();
            tc.Context.Device.OperatingSystem = Environment.OSVersion.ToString();

            // Log a page view:
            tc.TrackPageView("Form1");
            ...
        }

        protected override void OnClosing(CancelEventArgs e)
        {
            stop = true;
            if (tc != null)
            {
                tc.Flush(); // only for desktop apps
            }
            base.OnClosing(e);
        }

```

使用任何[api]的[应用程序理解 API]来发送遥测。 在 Windows 桌面应用程序，没有遥测自动发送。 通常会使用︰

* 在窗体、 页或选项卡切换 TrackPageView(pageName)
* 对于其他用户操作 TrackEvent(eventName)
* TrackMetric （名称、 值） 在后台任务发送定期报告未附加到特定事件的衡量标准。
* TrackTrace(logEvent) 的[诊断日志记录][诊断]
* 在 catch 子句中的 TrackException(exception)


若要确保在关闭应用程序之前发送所有遥测，使用`TelemetryClient.Flush()`。 通常情况下，遥测是批处理，在固定时间间隔发送。 （刷新建议只有当您使用的不仅仅是核心 API。 Web 和设备 Sdk 实现此行为自动。


#### 上下文的初始值设定项

若要查看的用户和会话的计数可以设置值对每个`TelemetryClient`实例。 或者，可以使用上下文初始值设定项的所有客户机执行此添加︰

```C#

    class UserSessionInitializer: IContextInitializer
    {
        public void Initialize(TelemetryContext context)
        {
            context.User.Id = Environment.UserName;
            context.Session.Id = Guid.NewGuid().ToString();
        }
    }

    static class Program
    {
        ...
        static void Main()
        {
            TelemetryConfiguration.Active.ContextInitializers.Add(
                new UserSessionInitializer());
            ...

```



## <a name="run"></a>运行您的项目

[运行您的应用程序，按 f5 键](http://msdn.microsoft.com/library/windows/apps/bg161304.aspx)并使用它，以生成一些遥测。 

在 Visual Studio 中，您将看到已发送的事件计数。

![](./media/app-insights-windows-desktop/appinsights-09eventcount.png)



## <a name="monitor"></a>查看监视器数据

返回到您应用程序刀片在 Azure 的门户。

第一个事件将出现在诊断搜索。 

如果您期待更多的数据，请单击刷新后几秒钟。

如果您使用 TrackMetric 或 TrackEvent 的测量参数，打开[度量资源管理器中][的指标]，然后打开筛选器刀片，可看到您的度量标准。



## <a name="usage"></a>下一步行动

[跟踪您的应用程序使用][knowUsers]

[捕获并搜索诊断日志][诊断]

[故障排除][通过]




<!--Link references-->

[诊断]: app-insights-diagnostic-search.md
[指标]: app-insights-metrics-explorer.md
[门户网站]: http://portal.azure.com/
[通过]: app-insights-troubleshoot-faq.md
[knowUsers]: app-insights-overview-usage.md
[api]: app-insights-api-custom-events-metrics.md
[CoreNuGet]: https://www.nuget.org/packages/Microsoft.ApplicationInsights
 
测试

---
ms.openlocfilehash: 81f4dfa1051d33eb4b670ce0727c321abefd1325
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="应用程序的 Windows Phone 的见解和存储应用程序 |Microsoft Azure"
    description="分析使用情况和性能 Windows 设备应用程序与应用程序的见解。"
    services="application-insights"
    documentationCenter="windows"
    authors="alancameronwills"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="06/16/2015"
    ms.author="awills"/>

# Windows Phone 和应用商店应用程序的应用程序理解

*在预览是应用程序的见解。*

[AZURE.INCLUDE [app-insights-selector-get-started](../../includes/app-insights-selector-get-started.md)]

Visual Studio 应用程序见解让您监视的已发布应用程序︰

* [**使用**][windowsUsage]/ 收益分析;了解您拥有的用户数量以及他们所做的与您的应用程序。
* [**崩溃**][windowsCrash]/ 收益分析;获得的故障诊断报告，了解它们对用户的影响。

![](./media/app-insights-windows-get-started/appinsights-d018-oview.png)

对于许多应用程序类型，几乎没有注意到你的[Visual Studio 可以添加到您的应用程序的应用程序理解](#ide)。 但是，由于您正在阅读本文以更好地了解这怎么回事，我们将向您通过一系列步骤手动。

您将需要︰

* 对[Microsoft Azure][azure]的订阅。
* 2013 或更高版本的 Visual Studio。

## 1.创建一个应用程序的见解资源

在[Azure 门户网站][门户]中，创建新的应用程序理解资源。

![选择新建，开发商服务，应用程序的见解](./media/app-insights-windows-get-started/01-new.png)

在 Azure 中[资源][的角色]是服务的实例。 此资源是将分析并向您显示从您的应用程序的遥测。

#### 复制检测密钥

键标识的资源。 您将需要使用它来配置 SDK 将数据发送到资源。

![打开精要拉抽屉，然后选择检测键](./media/app-insights-windows-get-started/02-props.png)


## 2.添加到您的应用程序应用程序深入 SDK

在 Visual Studio，向项目中添加相应的 SDK。

如果它是一个通用 Windows 应用程序，用于 Windows Phone 项目和 Windows 项目重复步骤。

1. 用鼠标右键单击解决方案资源管理器中的项目，然后选择**管理 NuGet 程序包**。

    ![](./media/app-insights-windows-get-started/03-nuget.png)

2. 搜索"应用程序的见解"。

    ![](./media/app-insights-windows-get-started/04-ai-nuget.png)

3. 选择**应用程序的 Windows 应用程序的见解**

4. 将 ApplicationInsights.config 文件添加到您的项目的根并插入来自门户的检测项。 该配置文件的一个示例 xml 如下所示。

    ```xml
        <?xml version="1.0" encoding="utf-8" ?>
        <ApplicationInsights>
            <InstrumentationKey>YOUR COPIED INSTRUMENTATION KEY</InstrumentationKey>
        </ApplicationInsights>
    ```

    设置的 ApplicationInsights.config 文件的属性︰**生成操作** == **内容**和**复制到输出目录** == **始终复制**。

    ![](./media/app-insights-windows-get-started/AIConfigFileSettings.png)

5. 添加下面的初始化代码。 最好是要添加到此代码`App()`构造函数。 如果您做它到其他地方，您可能错过第一 pageviews 自动集合。  

```C#
    public App()
    {
       // Add this initilization line.
       WindowsAppInitializer.InitializeAsync();

       this.InitializeComponent();
       this.Suspending += OnSuspending;
    }  
```

**Windows 通用应用程序**︰ 用于手机和存储项目重复步骤。 [Windows 8.1 通用应用程序的示例](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/Windows%208.1%20Universal)。

## <a name="network"></a>3.使您的应用程序的网络访问

如果您的应用程序已不存在[请求出站网络访问权限](https://msdn.microsoft.com/library/windows/apps/hh452752.aspx)，必须将它添加到其清单中，作为一项[必需的功能](https://msdn.microsoft.com/library/windows/apps/br211477.aspx)。

## <a name="run"></a>4.运行您的项目

[运行您的应用程序，按 f5 键](http://msdn.microsoft.com/library/windows/apps/bg161304.aspx)并使用它，以生成一些遥测。

在 Visual Studio 中，您将看到已接收到的事件数。

![](./media/app-insights-windows-get-started/appinsights-09eventcount.png)

在调试模式下，只要它生成发送遥测。 在发布模式下遥测设备上存储和发送应用程序恢复时只。


## <a name="monitor"></a>5.请参阅监视器数据

从项目中打开应用程序的见解。

![用鼠标右键单击项目，然后打开 Azure 门户](./media/app-insights-windows-get-started/appinsights-04-openPortal.png)


首先，您将看到一个或两个点。 例如︰

![单击，直至到达更多的数据](./media/app-insights-windows-get-started/appinsights-26-devices-01.png)

单击**刷新**几秒钟后如果您期待更多的数据。

单击任何图表以查看更多详细信息。


## <a name="deploy"></a>5.发布到存储应用程序

[发布您的应用程序](http://dev.windows.com/publish)并监视数据积累当用户下载并使用它。

## 自定义您的遥测

#### 选择收集器

应用程序深入 SDK 包含多个收集器，它会自动从您的应用程序收集不同类型的数据。 默认情况下，它们是所有活动。 但是，您可以选择要应用程序构造函数中初始化的收集器︰

    WindowsAppInitializer.InitializeAsync( "00000000-0000-0000-0000-000000000000",
       WindowsCollectors.Metadata
       | WindowsCollectors.PageView
       | WindowsCollectors.Session
       | WindowsCollectors.UnhandledException);

#### 遥测数据的发送

使用[API][api]来将事件、 指标和诊断数据发送给应用程序的见解。 摘要︰

```C#

 var tc = new TelemetryClient(); // Call once per thread

 // Send a user action or goal:
 tc.TrackEvent("Win Game");

 // Send a metric:
 tc.TrackMetric("Queue Length", q.Length);

 // Provide properties by which you can filter events:
 var properties = new Dictionary{"game", game.Name};

 // Provide metrics associated with an event:
 var measurements = new Dictionary{"score", game.score};

 tc.TrackEvent("Win Game", properties, measurements);

```

有关更多详细信息，请参阅[自定义事件和度量][api]。

## 下一步是什么？

* [检测和诊断在您的应用程序的崩溃][windowsCrash]
* [关于度量标准][指标]
* [关于诊断搜索][诊断]


## <a name="ide"></a>自动的安装

如果要让 Visual Studio 执行安装步骤，您可以执行的与 Windows Phone、 Windows 应用商店和许多其他类型的应用程序。

### <a name="new"></a>如果您正在创建新的 Windows 应用程序项目...

在**新建项目**对话框中选择**应用程序的见解**。

如果要求您登录，则使用的凭据 Azure 帐户 （它是独立于您的 Visual Studio 联机帐户）。

![](./media/app-insights-windows-get-started/appinsights-d21-new.png)


### <a name="existing"></a>或者如果它是一个现有的项目...

从解决方案资源管理器中添加应用程序的见解。


![](./media/app-insights-windows-get-started/appinsights-d22-add.png)

## 升级到新版本的 sdk

[新 SDK 版本发布](app-insights-release-notes-windows.md)时︰
* 右键单击项目，然后选择管理 NuGet 程序包。
* 选择安装的应用程序的见解程序包并选择**操作︰ 升级**。


## <a name="usage"></a>下一步行动


[检测和诊断在您的应用程序的崩溃][windowsCrash]

[捕获并搜索诊断日志][诊断]


[跟踪您的应用程序使用][windowsUsage]

[使用 API 来发送自定义的遥测][api]

[故障排除][通过]



<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[azure]: ../insights-perf-analytics.md
[诊断]: app-insights-diagnostic-search.md
[指标]: app-insights-metrics-explorer.md
[门户网站]: http://portal.azure.com/
[通过]: app-insights-troubleshoot-faq.md
[角色]: app-insights-resources-roles-access-control.md
[windowsCrash]: app-insights-windows-crashes.md
[windowsUsage]: app-insights-windows-usage.md

测试

---
ms.openlocfilehash: 8d005e3f16a68b1cfa63c68a25b09647686ad215
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="C + + 应用程序的应用程序理解" 
    description="分析使用情况和应用程序的见解与 c + + 应用程序的性能。" 
    services="application-insights" 
    documentationCenter="cpp"
    authors="crystk" 
    manager="jakubo""/>

<tags 
    ms.service="application-insights" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="universal" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/03/2015" 
    ms.author="crystk"/>

# C + + 应用程序的应用程序理解

Visual Studio 应用程序见解让您监视的使用情况，事件，移动应用程序和系统崩溃。

## 要求

您将需要︰

* 具有[Microsoft Azure](http://azure.com)的订阅。 您与 Microsoft 帐户，可能具有 Windows、 XBox Live，或其他 Microsoft 云服务的登录。
* 2015 或更高版本的 Visual Studio。
* Windows 10 通用应用程序

## 创建应用程序的见解资源

在[Azure 门户网站][门户]中，创建新的应用程序理解资源。 请选择 Windows Phone 或 Windows 应用商店选项。

![单击新建开发商服务，应用程序的见解](./media/app-insights-windows-cpp/01-universal.png)

打开刀片式服务器都将有关您的应用程序看到的性能和使用情况数据。 若要返回到它下次您登录到 Azure，您应该开始在屏幕上发现它平铺。 或者单击浏览进行查找。

####  采用检测密钥的副本。

键标识的资源，并将它很快在中安装 SDK 后，可将数据定向到该资源。

![单击属性，选择的注册表项，然后按 ctrl + C](./media/app-insights-windows-cpp/02-props-asp.png)

## <a name="sdk"></a> 在应用程序安装 SDK


1. 在 Visual Studio 中，编辑的 NuGet 程序包的桌面应用程序项目。

    ![用鼠标右键单击该项目，然后选择管理 Nuget 程序包](./media/app-insights-windows-cpp/03-nuget.png)

2. 为 c + + 应用程序安装应用程序的见解 SDK。

    ![选择 * * 包括预发布版 * * 并搜索"应用程序信息"](./media/app-insights-windows-cpp/04-nuget.png)

3. 在发布版本和调试您的项目设置︰ 
  - 添加 $(SolutionDir)packages\ApplicationInsights-CPP.1.0.0-Beta\src\inc 对项目属性-> VC + + 目录-> 包含目录
  - 添加 $(SolutionDir)packages\ApplicationInsights.1.0.0-Beta\lib\native\<平台类型 > \release\AppInsights_Win10-UAP 对项目属性-> VC + + 目录-> 库目录

4. 添加到项目的引用从 $ ApplicationInsights.winmd (SolutionDir)packages\ApplicationInsights.1.0.0-Beta\lib\native\<平台类型 > \release\ApplicationInsights
5. 添加从 $ AppInsights_Win10 UAP.dll (SolutionDir)packages\ApplicationInsights.1.0.0-Beta\lib\native\<平台类型 > \release\AppInsights_Win10-UAP。 转到属性，然后将内容设置为是。 这会将 dll 复制到生成目录。


#### 若要更新到未来版本的 SDK

新[SDK 发布](app-insights-release-notes-windows-cpp.md)时︰

* 在 NuGet 程序包管理器中，选择已安装的 SDK 并选择操作︰ 升级。
* 重复的安装步骤，使用新的版本号。

## 使用 SDK

初始化 SDK 并开始跟踪遥测。

1. 在 App.xaml.h: 
  - 添加︰`ApplicationInsights::CX::SessionTracking^ m_session;`
2. 在 App.xaml.cpp:
  - 添加︰`using namespace ApplicationInsights::CX;`

  - 在 App:App()
    
     `// this will do automatic session tracking and automatic page view collection`
     `m_session = ref new ApplicationInsights::CX::SessionTracking();`

  - 一旦创建根框架 （通常位于 App::OnLaunched 的结尾） 初始化 m_session:
    
    ```
    String^ iKey = L"<YOUR INSTRUMENTATION KEY>";
    m_session->Initialize(this, rootFrame, iKey);
    ```

3. 若要使用其他位置跟踪应用程序中，可以声明遥测客户端实例。


```

    using namespace ApplicationInsights::CX;
    TelemetryClient^ tc = ref new TelemetryClient(L"<YOUR INSTRUMENTATION KEY>");
    tc->TrackTrace(L"This is my first trace");
    tc->TrackEvent(L"I'M ON PAGE 1");
    tc->TrackMetric(L"Test Metric", 5.03);
```


## <a name="run"></a> 运行您的项目

运行应用程序以生成遥测。 可以在运行调试更在您的开发计算机上，或将其发布并让用户运行它。

## 查看您的数据在应用程序的见解

返回到 http://portal.azure.com 并浏览到您的应用程序理解资源。

单击搜索以打开[诊断搜索][诊断]-是第一个事件出现的位置。 如果您看不到任何东西，等待一分钟或两个，并单击刷新。

![单击搜索诊断](./media/app-insights-windows-cpp/21-search.png)

使用您的应用程序，则数据将出现在概述刀片式服务器。

![刀片式服务器概述](./media/app-insights-windows-cpp/22-oview.png)

单击任意图，以获取更多详细信息。 例如，崩溃︰

![单击故障图表](./media/app-insights-windows-cpp/23-crashes.png)


## <a name="usage"></a>下一步行动

[跟踪您的应用程序使用][跟踪]

[使用 API 来发送自定义事件和指标][api]

[诊断搜索][诊断]

[公制的资源管理器][指标]

[故障排除][通过]



<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[诊断]: app-insights-diagnostic-search.md
[指标]: app-insights-metrics-explorer.md
[门户网站]: http://portal.azure.com/
[通过]: app-insights-troubleshoot-faq.md
[跟踪]: app-insights-api-custom-events-metrics.md

 

测试

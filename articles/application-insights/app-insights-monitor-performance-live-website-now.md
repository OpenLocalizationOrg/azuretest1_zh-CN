---
ms.openlocfilehash: 0b2a1ed14acbcd9026955f48424620d5f739a108
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="诊断性能问题上正在运行的网站 |Microsoft Azure"
    description="监视网站的性能而不重新部署它。 使用独立或与应用程序的见解 SDK 来获取依赖项遥测。"
    services="application-insights"
    documentationCenter=".net"
    authors="alancameronwills"
    manager="ronmart"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="04/27/2015"
    ms.author="awills"/>


# 安装应用程序的见解状态监视器来监视站点的性能

*在预览是应用程序的见解。*

状态监视器的 Visual Studio 应用程序的见解可以诊断异常和任何 IIS 服务器中运行的 web 应用程序中的性能问题。 只需将其安装在您的 IIS web 服务器，它将检测 ASP.NET web 应用程序将数据发送到应用程序的见解门户，以便您可以搜索和分析，发现。 您可以在您对其具有管理员访问权限的任何物理或虚拟服务器上安装它。

![示例图表](./media/app-insights-monitor-performance-live-website-now/10-intro.png)

您可以选择的三种方法可以应用到 IIS web 应用程序的应用程序的见解︰

* **生成时间︰**[添加应用程序深入 SDK]向 web 应用程序代码中的[greenbrown] 。 这为您提供︰
 * 标准诊断和使用遥测范围。
 * 然后，如果您要编写自己的遥测，跟踪使用情况或诊断问题，可以使用[应用程序理解 API][api] 。
* **运行时间︰**状态监视器用于检测您服务器上的 web 应用程序。
 * 监视 web 应用程序已在运行︰ 无需重新生成或重新发布它们。
 * 标准诊断和使用遥测范围。
 * 依赖项诊断成本 / 收益分析; 找到在您的应用程序使用其他组件，如数据库、 REST Api 或其他服务故障或性能下降。
 * 解决任何问题与遥测。
* **同时︰**编译到您的 web 应用程序代码的 SDK，您的 web 服务器上运行状态监视器。  完美的结合︰
 * 标准诊断和使用遥测。
 * 依赖项诊断。
 * 编写自定义使用 API 的遥测。
 * 解决任何问题的 SDK 和遥测。



> [AZURE.TIP] [Azure 应用程序服务 Web 应用程序](../app-service-web/websites-learning-map.md)是您的应用程序？ [添加应用程序深入 SDK][greenbrown] ，然后从应用程序的 Microsoft Azure 控制面板中[添加应用程序的见解扩展](../insights-perf-analytics.md)。


## 在您的 IIS web 服务器上安装应用程序的见解状态监视器

1. 您需要[Microsoft Azure](http://azure.com)订阅。

1. 在 IIS web 服务器上，使用管理员凭据登录。
2. 下载并运行[状态监视器安装程序](http://go.microsoft.com/fwlink/?LinkId=506648)。

4. 在安装向导中，登录到 Microsoft Azure。

    ![使用 Microsoft 帐户凭据登录到 Azure](./media/app-insights-monitor-performance-live-website-now/appinsights-035-signin.png)

5. 选择已安装的 web 应用程序或网站，您想要监视，然后配置您要应用的见解门户中查看结果的资源。

    ![选择应用程序和资源。](./media/app-insights-monitor-performance-live-website-now/appinsights-036-configAIC.png)

    通常情况下，您选择配置新资源和[资源组]的[角色]。

    如果您已经设置了您的网站或[web 客户端监视][客户端]的[web 测试][的可用性]，否则，使用现有资源。

6. 重新启动 IIS。

    ![选择重新启动对话框的顶部。](./media/app-insights-monitor-performance-live-website-now/appinsights-036-restart.png)

    您的 web 服务将暂时中断。

6. 请注意，ApplicationInsights.config 已插入您想要监视的 web 应用程序。

    ![发现旁边的代码文件的 web 应用程序的.config 文件。](./media/app-insights-monitor-performance-live-website-now/appinsights-034-aiconfig.png)

   也有对 web.config 的某些更改。

### 想要 （重新） 稍后配置？

完成该向导后，可以重新配置该代理，只要您希望。 您还可以使用此如果您安装了代理，但没有初始设置的一些疑难。

![单击任务栏上的应用程序理解图标](./media/app-insights-monitor-performance-live-website-now/appinsights-033-aicRunning.png)

## 查看性能遥测

登录到[Azure 预览门户](http://portal.azure.com)、 浏览应用程序的见解并打开您创建的资源。

![选择浏览，应用程序的见解，然后选择您的应用程序](./media/app-insights-monitor-performance-live-website-now/appinsights-08openApp.png)

打开性能刀片式服务器以查看依赖项和其他数据。

![表现](./media/app-insights-monitor-performance-live-website-now/21-perf.png)

单击通过任何图表以查看更多详细信息。


![](./media/app-insights-monitor-performance-live-website-now/appinsights-038-dependencies.png)

#### Dependencies

图表标签为 HTTP，SQL，AZUREBLOB 显示响应时间和对依赖项的调用计数︰ 也就是说，应用程序使用的外部服务。



#### 性能计数器

单击任何性能计数器图表，可以更改它的显示。 或者，您可以添加一个新图表。

#### 异常

![通过服务器异常图表中单击](./media/app-insights-monitor-performance-live-website-now/appinsights-039-1exceptions.png)

您可以深入了解特定异常 （从最近七天） 并获取堆栈跟踪和上下文数据。


### 没有遥测吗？

  * 使用您的站点，来生成某些数据。
  * 等待几分钟以使数据到达，然后单击**刷新**。
  * 打开诊断搜索 （搜索平铺） 若要查看单个事件。 聚合数据在图表中显示之前，事件往往在诊断搜索中可见。
  * 打开状态监视器，然后选择左侧窗格上的应用程序。 检查是否存在此应用程序的"配置通知"部分中的所有诊断消息︰

  ![](./media/app-insights-monitor-performance-live-website-now/appinsights-status-monitor-diagnostics-message.png)

  * 请确保服务器防火墙允许 dc.services.visualstudio.com 到端口 443 上的传出通信。
  * 在服务器上，如果您看到一条有关"权限不足"消息，尝试以下方法︰
    * 在 IIS 管理器中，选择应用程序池，打开**高级设置**记录下**过程模型**的身份。
    * 在计算机管理控制面板中，将该标识添加性能监视器用户组。
  * 请参阅[疑难解答][qna]。

## 系统要求

应用程序的见解状态监视器服务器上的操作系统支持︰

- Windows 2008 Server
- Windows Server 2008 R2
- Windows Server 2012
- Windows 服务器 2012 R2

使用新的 SP 和.NET Framework 4.0 和 4.5

在客户端 Windows 7，8 和 8.1，再次与.NET Framework 4.0 和 4.5

IIS 支持︰ IIS 7 7.5，8，8.5 （IIS 是必需的）

## <a name="next"></a>下一步行动

* [创建 web 测试][可用性]确保您的站点保持生存。
* [搜索事件和日志][诊断]可以帮助诊断问题。
* [添加 web 客户端遥测][使用]可以查看 web 页代码中的异常，并允许您插入跟踪调用。
* [将应用程序的见解 SDK 中添加 web 服务代码][greenbrown] ，以便您可以插入跟踪和日志在服务器代码中调用。

## 视频

#### 性能监视

[AZURE.VIDEO app-insights-performance-monitoring]

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[可用性]: app-insights-monitor-web-app-availability.md
[客户端]: app-insights-javascript.md
[诊断]: app-insights-diagnostic-search.md
[greenbrown]: app-insights-start-monitoring-app-health-usage.md
[通过]: app-insights-troubleshoot-faq.md
[角色]: app-insights-resources-roles-access-control.md
[使用]: app-insights-web-track-usage.md

测试

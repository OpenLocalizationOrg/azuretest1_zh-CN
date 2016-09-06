---
ms.openlocfilehash: 7e021778a69eff37ab1ca6807a018f83e52e604e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="Azure 的 web 应用程序性能监控" 
    description="图表负载和响应时间、 相关性信息和性能设置警报。" 
    services="azure-portal"
    documentationCenter="na"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="azure-portal" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/17/2015" 
    ms.author="awills"/>

# Azure 的 web 应用程序性能监控

在[Azure 门户网站](http://portal.azure.com)中您可以设置监视[Azure 的 web 应用程序](../app-service-web/app-service-web-overview.md)或[虚拟机](../virtual-machines/virtual-machines-about.md)中的应用程序依赖收集的统计信息和详细信息。

Azure 支持应用程序性能监视 （或者， *APM*） 通过利用*扩展*。 这些扩展安装到您的应用程序并收集数据，并报告给监视的服务。 

应用程序的见解和新 Relic 是两种性能监视可用的扩展。 若要使用新的 Relic，您安装的代理在运行时。 若要使用应用程序的见解，重建您的代码使用 SDK 中;并且，您也可以安装扩展，提供对其他数据访问。 可以使用 SDK 编写代码来监视使用情况和性能的应用程序中更详细地。  

## 启用扩展

1. 单击**浏览**，然后选择 web 应用程序或虚拟机，您想要检测。

2. 添加应用程序见解或新 Relic 扩展名。 

    如果您要分析 web 应用程序︰

![设置扩展，添加，应用程序的见解](./media/insights-perf-analytics/05-extend.png)

或者，如果您使用的虚拟机︰

![单击分析拼贴](./media/insights-perf-analytics/10-vm1.png)

### 应用程序的见解︰ 重建 sdk

通过为您的应用程序安装 SDK 应用程序的见解工作。 

在 Visual Studio，向项目中添加应用程序深入 SDK。

![用鼠标右键单击该 web 项目，然后选择添加应用程序见解](./media/insights-perf-analytics/03-add.png)

当要求您登录时，则使用的凭据 Azure 帐户。

您可以通过在您的开发计算机中运行应用程序测试遥测或我们可以继续操作，并将其重新发布。 

SDK 提供了 API，以便您可以[编写自定义的遥测](../app-insights-api-custom-events-metrics.md)跟踪使用情况。

## 浏览数据

使用一段时间来生成某些遥测应用程序。

1. 然后，从您的 web 应用程序或虚拟机刀片，您会看到安装扩展。
2. 单击表示您的应用程序中导航到该提供程序的行︰![单击刷新](./media/insights-perf-analytics/06-overview.png)

此外可以使用**浏览**以直接转到应用程序的见解组件或所使用的新的 Relic 帐户。

后至刀片式服务器，获取应用程序的见解，例如，您可以︰
- 打开性能︰

![在应用程序的见解概述刀片式服务器，单击性能拼贴](./media/insights-perf-analytics/07-dependency.png)

- 钻取以查看单个请求︰

![在网格中，单击一个依赖项来查看相关的请求。](./media/insights-perf-analytics/08-requests.png)

- 下面是时间的一个示例，说明 SQL 依赖项包括 SQL 调用相关的统计数据，如平均持续时间和标准偏差数中所用量。 

![](./media/insights-perf-analytics/01-example.png) 



## 下一步行动

* [监视服务健康指标](insights-how-to-customize-monitoring.md)，以确保您的服务是可用的并且能够作出响应。
* [启用监视和诊断](insights-how-to-use-diagnostics.md)服务上收集详细的高频指标。
* [接收通知](insights-receive-alert-notifications.md)每当发生的操作事件或指标跨越阈值。
* 使用[应用程序理解 JavaScript 应用程序和网页](../app-insights-web-track-usage.md)以获取有关访问的网页浏览器客户端分析。
* [显示器的可用性和响应能力的任何 web 页](../app-insights-monitor-web-app-availability.md)与应用程序使您可以找出如果您的页面的见解是向下。
 
测试

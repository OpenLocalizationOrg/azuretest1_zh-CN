---
ms.openlocfilehash: 716ba92e8141758dd9ddde88eedcbab96ebfc165
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="从应用程序的见解的旧 Visual Studio 联机版本升级" 
    description="升级现有项目"
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
    ms.date="06/19/2015" 
    ms.author="awills"/>
 
# 从应用程序的见解的旧 Visual Studio 联机版本升级

本文档是您感兴趣的您只有在仍在使用旧版本的应用程序的见解，这一部分的 Visual Studio 联机项目。 将及时，关闭该版本，因此我们建议您升级到最新版本，这是在 Microsoft Azure 服务。

## 有了哪个版本？

如果您使用 Visual Studio 2013年更新 3 或更高版本的项目中添加应用程序的见解，它很可能使用 Azure 的新版本。

打开 ApplicationInsights.config。 如果它具有节点， `ActiveProfile` ，`Profiles`是旧版本，您应该升级。

或查看项目在 Visual Studio 解决方案资源管理器，并在引用下，选择 Microsoft.ApplicationInsights。 在属性窗口中找到版本。 如果小于 0.12 则应该升级。

## 如果您有一个 Visual Studio 项目...

1. 打开 Visual Studio 2013年更新 3 或更高版本的项目。
2. 删除 ApplicationInsights.config 
3. 从项目中移除应用程序的见解 NuGet 程序包。 若要执行此操作，请右击解决方案资源管理器中的项目，然后选择管理 NuGet 程序包。

    ![](./media/app-insights-upgrade-vso-azure/nuget.png)
4. 删除 AppInsightsAgent 文件夹和其包含的所有文件。

    ![](./media/app-insights-upgrade-vso-azure/folder.png)

5. 从 ServiceDefinition.csdef 和 ServiceConfiguration.csfg 中删除所有的 Microsoft.AppInsights 设置的名称和值

    ![](./media/app-insights-upgrade-vso-azure/csdef.png)
4. SDK︰ 右键单击项目，然后[选择添加应用程序理解][greenbrown]。 此 SDK 会将添加到您的项目，并还在 Azure 中创建新的应用程序理解资源。
5. 日志记录︰ 如果您的代码包含对 LogEvent() 如旧 API 调用，您会发现它们在尝试生成解决方案时。 它们更新为[使用新 API][跟踪]。
6. 网页︰ 如果您的项目包含网页，替换中的脚本<head>节。 通常只有一个副本中没有母版页如 Views\Shared\_Layout.cshtml。 [获取来自快速入门刀片式服务器在 Azure 您理解应用程序资源中的新脚本][使用情况]。 如果您的 web 页如 logEvent 或 logPage，[更新以使用新 API]的正文中包括遥测调用[api]。
7. 服务器监视器︰ 如果您的应用程序在 IIS 上运行的服务，从该服务器，然后[安装应用程序的见解状态监视器][redfield]卸载 Microsoft 监控代理。
8. Web 测试︰ 如果您正在使用 web 可用性测试，请[重新创建这些新门户][的可用性]，具有其预警。
9. 警报︰ 设置 Azure 门户中[指标预警][警报]。


## 如果您有一个 Java web 服务...

1. 在您的服务器计算机，通过删除 APM 代理 web 服务启动文件的引用以禁用旧的代理。 在 TomCat 服务器上，编辑 Catalina.bat。 在 JBoss 服务器上，编辑 Run.bat。 
2. 重新启动 web 服务。
3. 在 Microsoft Azure 门户，[添加新的应用程序理解资源][java]。 在您的开发计算机，向 web 项目添加[Java SDK][java] 。
4. 替换的脚本中<head>的网页部分。 (在服务器端可能只是一份包括。)[获取来自快速入门刀片式服务器在 Azure 新见解应用程序资源中的新脚本][使用情况]。 如果您的 web 页中如 logEvent 或 logPage，[更新以使用新 API][跟踪]的主体包括遥测调用。
8. Web 测试︰ 如果您正在使用 web 可用性测试，请[重新创建这些新门户][的可用性]，具有其预警。
9. 警报︰ 设置 Azure 门户中[指标预警][警报]。



<!--Link references-->

[警告]: app-insights-alerts.md
[api]: app-insights-api-custom-events-metrics.md
[可用性]: app-insights-monitor-web-app-availability.md
[greenbrown]: app-insights-start-monitoring-app-health-usage.md
[java]: app-insights-java-get-started.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[跟踪]: app-insights-api-custom-events-metrics.md
[使用]: app-insights-web-track-usage.md

 
测试

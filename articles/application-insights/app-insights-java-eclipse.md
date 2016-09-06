---
ms.openlocfilehash: 2e05f0bde2e6ddc9a56edd833709bd362ba2b6cb
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="开始使用应用程序与在 Eclipse 中的 Java 的见解" 
    description="使用 Eclipse 插件添加性能和使用监控到网站 Java 应用程序的见解" 
    services="application-insights" 
    documentationCenter="java"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/13/2015" 
    ms.author="awills"/>
 
# 开始使用应用程序与在 Eclipse 中的 Java 的见解

应用程序深入 SDK 将遥测发送从 Java web 应用程序，以便能够分析使用情况和性能。 Eclipse 插件的应用程序的见解会自动安装 SDK 项目中，以便摆脱框遥测中，再加上一个 API，可以用于编写自定义的遥测。   


## 先决条件

当前在 Eclipse 中的动态 Web 项目的插件工作。 ([为其他类型的 Java 项目添加应用程序理解][java])。

您将需要︰

* Oracle 1.6 或更高版本的 JRE
* 对[Microsoft Azure](http://azure.microsoft.com/)的订阅。 （您无法启动[免费试用版](http://azure.microsoft.com/pricing/free-trial/)。）
* [Java EE 开发 eclipse IDE](http://www.eclipse.org/downloads/)，靛蓝色或更高版本。
* Windows 7 或更高版本，或者 Windows Server 2008 或更高版本

## 在 Eclipse （一次） 上安装 SDK

您只需要做这一次，每台计算机。 此步骤安装工具包，然后添加到每个动态 Web 项目的 SDK。

1. 在 Eclipse 中，单击安装新软件的帮助。

    ![帮助，安装新的软件](./media/app-insights-java-eclipse/0-plugin.png)

2. 在 http://dl.windowsazure.com/eclipse，在 Azure Toolkit 下是 SDK。 
3. 取消选中**联系所有更新站点...**

    ![对于应用程序的见解 SDK，清除联系所有更新网站](./media/app-insights-java-eclipse/1-plugin.png)

按照每个 Java 项目的剩余步骤。

## 获取应用程序的见解检测密钥

在 Azure 的 web 门户的 Azure 资源中将显示使用情况和性能分析。 在此步骤中，您即设置为应用程序的 Azure 的资源。

1. 登录到[Microsoft Azure 门户](https://portal.azure.com)。 （您将需要[Azure 的订阅](http://azure.microsoft.com/)）。
2. 创建新的应用程序理解资源

    ![单击 +，然后选择应用程序的见解](./media/app-insights-java-eclipse/01-create.png)
3. 设置为 Java web 应用程序的应用程序类型。

    ![填充一个名称，选择 Java web 应用程序，并单击创建](./media/app-insights-java-eclipse/02-create.png)
4. 找到新资源的检测项。 您将需要粘贴到 Eclipse 中的项目。

    ![在新的资源概述中，单击属性并复制检测密钥](./media/app-insights-java-eclipse/03-key.png)

## 添加到 Java 项目的 SDK

1. 从您的 web 项目的上下文菜单中添加应用程序的见解。

    ![在新的资源概述中，单击属性并复制检测密钥](./media/app-insights-java-eclipse/4-addai.png)
2. 粘贴你从 Azure 门户的检测项。

    ![在新的资源概述中，单击属性并复制检测密钥](./media/app-insights-java-eclipse/5-config.png)


密钥和遥测的每一项发送，告诉应用程序理解，以便将其显示在所需的资源。

## 运行应用程序并查看规格

运行您的应用程序。

返回到 Microsoft Azure 中您应用程序的见解的资源。

HTTP 请求的数据将出现在概述刀片式服务器。 （如果没有，请稍候片刻，然后单击刷新。）

![服务器响应、 请求计数和失败 ](./media/app-insights-java-eclipse/5-results.png)
 

单击通过任何图表以查看更详细的指标。 

![按名称的请求计数](./media/app-insights-java-eclipse/6-barchart.png)


[了解更多有关度量。][指标]

 

然后，查看请求的属性时，您可以看到请求和例外情况等与之相关联的遥测事件。
 
![此请求的的所有痕迹](./media/app-insights-java-eclipse/7-instance.png)


## 客户端的遥测

从快速启动刀片式服务器，请单击获取代码来监视 web 页︰ 

![在您的应用程序概述刀片式服务器，选择快速启动上，获取代码来监视我的 web 页。 复制脚本。](./media/app-insights-java-eclipse/02-monitor-web-page.png)

将代码段插入 HTML 文件的头部。

#### 查看客户端数据

打开更新的 web 页，并使用它们。 等待一分钟或两个，然后返回到应用程序的见解并刷新使用刀片式服务器。

页面视图、 用户和会话指标将出现在使用刀片式服务器上︰

![会话、 用户和网页视图](./media/app-insights-java-eclipse/appinsights-47usage-2.png)

[了解更多有关如何设置客户端的遥测。][使用]

## 可用性 web 测试

应用程序的见解可以测试您的网站是向上的复选和响应以及定期。 设置，单击通过空 web 测试图表概述刀片式服务器，并提供您的公用 URL。 

如果您的网站出现故障，您将获得的响应时间，再加上电子邮件通知。

![Web 测试示例](./media/app-insights-java-eclipse/appinsights-10webtestresult.png)

[了解更多有关可用性 web 测试。][可用性] 

## 诊断日志

如果您使用的 Logback 或 Log4J （1.2 版或 2.0 版） 进行跟踪，您可以跟踪日志自动发送到应用程序您可以在这里浏览和搜索他们的见解。

[了解更多有关诊断日志][javalogs]

## 自定义遥测 

在 Java web 应用程序来了解用户用它做什么，或以帮助诊断问题中插入几行代码。 

您可以在 JavaScript 网页和服务器端 Java 中插入代码。

[关于自定义遥测][跟踪]



## 下一步行动

#### 检测和诊断问题

* [添加 web 客户端遥测][使用]，可以从 web 客户端的性能遥测。
* [设置 web 测试][可用性]，来确保您的应用程序保持实时和快速响应。
* [搜索事件和日志][诊断]可以帮助诊断问题。
* [捕获 Log4J 或 Logback 跟踪][javalogs]

#### 跟踪使用情况

* [添加 web 客户端遥测][使用]监视器页面视图和基本用户的指标。
* [自定义跟踪的事件和标准][跟踪]了解您的应用程序如何使用的在客户端和服务器。


<!--Link references-->

[可用性]: app-insights-monitor-web-app-availability.md
[诊断]: app-insights-diagnostic-search.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[指标]: app-insights-metrics-explorer.md
[跟踪]: app-insights-api-custom-events-metrics.md
[使用]: app-insights-web-track-usage.md

 
测试

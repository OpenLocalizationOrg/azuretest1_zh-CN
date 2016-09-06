---
ms.openlocfilehash: 999d95995e07ebdc8763d8cdd27d0e8af5046e6f
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="跟踪 Java web 应用程序中的 HTTP 请求" 
    description="应用程序的见解，可以测量站点 Java web 应用程序的性能" 
    services="application-insights" 
    documentationCenter=""
    authors="alancameronwills" 
    manager="keboyd"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/26/2015" 
    ms.author="awills"/>
 
# 跟踪 Java web 应用程序中的 HTTP 请求

如果您在运行 Java web 应用程序，您可以查看 HTTP 请求发送到您的应用程序，例如请求的资源、 失败的请求和响应时间，应用程序的见解门户中所有有关的信息。

安装[的 Java 应用程序的见解 SDK][java]中，如果已经没有这样的。


## 将二进制文件添加到项目

*选择适当的方式为您的项目。*

### 如果您正在使用 Maven...

如果已将项目设置为生成使用 Maven，合并 pom.xml 文件到代码的以下代码段。

然后刷新项目依赖项，以获取下载的二进制文件。

    <dependencies>
      <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>applicationinsights-web</artifactId>
        <version>[0.9,)</version>
      </dependency>
    </dependencies>

### 如果您使用的 Gradle...

如果您的项目已设置要用于生成 Gradle，合并代码到您的 build.gradle 文件的下面的代码段。

然后刷新项目依赖项，以获取下载的二进制文件。

    dependencies {
      compile group: 'com.microsoft.azure', name: 'applicationinsights-web', version: '0.9.+'
    }

## 向项目中添加应用程序理解 HTTP 筛选器

查找并打开 web.xml 文件在您的项目中，合并代码的 web 应用程序节点，您的应用程序筛选器的配置位置下面的代码段。

若要获得最准确的结果，应该在所有其他筛选器之前映射筛选器。

    <filter>
      <filter-name>ApplicationInsightsWebFilter</filter-name>
      <filter-class>
        com.microsoft.applicationinsights.web.internal.WebRequestTrackingFilter
      </filter-class>
    </filter>
    <filter-mapping>
       <filter-name>ApplicationInsightsWebFilter</filter-name>
       <url-pattern>/*</url-pattern>
    </filter-mapping>

## 向项目中添加 HTTP 模块

查找并打开 ApplicationInsights.xml 文件在您的项目中，合并代码的以下段<TelemetryModules>元素。

如果没有任何<TelemetryModules>在此文件中的元素添加下一个<ApplicationInsights>元素。

    <TelemetryModules>
      <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebRequestTrackingTelemetryModule"/>
      <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebSessionTrackingTelemetryModule"/>
      <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebUserTrackingTelemetryModule"/>
    </TelemetryModules>

## 添加遥测事件相关初始值设定项

与事件关联的 HTTP 请求和已发送请求处理，使用操作 ID 属性附加到每个遥测事件期间的遥测事件之间相关联。 这使得研究以及从被调用的事件的所有 HTTP 请求，从而有助于诊断和解决问题。

查找并打开 ApplicationInsights.xml 文件在您的项目中，合并代码的以下段<TelemetryInitializers>元素。

如果此文件没有 < TelemetryInitializers > 元素，添加一个在<ApplicationInsights>元素。

    <TelemetryInitializers>
     <Add  type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationIdTelemetryInitializer"/>
     <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationNameTelemetryInitializer"/>
     <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebSessionTelemetryInitializer"/>
     <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserTelemetryInitializer"/>
     <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserAgentTelemetryInitializer"/>
    </TelemetryInitializers>


## 查看应用程序的见解中请求信息

运行您的应用程序。

返回到 Microsoft Azure 中您应用程序的见解的资源。

HTTP 请求的数据将出现在概述刀片式服务器。 （如果没有，请稍候片刻，然后单击刷新。）

![](./media/app-insights-java-track-http-requests/5-results.png)
 

单击通过任何图表以查看更详细的指标。 

![](./media/app-insights-java-track-http-requests/6-barchart.png)


[了解更多有关度量。][指标]

 

然后，查看请求的属性时，您可以看到请求和例外情况等与之相关联的遥测事件。
 
![](./media/app-insights-java-track-http-requests/7-instance.png)




## 下一步行动

* [搜索事件和日志][诊断]可以帮助诊断问题。
* [捕获 Log4J 或 Logback 跟踪][javalogs]



<!--Link references-->

[诊断]: app-insights-diagnostic-search.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[指标]: app-insights-metrics-explorer.md

 
测试

---
ms.openlocfilehash: f35a34319c8135396d7e5f8951b16584897a8587
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="开始使用应用程序的见解在 Java web 项目 |Microsoft Azure"
    description="监视性能和使用情况的网站 Java 应用程序的见解"
    services="application-insights"
    documentationCenter="java"
    authors="alancameronwills"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="06/30/2015"
    ms.author="awills"/>

# 在 Java web 项目中开始使用应用程序的见解

*在预览是应用程序的见解。*

[AZURE.INCLUDE [app-insights-selector-get-started](../../includes/app-insights-selector-get-started.md)]

[应用程序的见解](https://azure.microsoft.com/services/application-insights/)是可扩展的分析服务，帮助您了解性能和实时应用程序的使用情况。 它[检测和诊断性能问题和异常](app-insights-detect-triage-diagnose.md)，以及[编写代码][api]来跟踪哪些用户与您的应用程序使用。

![示例数据](./media/app-insights-java-get-started/5-results.png)

[理解应用程序的 web 测试][可用性]监视应用程序的可用性。

您将需要︰

* Oracle JRE 1.6 或更高版本，或者祖鲁 JRE 1.6 或更高版本
* 对[Microsoft Azure](http://azure.microsoft.com/)的订阅。 （您无法启动[免费试用版](http://azure.microsoft.com/pricing/free-trial/)。）

*如果您有已实时 web 应用程序，您可以添加[运行时在 web 服务器上的 SDK](app-insights-java-live.md)遵循替代过程。 该替代方案可避免重新生成代码，但不能获得可以编写代码来跟踪用户活动。*


## 1.获得应用程序的见解检测密钥

1. 登录到[Microsoft Azure 门户](https://portal.azure.com)。
2. 创建新的应用程序理解资源。

    ![单击 +，然后选择应用程序的见解](./media/app-insights-java-get-started/01-create.png)
3. 设置为 Java web 应用程序的应用程序类型。

    ![填充一个名称，选择 Java web 应用程序，并单击创建](./media/app-insights-java-get-started/02-create.png)
4. 找到新资源的检测项。 您将需要粘贴到您的代码项目很快。

    ![在新的资源概述中，单击属性并复制检测密钥](./media/app-insights-java-get-started/03-key.png)

## 2.向项目中添加 Java 应用程序的见解 SDK

*选择适当的方式为您的项目。*

#### 如果要在 Eclipse 中创建一个动态 Web 项目...

使用[应用程序的 Java 插件的见解 SDK][eclipse]。

#### 如果您正在使用 Maven...

如果您的项目已经设置为使用 Maven，合并 pom.xml 文件与下面的代码。

然后，刷新以获取下载的二进制文件的项目依赖项。

    <repositories>
       <repository>
          <id>central</id>
          <name>Central</name>
          <url>http://repo1.maven.org/maven2</url>
       </repository>
    </repositories>

    <dependencies>
      <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>applicationinsights-web</artifactId>
        <!-- or applicationinsights-core for bare API -->
        <version>[1.0,)</version>
      </dependency>
    </dependencies>


* *生成或校验和验证错误？* 请尝试使用一个特定的版本，如︰ `<version>1.0.n</version>`。 在[SDK 发行说明](app-insights-release-notes-java.md)中或在我们[Maven 项目](http://search.maven.org/#search%7Cga%7C1%7Capplicationinsights)，您会发现最新的版本。
* *需要为新的 SDK 更新？* 刷新项目的依赖项。

#### 如果您使用的 Gradle...

如果您的项目已设置要用于生成 Gradle，合并以下代码到您的 build.gradle 文件。

然后刷新以获取下载的二进制文件的项目依赖项。

    repositories {
      mavenCentral()
    }

    dependencies {
      compile group: 'com.microsoft.azure', name: 'applicationinsights-web', version: '1.+'
      // or applicationinsights-core for bare API
    }

* *生成或校验和校验错误？请尝试使用一个特定的版本，如︰* `version:'1.0.n'`。 *您可以找到最新版本的[SDK 发行说明](app-insights-release-notes-java.md)中。*
* *若要更新到了新的 SDK*
 * 刷新项目的依赖项。

#### 否则为...

手动添加 SDK:

1. 下载[应用程序的 Java SDK 见解](http://dl.windowsazure.com/lib/applicationinsights/javabin/sdk.zip)。
2. 从 zip 文件解压二进制文件并将它们添加到您的项目。

### 问题...

* *之间的关系是什么`-core`，`-web`在 zip 中的组件？*

 * `applicationinsights-core` 为您提供裸机的 API。 您总是需要它。
 * `applicationinsights-web` 为您提供跟踪 HTTP 请求和响应时间的衡量标准。 如果您不希望自动收集此遥测时，可以省略此开关。 例如，如果您要编写您自己。

* *当我们发布的更改更新 SDK*
 * 下载最新[的 Java 应用程序的见解 SDK](http://dl.windowsazure.com/lib/applicationinsights/javabin/sdk.zip)并替换旧电池。
 * 在[SDK 发行说明](app-insights-release-notes-java.md)中介绍的更改。



## 3.添加一个应用程序的见解的.xml 文件

将 ApplicationInsights.xml 添加到资源文件夹在您的项目中，或者确保它被添加到项目的类路径部署。 在其中复制下面的 XML。

用来代替你从 Azure 门户的检测项。

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings" schemaVersion="2014-05-30">


      <!-- The key from the portal: -->

      <InstrumentationKey>** Your instrumentation key **</InstrumentationKey>


      <!-- HTTP request component (not required for bare API) -->

      <TelemetryModules>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebRequestTrackingTelemetryModule"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebSessionTrackingTelemetryModule"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebUserTrackingTelemetryModule"/>
      </TelemetryModules>

      <!-- Events correlation (not required for bare API) -->
      <!-- These initializers add context data to each event -->

      <TelemetryInitializers>
        <Add   type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationIdTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationNameTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebSessionTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserAgentTelemetryInitializer"/>

      </TelemetryInitializers>
    </ApplicationInsights>


* 检测项遥测的每一项一起发送并告诉应用程序理解，以便将其显示在所需的资源。
* HTTP 请求组件是可选的。 它自动发送到门户的遥测有关请求和响应时间。
* 事件关联是 HTTP 请求组件的补充。 它将标识符分配给服务器，接收的每个请求并向此作为属性遥测的每一项 Operation.Id 的属性。 它允许您通过将筛选器设置在[诊断搜索][诊断]与每个请求关联的遥测数据关联起来。

## 4.添加 HTTP 筛选器

最后一个配置步骤允许登录每个 web 请求的 HTTP 请求组件。 （不需要如果您只需要裸机 API。）

查找并打开 web.xml 文件在您的项目中，合并的节点下的 web 应用程序，您的应用程序筛选器的配置位置下面的代码。

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

#### 如果您使用的 MVC 3.1 或更高版本

编辑这些元素可以包含应用程序的见解的程序包︰

    <context:component-scan base-package=" com.springapp.mvc, com.microsoft.applicationinsights.web.spring"/>

    <mvc:interceptors>
        <mvc:interceptor>
            <mvc:mapping path="/**"/>
            <bean class="com.microsoft.applicationinsights.web.spring.RequestNameHandlerInterceptorAdapter" />
        </mvc:interceptor>
    </mvc:interceptors>

#### 如果您正在使用 Struts 2

将此项添加到 Struts 配置文件 （通常命名为 struts.xml 或 struts default.xml）︰

     <interceptors>
       <interceptor name="ApplicationInsightsRequestNameInterceptor" class="com.microsoft.applicationinsights.web.struts.RequestNameInterceptor" />
     </interceptors>
     <default-interceptor-ref name="ApplicationInsightsRequestNameInterceptor" />

（如果您具有定义默认堆栈中的拦截器，拦截器可以简单地添加到该堆栈。）

## 5.在该服务器上安装

在 Windows 服务器上安装︰

* [Microsoft Visual C++ 可再发行组件](http://www.microsoft.com/download/details.aspx?id=40784)

（这使性能计数器）。

## 6.运行您的应用程序

在调试模式下运行它在您的开发计算机上，或者发布到您的服务器。

## 7.在应用程序的见解中查看您的遥测

返回到[Microsoft Azure 门户](https://portal.azure.com)中所需应用程序的见解的资源。

HTTP 请求的数据将出现在概述刀片式服务器。 （如果没有，请稍候片刻，然后单击刷新。）

![示例数据](./media/app-insights-java-get-started/5-results.png)


单击通过任何图表以查看更详细的指标。

![](./media/app-insights-java-get-started/6-barchart.png)



然后，查看请求的属性时，您可以看到请求和例外情况等与之相关联的遥测事件。

![](./media/app-insights-java-get-started/7-instance.png)



[了解更多有关度量。][指标]

#### 智能地址名称计算

应用程序的见解认为 MVC 应用程序的 HTTP 请求的格式是︰ `VERB controller/action`


例如， `GET Home/Product/f9anuh81`， `GET Home/Product/2dffwrf5` ，`GET Home/Product/sdf96vws`将被分组到`GET Home/Product`。

这样请求，如请求数量和请求的平均执行时间的意义的聚合。

## 异常和请求失败

收集到的未处理的异常︰

![](./media/app-insights-java-get-started/21-exceptions.png)

要收集的其他异常数据，您有两个选项︰

* [插入对 TrackException 您的代码中调用][apiexceptions]。
* [安装 Java 代理服务器上](app-insights-java-agent.md)。 指定要监视的方法。


## 监视方法调用和外部依赖项

[安装 Java 代理](app-insights-java-agent.md)日志指定的内部方法和调用 JDBC，通过建立与计时数据。


## 性能计数器

单击**服务器**拼贴，然后您将看到一系列的性能计数器。


![](./media/app-insights-java-get-started/11-perf-counters.png)

### 自定义性能计数器集合

要禁用一组标准的性能计数器的集合，请添加下面的 ApplicationInsights.xml 文件的根节点下面的代码︰

    <PerformanceCounters>
       <UseBuiltIn>False</UseBuiltIn>
    </PerformanceCounters>

### 收集性能计数器

您可以指定要收集的其他性能计数器。

#### JMX 计数器 （公开的 Java 虚拟机）

    <PerformanceCounters>
      <Jmx>
        <Add objectName="java.lang:type=ClassLoading" attribute="TotalLoadedClassCount" displayName="Loaded Class Count"/>
        <Add objectName="java.lang:type=Memory" attribute="HeapMemoryUsage.used" displayName="Heap Memory Usage-used" type="composite"/>
      </Jmx>
    </PerformanceCounters>

*   `displayName` --应用程序理解门户显示的名称。
*   `objectName` – 在 JMX 对象名称。
*   `attribute` – 要获取的 JMX 对象名称该属性
*   `type` （可选）-JMX 对象的属性的类型︰
 *  默认值︰ 简单类型如 int 或 long 类型的值。
 *  `composite`︰ 性能计数器数据是 Attribute.Data 的格式
 *  `tabular`︰ 性能计数器数据是表行的格式



#### Windows 性能计数器

每个[Windows 性能计数器](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx)（以同样的方式，该字段是类的成员） 是一个类别的成员。 类别可以是全局的或可有编号或命名实例。

    <PerformanceCounters>
      <Windows>
        <Add displayName="Process User Time" categoryName="Process" counterName="%User Time" instanceName="__SELF__" />
        <Add displayName="Bytes Printed per Second" categoryName="Print Queue" counterName="Bytes Printed/sec" instanceName="Fax" />
      </Windows>
    </PerformanceCounters>

*   显示名称--应用程序理解门户网站中显示的名称。
*   类别名称 – 此性能计数器关联的性能计数器类别 （性能对象）。
*   取代 – 性能计数器的名称。
*   实例名称 – 名称性能计数器类别实例中，则为空字符串 ("")，如果该类别包含单个实例。 如果类别名称过程，并且您可能会希望收集性能计数器是从当前 JVM 进程上运行您的应用程序，指定`"__SELF__"`。

性能计数器是自定义度量[指标]的[测量数据资源管理器]的可见性。

![](./media/app-insights-java-get-started/12-custom-perfs.png)


### Unix 的性能计数器

* [安装与该应用程序的见解插件 collectd](app-insights-java-collectd.md)以获取各种系统和网络数据。

## 获取用户和会话数据

好的您从您的 web 服务器发送遥测。 现在若要获得完整的 360 度视图的应用程序，您可以添加多个监视︰

* [向网页添加遥测][使用]监视器页面视图和用户的指标。
* [设置 web 测试][可用性]，来确保您的应用程序保持实时和快速响应。

## 捕获日志跟踪

从 Log4J、 Logback 或其他日志记录框架，您可以使用切片和切块日志应用程序的见解。 可以将日志与 HTTP 请求和其他遥测相关联。 [了解如何][javalogs]。

## 发送您自己的遥测

现在，您已安装 SDK，可以使用 API 来发送自己的遥测。

* [自定义跟踪的事件和标准][api]用于了解用户与应用程序做什么。
* [搜索事件和日志][诊断]可以帮助诊断问题。






## 问题？ 有问题吗？

[Java 的疑难解答](app-insights-java-troubleshoot.md)



<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiexceptions]: app-insights-api-custom-events-metrics.md#track-exception
[可用性]: app-insights-monitor-web-app-availability.md
[诊断]: app-insights-diagnostic-search.md
[日蚀式]: app-insights-java-eclipse.md
[javalogs]: app-insights-java-trace-logs.md
[指标]: app-insights-metrics-explorer.md
[使用]: app-insights-web-track-usage.md

测试

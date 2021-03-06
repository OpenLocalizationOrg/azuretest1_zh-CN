---
ms.openlocfilehash: 6c8ac7e02b30e434d880376a79ffa7942a70e4d7
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="collectd︰ 在 Unix 应用程序的见解中的 Java 的性能统计" 
    description="扩展的网站 Java 插件 CollectD 监视的应用程序的见解" 
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
    ms.date="07/14/2015" 
    ms.author="awills"/>
 
# collectd: Unix 应用程序的见解中的性能指标

*在预览是应用程序的见解。*

若要了解 Unix 系统性能度量值在[应用程序的见解](app-insights-overview.md)，安装[collectd](http://collectd.org/)，以及插件的应用程序理解。 这一开放源代码解决方案收集各种系统和网络的统计数据。

通常您将使用 collectd 如果您已[检测 Java web 服务应用程序的见解与][java]中，以便您有更多的数据，可帮助您提高您的应用程序性能或诊断问题。 

![示例图表](./media/app-insights-java-collectd/sample.png)

## 获取密钥，检测

在[Microsoft Azure 门户](http://portal.azure.com)中，打开要显示的数据[应用程序的见解](start)资源。 （或[创建新的资源](app-insights-create-new-resource.md)）。

采用检测密钥，它标识该资源的副本。

![浏览所有，打开所需的资源，然后在 Esssentials 下拉列表，选择并复制检测密钥](./media/app-insights-java-collectd/02-props.png)



## 安装 collectd 和插件

在您的 Unix 服务器计算机︰

1. 安装[collectd](http://collectd.org/)版本含有 5.4.0 或更高版本。
2. 下载该[应用程序的见解 collectd 编写器插件](http://go.microsoft.com/fwlink/?LinkId=618634)。 记下版本号。
3. 复制该插件 JAR 到`/usr/share/collectd/java`。
3. 编辑`/etc/collectd/collectd.conf`:
 * 确保已启用该[Java 插件](https://collectd.org/wiki/index.php/Plugin:Java)。
 * 更新为 java.class.path JVMArg，以包括以下 JAR。 更新以匹配您所下载的版本号︰
  * `/usr/share/collectd/java/applicationinsights-collectd-0.9.4.jar`
 * 添加此代码段，使用检测从所需的资源的键︰

```

     LoadPlugin "com.microsoft.applicationinsights.collectd.ApplicationInsightsWriter"
     <Plugin ApplicationInsightsWriter>
        InstrumentationKey "Your key"
     </Plugin>
```

下面是一个示例配置文件的一部分︰

    ...
    # collectd plugins
    LoadPlugin cpu
    LoadPlugin disk
    LoadPlugin load
    ...

    # Enable Java Plugin
    LoadPlugin "java"

    # Configure Java Plugin
    <Plugin "java">
      JVMArg "-verbose:jni"
      JVMArg "-Djava.class.path=/usr/share/collectd/java/applicationinsights-collectd-0.9.4.jar:/usr/share/collectd/java/collectd-api.jar"

      # Enabling Application Insights plugin
      LoadPlugin "com.microsoft.applicationinsights.collectd.ApplicationInsightsWriter"
                
      # Configuring Application Insights plugin
      <Plugin ApplicationInsightsWriter>
        InstrumentationKey "12345678-1234-1234-1234-123456781234"
      </Plugin>

      # Other plugin configurations ...
      ...
    </Plugin>
.   ...

配置其他[collectd 插件](https://collectd.org/wiki/index.php/Table_of_Plugins)，它可以从不同的来源收集各种数据。

请 collectd 根据其[手动](https://collectd.org/wiki/index.php/First_steps)重新启动。

## 查看应用程序的见解中的数据

在您的应用程序理解资源打开[测量数据资源管理器和添加图表][指标]，选择您想要查看从自定义类别的指标。

![](./media/app-insights-java-collectd/result.png)

默认情况下，从中收集度量标准的所有主机计算机之间聚合的度量标准。 若要查看图表细节刀片式服务器中的每台主机，指标，打开分组，然后选择按 CollectD 主机进行分组。


## 要排除特定的统计信息上传

默认情况下，该应用程序的见解插件将发送读取插件的所有已启用 collectd 所收集的所有数据。 

要排除特定的插件或数据源的数据︰

* 编辑配置文件。 
* 在`<Plugin ApplicationInsightsWriter>`，添加指令行如下︰

指令 | Effect
---|---
`Exclude disk` | 排除所有数据收集的`disk`插件
`Exclude disk:read,write` | 排除源名为`read`，`write`从`disk`插件。

新行使用单独的指令。


## 有问题吗？

*我看不到门户网站中的数据*

* 打开[搜索][诊断]以查看原始事件是否均已到达。 有时，它们长在测量数据资源管理器中显示了。
* 在中启用跟踪应用程序的见解插件。 添加以下行内的`<Plugin ApplicationInsightsWriter>`:
 *  `SDKLogger true`
* 打开终端，启动 collectd 在详细模式中，若要查看其报告的任何问题︰
 * `sudo collectd -f`




<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiexceptions]: app-insights-api-custom-events-metrics.md#track-exception
[可用性]: app-insights-monitor-web-app-availability.md
[诊断]: app-insights-diagnostic-search.md
[日蚀式]: app-insights-java-eclipse.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[指标]: app-insights-metrics-explorer.md
[使用]: app-insights-web-track-usage.md

 
测试

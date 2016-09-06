---
ms.openlocfilehash: f5940fe42eafb151198073a75c9f35d223b21039
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="Java 的应用程序理解的发行说明" 
    description="最新的更新。" 
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
    ms.date="06/18/2015" 
    ms.author="awills"/>
 
# 应用程序理解 Java SDK 的发行说明

[对于 Java 应用程序的见解 SDK](app-insights-java-get-started.md)将遥测实时应用程序有关发送到[应用程序的见解](http://azure.microsoft.com/services/application-insights/)，可以分析其使用情况和性能。

#### 若要安装 SDK 应用程序中

请参阅[开始使用 Java SDK](app-insights-java-get-started.md)。

#### 若要升级到最新 SDK 

升级后，您将需要合并回对 ApplicationInsights.xml 所做的任何自定义。 需要一份它与新的文件进行比较。

*如果您正在使用 Maven 或 Gradle*

1. 如果在 pom.xml 或 build.gradle 中指定了某一特定版本号，请进行更新。
2. 刷新项目的依赖项。

*否则*

* 下载最新版本的[Java 的 Azure 库](http://dl.msopentech.com/lib/PackageForWindowsAzureLibrariesForJava.html)并替换旧电池。 
 
比较旧的和新 ApplicationInsights.xml。 您看到的更改大部分是因为我们能够添加和删除模块。 恢复您所做的任何自定义。

## 1.0.1 版
- Java 代理支持收集关于以下的相关性信息︰
    - 通过 HttpClient、 OkHttp 和 RestTemplate （弹簧） 的 HTTP 调用。
    - Redis 通过 Jedis 客户端进行调用。 当传递一个可配置的阈值时，SDK 还将提取调用参数。
    - JDBC 与 Oracle 数据库和 Apache Derby 数据库客户端进行调用。
    - 预准备语句的支持将 executeBatch 查询类型 – SDK 将显示该语句使用的批数。
    - 为支持该 (MySql，PostgreSql) 的 JDBC 客户机提供查询计划 – 仅在可配置的阈值时，则提取查询计划

## 1.0.0 版
- CollectD 的见解应用程序编写器插件添加支持。
- 添加对应用程序理解 Java 代理支持。
- 支持 HttpClient 版本 4.2 及更高版本的兼容性问题的的修补程序。

## 版本 0.9.6
- 使 Java SDK HttpClient 预 v4.3 servlet v2.5 与兼容。
- 添加 Java EE 拦截器的支持。
- 从 Logback appender 删除冗余的依存关系。

## 版本 0.9.5  

- 问题的修复程序中的自定义事件不相关与用户/会话由于分析错误的 cookie。  
- 改进用于解析 ApplicationInsights.xml 配置文件的位置的逻辑。
- 将不会在服务器端生成匿名用户和会话 cookie。 若要实现用户和 web 应用程序的跟踪会话，JavaScript sdk 工具现在是必需 – 从 JavaScript SDK 的 cookie 仍会考虑。 请注意，此更改可能导致用户巨大重述会话数为现在计仅用户发起会话。

## 版本 0.9.4

- 从 32 位 Windows 计算机收集性能计数器的支持。
- 支持手动跟踪依赖关系使用一个新的```trackDependency```API 方法。
- 通过添加标记项为综合遥测能力```SyntheticSource```到报告项属性。
 

测试

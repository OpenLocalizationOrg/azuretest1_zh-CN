---
ms.openlocfilehash: 1762b560b5bcbc19f141928e2fe8de3057703852
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="设置服务结构应用程序的应用程序的见解"
   description="接收应用程序理解服务结构为应用程序的事件。"
   services="service-fabric"
   documentationCenter=".net"
   authors="mattrowmsft"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/20/2015"
   ms.author="mattrow"/>

# 设置服务结构应用程序的应用程序的见解
 这篇文章将引导您完成服务结构应用程序中启用应用程序的见解。

## 先决条件

本文假定您已在 Visual Studio 中创建了一个服务结构应用程序。 若要了解如何做[，请单击此处](service-fabric-reliable-services-quick-start.md)。

## NuGet 程序包安装
作为一部分的服务结构 SDK 发布是我们 nuget 程序包 Microsoft.ServiceFabric.Telemetry.ApplicationInsights 的预发行版本。 该软件包将联系起来服务结构 EventSource 事件与应用程序理解，以便为您提供自动化服务结构的应用程序的检测。
该软件包将继续更新与新的事件，这将由您的应用程序会自动发出。

可以使用以下步骤安装软件包︰

1. 打开服务结构 NuGet 程序包管理器的应用程序。  这可以通过右键单击您在 Visual Studio 中的项目并选择管理 NuGet 程序包...。
2. 您将需要选择 Microsoft Azure 服务结构作为服务结构 SDK 中包括的列表程序包源。 
![VS2015 NuGet 程序包管理器](media/service-fabric-diagnostics-application-insights-setup/AI-nuget-package-manager.jpg)
3. 选择左侧的 Microsoft.ServiceFabric.Telemetry.ApplicationInsights 包。
4. 单击安装以开始安装过程。
5. 查看并接受最终用户许可协议。

## 启用服务结构事件
要自动接收服务结构事件在应用程序的见解需要启用我们侦听器。
您可以通过将下面这行代码插入到您的应用程序来执行此操作。

```csharp
    Microsoft.ServiceFabric.Telemetry.ApplicationInsights.Listener.Enable(EventLevel.Verbose);
```
 
### StatefulActor\Program.cs 的示例︰

```csharp
    public static void Main(string[] args)
    {
        Microsoft.ServiceFabric.Telemetry.ApplicationInsights.Listener.Enable(EventLevel.Verbose);
        try
        {
            using (FabricRuntime fabricRuntime = FabricRuntime.Create())
            {
                fabricRuntime.RegisterActor(typeof(StatefulActor));

                Thread.Sleep(Timeout.Infinite);
            }
        }
        catch (Exception e)
        {
            ActorEventSource.Current.ActorHostInitializationFailed(e);
            throw;
        }
    }
```

您可以了解从可靠者运行时发出的事件[在此处](service-fabric-reliable-actors-diagnostics.md)和可靠的服务运行时[这里](service-fabric-reliable-services-diagnostics.md)。

请注意，为了获得可靠的演员运行时方法调用，EventLevel.Verbose 必须使用 （如上面的示例中所示）。

## 设置应用程序的见解
规范关键在于您服务结构的应用程序到应用程序的见解资源联系起来。  您可以学习如何获取以下[应用程序分析指南](../app-insights-create-new-resource.md#create-an-application-insights-resource)由检测密钥。
时，选择其他应用程序类型创建资源。

![选择其他 AI 的应用程序类型](media/service-fabric-diagnostics-application-insights-setup/AI-app-type-other.JPG)

一旦检测键，您可以将其插入 ApplicationInsights.config 文件如下所示︰

```xml
    <InstrumentationKey>INSERT YOUR KEY HERE</InstrumentationKey>
```

## 查看数据
您可以[自定义应用程序的见解刀片式服务器](../app-insights-metrics-explorer.md)，满足您的需要。 大多数服务结构事件将显示为自定义事件，同时结构主角方法调用和服务调用将显示为请求的 RunAsync()。  
建模为请求这些事件使您可以使用请求名称维度和请求持续时间跃点数来构建图表。
新图表、 数据和事件仍将添加将能够利用将来的。

## 下一步行动
了解有关使用应用程序的见解来检测您服务结构的应用程序。

- [要开始使用应用程序的见解](../app-insights-get-started.md)
- [了解如何创建您自己的自定义事件和标准](../app-insights-custom-events-metrics-api.md)
 

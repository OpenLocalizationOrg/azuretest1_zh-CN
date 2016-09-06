---
ms.openlocfilehash: 320a14fddcfc20c6b03375076fcb565affa4203a
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="开始使用可靠的参与者 |Microsoft Azure"
   description="本教程将带领您一步步地创建、 调试和部署使用服务结构可靠参与者 HelloWorld 服务规范。"
   services="service-fabric"
   documentationCenter=".net"
   authors="jessebenson"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/05/2015"
   ms.author="claudioc"/>

# 规范 HelloWorld 考察方案的可靠参与者︰
本文介绍服务结构可靠参与者的基本常识，并指导您完成创建、 调试和部署 Visual Studio 中的简单 HelloWorld 应用程序。

## 安装和设置
在开始之前，请确保您有在您的计算机上的服务结构开发环境设置。
详细的说明如何设置开发环境可以找到[这里](service-fabric-get-started.md)。

## 基本概念
为了开始使用可靠的演员只被需要了解 4 的基本概念︰

* **使用者服务**。 可靠的演员被打包在可以服务结构基础结构中部署的服务中。 服务可以承载一个或多个参与者。 我们以后会进入一个与每个服务的多个参与者的权衡的更多详细信息。 现在让我们假设我们需要实现一个主角。
* **使用者界面**。 使用者接口用于定义参与者的公共接口。 在主角模型术语定义的主角是能够了解进程的消息的类型。 使用者接口由其他参与者或客户端应用程序用于 '' （异步） 将消息发送到的参与者。 可靠的参与者可以实现多个接口，我们将会看到的实现 IHelloWorld 接口，而且还用于定义不同的邮件功能的 ILogging 接口 HelloWorld 参与者可以。
* **使用者注册**。 在使用者服务，需要使服务结构注意到新的类型，可以使用它来创建新的参与者注册参与者类型。
* **ActorProxy 类**。 ActorProxy 类用于绑定到操作者和调用通过其接口公开的方法。 ActorProxy 类提供了两个重要功能︰
    * 名称解析︰ 能够找到群集中的主角 （查找哪个节点的群集中托管）。
    * 处理失败︰ 它可以重试方法调用并为例，要求参与者要重新定位到另一个群集中的节点出现故障之后重新确定的主角位置。

## 在 Visual Studio 中创建一个新项目
Visual Studio 安装服务结构工具后，您可以创建新的项目类型。 在新建项目对话框的云类别下的新的项目类型


![VS-新项目的服务结构工具][1]

在下一个对话框中您可以选择您想要创建的项目的类型。

![服务项目模板结构][5]

HelloWorld 项目，我们将使用服务结构使用者服务。

创建解决方案后，您应看到以下结构︰

![服务结构项目结构][2]

## 可靠的演员基本构造块

典型的可靠参与者解决方案由 3 项目的︰

* 应用程序项目 (HelloWorldApplication)。 这是打包在一起进行部署的所有服务项目。 它包含用于管理应用程序的 ApplicationManifest.xml 和 PowerShell 脚本。

* 界面项目 (HelloWorld.Interfaces) 中。 这是包含接口定义为主角的项目。 在接口项目可以定义的接口，将由解决方案中的主角。

```csharp

using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceFabric.Actors;

namespace HelloWorld.Interfaces
{
    public interface IHelloWorld : IActor
    {
        Task<string> SayHello(string greeting);
    }
}

```

* 服务项目 (HelloWorld)。 这是用来定义要承载主角服务结构服务的项目。 它包含一些样本代码，不需要编辑在大多数情况下 (ServiceHost.cs) 和实现中的主角。 主角的实现涉及实现派生自基类型 （使用者） 和实现接口中定义的类。接口的项目。

```csharp

using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading;
using System.Threading.Tasks;
using HelloWorld.Interfaces;
using Microsoft.ServiceFabric;
using Microsoft.ServiceFabric.Actors;

namespace HelloWorld
{
    public class HelloWorld : Actor, IHelloWorld
    {
        public Task<string> SayHello(string greeting)
        {
            return Task.FromResult("You said: '" + greeting + "', I say: Hello Actors!");
        }
    }
}

```

主角服务项目包含创建服务结构服务，在服务定义中的代码，因此它们可用于实例化新参与者注册使用者类型。

```csharp

public class Program
{
    public static void Main(string[] args)
    {
        try
        {
            using (FabricRuntime fabricRuntime = FabricRuntime.Create())
            {
                fabricRuntime.RegisterActor(typeof(HelloWorld));

                Thread.Sleep(Timeout.Infinite);
            }
        }
        catch (Exception e)
        {
            ActorEventSource.Current.ActorHostInitializationFailed(e);
            throw;
        }
    }
}  

```

如果您在 Visual Studio 中的新项目从启动，并且您有只有一个使用者定义，默认情况下，Visual Studio 生成的代码中包含注册。 如果您定义其他参与者的服务中，您需要添加参与者注册使用︰

```csharp

fabricRuntime.RegisterActor(typeof(MyNewActor));


```

## 调试

Visual Studio 的服务结构工具支持在本地计算机上进行调试。 您可以通过按 F5 启动调试会话。 Visual Studio 生成 （如果需要）、 打包和部署本地服务结构群集上的应用程序并附加调试器。 经验是类似于调试 ASP.NET 应用程序。
在部署过程中，您可以看到在输出窗口中的进度

![服务结构调试输出窗口][3]


## 下一步行动

[介绍服务结构可靠者](service-fabric-reliable-actors-introduction.md)
[演员 Api 参考文档](https://msdn.microsoft.com/library/azure/dn971626.aspx)
[示例代码](https://github.com/Azure/servicefabric-samples)


<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject.PNG
[2]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-projectstructure.PNG
[3]: ./media/service-fabric-reliable-actors-get-started/debugging-output.PNG
[4]: ./media/service-fabric-reliable-actors-get-started/vs-context-menu.png
[5]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject1.PNG

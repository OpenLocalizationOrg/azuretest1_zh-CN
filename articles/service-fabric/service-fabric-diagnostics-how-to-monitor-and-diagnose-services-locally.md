---
ms.openlocfilehash: 4cbd064fb5d2b03fd024db353b50487dc2086a85
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="Microsoft Azure 服务结构如何监视和诊断服务本地"
   description="本文介绍了如何监视和诊断服务在本地开发计算机上使用 Microsoft Azure 服务结构编写的。"
   services="service-fabric"
   documentationCenter=".net"
   authors="kunaldsingh"
   manager="samgeo"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/22/2015"
   ms.author="kunalds"/>


# 监视和诊断的服务在本地计算机的开发设置
监视、 检测、 诊断和故障排除允许服务继续对用户体验的中断降至最低。 关键在实际部署的生产环境中时，效力将取决于服务以确保其正常工作移到现实世界中安装时的开发过程中采用类似的模型。 服务结构方便服务开发人员实现无缝跨单个计算机本地开发及实际生产群集设置工作的诊断。

## 跟踪和日志记录
[Windows 事件跟踪](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx)(ETW) 是服务结构中的跟踪消息的推荐的技术。 此问题的原因是︰

* ETW 是快速的。 它是构建作为一种跟踪技术对您的代码的执行时间的影响最小。
* ETW 跟踪跨本地开发环境和真实世界群集设置也无缝工作。 这意味着您不必重新编写跟踪代码时您就可以将代码部署到真正的群集。
* 服务连接结构系统代码还对内部跟踪使用 ETW。 这使您可以查看应用程序跟踪交错在一起服务结构系统跟踪信息，使其更易于理解和序列和您的应用程序代码和基础系统中的事件之间的相互关系。
* 没有服务结构 Visual Studio 工具来查看 ETW 事件的内置支持。


## 在 Visual Studio 中查看服务结构系统事件

服务结构发出 ETW 事件可帮助应用程序开发人员了解在平台中发生了什么。 若要查看这些事件，请执行以下步骤︰

1. 您必须安装下列预 requisties。
   * Visual Studio 2015 年
   * 服务结构 SDK

2. 作为管理员启动 Visual Studio。

3. 创建 （或打开一个现有的） 有状态或无状态操作者或服务的项目。

  ![创建服务结构的应用程序](./media/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally/CreateServiceFabricProject.png)

  ![创建服务结构服务](./media/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally/CreateServiceFabricProject-2.png)

4. 按 F5 来调试应用程序。 在诊断事件窗口中应显示的服务结构事件。 每个事件都有它告诉您的节点、 应用程序和服务事件来自标准的元数据信息。 也可以使用窗口顶部的"筛选事件"框中的事件的列表，例如筛选节点名称或服务名称。

  ![Visual Studio 诊断事件查看器](./media/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally/DiagEventsExamples2.png)

5. 如果诊断事件窗口不会自动显示，请转到 Visual Studio 中服务器资源管理器选项卡，右键单击群集服务结构，并在上下文菜单中选择"查看诊断事件"。

  ![打开 Visual Studio 诊断事件查看器](./media/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally/ServerExViewDiagEvents.png)

## 应用程序代码中添加您自己的自定义跟踪
服务结构 Visual Studio 项目模板包含示例代码。 该代码演示如何添加自定义的应用程序代码 ETW 跟踪系统跟踪服务结构与 Visual Studio ETW 查看器中的显示。 此方法的优点是，元数据会自动添加到跟踪，并已配置 Visual Studio 的诊断查看器以显示它们。

（无状态或有状态） 从**服务模板**创建项目︰

1. 打开**Service.cs**文件。 对`ServiceEventSource.Current.Message`在*RunAsync*方法显示应用程序代码中的自定义 ETW 跟踪的示例。
2. 在**ServiceEventSource.cs**文件中，重载`ServiceEventSource.Message`方法显示如何编写自定义 ETW 跟踪选项。

（无状态或有状态），从**使用者模板**创建项目︰

1. 打开**"项目名称".cs**文件所在*项目名称*名称为 Visual Studio 项目选择。  
2. 查找代码`ActorEventSource.Current.ActorMessage(this, "Doing Work");` *DoWorkAsync*方法中。  这是从应用程序代码的自定义 ETW 跟踪的示例。  
3. 在文件**ActorEventSource.cs**，重载的`ActorEventSource.ActorMessage`方法显示如何编写自定义 ETW 跟踪选项。

添加自定义 ETW 跟踪到后服务代码，可以构建、 部署和运行应用程序再次看到您诊断的查看器中的事件。 如果调试应用程序，按 f5 键，则会自动打开诊断的查看器。

##即将推出
相同的跟踪代码添加到本地诊断为上述应用程序将使用工具，可用于查看在 Azure 的群集上运行相同的代码时这些事件。 此细节即将登场。

## 下一步行动

* [服务结构状况简介](service-fabric-health-introduction.md)
* [理解应用程序安装](service-fabric-diagnostics-application-insights-setup.md)
* [Azure 服务结构演员诊断和性能监视](service-fabric-reliable-actors-diagnostics.md)
* [有状态的可靠服务诊断](service-fabric-reliable-services-diagnostics.md)

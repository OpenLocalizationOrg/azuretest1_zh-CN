---
ms.openlocfilehash: 28c0555f6cb6856cdc53ea7a6a828156f27b6274
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="调试服务结构中使用 F5 Visual Studio 应用程序"
   description="提高了可靠性和使用 Visual Studio 和本地开发群集服务的性能。"
   services="service-fabric"
   documentationCenter=".net"
   authors="jessebenson"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/26/2015"
   ms.author="jesseb"/>

# 调试服务结构中使用 F5 Visual Studio 应用程序

您可以通过部署和调试服务结构应用程序在本地计算机开发群集中的节省时间和金钱。 Visual Studio 可以部署到本地群集应用程序，并自动将调试器连接到您的应用程序的所有实例。

1. 启动本地开发群集[服务结构开发环境的设置](service-fabric-get-started.md)中的步骤。

2. 按**F5**或单击**调试** > **开始调试**

    ![开始调试应用程序][startdebugging]

3. 通过单击**调试**菜单中的命令，在您的代码并单步执行应用程序中设置断点。

    > [AZURE.NOTE] Visual Studio 将附加到您的应用程序的所有实例。 逐句通过代码，同时导致并发会话的多个进程可以获取命中的断点。 请尝试被命中，使断点条件的线程 ID，或使用诊断事件之后禁用断点。

4. **诊断事件**窗口将自动打开以查看实时诊断事件。

    ![实时查看诊断事件][diagnosticevents]

5. 您还可以在服务器资源管理器中打开**诊断事件**窗口。  在**Azure**下，右击**群集服务结构** > **查看诊断事件...**

    ![打开诊断事件窗口][viewdiagnosticevents]

6. 诊断的事件自动生成**ServiceEventSource.cs**中可以看到，从应用程序代码调用。

    ```csharp
    ServiceEventSource.Current.ServiceTypeRegistered(Process.GetCurrentProcess().Id, Service.ServiceTypeName);
    ```

7. **诊断事件**窗口支持筛选、 暂停和检查实时事件。  筛选器是简单的字符串搜索的事件消息，包括其内容。

    ![筛选、 暂停和继续或检查实时事件][diagnosticeventsactions]

8. 调试服务等同于任何其他应用程序的调试。 可以设置断点通常通过 Visual Studio 易于调试。 尽管可靠的集合复制跨多个节点，它们仍实现 IEnumerable，这意味着可以使用结果视图在 Visual Studio 中调试时看到已经存储在内部。 只需设置中断点任何地方在您的代码中。

    ![开始调试应用程序][breakpoint]

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## 下一步行动

- [测试服务结构服务](service-fabric-test-your-service-index.md)。
- [管理您在 Visual Studio 中的服务结构应用程序](service-fabric-manage-application-in-visual-studio.md)。

<!--Image references-->
[startdebugging]: ./media/service-fabric-debugging-your-application/startdebugging.png
[diagnosticevents]: ./media/service-fabric-debugging-your-application/diagnosticevents.png
[viewdiagnosticevents]: ./media/service-fabric-debugging-your-application/viewdiagnosticevents.png
[diagnosticeventsactions]: ./media/service-fabric-debugging-your-application/diagnosticeventsactions.png
[断点]: ./media/service-fabric-debugging-your-application/breakpoint.png
 

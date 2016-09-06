---
ms.openlocfilehash: eb3b6c0654b7227375902ac31eba4fb519c6eb7a
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="调试 Azure 的云服务 |Microsoft Azure"
   description="调试的 Azure 的云服务"
   services="visual-studio-online"
   documentationCenter="n/a"
   authors="patshea123"
   manager="douge"
   editor="tlee" />
<tags 
   ms.service="visual-studio-online"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/14/2015"
   ms.author="patshea" />

# 调试的云服务

可以使用不同的方法为 Microsoft Visual Studio 和 Azure SDK 中使用 Azure 工具调试 Azure 应用程序︰

- 就像任何 C# 或 Visual Basic 应用程序一样，都可以开发时, 调试 Azure 应用程序从 Visual Studio。 有关详细信息，请参见[调试的云服务或在 Visual Studio 中的虚拟机](http://go.microsoft.com/fwlink/p/?LinkID=623018)。

- 您可以使用 Azure 诊断无论角色运行在开发环境中或在 Azure 角色内运行的代码记录的详细的信息。 有关详细信息，请参阅[收集记录使用 Azure 诊断数据](http://go.microsoft.com/fwlink/p/?LinkId=400450)。

- 如果使用的 Visual Studio 企业编写面向.NET Framework 4 或.NET Framework 4.5 的角色，您可以在您部署从 Visual Studio Azure 的云服务时启用 IntelliTrace。 IntelliTrace 提供日志，可以使用 Visual Studio 中使用，好像它运行在 Azure 中调试应用程序。 有关详细信息，请参见[调试 Visual Studio IntelliTrace 与已发布的云服务]( http://go.microsoft.com/fwlink/p/?LinkId=623016)。

- 您可以在 Visual Studio 中的云服务的部署时的时间在云服务上的远程调试。 如果您选择启用远程调试的部署，运行每个角色实例的虚拟机上安装远程调试服务。 这些服务，例如 msvsmon.exe，不影响性能或导致额外的成本。 有关详细信息，请参见[调试的云服务或在 Visual Studio 中的虚拟机](http://go.microsoft.com/fwlink/p/?LinkID=623018)。




测试

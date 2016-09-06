---
ms.openlocfilehash: ca53788ba8dbf1cd06d9243c1803760f97a32561
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="什么是 Azure 自动化"
    description="了解 Azure 自动化提供了什么价值并获取常见问题的答案，以便您可以开始创建和使用运行手册。"
    services="automation"
    documentationCenter=""
    authors="bwren"
    manager="stevenka"
    editor=""/>

<tags
    ms.service="automation"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="07/06/2015"
    ms.author="bwren"/>

# 什么是 Azure 自动化？

Microsoft Azure 自动化提供用户自动化云和企业环境中常用的手动、 运行时间较长、 易出错，而且经常重复任务的方法。  本文提供了有关 Azure 自动化常见的问题的简要解答。  您可以参考有关不同主题的更多详细信息此库中的其他文章。

## Azure 自动化提供了什么价值？

Azure 自动化可以节省时间并提高了您在云中和企业环境中执行的常规管理任务的可靠性。  您可以为运行手册可以执行多项任务，无需人工干预，甚至安排他们在固定的时间间隔自动执行实现这些进程。   

## Runbook 是什么？

Runbook 是一套在 Azure 自动化执行某些自动化的进程的任务。  它可能是一个简单的过程，如启动虚拟机或创建日志项，或可能有复杂的 runbook 相结合其他较小的运行手册，若要跨越多个资源或更多个群执行一个复杂的过程。   

例如，可能有配置新的虚拟机，其中包括多个步骤，如创建虚拟机，将其连接到网络、 为其分配 IP 地址，然后通知用户就可以现有的手动过程。  而不是手动执行每个步骤，您可以创建将作为一个单独的进程中执行所有这些任务 runbook。  将启动 runbook、 提供所需的信息，如虚拟机名称、 IP 地址和收件人电子邮件，然后坐在过程完成而无需您的同时。 


## 运行手册可以自动化？

在 Azure 自动化运行手册基于 PowerShell 的工作流，因此它们执行 PowerShell 可以执行的任何操作。  如果应用程序或服务有一个 API，runbook 可以使用它。  如果它有 PowerShell 模块，您可以将该模块加载到 Azure 自动化并在您 runbook 中包含这些 cmdlet。  这样，才可以访问云中的任何资源或可从云中的外部资源，azure 的自动化运行手册在 Azure 云运行。  使用[混合 Runbook 工作人员](automation-hybrid-runbook-worker.md)，可在您的本地数据中心管理的本地资源运行运行手册。


## 从哪里获得运行手册？

[Runbook 库](http://msdn.microsoft.com/library/azure/dn781422.aspx)包含来自 Microsoft 的运行手册，社区可以使用在您的环境不变，或自行定制它们以自己的目的。  它们也是用作参考以了解如何创建您自己的运行手册。 甚至可以参与到库，您认为其他用户可能会发现有用自己运行手册。


## 如何创建自定义运行手册？

您可以从头[创建自己的运行手册](http://msdn.microsoft.com/library/azure/dn643637.aspx)或修改运行手册从[Runbook 库](http://msdn.microsoft.com/library/azure/dn781422.aspx)对自己的要求。  如果想直接使用 PowerShell 代码，您可以在 Azure 门户或编辑如果脱机[编辑 runbook 使用文本编辑器](http://msdn.microsoft.com/library/azure/dn879137.aspx)。  如果您更喜欢不公开的基础代码的情况下编辑 runbook，然后可以在 Azure 预览门户中使用[图形编辑器](automation-graphical-authoring-intro.md)。


## Azure 自动化与其他自动化工具关系？

[服务管理自动化 (SMA)](http://technet.microsoft.com/library/dn469260.aspx)被用于自动处理在私有云的管理任务。  它安装在本地数据中心中的[Windows Azure 包](http://www.microsoft.com/server-cloud/products/windows-azure-pack/default.aspx)某一组件。 SMA 和 Azure 自动化使用相同的 runbook 格式根据 Windows PowerShell 工作流，但是 SMA 不支持[图形化运行手册](automation-graphical-authoring-intro.md)。 

[System Center 2012 控制器](http://technet.microsoft.com/library/hh237242.aspx)适用于自动化的内部资源。 它使用不同的 runbook 格式比 Azure 自动化和服务管理自动化和图形化的界面，而无需任何脚本创建运行手册。 其运行手册组成专门为控制器编写的集成包中的活动。 

## 从哪里可以获得更多的信息？

各种资源都可供您了解更多有关 Azure 自动化和创建您自己的运行手册。

- **Azure 自动化库**是您是现在。  此库中的项目的配置和管理 Azure 自动化和创作您自己的运行手册提供了完整的文档。
- [Azure PowerShell cmdlet](http://msdn.microsoft.com/library/jj156055.aspx)提供自动化使用 Windows PowerShell 的 Azure 运营的信息。  运行手册使用这些 cmdlet 使用 Azure 的资源。
- [Azure 自动化博客](http://azure.microsoft.com/blog/tag/azure-automation)从 Microsoft Azure 自动化提供的最新信息。  您应该订阅此博客保持最新从 Azure 自动化团队最新的。
- [自动化论坛](http://go.microsoft.com/fwlink/p/?LinkId=390561)可以发布 Azure 自动化由 Microsoft 和自动化社区来解决有关问题。

## 我可以提供反馈？

**请给我们反馈 ！**  如果您正在寻找一个 Azure 自动化 runbook 解决方案或集成模块，请在脚本中心上发布脚本请求。 如果您有反馈或功能请求的 Azure 自动化，则在[用户语音](http://feedback.windowsazure.com/forums/34192--general-feedback)上发布它们。 谢谢 ！

测试

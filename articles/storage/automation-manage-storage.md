---
ms.openlocfilehash: 4b63f5a02784ba7cf6e259eb459573fdc731663e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="管理 Azure 存储使用 Azure 自动化"
    description="了解如何使用 Azure 自动化服务管理 Azure 存储在规模较大。"
    services="storage, automation"
    documentationCenter=""
    authors="jodoglevy"
    manager="eamono"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2015"
    ms.author="jolevy"/>



#Azure 存储使用 Azure 自动化管理

本指南将向您介绍 Azure 自动化服务，以及如何使用它来简化的 Azure 存储 blob、 表和队列管理。


## 什么是 Azure 自动化？

[Azure 自动化](http://azure.microsoft.com/services/automation/)是简化了流程自动化通过云管理 Azure 服务。 使用 Azure 的自动化功能，长时间运行、 手动、 易出错，而且经常重复的任务可以自动执行以提高可靠性、 效率和时间值为您的组织。

Azure 的自动化提供了一个具有高可靠性和高可用性的工作流执行引擎，可以进行扩展以满足您的需求，随着组织的成长。 在 Azure 自动化进程可以是开始时间手动，由第三方系统，或在计划的时间间隔以便任务发生所需的确切时间。

降低操作开销和释放 IT / DevOps 员工将精力集中在工作中添加业务价值通过移动云管理任务由 Azure 自动化中自动运行。


## Azure 自动化如何帮助管理 Azure 存储？

可以通过使用[Azure PowerShell 工具](https://msdn.microsoft.com/library/azure/jj156055.aspx)中可用的 PowerShell cmdlet Azure 自动化中管理 azure 存储。 Azure 自动化开箱即用，已可用这些存储 PowerShell cmdlet，以便可以执行所有您的 blob、 表和队列服务中的管理任务。 此外可以对 Azure 自动化中使用这些 cmdlet 使用的 cmdlet 的其他 Azure 服务，能够跨 Azure 服务和第三方系统自动化复杂的任务。

[Azure 自动化 runbook 库](http://azure.microsoft.com/blog/2014/10/07/introducing-the-azure-automation-runbook-gallery/)包含多种产品小组和社区运行手册，开始自动管理 Azure 存储、 其他 Azure 服务和第三方系统。 库运行手册包括︰

 * [删除早于 X 天的 Azure 存储 blob](https://gallery.technet.microsoft.com/scriptcenter/Remove-Storage-Blobs-that-aae4b761)
 * [下载从 Azure 存储 blob，Azure 自动化](https://gallery.technet.microsoft.com/scriptcenter/a-Blob-from-Azure-Storage-6bc13745)
 * [在 Azure 云服务中创建 Azure VM 数据磁盘的副本](https://gallery.technet.microsoft.com/scriptcenter/Make-copies-of-Azure-VM-065a6394)


## 下一步行动

既然已经学习了 Azure 自动化以及如何使用它来管理 Azure 存储 blob、 表和队列的基础知识，请按照这些链接以了解更多有关 Azure 自动化。

请参阅 Azure 自动化[快速入门教程](../automation-create-runbook-from-samples.md)
 

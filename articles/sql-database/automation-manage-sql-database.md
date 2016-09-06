---
ms.openlocfilehash: 127e63cd57f031404c2d639e6ac1ef3ca6faa264
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="管理 Azure SQL 数据库使用 Azure 自动化"
    description="了解如何使用 Azure 自动化服务管理在规模较大的 SQL Azure 数据库。"
    services="sql-database, automation"
    documentationCenter=""
    authors="jodoglevy"
    manager="jeffreyg"
    editor="monicar"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2015"
    ms.author="jolevy"/>



#管理 Azure SQL 数据库使用 Azure 自动化

本指南将向您介绍 Azure 自动化服务，以及如何使用它来简化 SQL Azure 数据库的管理。


## 什么是 Azure 自动化？

[Azure 自动化](http://azure.microsoft.com/services/automation/)是简化了流程自动化通过云管理 Azure 服务。 使用 Azure 的自动化功能，长时间运行、 手动、 易出错，而且经常重复的任务可以自动执行以提高可靠性、 效率和时间值为您的组织。

Azure 的自动化提供了一个具有高可靠性和高可用性的工作流执行引擎，可以进行扩展以满足您的需求，随着组织的成长。 在 Azure 自动化进程可以是开始时间手动，由第三方系统，或在计划的时间间隔以便任务发生所需的确切时间。

降低操作开销和释放 IT / DevOps 员工将精力集中在工作中添加业务价值通过移动云管理任务由 Azure 自动化中自动运行。


## Azure 自动化如何帮助管理 SQL Azure 数据库？

可以通过使用[Azure PowerShell 工具](https://msdn.microsoft.com/library/azure/jj156055.aspx)中可用的[Azure SQL 数据库 PowerShell cmdlet](https://msdn.microsoft.com/library/azure/dn546726.aspx) Azure 自动化中管理 azure 的 SQL 数据库。 Azure 自动化开箱即用，已可用这些 Azure SQL 数据库 PowerShell cmdlet，以便您可以执行所有 SQL 数据库管理任务，在服务中。 此外可以对 Azure 自动化中使用这些 cmdlet 使用的 cmdlet 的其他 Azure 服务，能够跨 Azure 服务和第三方系统自动化复杂的任务。

Azure 的自动化也有与 SQL 服务器直接发出 SQL 命令使用 PowerShell 通信的能力。

[Azure 自动化 runbook 库](http://azure.microsoft.com/blog/2014/10/07/introducing-the-azure-automation-runbook-gallery/)包含多种产品小组和社区运行手册，开始自动管理 Azure SQL 数据库、 其他 Azure 服务和第三方系统。 库运行手册包括︰

 * [对 SQL Server 数据库运行 SQL 查询](https://gallery.technet.microsoft.com/scriptcenter/How-to-use-a-SQL-Command-be77f9d2)
 * [垂直扩展 （向上或向下） 按计划 Azure SQL 数据库](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-e957354f)
 * [截断的 SQL 表，如果其数据库接近其最大大小](https://gallery.technet.microsoft.com/scriptcenter/Azure-Automation-Your-SQL-30f8736b)
 * [SQL Azure 数据库中的表的索引，如果它们大量的碎片](https://gallery.technet.microsoft.com/scriptcenter/Indexes-tables-in-an-Azure-73a2a8ea)

## 下一步行动

现在，您已经学习了 Azure 自动化以及如何使用它来管理 SQL Azure 数据库的基本知识，按照这些链接以了解更多有关 Azure 自动化。

 * 请参阅 Azure 自动化[快速入门教程](../automation-create-runbook-from-samples.md)
 * 读[Azure 自动化︰ 您在云中的 SQL 代理](http://azure.microsoft.com/blog/2014/06/26/azure-automation-your-sql-agent-in-the-cloud/)博客张贴内容
 

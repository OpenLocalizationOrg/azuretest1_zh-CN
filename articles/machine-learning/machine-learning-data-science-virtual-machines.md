---
ms.openlocfilehash: afe294e8238543a8309e3d2d4c9e33493960f62d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="在 Azure 数据科学虚拟机 |Microsoft Azure"
    description="设置数据科学虚拟 Machinee"
    metaKeywords=""
    services="machine-learning"
    solutions=""
    documentationCenter=""
    authors="msolhab"
    manager="paulettm" 
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2015"
    ms.author="mohabib;xibingao;bradsev" />

# 在 Azure 数据科学虚拟机

可以设置并配置为基于云的数据科学环境的一部分用作 Azure 的虚拟机的几种类型。 有关的决策的风格虚拟机的使用取决于要进行建模与机器学习和数据在云中的目标的类型和数量的数据。 


* 在作出此项决定时需要考虑的问题的指南，请参阅[规划 Azure 机器学习数据科学环境](machine-learning-data-science-plan-your-environment.md)。 
* 在某些情形下进行高级分析功能时可能遇到的目录，请参阅[高级的分析过程和 Azure 机器学习的技术方案](../machine-learning-data-science-plan-sample-scenarios.md)

描述如何设置 Azure VM 和 Azure VM 与 SQL 服务作为 IPython 笔记本服务器提供了说明。 Windows 虚拟机配置支持 IPython 笔记本，Azure 存储浏览器和 AzCopy，以及其他实用程序，可用于数据科学项目的工具。 Azure 存储浏览器和 AzCopy，例如，提供了便捷的方法，以将数据上载到 Azure 存储中，从您的本地计算机或将其下载到本地计算机存储中。 提供了两组说明︰

* [设置 Azure 的虚拟机作为 IPython 笔记本服务器高级分析功能](machine-learning-data-science-setup-virtual-machine.md)演示如何 IPython 笔记本和其他工具用于执行操作的一种形式的非 SQL Azure 存储可用于存储数据的情况下的数据科学调配 Azure 的虚拟机。

* [设置 Azure SQL Server 虚拟机作为 IPython 笔记本服务器高级分析功能](machine-learning-data-science-setup-sql-server-virtual-machine.md)演示如何 IPython 笔记本和其他工具用于执行操作的情况下，可以使用 SQL 数据库来存储数据的数据科学调配 Azure SQL Server 虚拟机。

已设置并配置后，这些虚拟机的形式供 IPython 笔记本服务器，勘探和处理数据，并结合 Azure 机器学习和云数据科学过程中需要执行其他任务。 数据科学过程中的下一个步骤映射中[学习指南︰ 高级 Azure 中的数据处理](machine-learning-data-science-advanced-data-processing.md)，并可能包括移到 SQL Server 或 HDInsight，数据处理和示例不准备使用 Azure 机器学习数据中学习过程中的步骤。


> [AZURE.NOTE] Azure 的虚拟机进行定价作为**支付仅为您的使用**。 若要确保，您不被计入在不使用虚拟机，它具有从[Azure 管理门户](http://manage.windowsazure.com/)**已停止 (Deallocated)**状态。 有关分步说明如何释放您的虚拟机，请参阅[关闭并释放在不使用时虚拟机](machine-learning-data-science-setup-virtual-machine.md#shutdown)
 
测试

---
ms.openlocfilehash: d08ede47c0f32aa2524e2ce1722c8fcfbd7e8f28
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="从控制器迁移到 Azure 自动化 |Microsoft Azure"
   description="描述如何将系统中心控制器运行手册和集成包迁移到 Azure 自动化。"
   services="automation"
   documentationCenter=""
   authors="bwren"
   manager="stevenka"
   editor="tysonn" />
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/18/2015"
   ms.author="bwren" />


# 从控制器迁移到 Azure 的自动化

在[系统中心控制器](http://technet.microsoft.com/library/hh237242.aspx)运行手册根据活动虽然是在 Azure 自动化运行手册基于 Windows PowerShell 工作流编写专门为控制器的集成包。  Azure 自动化中的图形化运行手册有控制器运行手册到类似的外观与他们表示 PowerShell cmdlet、 子运行手册和资产的活动。

[系统中心控制器迁移 Toolkit](http://www.microsoft.com/download/details.aspx?id=47323&WT.mc_id=rss_alldownloads_all)包括工具来帮助您从通讯到 Azure 自动化转换运行手册。  除了转换本身运行手册，必须转换为与 Windows PowerShell cmdlet 的集成模块的集成包与使用它们的活动。  

下面是转换到 Azure 自动化控制器运行手册的基本过程。  以下各节将详细介绍上述每个步骤。

1.  下载该[系统中心控制器迁移 Toolkit](http://www.microsoft.com/download/details.aspx?id=47323&WT.mc_id=rss_alldownloads_all)包含的工具和本文中讨论的模块。
2.  [标准活动模块](#standard-activities-module)安装到 Azure 的自动化。  这包括可能由转换运行手册的标准通讯活动的转换的版本。
2.  通过运行手册使用这些集成包安装到 Azure 自动化的[系统中心控制器集成模块](#system-center-orchestrator-integration-modules)。
3.  转换使用[集成包转换器](#integration-pack-converter)的自定义和第三方集成包并安装在 Azure 自动化中。
4.  由于没有自动的方法来执行此迁移，则手动重新创建在 Azure 自动化控制器中的全球资产。
5.  转换控制器运行手册使用[Runbook 转换器](#runbook-converter-coming-soon)（即将提供） 和安装在 Azure 自动化中。
6.  在您的本地数据中心运行转换运行手册配置[混合 Runbook 工作人员](#hybrid-runbook-worker)。

## 服务管理自动化

[服务管理自动化](http://technet.microsoft.com/library/dn469260.aspx)(SMA) 存储和运行控制器，如您的本地数据中心中运行手册，它将 Azure 自动化使用相同的集成模块。  可用时， [Runbook 转换器](#runbook-converter-coming-soon)会将控制器运行手册为图形化运行手册虽然在 SMA 中不受支持的。  您仍可以安装的[标准活动模块](#standard-activities-module)和[系统中心控制器集成模块](#system-center-orchestrator-integration-modules)到 SMA，但是您必须手动[重新编写运行手册](http://technet.microsoft.com/library/dn469262.aspx)。

## 混合的 Runbook 工作人员

在控制器中运行手册都存储在数据库服务器上并运行 runbook 在服务器上，同时在您的本地数据中心中。  在 Azure 自动化运行手册在 Azure 云存储中，可以使用[混合 Runbook 辅助](automation-hybrid-runbook-worker.md)本地数据中心中运行。  这是您通常的运行方式转换控制器中，因为它们被设计为运行在本地服务器上运行手册。

## 集成包转换器

集成包转换器转换集成包使用基于 Windows PowerShell 可以导入到 Azure 自动化或服务管理自动化的集成模块到控制器集成 Toolkit (OIT) 创建的。  

集成包转换器运行时，系统将显示一个向导，它将允许您选择一个集成软件包 (.oip) 文件中。  然后，向导列出了该集成包中包含的活动，并允许您选择将迁移。  在完成向导时，它将创建该模块中包含原始的集成包中的活动的每个对应的 cmdlet。


### 参数

集成包中的活动的任何属性都转换为相应的 cmdlet 集成模块中的参数。  Windows PowerShell cmdlet 都有一套可用于所有的 cmdlet 的[通用参数](http://technet.microsoft.com/library/hh847884.aspx)。  例如，-详细参数会导致用于输出它的操作的详细的信息。  没有 cmdlet 可能必须具有公共的参数相同的名称的参数。  如果一个活动有带有公共参数相同的名称的属性，向导将提示您提供该参数的另一个名称。

### 监视活动

控制器中的监视器运行手册与[监视活动](http://technet.microsoft.com/library/hh403827.aspx)启动和运行持续等待被调用的特定事件。  Azure 自动化不支持监视器运行手册，因此将不会转换任何集成包中的监视活动。  相反，监视活动的集成模块中创建占位符 cmdlet。  此 cmdlet 都有任何功能，但它允许使用它来安装任何转换后的 runbook。  此 runbook 将不能运行在 Azure 自动化，但它可以安装，以便用户可以对其进行修改。

### 不能转换的集成包

无法使用此工具转换集成包不通过 OIT，包括一些由 Microsoft 创建的。  由 Microsoft 提供的集成包已转换为集成模块，以便它们可以在 Azure 自动化或服务管理自动化安装。


## 标准活动模块

控制器包含一组[标准的活动](http://technet.microsoft.com/library/hh403832.aspx)，不包括在集成包，而由许多运行手册。  标准活动模块是为每种活动包括等效 cmdlet 的集成模块。  在导入任何转换的运行手册，使用标准的活动之前，必须在 Azure 自动化安装此集成模块。

除了支持转换的运行手册，标准活动模块中的 cmdlet 可以用于被人熟悉控制器生成运行在 Azure 自动化的新手册。  使用 cmdlet 可以执行所有标准的活动的功能，而他们可能会以不同的方式运行。  在已转换的标准活动模块的 cmdlet 将工作及其相应的活动相同并使用相同的参数。  这可以帮助他们过渡到 Azure 自动化运行手册中现有的控制器 runbook 作者。

## 系统中心控制器集成模块
Microsoft 提供了用于生成运行手册来自动化系统中心组件和其他产品的[集成包](http://technet.microsoft.com/library/hh295851.aspx)。  目前，在从[TechNet](http://www.microsoft.com/download/details.aspx?id=39622)下载一些这些集成包时，它们不能与已知问题，将解决与 RC 版本的系统中心控制器迁移 Toolkit 由于集成包转换器转换。  [系统中心控制器集成模块](http://www.microsoft.com/download/details.aspx?id=47324&WT.mc_id=rss_alldownloads_all)包括以前的发行版在 Azure 自动化和服务管理自动化功能可以导入这些集成包的转换的版本。

## Runbook 转换器 （即将提供）

此工具会将控制器运行手册转换图形可以导入到 Azure 自动化的运行手册。  可用时将此处提供此工具的详细信息。

## 相关的文章

- [System Center 2012-控制器](http://technet.microsoft.com/library/hh237242.aspx)
- [服务管理自动化](https://technet.microsoft.com/library/dn469260.aspx)
- [混合的 Runbook 工作人员](automation-hybrid-runbook-worker.md)
- [通讯标准活动](http://technet.microsoft.com/library/hh403832.aspx)
 

测试

---
ms.openlocfilehash: edb9333c185999a13851209318f91430dcee4d12
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="Azure 批处理技术概述 |Microsoft Azure"
    description="了解有关概念、 工作流程和 Azure 批服务的方案"
    services="batch"
    documentationCenter=""
    authors="dlepow"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.workload="big-compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/13/2015"
    ms.author="danlep"/>


# Azure 批处理技术概述
Azure 批可帮助您运行大规模的并行和高性能计算 (HPC) 有效地在云中的应用程序。 它是一个提供作业级排产的平台服务和托管的虚拟机 (Vm) 要运行的作业集合的自动缩放。 通过使用批处理服务，您可以配置批处理工作负载运行在 Azure，按需或按照日程安排，并不担心配置和 HPC 群集、 虚拟机或作业计划程序管理的复杂性。

>[AZURE.NOTE] 若要使用批处理，您需要 Azure 帐户。 如果您没有帐户，您可以在几分钟创建免费的试用帐户。 有关详细信息，请参阅[创建 Azure 帐户](http://azure.microsoft.com/develop/php/tutorials/create-a-windows-azure-account/)。


## 使用案例

批次使用弹性和云的比例以帮助您进行*批次处理*或*批处理计算*的运行大量的类似任务，才能获得一些所需的结果。 命令行程序或脚本采用一套数据文件作为输入、 处理的数据在一系列的任务，并产生一组输出文件。 输出文件可能的最终结果或更大的工作流程中的一个中间步骤。

批处理计算是一种常见模式对于处理、 转换和分析大量的数据，或者定期或按需组织。 它包括结束的周期处理功能，如银行的每日风险报告或工资必须按时完成。 它还包括大规模商业、 科学和工程应用程序通常需要的工具和资源的计算群集或网格。 应用程序域范围从数字内容创建到金融服务，为生命科学研究中包括传统 HPC 应用程序，如流体动力学模拟，以及专门工作负载。

批次本质上适用于 （有时称为"embarrassingly 并行"） 的并行应用程序或工作负载，同样适用于多台计算机，如计算由批服务管理的虚拟机上运行尽可能并行任务。

![并行任务][parallel]

**图 1。 在多台计算机上运行的并行任务**

示例包括︰

* 财务风险建模
* 图像呈现和图像处理
* 媒体编码和转码
* 基因序列分析
* 软件测试

您可以使用批处理并行计算在结束时和其他更复杂的并行工作负荷减少步骤。

>[AZURE.NOTE]这一次只可以批次运行 Windows 服务器工作负载。 此外，批目前不支持消息传递接口 (MPI) 的应用程序。

## 开发人员方案

批次支持不同的开发人员方案，以帮助您配置和运行您的大规模并行工作负载与批服务。 这些方案使用 Api 来创建和管理虚拟机 （计算节点） 的池和安排在其运行的任务和作业。 有关批处理概念的详细信息，请参阅[Azure 批处理 API 基础知识](batch-api-basics.md)。

在下面的部分是典型批次开发人员方案。

### 并行工作负载扩展

使用批处理 API 来扩张等多达数千个计算内核池上的图像呈现的本质上并行工作。 而不是设置计算群集或写代码到队列并安排您的作业，您移动所需的输入和输出数据自动化的大型计算作业计划编制和扩展计算 Vm 向上和向下运行它们的池。 您可以编写客户端应用程序或前端运行作业和任务需求，按照日程安排，或作为管理工具，例如[Azure 数据工厂](https://azure.microsoft.com/documentation/services/data-factory/)较大流的一部分。

图 2 显示要提交到批处理池何处分发进行处理的应用程序的简化工作流。

![工作项工作流][work_item_workflow]

**图 2。 批次平行工作负载扩展**

1.  上载到 Azure 存储帐户作业所需的输入的文件 （如源数据或图像）。 这些文件必须存储帐户，以便批服务可以访问它们。 在任务运行时，批处理服务加载到计算节点上的文件。
2.  将相关的二进制文件上载到的存储帐户。 二进制文件包含任务和依赖程序集运行的程序。 这些文件还必须从存储访问并被加载到计算节点上。
3.  创建池的计算节点，指定属性，例如其 VM 大小和它们所运行的操作系统。 您还可以定义如何在池中的节点数进行扩展向上或向下对工作负荷的响应。 任务运行时，它从该池中分配一个节点。
4.  定义要在池上运行的作业。
5.  将任务添加到该作业中。 每个任务使用从您上载的文件上载过程的信息的程序。
6.  运行应用程序并监视输出的结果。


### 云-启用计算密集型应用程序

可以使用预览批处理应用程序 API 来包装现有的应用程序，以便它在池的批管理背景中的计算节点上作为服务运行。 应用程序可能运行在客户端工作站上的今天或计算群集。 您可以开发的服务，以使用户可以将转移到云，峰值工作或完全在云环境中运行他们的工作。 批处理应用程序框架处理输入和输出文件拆分为多个任务的作业、 作业和任务处理和数据持久性的移动。

>[AZURE.IMPORTANT] Azure 只提供了批处理应用程序 API 中预览表单。 只应该测试或概念证明项目的开发与它。 关键能力在将来集成到批处理 API 的批处理应用程序服务的版本。

图 3 显示了基本的工作流，通过使用批处理应用程序 API 发布应用程序，然后允许用户将作业提交到应用程序。

![应用程序发布工作流][app_pub_workflow]

**图 3。 工作流发布和批处理应用程序与运行应用程序**

1.  准备**应用程序图像**-您现有的应用程序可执行文件的 zip 文件和任何所需的支持文件。 这些可能是您在传统的服务器场或群集中运行相同的可执行文件。
2.  创建 zip 文件的**云程序集**的调用和派单记录批服务到您的工作负载。 这包含两个组件︰

    一。 **作业分隔**-将作业分成可以单独处理的任务。 例如，在动画方案中，作业拆分器将影片呈现求职并将其分成各个帧。

    b。 **任务处理器**-调用给定任务的可执行文件的应用程序。 例如，在动画方案中，任务处理器将调用呈现程序来呈现指定任务的单个框架。

3.  批处理应用程序 API 或开发人员工具用于上载到 Azure 存储帐户前两个步骤中所准备的 zip 文件。 这些文件必须存储帐户，以便批服务可以访问它们。 通常这是一次每个应用程序，服务管理员。
4.  提供一种方法，以将作业提交到 Azure 中的启用的应用程序服务。 这可能是在您的应用程序用户界面、 web 门户或无人参与的服务作为后端系统的一部分的插件。

    若要运行作业︰

    一。 输入的文件上载 （如源数据或图像） 特定于用户的作业。 这些文件必须存储帐户，以便批服务可以访问它们。

    b。 提交作业所需的参数和文件的列表。

    c。 通过使用 Api 或批处理应用程序门户监视作业进度。



## <a id="BKMK_Account">批次帐户</a>
您需要创建一个或多个唯一**批帐户**以使用和开发与批服务。 对批服务所做的所有请求必须使用帐户名称和访问键进行身份都验证。

您可以创建批处理帐户和管理在 Azure 预览门户或使用[批处理 PowerShell cmdlet](batch-powershell-cmdlets-get-started.md)的帐户的访问键。

在门户中创建一个批次的帐户︰

1. 登录到[Azure 预览门户](https://portal.azure.com)。

2. 单击**新建** > **计算** > **市场** > **的所有内容**，然后在搜索框中输入*批次*。

    ![在市场上的批次][marketplace_portal]

3. 在搜索结果中，单击**批服务**，然后单击**创建**。

4. 在**新批帐户**刀片式服务器，输入以下信息︰

    一。 在**帐户名称**中，输入批处理帐户 URL 中使用的唯一名称。

    >[AZURE.NOTE] 批帐户名称必须是唯一的 Azure，介于 3 和 24 个字符，并使用小写字母和数字。

    b。 单击要选择的科目，现有资源组的**资源组**，或创建一个新。

    c。 如果您有多个订阅，请单击以选择将在其中创建该帐户可用的订阅的**订阅**。

    d。 在**位置**中选择批次可 Azure 地区。

    ![创建一个批处理帐户][account_portal]

5. 单击**创建**以完成创建帐户。


创建帐户之后，您可以在门户管理访问键和其他设置发现。 例如，请单击钥匙图标来管理访问键。

![批次帐户密钥][account_keys]

## 其他资源

* [学习如何使用 Azure 批库.net](batch-dotnet-get-started.md)
* [Azure 批次开发库和工具](batch-development-libraries-tools.md)
* [Azure 批 REST API 参考](http://go.microsoft.com/fwlink/p/?LinkId=517803)
* [Azure 的批处理应用程序其余部分 API 参考](http://go.microsoft.com/fwlink/p/?LinkId=517804)

[并行]: ./media/batch-technical-overview/parallel.png
[marketplace_portal]: ./media/batch-technical-overview/marketplace_batch.PNG
[account_portal]: ./media/batch-technical-overview/batch_acct_portal.png
[account_keys]: ./media/batch-technical-overview/account_keys.PNG
[work_item_workflow]: ./media/batch-technical-overview/work_item_workflow.png
[app_pub_workflow]: ./media/batch-technical-overview/app_pub_workflow.png

测试

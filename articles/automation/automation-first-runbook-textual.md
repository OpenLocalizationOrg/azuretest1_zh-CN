---
ms.openlocfilehash: dec40a5a4d0ed2a0c59a9e00c428dab9db8b4e4e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="我 Azure 自动化中的第一个文本 runbook |Microsoft Azure"
    description="引导您完成创建、 测试和发布的简单文本 runbook，使用 PowerShell 流的教程。  几个概念涵盖如对 Azure 的资源进行身份验证和输入参数。"
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
    ms.topic="get-started-article" 
    ms.date="08/18/2015"
    ms.author="bwren"/>


# 我的第一个文本 runbook

> [AZURE.SELECTOR]
- [图形](automation-first-runbook-graphical.md)
- [文本](automation-first-runbook-textual.md)

本教程将引导您完成创建的[文本的 runbook](automation-powershell-workflow.md) Azure 自动化中。  我们将从简单的 runbook，我们将测试并发布，同时我们解释如何跟踪 runbook 作业的状态开始。  然后我们将修改 runbook 实际管理 Azure 的资源，在这种情况下启动 Azure 的虚拟机。  我们然后将 runbook 更可靠加上 runbook 参数。  

## 先决条件

若要完成本教程，您需要。

- Azure 的订阅。 如果您没有一个，您可以[激活您的 MSDN 订户权益](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)或<a href="/pricing/free-trial/" target="_blank">[注册免费试用版](http://azure.microsoft.com/pricing/free-trial/)。
- [自动化的帐户](automation-configuring.md)来保存 runbook。
- Azure 的虚拟机。  我们将停止并启动该计算机，因此它不应是生产。
- [Azure Active Directory 用户和自动化凭据资产](automation-configuring.md)对 Azure 的资源进行身份验证。  此用户必须有权启动和停止虚拟机。

## 步骤 1-创建新 runbook

我们首先创建输出文本*Hello World*简单 runbook。

1. 在 Azure 预览门户中，打开您自动化的帐户。  
自动化帐户页面使此帐户中快速查看的资源。  您应该已经有些资产。  这些大多数都是自动被纳入一个新的自动化帐户模块。  此外应具有凭据资产所述[先决条件](#prerequisites)。
2. 单击打开列表运行手册，**运行手册**方块。<br>
![运行手册控制](media/automation-first-runbook-textual/runbooks-control.png)
2. 通过单击**添加 runbook**按钮，然后**创建新的 runbook**上创建新的 runbook。
3. 为 runbook *MyFirstRunbook 文本*名称。
4. 在这种情况下，我们将创建基于 PowerShell 流[文本 runbook](automation-powershell-workflow.md)是**Runbook**类型︰ 选择**Powershell 工作流**。<br>
![新 runbook](media/automation-first-runbook-textual/new-runbook.png)
5. 单击**创建**以创建 runbook，然后打开文本编辑器中。

## 步骤 2-将代码添加到 runbook

您可以将任一类型代码直接到 runbook，或可以从库控件选择 cmdlet、 运行手册及资产，并将它们添加到任何相关的参数与 runbook。  在本演练中，我们将直接键入 runbook。

1. 我们 runbook 是当前为空，只需要的*工作流*关键字，我们的 runbook 和大括号，将所采用的整个工作流的名称。 <br>
![运行手册控制](media/automation-first-runbook-textual/empty-runbook.png)
2. 类型*写入输出"Hello 世界。"* 在大括号。 <br>
![世界您好](media/automation-first-runbook-textual/hello-world.png)
3.   通过单击**保存**保存 runbook。<br>
![保存 runbook](media/automation-first-runbook-textual/runbook-edit-toolbar-save.png)

## 第 3 步 – 测试 runbook

我们发布了 runbook，以使其可在生产环境中之前，我们希望其进行测试以确保它运行正常。  Runbook 测试时，运行其**草稿**版本，并以交互方式查看相应的输出。  
 
2. 单击**测试窗格中**打开测试窗格。<br>
![测试窗格](media/automation-first-runbook-textual/runbook-edit-toolbar-test-pane.png)
2. 单击**启动**以启动测试。  这应该是唯一已启用的选项。
3. 将创建一个[runbook 作业](automation-runbook-execution)，并显示其状态。  
作业状态将开始为*排队*，表明它正在等待云的 runbook 工作人员来提供。  它会再移到*开始*时工作人员声称该作业，然后*运行*runbook 实际启动运行时。  
4. Runbook 作业完成时将显示其输出。  在我们的例子中，我们看到*Hello World*。<br>
![世界您好](media/automation-first-runbook-textual/test-output-hello-world.png)
5. 关闭测试窗格返回到画布上。

## 步骤 4-发布和启动 runbook

我们刚刚创建的 runbook 仍处于草稿模式。 我们需要发布它，我们才能在生产环境中运行它。  当 runbook 发布时，您覆盖现有的已发布版本的草稿版本。  在我们的例子中，我们已发布版本还没有因为我们刚刚创建的 runbook。 

1. **发布**发布 runbook 单击，然后单击**是**时出现提示。<br>
![发布](media/automation-first-runbook-textual/runbook-edit-toolbar-publish.png)
2. 如果您向左滚动以现在**运行手册**窗格中查看 runbook，它将显示**已发布**的**创作状态**。
3. 回滚到右侧以查看**MyFirstRunbook 文本**窗格。  
顶部的选项使我们能够启动 runbook，它在将来某一时间开始安排或创建[webhook](automation-webhooks.md) ，以便可以启动通过 HTTP 调用。 
4. 我们只想开始 runbook 因此**开始**单击，然后单击**是**时出现提示。<br>
![启动 runbook](media/automation-first-runbook-textual/runbook-toolbar-start.png)
5. 我们刚刚创建的 runbook 作业就会打开作业窗格。  我们可以关闭该窗格，但在这种情况下我们将使它打开以便我们可以观察到该作业的进度。
6.  作业状态所示**作业摘要**，与我们看到的在我们测试的 runbook 时的状态相匹配。<br>
![作业摘要](media/automation-first-runbook-textual/job-pane-summary.png)
7.  一旦 runbook 状态显示*已完成*时，单击**输出**。  打开**输出**窗格中，然后我们可以看到我们的*Hello World*。<br>
![作业摘要](media/automation-first-runbook-textual/job-pane-output.png)  
8.  关闭输出窗格。
9.  单击 runbook 作业中的流的**流**。  我们将仅仅显示*Hello World*输出流，但这可以显示 runbook 作业如冗余和错误的其他流，如果 runbook 写入它们。<br>
![作业摘要](media/automation-first-runbook-textual/job-pane-streams.png) 
9. 关闭流窗格和工作窗格返回到 MyFirstRunbook 窗格中。
9.  单击以打开此 runbook 的工作窗格中的**作业**。  这将列出所有此 runbook 创建的作业。  我们应该只能看到一个作业列出由于我们仅运行此作业一次。<br>
![作业](media/automation-first-runbook-textual/runbook-control-jobs.png) 
9. 您可以单击此作业打开我们时，我们开始 runbook 查看同一工作窗格上。  这使您可以按时间顺序返回并查看为特定 runbook 创建的任何作业的详细信息。

## 步骤 5-添加身份验证管理 Azure 的资源

我们已经测试并发布我们的 runbook，但到目前为止，它不执行任何操作非常有用。  我们想要它管理 Azure 的资源。  它不能做到这一点，但除非我们已验证使用[的前提条件](#prerequisites)中引用的凭据。  与**添加 AzureAccount** cmdlet 我们做到这一点。

1.  通过单击**编辑**MyFirstRunbook 文本窗格中打开文本编辑器。<br>
![编辑 runbook](media/automation-first-runbook-textual/runbook-toolbar-edit.png) 
2.  我们不再需要**写输出**行，因此继续操作并删除它。
3.  将光标放在大括号之间空行上。
3.  在库控件中，展开**资产**和**凭据**。
4.  右键单击您的凭据并单击**添加以画布**。  这将添加**Get AutomationPSCredential**活动，您的凭据。
5.  **获得 AutomationPSCredential**，前面键入*$Credential =*将凭据分配给一个变量。 
3.  在下一行中，键入*添加 AzureAccount-凭据 $Credential*。 <br>
![进行身份验证](media/automation-first-runbook-textual/authentication.png) 
3. 这样，我们可以测试 runbook，请单击**测试窗格中**。
10. 单击**启动**以启动测试。  当它完成后时，您应该收到输出类似于以下凭据中返回用户的信息。  这证实的凭据有效。<br>
![进行身份验证](media/automation-first-runbook-textual/authentication-test.png) 

## 步骤 6-添加代码以启动虚拟机

现在，我们 runbook 身份验证向我们 Azure 的订购，我们可以管理资源。  我们将添加一个命令以启动虚拟机。  您可以在 Azure 的订阅中，选取任何虚拟机，现在我们可以将 cmdlet 名的硬。 


1. *添加 AzureAccount*之后, 键入*开始 AzureVM-名称 VMName' 服务 'VMServiceName'*提供的名称和服务名称的虚拟机启动。 <br>
![进行身份验证](media/automation-first-runbook-textual/start-azurevm.png) 
9. 保存 runbook，然后单击**测试窗格**，以便我们可以对其进行测试。
10. 单击**启动**以启动测试。  一旦它完成，请检查启动虚拟机。


## 第 7 步-向 runbook 添加输入的参数

当前，我们的 runbook 启动的虚拟机，我们在 runbook 中，硬编码，但如果我们无法指定虚拟机启动 runbook 时将更有用。  现在，我们将添加输入的参数 runbook，以提供该功能。

1. *VMName*和*VMServiceName* runbook 中添加参数，如下面的图像所示**开始 AzureVM** cmdlet 使用这些变量。 <br>
![进行身份验证](media/automation-first-runbook-textual/params.png) 
9. 保存 runbook 并打开测试窗格。  请注意，您现在可以将测试中使用的两个输入变量提供值。 
11.  关闭测试窗格。
12.  单击**发布**来发布新版本的 runbook。
13.  停止在上一步中启动虚拟机。
13.  单击**启动**以启动 runbook。  **VMName**和**VMServiceName**中输入您要启动虚拟机。<br>
![启动 Runbook](media/automation-first-runbook-textual/start-runbook-input-params.png) 

14.  Runbook 完成后，检查启动虚拟机。


## 相关的文章

- [我的第一个图形 runbook](automation-first-runbook-graphical.md)测试

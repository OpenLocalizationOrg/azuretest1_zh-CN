---
ms.openlocfilehash: f21d898f4a0911ea663de164d5cff3b08a85dd9d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="开始使用 Azure 自动化 |Microsoft Azure"
    description="了解如何导入和 Azure 中运行自动化作业。"
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
    ms.topic="hero-article"
    ms.date="05/19/2015"
    ms.author="bwren"/>


# 开始使用 Azure 自动化

## 什么是 Azure 自动化？

Microsoft Azure 自动化提供了为用户自动执行手动、 运行时间较长、 易出错且频繁执行的任务的是通常在云环境中的一种方法。 您可以创建、 监视、 管理和在 Azure 使用运行手册，它基于 Windows PowerShell 工作流环境中部署的资源。 在本文中，您将经历运行简单示例 runbook 的教程。 然后，您会发现资源为探索更多高级的功能的服务。

## 教程
本教程将带领您完成创建自动化帐户、 导入 Azure 自动化的示例"Hello World"runbook、 执行该 runbook，然后查看相应的输出。

若要完成本教程，您需要订阅了 Azure。 如果您没有一个，您可以[激活您的 MSDN 订户权益](../pricing/member-offers/msdn-benefits-details/)或[注册免费试用版](../pricing/free-trial.md)</a>。

[AZURE.INCLUDE [automation-note-authentication](../../includes/automation-note-authentication.md)]

## 视频的演练

这是本教程的演练。

[AZURE.VIDEO get-started-with-azure-automation]

## <a name="automationaccount"></a>创建自动化客户

自动化客户是 Azure 自动化资源的容器。 它提供了一种单独的环境或进一步组织您的工作流方法。 有关详细信息，请参见[自动化客户](http://aka.ms/runbookauthor/azure/automationaccounts)自动化库中。  如果您已经创建了一个自动化帐户，则可以跳过此步骤。

1.  登录到[Azure 的门户](http://manage.windowsazure.com)。

2.  在 Azure 的门户中，单击**创建自动化客户**。  

    ![创建帐户](./media/automation-create-runbook-from-samples/automation_01_CreateAccount.png)

3.  在**添加新的自动化帐户**页中，输入名称，然后选择帐户的区域。 该区域指定用于存储的自动化资源的帐户中。 这不会影响您的帐户的功能，但如果您的帐户所在的地区已接近存储其他 Azure 资源运行手册可能会更快地执行。 准备就绪之后，请单击该复选标记。

    ![添加新帐户](./media/automation-create-runbook-from-samples/automation_02_addnewautoacct.png)

## <a name="importrunbook"></a>从 Runbook 库中导入 runbook

[Runbook 库](http://aka.ms/runbookgallery)包括示例运行手册，您可以直接导入到 Azure 自动化客户，从而可以利用其他 Azure 自动化和 PowerShell 用户的工作。 在此步骤中，您将使用库导入"Hello World"示例 runbook。

4.  在**自动化**页面中，单击刚创建的新帐户。

    ![新帐户](./media/automation-create-runbook-from-samples/automation_03_NewAutoAcct.png)

5.  单击**运行手册**。

    ![运行手册选项卡](./media/automation-create-runbook-from-samples/automation_04_RunbooksTab.png)

6.  单击**新建** > **Runbook** > **库中**。

    ![Runbook 库](./media/automation-create-runbook-from-samples/automation_05_ImportGallery.png)

7.  选择**教程**类别和**Hello World 的 Azure 自动化**。 单击向右箭头按钮。

    ![导入 Runbook](./media/automation-create-runbook-from-samples/automation_06_ImportRunbook.png)

8.  复查的 runbook，内容，然后单击向右箭头按钮。

    ![Runbook 定义](./media/automation-create-runbook-from-samples/automation_07_RunbookDefinition.png)

8.  Runbook 的详细信息，查看，然后单击复选标记按钮。

    ![Runbook 的详细信息](./media/automation-create-runbook-from-samples/automation_08_RunbookDetails.png)

## <a name="publishrunbook"></a>将发布 runbook

在草拟模式下，首次导入 runbook。 这意味着您可以继续授权它以新版本可以运行之前对其进行工作。 由于此示例 runbook 不需要任何额外的配置，您将立即发布其作为-是。  有关详细信息，请参阅[发布 Runbook](http://aka.ms/runbookauthor/azure/publishrunbook)。

9.  Runbook 已完成导入，请单击**写 HelloWorld**。

    ![导入的 Runbook](./media/automation-create-runbook-from-samples/automation_07_ImportedRunbook.png)

9.  **作者**，请单击，然后单击**草稿**。  

    您可以修改在草拟模式下 runbook 的内容。 对此 runbook，不需要进行任何修改。

    ![作者的草稿](./media/automation-create-runbook-from-samples/automation_08_AuthorDraft.png)  

10. 单击**发布**以促进 runbook，将其标记为可供生产使用。

    ![发布](./media/automation-create-runbook-from-samples/automation_085_Publish.png)

11. 当提示您确认时，请单击**是**。

    ![保存并发布提示](./media/automation-create-runbook-from-samples/automation_09_SavePubPrompt.png)

## <a name="startrunbook"></a>启动 runbook

与导入并发布 runbook，可以立即运行它，然后检查输出。  有关详细信息，请参阅[启动 Runbook](http://aka.ms/runbookauthor/azure/startrunbook)和[Runbook 的输出和消息](http://aka.ms/runbookauthor/azure/runbookoutput)。

12. 与**写 HelloWorld** runbook 打开时，单击**开始**。

    ![已发布](./media/automation-create-runbook-from-samples/automation_10_PublishStart.png)

13. **指定 runbook 参数值**在页上，键入的**名称**将用作写入 HelloWorld.ps1 脚本，输入参数，然后单击复选标记。

    ![Runbook 参数](./media/automation-create-runbook-from-samples/automation_11_RunbookParams.png)

14. 单击**作业**来检查您刚启动 runbook 作业的状态，然后单击要查看摘要作业的**作业开始**列中的时间戳。

    ![Runbook 状态](./media/automation-create-runbook-from-samples/automation_12_RunbookStatus.png)

15. 在**摘要**页上，您可以看到摘要，输入的参数，以及作业的输出。

    ![Runbook 摘要](./media/automation-create-runbook-from-samples/automation_13_RunbookSummary_callouts.png)

祝贺您 ！ 您已经完成本教程。

## <a name="nextsteps"></a>下一步行动
1. 在本教程*不会管理 Azure 服务*简单 runbook。 大多数运行手册将使用[Azure cmdlet](http://msdn.microsoft.com/library/jj156055.aspx)来做到这一点，其中要求到 Azure 订购的身份验证。 按照在[配置为运行手册由管理的 Azure](http://aka.ms/azureautomationauthentication)的说明配置您要使用这些 cmdlet 的 Azure 订阅。  
2. 请参阅[资源](#resources)下面列出有关 Azure 自动化功能的详细信息。
3. [Azure 自动化博客](http://azure.microsoft.com/blog/tag/azure-automation)从 Azure 自动化团队的最新最新订阅。

## <a name="resources"></a>资源

各种其他资源都可供您了解更多有关 Azure 自动化和创建您自己的运行手册。

- [Azure 自动化库](http://go.microsoft.com/fwlink/p/?LinkId=392860)的配置和管理 Azure 自动化和创作您自己的运行手册提供完整的文档。
- [Azure PowerShell cmdlet](http://msdn.microsoft.com/library/jj156055.aspx)提供自动化的信息使用 Windows PowerShell 的 Azure 操作。  运行手册使用这些 cmdlet 使用 Azure 的资源。
- [Azure 自动化博客](http://azure.microsoft.com/blog/tag/azure-automation)从 Microsoft Azure 自动化提供的最新信息。
- [自动化论坛](http://go.microsoft.com/fwlink/p/?LinkId=390561)可以发布 Azure 自动化由 Microsoft 和自动化社区来解决有关问题。


## 示例和实用工具运行手册

Microsoft 和 Azure 自动化社区提供了示例运行手册，可帮助您开始创建您自己的解决方案和实用程序运行手册，可以用作构建基块的更大的自动化任务。 可以从[脚本中心](http://azure.microsoft.com/documentation/scripts/)下载这些运行手册，或直接到 Azure 自动化使用[Runbook 库](http://aka.ms/runbookgallery)中导入它们。


## 反馈

**向我们提供反馈 ！**  如果您正在寻找一个 Azure 自动化 runbook 解决方案或集成模块，请在脚本中心上发布脚本请求。 如果您有反馈或功能请求的 Azure 自动化，则在[用户语音](http://feedback.windowsazure.com/forums/34192--general-feedback)上发布它们。 谢谢 ！

测试

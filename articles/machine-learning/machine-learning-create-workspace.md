---
ms.openlocfilehash: 1399bc0c9c23d08fe6a6d6ceffe667bc102d8ae4
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="创建机器学习区 |Microsoft Azure" 
    description="如何为 Azure 机器学习 Studio 创建工作区" 
    services="machine-learning" 
    documentationCenter="" 
    authors="garyericson" 
    manager="paulettm" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/01/2015" 
    ms.author="garye"/>


# 创建一个 Azure 机器学习工作区 

若要使用 Azure 机器学习 Studio，需要有一个机器学习工作区。 此工作区包含您需要创建、 管理和发布的实验工具。 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## 若要创建工作区

1. 登录到您的 Microsoft Azure 帐户。
2. 在 Microsoft Azure 服务面板中，单击**机器学习**。

    ![机器学习服务][1]

3. 请单击**+ 新建**窗口的底部。
4. 单击**数据服务**然后**机器学习**，然后**快速创建**。

    ![新工作区快速 Create][3]

5. 为您的工作区输入一个**工作区名称**并指定**工作区所有者**。 工作区所有者必须是一个有效的 Microsoft 帐户 (例如，name@outlook.com)。

    > [AZURE.NOTE] 以后，您可以共享您正从事的其他人邀请到您的工作区的实验。 你可以在机器学习 Studio 中**设置**页面上。 只需为每个用户需要 Microsoft 客户或组织的帐户。

6. 指定 Azure 的**位置**，然后输入现有 Azure**存储帐户**，或选择**创建新的存储帐户**创建一个新。
7. 单击**创建 ML 工作区**。

您学习计算机的工作区创建后，您将看到它的**机器学习**页上列出。

有关管理您的工作区的信息，请参阅[管理 Azure 机器学习区]。
如果您遇到问题创建工作区，请参阅[故障排除指南︰ 创建和连接到机器学习区]。

[管理 Azure 机器学习区]: machine-learning-manage-workspace.md
[故障排除指南︰ 创建并连接到一个机器学习工作区]: machine-learning-troubleshooting-creating-ml-workspace.md
 
<!-- ![List of Machine Learning workspaces][2] -->

<!--Anchors-->
[若要创建工作区]: #createworkspace

<!--Image references-->
[1]: media/machine-learning-create-workspace/cw1.png
[2]: media/machine-learning-create-workspace/cw2.png
[3]: media/machine-learning-create-workspace/cw3.png



<!--Link references-->
 

测试

---
ms.openlocfilehash: f2d538fd74e7c7f92f2019b03411b21203d5dc9e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

<properties 
    pageTitle="Office 中使用 Azure 的远程应用程序" 
    description="了解 Office 和 Azure RemoteApp 如何协同工作" 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/02/2015" 
    ms.author="elizapo" />

# Office 中使用 Azure 的远程应用程序

必须驻留在 Azure RemoteApp Office 应用程序的两种选择︰ Office 365 ProPlus 或 Office 2013 专业加试验。

**嗨，您知道我们有一个新的更好的文章，很快就会替换此？ 了解[如何使用 Azure RemoteApp Office 365 订阅](remoteapp-officesubscription.md)。 它涉及使用 Office 365 + Azure 的远程应用程序所需的所有信息。**

## Office 365 ProPlus 
您可以创建使用 Office 365 ProPlus 模板图像的远程应用程序集合。 此选项允许您将您的 Office 365 服务扩展到远程应用程序。 您必须将现有的订阅计划和您的用户必须为 Office 365 ProPlus 服务，无论它们是单机许可或者通过 Office 365 提供服务计划。 

远程应用程序支持 Office 365 共享计算机激活。 当您启用共享的计算机，并使用[Office 部署工具](http://www.microsoft.com/download/details.aspx?id=36778)进行安装时，Office 365 ProPlus 安装过程中未被激活。 当为一个集合，其中包含 Office 365 提供用户迹象时，办公室检查是否已为 Office 365 ProPlus 置备了用户。 因此，办公室暂时激活 Office 365 ProPlus-如果此激活一直保持，直到停止服务的用户号。 

若要使用 Office 365 共享计算机激活，您需要创建[自定义模板](remoteapp-create-custom-image.md)，并按照[这些指导](https://technet.microsoft.com/library/dn782858.aspx)，安装 Office 365 ProPlus。

您可以管理在[Office 365 管理门户](https://portal.office365.com/)的用户的 Office 365 提供许可证。 阅读有关[Office 365 提供服务计划](http://technet.microsoft.com/library/office-365-plan-options.aspx)的详细信息。  


## Office 2013 专业人员正试用版 
RemoteApp 的 30 天试用版，在可以使用 Office 2013 专业加 （试验） 的模板映像创建远程应用程序集合。 您可以将用户分配到此试用集合并使用其 Azure Active Directory 工作帐户或 Microsoft 帐户。 没有其他订阅是必需的。

这是启动轮胎并获得办公室中 RemoteApp 的良好感觉是好的选择。 但是，此选项才用于评估和测试。 创建使用 Office 2013 专业加 （试验） 的模板图像的远程应用程序集不能过渡到生产模式，并将禁用在试用期结束。

## 从试用版转换到生产
当您开始您的 30 天免费试用版时，门户的远程应用程序一节中将告诉您多长时间您离开中试用软件之前需要转换为付费帐户。 您可以激活您的帐户并切换到使用本文中的链接的生产模式。 

当您激活您的帐户时，这会影响您的帐户中的所有远程应用程序集合。 

- 正在运行与 Windows Server 2012 R2 或 Office 365 ProPlus 模板图像的集合将无缝地过渡到生产。 所有用户数据和设置，包括正在进行的会话，则都保持不变。
- 如果您已上载的自定义模板图像，使用这些图像的集合将还可以无缝转换。
- 仅用于评估旨在 2013年办公室专业加 （试验） 的模板图像。 运行此模板图像的集合不能过渡到生产中。 它们将被放入"已禁用"状态。


如果您不做您的试用版已过期过渡到生产模式，将禁用远程应用程序集合。 不要担心-您的设置和用户的数据保存为另一个 90 天，以便您仍可以激活您的服务并切换到生产模式不会丢失任何数据。
 
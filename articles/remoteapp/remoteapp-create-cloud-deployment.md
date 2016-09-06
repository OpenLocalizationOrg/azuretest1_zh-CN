---
ms.openlocfilehash: bdeee8dd34d17c39ab5ece961c8964fd8a5e600c
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何创建 Azure RemoteApp 的云集合" 
    description="了解如何创建将数据保存在 Azure 的云的 Azure 远程应用程序的部署。" 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" 
    editor=""/>

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/02/2015" 
    ms.author="elizapo"/>

# 如何创建 Azure RemoteApp 的云集合

有两种类型的 Azure RemoteApp 集合︰ 

- 完全位于 Azure 云:。 您可以选择将所有数据都保存在云中 (因此只有云集合) 或要连接到 VNET 的收藏和都保存那里数据。   
- 混合︰ 包括虚拟网络内部访问-这需要使用 Azure 的 AD 和内部部署 Active Directory 环境。

本教程将引导您完成创建云集合的过程。 有四个步骤︰ 

1.  创建一个远程应用程序集合。
2.  （可选） 配置目录同步。 如果您正在使用 Azure AD + Active Directory，您必须为同步用户、 联系人和 Azure AD 租户从内部活动目录的密码。
5.  发布远程应用程序的应用程序。
6.  配置用户的访问权限。

**请注意***本主题是在中间被改编。我已经进行了更新以反映新的 UI 的步骤，但我不是还能够重新发布整个主题。我正在研究几个将使其更容易地确定您的身份验证和收集选项的新文章。因此，如果你感到困惑，请知道我知道，我可以给您获得更好的信息快速处理。谢谢。*

**在开始之前**

您需要创建集合之前执行以下操作︰

- [注册](http://azure.microsoft.com/services/remoteapp/)Azure 远程应用程序。 
- 收集有关您想要授予访问权限的用户的信息。 这可以是 Microsoft 帐户信息或 Active Directory 工作的用户的帐户信息。
- 此过程假定您正在或者打算使用一幅模板图像作为订阅的一部分而提供或您已上载要使用的模板图像。 如果您需要上传其他模板映像，您可以从模板图像页做到。 只需单击**上载模板图像**，然后按照向导中的步骤。 
- 要使用 Office 365 ProPlus 图像？ 签出信息[在此处](remoteapp-officesubscription.md)。
- 要提供自定义的应用程序或 LOB 的程序？ 创建一个新[图像](remoteapp-imageoptions.md)，并使用它云集合中。
- 确定您是否需要连接到 VNET。 如果您选择连接到 VNET，请确保它满足大小调整的原则，它可以连接到远程应用程序。
- 如果您使用的 VNET，决定是否要将它加入到您的本地 Active Directory 域。

## 步骤 1︰ 创建一个云-带有或不带 VNET##


使用以下步骤以**创建仅云集合**︰

1. 在管理门户中，转到远程应用程序页。
2. 单击**新 > 快速创建**。
3. 输入您的收藏的名称并选择您所在的地区。
4. 选择您想要使用的标准或基本的计划。
5. 选择要用于此集合的模板。 

    **提示︰**您对的订购 RemoteApp 带有[模板映像](remoteapp-images.md)包含 Office 365 或 Office 2013 （供试验使用） 程序、 一些发布 （如 Word) 和其他人发布准备就绪。 您还可以创建一个新[图像](remoteapp-imageoptions.md)，并使用它云集合中。


1. 单击**创建远程应用程序集合**。
    
    **重要︰**可能需要 30 分钟才能提供您的地图收藏。

在创建远程应用程序集合之后，双击的集合的名称。 该操作将显示**快速启动**页-这是您完成配置集合。

使用以下步骤创建一个**云 + VNET 集合**︰

1. 在管理门户中，转到远程应用程序页。
2. 单击**新建** > **使用 VNET 创建**。
3. 输入您的收藏集的名称。
4. 选择您想要使用的标准或基本的计划。
5. 选择已创建的 VNET。 不知道该怎么做？ 现在，这些步骤是[混合](remoteapp-create-hybrid-deployment.md)主题中。
6. 决定是否要加入到您的域的集合。 如果是的话，您需要使用 AD 连接集成 Azure 广告和活动目录环境。 包含在下面**步骤 2**中。
6. 单击**创建远程应用程序集合**。


## 步骤 2︰ 配置 （可选） 的 Active Directory 目录同步 ##

如果您想要使用 Active Directory，RemoteApp 要求 Azure Active Directory 和内部活动目录同步用户、 联系人和密码的 Azure Active Directory 租户之间的目录同步。 规划信息，请参阅[配置 Azure RemoteApp 的活动目录](remoteapp-ad.md)。 此外，还可以直接到[AD 连接](http://blogs.technet.com/b/ad/archive/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect.aspx)的信息。

## 步骤 3︰ 将远程应用程序的应用程序发布 ##

远程应用程序的应用程序是应用程序或程序，为用户提供。 它位于中找不到您上载的模板图像。 当用户访问远程应用程序的应用程序时，该应用程序看起来运行在他们本地的环境，但它真的在运行在 Azure。 

您的用户可以访问应用程序之前，您需要将它们发布到订阅源--最终用户的您的用户通过远程桌面客户端访问的可用应用程序列表。
 
可以将多个应用程序发布到远程应用程序集合。 从远程应用程序发布页上，单击**发布**中添加程序。 您可以从 「 开始 」 菜单模板图像的或通过指定的路径上的模板图像的应用程序发布。 如果您选择从 「 开始 」 菜单中添加，选择要发布的应用程序。 如果您选择提供应用程序的路径，提供应用程序和模板图像上的安装位置的路径的名称。

## 步骤 4︰ 配置用户访问权限 ##

现在，您已经创建了您的远程应用程序集，您需要添加您希望能够使用远程资源的用户。 如果您正在使用 Active Directory，您提供需要存在于 Active Directory 组织与预订关联的访问权限的用户您用于创建此远程应用程序集合。

1.  从快速启动页上，单击**配置用户访问**。 
2.  请输入您想要授予访问权限的工作 （从 Active Directory) 的帐户或 Microsoft 帐户。

    **注释︰** 

    请确保您使用"user@domain.com"的格式。

    如果您使用 Office 365 ProPlus 集合中，必须为您的用户使用 Active Directory 标识。 这有助于验证授权。 

3.  验证用户后，单击**保存**。


## 下一步行动 ##

就是这样-已成功创建并部署您的 RemoteApp 云集合。 下一步是让您的用户下载并安装远程桌面客户端。 URL 可以查找远程应用程序快速起始页上的客户端。 然后，有用户登录到客户端并访问您发布的应用程序。

 
---
ms.openlocfilehash: f8157eee6693b31ba510f5557cf2e5bd3e32d900
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="Azure RemoteApp 是什么？" 
    description="请查阅 Azure 的远程应用程序。" 
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
    ms.topic="get-started-article" 
    ms.date="08/05/2015" 
    ms.author="elizapo"/>

# Azure RemoteApp 是什么？

Azure 的远程应用程序将通过远程桌面服务，备份到 Azure 的内部 Microsoft RemoteApp 程序的功能。 Azure RemoteApp 可帮助您从许多不同的用户设备向应用程序提供安全的远程访问。

到 Azure 移动远程应用程序时，您可以利用而无需担心复杂的内部部署配置的存储、 可扩展性和 Azure 的全球市场。 Microsoft 提供的确保其可靠性，让您专注于更重要的问题，类似于创建为您的业务使用的最佳应用程序到 Azure，维护。 Azure RemoteApp 的另一个优点是可访问性-您的用户可以访问 RemoteApp 程序从 Windows、 iOS、 Mac OS X 和 Android 设备。 他们可以在他们的喜欢，虽然使用 Azure 管理门户来管理应用程序的环境中使用您的应用程序。 

阅读更多有关远程应用程序，或者，如果我们已经确信您[现在试出来](http://azure.microsoft.com/services/remoteapp/)。

有任何疑问 Azure RemoteApp 吗？ 签出我们的[常见问题解答](remoteapp-faq.md)。

Azure RemoteApp 是[Microsoft 虚拟桌面基础结构](http://www.microsoft.com/server-cloud/products/virtual-desktop-infrastructure/explore.aspx)的一部分。

**新增功能！** 想要了解更多有关 Azure 远程应用程序？ 或者准备大规模验证远程应用程序？ 加入我们每周[询问专家网络研讨会](https://azureinfo.microsoft.com/AzureRemoteAppAskTheExperts-Registration-Page.html?ls=Website)。

## 远程应用程序集
有两种类型的远程应用程序的集合︰


- 在承载和将程序的所有数据都存储在 Azure 云**云集合**。 用户可以通过登录其 Microsoft 客户或企业凭据同步或使用 Azure Active Directory 联合访问应用程序。
- **混合集合**中承载并将数据存储在 Azure 云还允许用户访问数据存储在您的本地网络上的资源。 用户可以通过同步或使用 Azure Active Directory 联合其企业凭据登录访问应用程序。

### 云的集合

[RemoteApp 集合云](remoteapp-create-cloud-deployment.md)提供宿主应用程序在云中以独立的方式。 仅在 Azure 的云，而不是连接到您的本地网络中存在云集合。

作为试验远程应用程序的一部分，我们为您提供 Office 365 ProPlus 或 Office 2013 的应用程序预安装和准备与用户共享。 如果您选择利用可用的软件，您可以快速设置您的服务。

与 Office 应用程序中使用云集合的另外一个优点是，应用程序和操作系统 （依据建立您的服务） 始终保持最新通过的定期更新，并且 Microsoft 反恶意软件的终端防护提供了连续的防御。 您的最终用户使用其 Microsoft 客户或公司的凭据来访问应用程序。 所有管理员，您不必担心找出谁应具有访问权限的应用程序。

您还可以创建云集合共享自定义应用程序或一组应用程序为您的用户。 若要执行此操作，需要[创建自定义的图像](remoteapp-imageoptions.md)（它是如何在我们发布到远程应用程序的应用程序），您只需选择该图像 （而不是办公室 2013年图像中） 当您创建您的收藏。 

#### 何时选择云

选择一个云集合时您要共享的应用程序不需要连接到任何资源公司的专用网络 （例如，通过 VPN 设备）。 如果应用程序使用资源在互联网、 OneDrive，或者 Azure，云集合将适用于您。 它也是最快来创建。


### 混合的集合
[混合 RemoteApp 集合](remoteapp-create-hybrid-deployment.md)，可以提供给用户的应用程序的两个自定义组和访问数据和您的本地网络中的资源。 与云集合一起使用的自定义图像，不同混合集合创建的图像在加入域的环境中，授予您本地网络和数据的完全访问权限运行的应用程序。

通过 Active Directory 集成与 Azure Active Directory （使用目录同步） 时，您的用户可以使用他们公司的凭据访问应用程序和数据。 在 Active Directory 中使用工作帐户时，您可以您的公司策略到云来控制通过远程应用程序提供了您的应用程序。

只要 Windows Server 2012 R2 上生成模板图像的远程桌面会话主机角色服务时，您可以为您的用户发布的应用程序有几个限制。 如果应用程序在该模板图像环境中正常工作，您的最终用户可以通过远程应用程序来访问它们。 

#### 何时选择混合

如果您需要连接到您公司的专用网络上的资源，请选择混合集合。 例如，如果应用程序需要访问下列情况之一︰

- 在 intranet 上的文件服务器
- 加快
- 在防火墙后面的数据库

这是通常更适用于大公司，有很多，他们不能移动到云的专用网络上的资源。

### 更新您的地图收藏
混合和云集合之间的主要区别之一就是如何处理的软件更新。 使用预安装的 Office 365 ProPlus 或 Office 2013 图像云集合，不需要担心任何更新。 该服务维护本身，滚出更新的应用程序和操作系统的日常。

对于混合集合，以及使用自定义模板图像的云集合，您是负责维护的映像和应用程序。 用于加入域的图像，您可以通过使用 Windows 更新、 组策略或 System Center 等工具控制更新。

更新您的自定义模板图像之后，将新图像上载到 Azure 的云，然后更新该集合以使用新的图像。 （你可以从仪表板或远程应用程序**快速启动**页面。）

有关详细信息，请参阅[更新您的收藏](remoteapp-update.md)。

## 支持远程应用程序客户端
Azure RemoteApp Mac、 iOS 和 Android 支持远程应用程序客户端应用程序，用于 Windows 和 Windows RT，以及 Microsoft 远程桌面应用程序。 您的用户可以在他们的手机上使用这些应用程序，或计算设备来访问新的 RemoteApp 程序。

有关客户端的详细信息，请参阅[访问您在 Azure 远程应用程序的应用程序](remoteapp-clients.md)。

## 下一步行动
Go ！ 试一下 ！ 这些文章可以帮助您开始远程应用程序︰

- [创建远程应用程序映像](remoteapp-imageoptions.md)
- [如何创建远程应用程序的云集合](remoteapp-create-cloud-deployment.md)
- [如何创建远程应用程序的混合集合](remoteapp-create-hybrid-deployment.md)
- [授权的工作方式在远程应用程序](remoteapp-licensing.md)
- [使用 Azure 远程应用程序的最佳做法](remoteapp-bestpractices.md)
- [Azure RemoteApp 常见问题解答](remoteapp-faq.md)
 
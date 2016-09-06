---
ms.openlocfilehash: ab1225f3a9058c9ccc35253e13656ff832a05020
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何创建 Azure RemoteApp 混合集合" 
    description="了解如何创建连接到内部网络的远程应用程序的部署。" 
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

# 如何创建 Azure RemoteApp 混合集合

有两种类型的远程应用程序的集合︰ 

- 完全位于 Azure 云:。 您可以选择将所有数据都保存在云中 (因此只有云集合) 或要连接到 VNET 的收藏和都保存那里数据。   
- 混合︰ 包括虚拟网络内部访问-这需要使用 Azure 的 AD 和内部部署 Active Directory 环境。


**请注意***本主题是在中间被改编。我正在研究几个将使其更容易地确定您的身份验证和收集选项的新文章。因此，如果你感到困惑，请知道我知道，我可以给您获得更好的信息快速处理。谢谢。*

本教程将引导您完成创建一个混合的过程。 有八个步骤︰ 

1.  决定哪些[图像](remoteapp-imageoptions.md)用于集合。 可以创建自定义映像，也可以使用包含在您的订阅 Microsoft 图像中的一个。
2. 设置虚拟网络。
2.  创建一个远程应用程序集合。
2.  将您的集合加入到本地域。
3.  将模板图像添加到您的收藏。
4.  配置目录同步。 远程应用程序要求与任一 1） 配置 Azure 活动目录同步的密码同步选项，或 2） 配置 Azure 活动目录同步无需密码同步选项，但使用域到 AD FS 联合 Azure Active Directory 集成。 签出[Active Directory 与远程应用程序的配置信息](remoteapp-ad.md)。
5.  发布远程应用程序的应用程序。
6.  配置用户的访问权限。

**在开始之前**

您需要创建集合之前执行以下操作︰

- 远程应用程序[注册](http://azure.microsoft.com/services/remoteapp/)。 
- 在活动目录中才能为远程应用程序服务帐户使用创建的用户帐户。 限制此帐户的权限，以便只可以向域中加入的计算机。
- 收集有关您的内部网络信息︰ IP 地址信息以及 VPN 设备的详细信息。
- [Azure PowerShell](../install-configure-powershell.md)模块安装。
- 收集有关您想要授予访问权限的用户的信息。 您将需要为每个用户的 Azure Active Directory 用户主体名称 (例如，name@contoso.com)。 确保 UPN 与匹配之间 Azure 广告和活动目录。
- 选择模板图像。 远程应用程序模板图像包含的应用程序和您要为您的用户发布的程序。 有关详细信息，请参阅[远程应用程序图像选项](remoteapp-imageoptions.md)。 
- 要使用 Office 365 ProPlus 图像？ 签出信息[在此处](remoteapp-officesubscription.md)。
- [配置为远程应用程序的活动目录](remoteapp-ad.md)。



## 步骤 1︰ 设置虚拟网络
您可以部署一个混合使用，现有 Azure 虚拟网络的远程应用程序集合，或者可以创建新的虚拟网络。 虚拟的网络允许通过远程应用程序远程资源在本地网络上的用户访问数据。 使用 Azure 的虚拟网络与其他 Azure 服务和虚拟机部署到该虚拟网络使集合直接网络访问。

请确保在创建您的 VNET 之前查看[VNET 大小](remoteapp-vnetsizing.md)信息。

### 创建 Azure VNET，并将它加入到 Active Directory 部署

首先创建一个[虚拟网络](../virtual-network/virtual-networks-create-vnet.md)。 这是 Azure 管理门户中的**网络**选项卡上。 您需要将虚拟网络连接到 Active Directory 部署同步到 Azure Active Directory 租户。

有关更多信息，请参见[关于虚拟网络设置管理门户中](../virtual-network/virtual-networks-settings.md)。

### 确保虚拟网络可供远程应用程序
在创建远程应用程序集合之前，让我们确保新虚拟网络就可以。 可以通过以下方法来验证︰

1. 创建 Azure 的虚拟机，您刚刚创建的远程应用程序的虚拟网络的子网内。
2. 使用远程桌面连接到虚拟机。 （单击**连接**）。
3. 将它们加入到您要为远程应用程序使用同一 Active Directory 部署。

它管用了吗？ 虚拟的网络和子网准备 RemoteApp ！

您可以找到有关创建 Azure 的虚拟机和连接到这些远程桌面[这里](https://msdn.microsoft.com/library/azure/jj156003.aspx)的详细信息。

## 步骤 2︰ 创建一个远程应用程序 ##



1. 在[Azure 的门户](http://manage.windowsazure.com)中，转到远程应用程序页。
2. 单击**新 > 创建 VNET 与**。
3. 输入您的收藏集的名称。
4. 选择您想要使用的标准或基本的计划。
5. 从下拉列表中，然后您的子网中选择您的 VNET。
6. 选择要将它加入到您的域。
5. 单击**创建远程应用程序集合**。

在创建远程应用程序集合之后，双击的集合的名称。 该操作将显示**快速启动**页-这是您完成配置集合。

## 步骤 3︰ 将您的集合链接到本地域 ##

 
1. 在**快速启动**页上，单击**加入本地域**。
2. 将远程应用程序服务帐户添加到本地 Active Directory 域。 您将需要的域的名称、 组织单位、 服务帐户的用户名和密码。 

    这是如果您遵循的步骤[配置的 Azure RemoteApp 的 Active Directory](remoteapp-ad.md)中收集到的信息。


## 步骤 4︰ 链接到远程应用程序图像 ##

远程应用程序模板图像包含您想要与用户共享的程序。 您可以创建一个新[模板图像](remoteapp-imageoptions.md)，或链接到现有图像 （一个已经导入或上载到 Azure RemoteApp）。 您还可以链接到包含 Office 365 或 Office 2013 （供试验使用） 程序远程应用程序[模板映像](remoteapp-images.md)中的一个。 

如果您要上载新的映像，您需要输入名称，然后选择该图像的位置。 在向导的下一页上，您将看到的 PowerShell cmdlet 的副本集这些 cmdlet 从提示符并运行提升 Windows PowerShell 上载指定的图像。

如果要链接到现有模板图像，只需指定映像名称、 位置和相关联的 Azure 订阅。



## 步骤 5︰ 配置 Active Directory 目录同步 ##

远程应用程序要求与任一 1） 配置 Azure 活动目录同步的密码同步选项，或 2） 配置 Azure 活动目录同步无需密码同步选项，但使用域到 AD FS 联合 Azure Active Directory 集成。 

签出[广告连接](http://blogs.technet.com/b/ad/archive/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect.aspx)-这篇文章可以帮助您设置目录集成四个步骤。

计划信息和详细的步骤，请参阅[目录同步的路线图](http://msdn.microsoft.com//library/azure/hh967642.aspx)。

## 第 6 步︰ 发布远程应用程序的应用程序 ##

远程应用程序的应用程序是应用程序或程序，为用户提供。 它位于中找不到您上载的模板图像。 当用户访问应用程序时，它似乎在自己的本地环境中运行，但它真的在运行在 Azure。 

您的用户可以访问远程应用程序的应用程序之前，您需要将它们发布到订阅源--最终用户的您的用户通过远程桌面客户端访问的可用应用程序列表。
 
可以将多个应用程序发布到远程应用程序集合。 RemoteApp 发布页上，单击要添加一个应用程序**发布**。 您可以从 「 开始 」 菜单模板图像的或通过指定的路径上的模板图像的应用程序发布。 如果您选择从 「 开始 」 菜单中添加，选择到应用程序使用的程序。 如果您选择提供应用程序的路径，提供应用程序和模板图像上的安装位置的路径的名称。

## 第 7 步︰ 配置用户访问 ##

现在，您已经创建了您的远程应用程序集，您需要添加您希望能够使用远程资源的用户。 用来创建此 RemoteApp 集合您提供需要存在于 Active Directory 组织与预订关联的访问权限的用户。

1.  从快速启动页上，单击**配置用户访问**。 
2.  请输入您想要授予访问权限的工作 （从 Active Directory) 的帐户或 Microsoft 帐户。

    **注释︰** 

    请确保您使用"user@domain.com"的格式。

    如果您使用 Office 365 ProPlus 集合中，必须为您的用户使用 Active Directory 标识。 这有助于验证授权。 


3.  一旦用户进行验证，单击**保存**。


## 下一步行动 ##
就是这样-成功地创建并部署您的远程应用程序混合集合。 下一步是让您的用户下载并安装远程桌面客户端。 URL 可以查找远程应用程序快速起始页上的客户端。 然后，有用户登录到客户端并访问您发布的应用程序。


 

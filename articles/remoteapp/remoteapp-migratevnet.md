---
ms.openlocfilehash: 7263cadda6b47a1d23f8cae99776bc2da6a3c622
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何将从远程应用程序 VNET 迁移到 Azure VNET"
    description="了解如何将从远程应用程序 VNET 迁移到 Azure VNET" 
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



# 如何将从远程应用程序 VNET 的混合收集迁移到 Azure VNET

好消息 ！ 我们已使您能够部署混合直接插入现有 Azure 虚拟网络 (VNETs) 而不是创建特定远程应用程序的 VNETs 的远程应用程序集合。 这允许您利用最新的 VNET 功能 （如 ExpressRoute)，将混合集合直接网络访问提供给其他 Azure 服务和虚拟机部署到该 VNET。  （这将使您更好的性能和更容易比 VNET 到 VNET 配置设置）。


假设，您已创建混合调用远程应用程序名为*RemoteAppVNET*的 VNET 与*OriginalCollection*的远程应用程序集合。 以下是要将其迁移到新的 Azure VNET 名为*AzureVNET*的步骤。

1.  [管理门户](http://manage.windowsazure.com/)的**网络**选项卡上，创建名为*AzureVNET*，使用相同的位置、 DNS 配置和地址空间 （至少一个*AzureVNET*子网） 作为您使用*RemoteAppVNET*VNET。
2.  对任一主机配置*AzureVNET*或者*OriginalCollection*已经加入到域的 Active Directory 部署的网络连接。
3.  **Remoteapp**选项卡上，创建一个名为*新收藏集*的新远程应用程序集合。 （使用**VNET 创建**选项，**快速创建**）。
3.  配置*NewCollection*以将其部署到*AzureVNET*中的子网。
4.  配置要使用同一个图像和域联接信息作为您使用*OriginalCollection*的*NewCollection* 。
5.  几个小时后， *NewCollection*将显示状态为活动集合列表中。

现在，如果您不需要原始集合中的任何用户信息迁移到新的集合，这些步骤下一步操作︰

6.  删除*OriginalCollection*。
7.  删除*RemoteAppVNET*。

而且，大功告成 ！

或者，如果您需要将原始集合中的用户信息迁移到新集合，这些步骤下一步操作︰ 

6.  向[remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com?subject=Azure%20RemoteApp%20user%20information%20migration) Azure 的订阅 ID，您原始集合的名称与您的新收藏的名称发送一封电子邮件，要求他们迁移您的用户信息。
7.  在 2 个工作日内 RemoteApp 团队将用户访问列表和所有用户的文档和用户设置原始集合中的新集合。
8.  删除*OriginalCollection*。
9.  删除*RemoteAppVNET*。

而且现在，大功告成 ！

如果您有任何疑问或需要特殊帮助，请通过电子邮件发送[remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com?subject=Azure%20RemoteApp%20VNET%20migration%20help)。
 
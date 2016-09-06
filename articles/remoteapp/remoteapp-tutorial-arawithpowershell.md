---
ms.openlocfilehash: 988759247fabce15b6810e5208a1e76831a5309e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="在 Azure 使用 Powershell 的远程应用程序入门"
   description="了解如何开始在 Azure 使用 Powershell 的远程应用程序"
   services="remoteapp"
   documentationCenter=""
   authors="guscatalano"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/12/2015"
   ms.author="guscatal;spatnaik;elizapo"/>



# 在 Azure 使用 Powershell 的远程应用程序入门
=====================================


## 获取这些 cmdlet 
-------------
首先我们需要下载 Azure Powershell commandlets[这里](http://go.microsoft.com/?linkid=9811175)，包括在它的远程应用程序。 

签出的 Azure RemoteApp cmdlet 帮助[这里](https://msdn.microsoft.com/library/mt428031.aspx)。

## 配置 Azure 的 cmdlet 使用您的订购
------------------
按照[本指南](../powershell-install-configure.md)，以便您可以针对 Azure 订阅使用 cmdlet。

## 创建云集合
--------------------
这很简单，运行以下命令︰

    New-AzureRemoteAppCollection -Collectionname RAppO365Col1 -ImageName "Office 365 ProPlus (Subscription required)" -Plan Basic -Location "West US" - Description "Office 365 Collection."

上面的命令自动发布 Microsoft Office 365 提供应用程序 （Excel，OneNote、 Outlook、 PowerPoint、 Visio 和单词）。

创建集可能需要 30 分钟或更长时间才能完成。 因此，此命令返回的跟踪 ID，您可以按以下方式使用︰


    Get-AzureRemoteAppOperationResult -TrackingId xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

集合完毕后，您可以将用户添加到具有以下命令的集合︰

    Add-AzureRemoteAppUser -CollectionName RAppO365Col1 -Type microsoftAccount -UserUpn someone@domain.com

大功告成 ！ 用户应该能够连接到应用程序使用 Azure 远程应用程序客户端找到[这里](https://www.remoteapp.windowsazure.com/)。

## 可用的 cmdlet
有很多的其他命令的我们，将很快为他们的文档︰

基本的 cmdlet RemoteApp 集合︰ 

- 新 AzureRemoteAppCollection
- 获得 AzureRemoteAppCollection
- 一组 AzureRemoteAppCollection
- 更新 AzureRemoteAppCollection
- 删除 AzureRemoteAppCollection
- 添加 AzureRemoteAppUser
- 获得 AzureRemoteAppUser
- 删除 AzureRemoteAppUser
- 获得 AzureRemoteAppSession
- 断开连接 AzureRemoteAppSession
- 调用 AzureRemoteAppSessionLogoff
- 发送 AzureRemoteAppSessionMessage
- 获得 AzureRemoteAppProgram
- 获得 AzureRemoteAppStartMenuProgram
- 将发布 AzureRemoteAppProgram
- 取消发布 AzureRemoteAppProgram
- 获得 AzureRemoteAppCollectionUsageDetails
- 获得 AzureRemoteAppCollectionUsageSummary
- 获得 AzureRemoteAppPlan

远程应用程序虚拟网络 cmdlet:

- 新 AzureRemoteAppVNet
- 获得 AzureRemoteAppVNet
- 一组 AzureRemoteAppVNet
- 删除 AzureRemoteAppVNet
- 获得 AzureRemoteAppVpnDevice
- 获取-AzureRemoteAppVpnDeviceConfigScript
- 重置-AzureRemoteAppVpnSharedKey

远程应用程序模板图像 cmdlet:

- 新 AzureRemoteAppTemplateImage
- 获得 AzureRemoteAppTemplateImage
- 重命名 AzureRemoteAppTemplateImage
- 删除 AzureRemoteAppTemplateImage

其他 RemoteApp cmdlet:

- 获得 AzureRemoteAppLocation
- 获得 AzureRemoteAppWorkspace
- 一组 AzureRemoteAppWorkspace
- 获得 AzureRemoteAppOperationResult
 
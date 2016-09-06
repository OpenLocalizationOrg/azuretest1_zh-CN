---
ms.openlocfilehash: cb60d327be7fa7092cb67c18bfe34791baa690e3
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="更新您的 Azure RemoteApp 集合"
   description="了解如何更新您的 Azure RemoteApp 集合"
   services="remoteapp"
   documentationCenter=""
   authors="lizap"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="compute"
   ms.date="08/12/2015"
   ms.author="elizapo"/>

# 更新在 Azure RemoteApp 的集合

会那里需要的时候，不可避免地，您要更新的应用程序或 Azure RemoteApp 集合中的图像。 如果您正在使用 Azure RemoteApp 订阅，云或混合集合中包含的图像以便可以高枕无忧通过 Azure RemoteApp 本身，处理所有的更新。

但是，如果您使用自定义图像 （从零开始生成或通过修改我们的图像之一来创建），您将负责维护的映像和应用程序。 

因此，如何转有关更新您的集合？ 它是相当简单的︰

1. 更新您在您的收藏中使用的图像。 应用任何修补程序或更新所需，，然后用新名称保存它。
2. [上载](remoteapp-uploadimage.md)或[导入](remoteapp-image-on-azurevm)的图像到远程应用程序。
3. 现在，在集合页上，单击**更新**。
4. 从**模板图像**列表中选择新的映像。
4. 以下是棘手部分-您需要决定如何处理集合中当前正在使用的应用程序的任何用户。 您有以下选择︰
    - **为用户提供更新后的 60 分钟**。 只要更新完成，Azure RemoteApp 将向所有活动用户告诉他们要保存他们的工作和注销并重新登录在显示消息。 60 分钟后，不注销任何活动的用户将被自动注销。 用户可以立即重新登录。 
    - **立即注册用户**。 只要更新完成后，注销所有用户自动不发出任何警告。 如果您选择此选项，则用户可能会丢失数据。 但是，它们可以重新连接到该应用程序立即。

1. 单击复选标记以开始更新。

 
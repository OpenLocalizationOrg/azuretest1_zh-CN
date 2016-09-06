---
ms.openlocfilehash: 8dc08b5cce0302ac545be5461635541e02761b69
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

<properties 
    pageTitle="解决 RemoteApp 云集合的创建"
    description="了解如何解决远程应用程序云集合创建失败" 
    services="remoteapp" 
    documentationCenter="" 
    authors="vkbucha" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/12/2015" 
    ms.author="vikbucha" />



# 疑难解答创建 RemoteApp 云集合

Azure 管理门户中出现的常见错误︰

    DNS server could not be reached
    ProvisioningTimeout

通常云集合失败期间创建，因为您正在使用自定义图像。  如果您看到上面的错误之一，您使用的自定义图像创建集合，请检查下列内容︰

- 确保您已上载的自定义图像满足图像的要求。 
- 通常最常见的问题是，该图像未正确 syspreped。  
- 验证映像可以在 Hyper-V 中引导或尝试直接在 Azure 订阅使用的映像创建 IAAS VM。 如果虚拟机无法启动，启动，然后这通常表示未被正确地准备自定义图像。  验证以下方式生成自定义映像为远程应用程序创建自定义模板图像

如果使用 Microsoft 图像包含在您的订阅，请尝试创建再次集合。 如果问题仍然存在，请与 Microsoft 技术支持。

    PlatformImageTrialModeOnly

如果看到此错误这通常意味着已升级到付费帐户，但您正在使用 Microsoft 提供图像仅在试用服务的模式是有效的。 在这种情况下，尝试创建您的云集合，但请确保指定了正确的图像。
 
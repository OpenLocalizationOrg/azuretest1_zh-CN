---
ms.openlocfilehash: 95b5e35ecb1f5314700a878cd259a9ce81a5a340
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="Windows 通用 SDK 概述" 
    description="Windows Azure 的移动服务为通用 SDK 的概述"                                     
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-store" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/10/2015" 
    ms.author="piyushjo" />

#Windows Azure 的移动服务的通用 SDK 概述

从这里开始了解如何集成 Azure 移动参与 Windows 通用应用程序的详细信息。 如果您想要试一试前，确保完成我们的[15 分钟教程](mobile-engagement-windows-store-dotnet-get-started.md)。

单击此处可查看[SDK 内容](mobile-engagement-windows-store-sdk-content.md)

##集成步骤

1. 从这里开始︰[如何集成通用 Windows 应用程序中的移动服务](mobile-engagement-windows-store-integrate-engagement.md)

2. 通知︰[如何集成通用 Windows 应用程序中的范围 （通知）](mobile-engagement-windows-store-integrate-engagement-reach.md)

3. 添加标签计划的实施︰[如何使用标记 API 通用 Windows 应用程序中的此高级的移动服务](mobile-engagement-windows-store-use-engagement-api.md)

##发行说明

###3.1.0 （2015 年 05/21）

-   移动服务的设备 id 现在取决于在安装时生成的 GUID

早期版本，请参阅[完整的发行说明](mobile-engagement-windows-store-release-notes.md)

##升级过程

如果已经有到应用程序中集成服务的较早版本，您必须升级 SDK 时要考虑以下几点。

您可能需要按照几个步骤，如果错过了几个版本的 SDK 查看完成[升级过程](mobile-engagement-windows-store-upgrade-procedure.md)。 例如，如果迁移从 0.10.1 到 0.11.0，您必须首先按照"从 0.10.1 到 0.9.0"步骤再"发件人为 0.11.0 0.10.1"过程。

###从 2.0.0 为 3.0.0

#### 资源
此步骤涉及自定义的资源。 如果您已自定义资源提供的 SDK （html、 图像、 覆盖） 然后您必须在升级之前备份它们并重新应用您的自定义升级资源上。

### 从旧版本升级

测试[升级过程](mobile-engagement-windows-store-upgrade-procedure/)，请参阅

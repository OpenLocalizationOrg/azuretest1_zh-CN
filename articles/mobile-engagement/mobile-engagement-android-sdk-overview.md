---
ms.openlocfilehash: d7887d8d4308a65846e2648c934669eab1459173
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="Azure 移动接洽 Android SDK 集成" 
    description="最新的更新和 Android SDK Azure 移动服务的步骤"
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-android" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/10/2015" 
    ms.author="piyushjo" />


#Android SDK Azure 的移动服务

从这里开始，以获取有关如何集成 Azure 移动接洽，Android 应用程序中的所有详细信息。 如果您想要试一试前，请确保您做我们的[15 分钟指南](mobile-engagement-android-get-started.md)。

单击此处，请参阅[SDK 内容](mobile-engagement-android-sdk-content.md)。

##集成步骤
1. 从这里开始︰[如何集成 Android 应用程序中的移动服务](mobile-engagement-android-integrate-engagement.md)

2. 通知︰[如何集成 Android 应用程序中的范围 （通知）](mobile-engagement-android-integrate-engagement-reach.md)
    1. Google 云消息 (GCM):[如何集成 GCM 与移动服务](mobile-engagement-android-gcm-integrate.md)
    2. Amazon 设备消息传递 (ADM):[如何集成 ADM 与移动服务](mobile-engagement-android-adm-integrate.md)

3. 添加标签计划的实施︰[如何使用高级的移动服务标记 Android 应用程序中的 API](mobile-engagement-android-use-engagement-api.md)


##发行说明

##4.1.0 （2015 年 08/25）

- 为 Android M 处理新的权限模型。
- 现在可以配置位置功能在运行时，而不是使用`AndroidManifest.xml`。
- 为修复权限错误︰ 如果您使用`ACCESS_FINE_LOCATION`，然后`ACCESS_COARSE_LOCATION`再也不需要。
- 稳定性的改进。

对于早期版本，请参阅[完整的发行说明](mobile-engagement-android-release-notes.md)。

##升级过程

如果您已经具有集成我们 SDK 的较旧版本应用程序请到咨询[升级过程](mobile-engagement-android-upgrade-procedure.md)。
测试

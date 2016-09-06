---
ms.openlocfilehash: ef67e502d37861125cc6a83f3d5ffdba89f00f31
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="Azure 移动接洽 iOS SDK 概述"
    description="最新的更新和 iOS Azure 移动接洽的 SDK 的过程"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="08/05/2015"
    ms.author="MehrdadMzfr" />

#iOS SDK 的 Azure 移动服务

从这里开始，以获取关于如何在 iOS 应用程序集成 Azure 移动服务的详细信息。 如果您想要试一试前，请确保您做我们的[15 分钟指南](mobile-engagement-ios-get-started.md)。

单击此处可查看[SDK 内容](mobile-engagement-ios-sdk-content.md)

##集成步骤
1. 从这里开始︰[如何集成 iOS 应用程序中的移动服务](mobile-engagement-ios-integrate-engagement.md)

2. 通知︰[如何集成 iOS 应用程序中的范围 （通知）](mobile-engagement-ios-integrate-engagement-reach.md)

3. 添加标签计划的实施︰[如何使用高级的移动服务标记在 iOS 应用程序的 API](mobile-engagement-ios-use-engagement-api.md)


##发行说明

###3.1.0 （2015 年 08/26）

-   与第三方库修复 iOS 9 兼容性错误。 它导致崩溃时发送轮询结果、 应用程序信息或额外的数据。

早期版本，请参阅[完整的发行说明](mobile-engagement-ios-release-notes.md)

##升级过程

如果已经有到应用程序中集成服务的较早版本，您必须升级 SDK 时要考虑以下几点。

您可能需要按照几个步骤，如果错过了几个版本的 SDK 查看完成[升级过程](mobile-engagement-ios-upgrade-procedure.md)。

每个新版本的 SDK，您必须首先替换 （删除并重新导入 xcode） EngagementSDK 和 EngagementReach 文件夹。

###从 2.0.0 为 3.0.0
除去支持 iOS 4.X。 从这个版本开始部署目标的应用程序必须至少为 iOS 6。

如果您将到达在您的应用程序，您必须添加`remote-notification`值`UIBackgroundModes`Info.plist 文件以便接收远程通知中的数组。

方法`application:didReceiveRemoteNotification:`需要替换为`application:didReceiveRemoteNotification:fetchCompletionHandler:`在您的应用程序代理。

"AEPushDelegate.h"不建议使用的接口，并且您需要删除所有引用。 此过程包括卸下`[[EngagementAgent shared] setPushDelegate:self]`和委托方法从您的应用程序代理︰

    -(void)willRetrieveLaunchMessage;
    -(void)didFailToRetrieveLaunchMessage;
    -(void)didReceiveLaunchMessage:(AEPushMessage*)launchMessage;


测试

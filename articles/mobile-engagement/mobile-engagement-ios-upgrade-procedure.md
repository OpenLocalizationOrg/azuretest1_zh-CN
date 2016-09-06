---
ms.openlocfilehash: 667a005ca50213ca6551aa1649e43df6ca304cba
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="Azure 移动接洽 iOS SDK 升级过程"
    description="最新的更新和 iOS Azure 移动接洽的 SDK 的过程"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="MehrdadMzfr"
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

#升级过程

如果已经有到应用程序中集成服务的较早版本，您必须升级 SDK 时要考虑以下几点。

每个新版本的 SDK，您必须首先替换 （删除并重新导入 xcode） EngagementSDK 和 EngagementReach 文件夹。

##从 2.0.0 为 3.0.0
除去支持 iOS 4.X。 从这个版本开始部署目标的应用程序必须至少为 iOS 6。

如果您将到达在您的应用程序，您必须添加`remote-notification`值`UIBackgroundModes`Info.plist 文件以便接收远程通知中的数组。

方法`application:didReceiveRemoteNotification:`需要替换为`application:didReceiveRemoteNotification:fetchCompletionHandler:`在您的应用程序代理。

"AEPushDelegate.h"不建议使用的接口，并且您需要删除所有引用。 此过程包括卸下`[[EngagementAgent shared] setPushDelegate:self]`和委托方法从您的应用程序代理︰

    -(void)willRetrieveLaunchMessage;
    -(void)didFailToRetrieveLaunchMessage;
    -(void)didReceiveLaunchMessage:(AEPushMessage*)launchMessage;

##从 1.16.0 到 2.0.0
下面介绍如何从 Azure 移动合作由应用程序提供的 Capptain SA Capptain 服务迁移 SDK 集成。
如果要从早期版本迁移，请参考 Capptain 网站首先迁移到 1.16，则应用下面的过程。

>[Azure.IMPORTANT] Capptain 和移动签约不是相同的服务，下面给出的过程只重点介绍如何将客户端应用程序的迁移。 迁移的 SDK 应用程序中不会迁移数据从 Capptain 服务器到移动服务服务器

### 代理

方法`registerApp:`已替换为对新方法`init:`。 您的应用程序代理必须相应地进行更新，并使用连接字符串︰

            - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
            {
              [...]
              [EngagementAgent init:@"YOUR_CONNECTION_STRING"];
              [...]
            }

已从 SDK，您只需删除的所有实例中删除 SmartAd 跟踪`AETrackModule`类

### 类名称更改

作为品牌的一部分，有两个需要更改的类/文件名称。

用"AE"前缀重命名为"CP"前缀的所有类。

示例：

-   `CPModule.h` 重命名为`AEModule.h`。

以"Capptain"为前缀的所有类都以"交往"的前缀重都命名。

示例︰

-   此类`CapptainAgent`重命名为`EngagementAgent`。
-   此类`CapptainTableViewController`重命名为`EngagementTableViewController`。
-   此类`CapptainUtils`重命名为`EngagementUtils`。
-   此类`CapptainViewController`重命名为`EngagementViewController`。

测试

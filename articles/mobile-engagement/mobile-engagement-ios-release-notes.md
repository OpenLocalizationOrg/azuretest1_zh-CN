---
ms.openlocfilehash: e4194de0eb468de5e2183317ad527e171c86dcd5
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="Azure 移动接洽 iOS SDK 发行说明"
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

#发行说明

##3.1.0 （2015 年 08/26）

-   与第三方库修复 iOS 9 兼容性错误。 它导致崩溃时发送轮询结果、 应用程序信息或额外的数据。

##3.0.0 （2015 年 06/19）

-   移动服务使用静默推送通知。
-   除去支持 iOS 4.X。 从这个版本开始部署目标的应用程序必须至少为 iOS 6。

##2.2.0 （2015 年 05/21）

-   移动服务设备 id 的设备 < iOS 6 现在取决于在安装时生成的 GUID。

##2.1.0 （2015 年 04/24）

-   增加反应迅速的兼容性。
-   当单击一个通知，执行该操作 URL 现在是右打开应用程序后。
-   在 SDK 包的添加缺少头文件。
-   移动服务崩溃报告器被禁用时解决了问题。

##2.0.0 （2015 年 02/17）

-   最初发布 Azure 的移动服务
-   应用程序标识/sdkKey 配置将替换为连接字符串配置。
-   删除 API 来发送和接收任意 XMPP 消息从任意 XMPP 实体。
-   删除 API 来发送和接收设备之间的消息。
-   安全性得到了改进。
-   SmartAd 跟踪删除。

测试

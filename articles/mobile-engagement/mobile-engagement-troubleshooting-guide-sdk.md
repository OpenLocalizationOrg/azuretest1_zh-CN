---
ms.openlocfilehash: efc871f1cd0abb544b99fb3aca18d162a2f58a43
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="故障排除指南-SDK 的 azure 移动服务" 
   description="SDK Azure 移动服务的集成问题疑难解答" 
   services="mobile-engagement" 
   documentationCenter="" 
   authors="piyushjo" 
   manager="dwrede" 
   editor=""/>

<tags
   ms.service="mobile-engagement"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="mobile-multiple"
   ms.workload="mobile" 
   ms.date="06/18/2015"
   ms.author="piyushjo"/>

# 有关 SDK 集成问题的故障排除指南

以下是可能使用 Azure 移动服务是如何集成到您的应用程序可能会遇到的问题。

## 所发现的另一个区域，您的应用程序失败的 SDK 问题

### 问题
- 用户界面数据集合失败 （分析、 监控、 分段或仪表板）。
- 推入失败 （将在应用程序中，从和 / 或应用程序中，不起作用）。
- 高级功能故障 （跟踪、 地理位置或不使用特定推送的平台）。
- API 失败 （Api 失败通常以静默方式并且没有错误消息）。
- 服务 （无 Azure 移动服务适合于您的应用程序） 故障。

### 原因

- 需要使用 Azure 移动服务 SDK 解决的大多数问题将由您的应用程序 （例如，用户界面数据集合失败、 推故障、 高级的功能故障、 API 故障、 应用程序崩溃或明显的服务中断） 中的故障被发现。  
- 如果在您的应用程序之前曾永远不会到 Azure 移动合作的某一特定功能，您需要完成集成。 
- 如果 Azure 移动合作的某一特定功能工作正常并且停止，您可能需要 Azure 移动服务 sdk 版本升级到最后。 请记住，Azure 移动服务 （Android、 iOS、 窗口和 Windows Phone） 支持每个平台 Azure 移动服务 SDK 的其他版本。

#### SDK 集成

- Azure 移动服务没有正确地集成在 SDK （分析）。
- 访问不正确集成在 SDK （在应用程序和应用程序推送出的）。
- 证书已过期或不正确制作与开发 (仅适用于 iOS)。
- GCM 或 ADM 没有正确地集成在 SDK (Android 只-服务特定推送安装)。
- 跟踪未正确集成在 SDK （安装存储跟踪）。
- 迟钝的位置或 GPS 位置不正确集成在 SDK （按地理位置为目标）。


**另请参见︰**

- [SDK 文档的集成指南][链接 5] 
- [故障排除指南-推][链接 23]

#### SDK 升级

- 需要升级 SDK 来解决问题的较旧版本的 SDK （通常与较新版本的操作系统的设备）。
- 从设备中卸载您的应用程序的所有以前版本并重新安装最新版本的应用程序，重新注册您的设备 ID 从 Azure 接洽移动用户界面以确认您的设备正在使用您的应用程序的最新版本。

**另请参见︰**

- [SDK 文档中的发行说明](http://go.microsoft.com/fwlink/?LinkId= 525554) 
- [SDK 文档-升级指南](http://go.microsoft.com/fwlink/?LinkId= 525554)

#### 其他 SDK

- 应用程序清单"AndroidManifest.xml"中的错误可能会导致 Azure 移动服务无法正常工作 (只适用于 Android)。
- 与 SDK 集成和 API 使用一个常见的问题是混淆的 SDK 键和 API 密钥。

**另请参见︰**

- [概念的术语表][6 链接]

## 高级编码问题

### 问题
-  平台特定代码不直接与 Azure 移动合作可在 iOS 和 Android，Windows Phone 导致问题。

### 原因

- 许多高级 Azure 移动接洽的编码问题是由不直接与 Azure 移动合作编写不当的平台特定代码引起的。 您将需要参考文档特定于平台的开发以及 Azure 移动服务文档 （Android、 iOS、 Web、 窗口和 Windows Phone）。
- 未正确配置"类别"，可以防止从通知链接到另一个位置的内部或外部应用程序 (仅适用于 Android)。 
- 如果没有设置"UIKit.framework""可选"在 iOS 代码中，显示"未找到错误的符号"一和/或在旧版本的 iOS 设备 (仅适用于 iOS) 崩溃。
- 已过期的证书或没有正确地使用证书，原因的开发或生产版本推问题 (仅适用于 iOS)。
- 有到平台 Azure 移动签约不能控制 （如系统中心工作原理用于从应用程序将在 Android 和 iOS） 固有的一些限制。
- Azure 移动合作发布供参考 iOS 和 Android 使用 Azure 移动服务的内部包的完整列表。 请记住，Azure 移动合作的某些功能是特定于平台 （Android、 iOS、 Web、 窗口和 Windows Phone）。

### 请参见

 - [故障排除指南-推][链接 23] 
 - [SDK 文档中的发行说明][链接 5]
 - [SDK 文档-升级指南][链接 5]

## 应用程序崩溃

### 问题
- 在最终用户的设备上，您的应用程序崩溃。

### 原因

- 在*分析用户界面*或*分析 API*可以查看故障转储信息
- 可以查找您的测试设备的设备 ID 并采取相同的措施导致您的应用程序崩溃的最终用户能够帮助您识别您崩溃的原因。
- 通过升级到最新版本的 SDK 有时解析 Azure 移动服务 sdk 已知会导致应用程序崩溃的问题。 请务必查看有关您的平台的发行说明，调查崩溃时。

### 请参见

- [SDK 文档中的发行说明][链接 5]
- [SDK 文档-升级指南][链接 5]

## 应用程序存储上载失败

### 问题
- 将您的应用程序的最新版本上载到苹果、 Google 或 Windows 应用商店与相关的错误。

### 原因

- 应用程序有时会阻止应用程序使用存储启用某些功能 （如苹果商店可防止应用程序在存储中的使用 IDFV 和 GooglePlay 存储阻止的应用程序应用程序之间的信息共享）。 
- 请务必检查有关平台和当前版本的 SDK 发行说明，如果遇到困难将应用程序上载到存储库。

<!--Link references-->
[链接 1]: mobile-engagement-user-interface.md
[链接 2]: mobile-engagement-troubleshooting-guide.md
[链接 3]: mobile-engagement-how-tos.md
[链接 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[链接 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[6 链接]: http://go.microsoft.com/fwlink/?LinkId=525555
[链接 7]: https://account.windowsazure.com/PreviewFeatures
[链接 8]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=azuremobileengagement
[链接 9]: http://azure.microsoft.com/en-us/services/mobile-engagement/
[10 链接]: http://azure.microsoft.com/en-us/documentation/services/mobile-engagement/
[链接 11]: http://azure.microsoft.com/en-us/pricing/details/mobile-engagement/
[链接 12]: mobile-engagement-user-interface-navigation.md
[链接 13]: mobile-engagement-user-interface-home.md
[链接 14]: mobile-engagement-user-interface-my-account.md
[15 链接]: mobile-engagement-user-interface-analytics.md
[链接 16]: mobile-engagement-user-interface-monitor.md
[链接 17]: mobile-engagement-user-interface-reach.md
[链接 18]: mobile-engagement-user-interface-segments.md
[链接 19]: mobile-engagement-user-interface-dashboard.md
[链接 20]: mobile-engagement-user-interface-settings.md
[链接 21]: mobile-engagement-troubleshooting-guide-analytics.md
[链接 22]: mobile-engagement-troubleshooting-guide-apis.md
[链接 23]: mobile-engagement-troubleshooting-guide-push-reach.md
[链接 24]: mobile-engagement-troubleshooting-guide-service.md
[链接 25]: mobile-engagement-troubleshooting-guide-sdk.md
[链接 26]: mobile-engagement-troubleshooting-guide-sr-info.md
[链接 27]: mobile-engagement-user-interface-reach-campaign.md
[链接 28]: mobile-engagement-user-interface-reach-criterion.md
[链接 29]: mobile-engagement-user-interface-reach-content.md
 
测试

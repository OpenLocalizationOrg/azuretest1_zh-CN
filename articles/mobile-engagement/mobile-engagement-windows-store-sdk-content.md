---
ms.openlocfilehash: 7b64c52c0b81e1c26cceddacaf486a00a4744a06
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="Windows 通用应用程序 SDK 内容" 
    description="了解 Windows 通用应用程序 SDK 中的内容为 Azure 移动服务"                    
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

#Windows 通用应用程序 SDK 内容

本文档列出并描述通过 SDK 应用程序中部署的内容。

##`/Resources`文件夹

此文件夹包含所有移动服务需要的资源。 您还可以自定义以适合您的应用程序。

- `EngagementConfiguration.xml` ︰ 此移动服务的配置文件，这是您可以自定义移动服务设置 （报告崩溃...移动服务连接字符串）。

### /html 文件夹

- `EngagementNotification.html` : `Notification` Web 查看 html 设计。

- `EngagementAnnouncement.html` : `Announcement` Web 查看 html 设计。

### /images 文件夹

- `EngagementIconNotification.png` ︰ 左边的通知显示品牌图标替换这个品牌图标。

- `EngagementIconOk.png` :`Ok`到达内容页的操作或验证按钮的图标。

- `EngagementIconNOK.png` :`NOK`到达内容页的验证按钮处于禁用状态时使用的图标。
 
- `EngagementIconClose.png` :`Close`到达通知和消除按钮的内容的图标。

### /overlay 文件夹

- `EngagementOverlayAnnouncement.xaml` : `Announcement` Xaml 设计。

- `EngagementOverlayAnnouncement.xaml.cs` :`EngagementOverlayAnnouncement.xaml`链接的代码。
 
- `EngagementOverlayNotification.xaml` : `Notification` Xaml 设计。
 
- `EngagementOverlayNotification.xaml.cs` :`EngagementOverlayNotification.xaml`链接的代码。
 
- `EngagementPageOverlay.cs` :`Overlay`公告和通知显示代码。
  
测试

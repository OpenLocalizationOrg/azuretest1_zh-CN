---
ms.openlocfilehash: 8447dbb36a99c94996e4bf675dfdb14566b0484a
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

#如何使用移动服务集成 GCM

> [AZURE.IMPORTANT] 您必须遵循本指南之前 Android 的文档介绍集成服务如何集成过程。
>
> 本文档是集成任何时间市场支持范围模块时才有用。 集成到达活动应用程序中的，请先阅读如何集成在 Android 上达到接洽。

##简介

将集成 GCM 允许即使不运行时将您的应用程序。

通过 GCM 实际发送市场活动数据，只需告诉应用程序提取接洽推的背景信号。 如果应用程序未运行时接收 GCM 推，触发与获取推送服务服务器的连接，接洽连接一直保持大约一分钟的活动，以防用户启动应用程序推送到响应中。

接洽您的信息，为使用只[发送的同步]消息与`engagement.tickle`折叠项。

> [AZURE.IMPORTANT] 只有设备运行 Android 2.2 之上，让 Google 播放安装并让 Google 启用后台连接可以可以唤醒的 GCM; 或但是，您可以集成此代码安全地在旧版本的 Android SDK 并不能支持的 GCM （它只是使用方法） 设备上。 如果应用程序不能通过 GCM 唤醒，订婚通知将接收到下一次启动应用程序。


> [AZURE.WARNING] 如果客户端代码管理 C2DM 注册标识符，而签约 SDK 配置为使用 GCM，注册标识符冲突发生时，只有在您自己的代码不使用 C2DM 使用 GCM 合约的。

##注册于 GCM 并启用 GCM 服务

如果尚未执行此操作，您必须启用 GCM 服务您的 Google 帐户上。

在编写本文档 (2 月 5 日，2014)，您可以按照中的步骤︰ [<http://developer.android.com/guide/google/gcm/gs.html>]。

执行该过程只是为了在您的帐户上启用 GCM。 当达到**获取一个 API 键**部分时，不读取它并回到此页而不是进一步的 Google 过程。

过程说明，**项目号**用作**GCM 发件人 ID**，需要以后在此过程中。

> [AZURE.IMPORTANT] **项目编号**是不会混淆使用**项目 ID**。 现在可以在不同项目 ID （它是上新项目的名称）。 您需要集成服务 SDK 中**项目编号**， [Google 开发人员控制台]中的**概述**菜单中显示。

##SDK 集成

### 管理设备登记

每个设备必须将注册命令发送到 Google 服务器，否则不能访问它们。

设备还可以注销 GCM 通知 （卸载应用程序时，该设备将是自动注册）。

如果您使用[GCM 客户端库]，您可以直接阅读 android 的 sdk-gcm 接收。

如果您没有已登记意向给自己发送，可以使合作为您自动注册该设备。

若要启用此功能，将以下内容添加到您`AndroidManifest.xml`文件中，内侧`<application/>`标记︰

            <!-- If only 1 sender, don't forget the \n, otherwise it will be parsed as a negative number... -->
            <meta-data android:name="engagement:gcm:sender" android:value="<Your Google Project Number>\n" />

### 交流合作推动服务的注册 id 和接收通知

为了通信接洽推送服务到设备的注册 id 和接收其通知，添加以下内容您`AndroidManifest.xml`文件中，内侧`<application/>`标记 （即使您自己管理设备登记）︰

            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT" />
              </intent-filter>
            </receiver>
            
            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMReceiver" android:permission="com.google.android.c2dm.permission.SEND">
              <intent-filter>
                <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<your_package_name>" />
              </intent-filter>
            </receiver>

确保您具有下列权限您`AndroidManifest.xml`(后`</application>`标记)。

            <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
            <uses-permission android:name="<your_package_name>.permission.C2D_MESSAGE" />
            <permission android:name="<your_package_name>.permission.C2D_MESSAGE" android:protectionLevel="signature" />

##授予对服务器 API 密钥服务访问权限

如果尚未执行此操作，在[Google 开发人员控制台]创建**服务器 API 密钥**。

**不必须有 IP 限制**的服务器密钥。

在编写本文档 (2 月 5 日，2014)，时间的过程都是如下︰

-   为此，请打开[Google 开发人员控制台]。
-   与前面过程中选择的相同的项目 (**项目编号**中集成的一个`AndroidManifest.xml`)。
-   转到 Api 和授权-\>的凭据，请单击"公用 API 访问"部分中的"创建新密钥"。
-   选择"服务器密钥"。
-   在下一个屏幕上，将其保留为空**（无 IP 限制）**，然后单击创建上。
-   将复制生成的**API 密钥**。
-   转到 $/\#应用程序/您\_合约\_应用程序标识/本机推。
-   在 GCM 部分编辑的一只生成和复制的 API 密钥。

现在可在创建到达通知和轮询时，请选择"任何时间"。

> [AZURE.IMPORTANT] 交往的实际需要一个**服务器密钥**、 接洽服务器无法使用 Android 的键。

##测试

现在请通过阅读来验证您的集成如何在 Android 上的测试服务集成。


[发送到同步]:http://developer.android.com/google/gcm/adv.html#collapsible
[< http://developer.android.com/guide/google/gcm/gs.html>]:http://developer.android.com/guide/google/gcm/gs.html
[Google 开发人员控制台]:https://cloud.google.com/console
[GCM 客户端库]:http://developer.android.com/guide/google/gcm/gs.html#libs
[Google 开发人员控制台]:https://cloud.google.com/console

 
测试

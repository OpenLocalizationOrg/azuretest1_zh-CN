---
ms.openlocfilehash: 596aa58fd63c2acb7cbb037dcc911b4ed76e923e
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


#如何将 ADM 集成与合作

> [AZURE.IMPORTANT] 您必须遵循本指南之前 Android 的文档介绍集成服务如何集成过程。
>
> 本文档是市场支持任何时间集成范围模块时才有用。 集成到达活动应用程序中的，请先阅读如何集成在 Android 上达到接洽。

##简介

ADM 的集成允许您的应用程序，即使不运行时被推。

通过 ADM 实际发送市场活动数据，只需告诉应用程序提取接洽推的背景信号。 如果应用程序未运行时接收 ADM 推，触发与获取推送服务服务器的连接，接洽连接一直保持大约一分钟的活动，以防用户启动应用程序推送到响应中。

> [AZURE.IMPORTANT] 只有 Amazon 的 Kindle 设备运行 Android 4.0.3 或以上支持由 Amazon 设备消息;但是，您可以集成此代码安全地在其他设备上。 如果应用程序不能唤醒的 ADM，订婚通知将接收到下一次启动应用程序。

##注册到 ADM

如果尚未执行此操作，您必须启用 ADM Amazon 帐户上。

步骤详细信息请参阅︰ [<https://developer.amazon.com/sdk/adm/credentials.html>]。

在完成此过程，您将得到︰

-   OAuth 的凭据 （客户机 ID 和客户端密钥） 接洽，以便能够推动您的设备。
-   必须集成到您的应用程序的 API 密钥。

##SDK 集成

### 管理设备登记

每个设备必须将注册命令发送到 ADM 服务器，否则不能访问它们。

如果您已经使用[ADM 客户端库]，并已[集成的 ADM]可以直接转到 android sdk — adm 的接收。

如果您尚未集成 ADM，合作会有更简单的方法来在您的应用程序中启用它︰

编辑您`AndroidManifest.xml`文件︰

-   添加 Amazon 的命名空间，该文件应这样开始︰

        <?xml version="1.0" encoding="utf-8"?>
        <manifest xmlns:android="http://schemas.android.com/apk/res/android"
                  xmlns:amazon="http://schemas.amazon.com/apk/res/android"

-   在`<application/>`标记，请添加此内容︰

        <amazon:enable-feature
           android:name="com.amazon.device.messaging"
           android:required="false"/>
        
        <meta-data android:name="engagement:adm:register" android:value="true" />

-   添加后的 amazon 标记，您可能必须生成错误如果您的项目生成目标低于 Android 2.1。 您必须使用**Android 2.1 +**生成目标 (别担心，您仍可以`minSdkVersion`设置为 4)。
-   按下[此过程]为一项资产集成 ADM API 密钥。

然后按照下一节的说明进行操作。

### 交流合作推动服务的注册 id 和接收通知

为了通信接洽推送服务到设备的注册 id 和接收其通知，添加以下内容您`AndroidManifest.xml`文件中，内侧`<application/>`标记 （即使您使用 ADM 不合作的情况下）︰

        <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMEnabler"
          android:exported="false">
          <intent-filter>
            <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT"/>
          </intent-filter>
        </receiver>
        
         <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMReceiver"
           android:permission="com.amazon.device.messaging.permission.SEND">
          <intent-filter>
            <action android:name="com.amazon.device.messaging.intent.REGISTRATION"/>
            <action android:name="com.amazon.device.messaging.intent.RECEIVE"/>
            <category android:name="<your_package_name>"/>
          </intent-filter>
        </receiver>   

确保您具有下列权限您`AndroidManifest.xml`(前`</application>`标记)。

        <uses-permission android:name="android.permission.WAKE_LOCK"/>
        <uses-permission android:name="com.amazon.device.messaging.permission.RECEIVE"/>
        <uses-permission android:name="<your_package_name>.permission.RECEIVE_ADM_MESSAGE"/>
        <permission android:name="<your_package_name>.permission.RECEIVE_ADM_MESSAGE" android:protectionLevel="signature"/>

##授权签约 OAuth 凭据

提交您 OAuth 的凭据 （客户机 ID 和客户端密钥） 在 $/\#应用程序/您\_应用程序标识/本机推。

现在，您可以创建到达通知和轮询时选择"任何时间"。


[< https://developer.amazon.com/sdk/adm/credentials.html>]:https://developer.amazon.com/sdk/adm/credentials.html
[ADM 客户端库]:https://developer.amazon.com/sdk/adm/setup.html
[集成的 ADM]:https://developer.amazon.com/sdk/adm/integrating-app.html
[此过程]:https://developer.amazon.com/sdk/adm/integrating-app.html#Asset
 
测试

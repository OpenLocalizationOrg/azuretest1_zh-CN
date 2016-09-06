---
ms.openlocfilehash: 4b96b91fe7ce227b01712fc5c75e682731959771
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="对于 Cordova/Phonegap Azure 移动服务入门"
    description="了解如何使用 Azure 移动合作分析和推送通知 Cordova/Phonegap 应用程序。"
    services="mobile-engagement"
    documentationCenter="Mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-phonegap"
    ms.devlang="js"
    ms.topic="article" 
    ms.date="07/02/2015"
    ms.author="piyushjo" />

# 对于 Cordova/Phonegap Azure 移动服务入门

> [AZURE.SELECTOR]
- [Windows 世界](mobile-engagement-windows-store-dotnet-get-started.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-get-started.md)
- [iOS 的 Obj C](mobile-engagement-ios-get-started.md)
- [iOS 的 Swift](mobile-engagement-ios-swift-get-started.md)
- [Android](mobile-engagement-android-get-started.md)
- [Cordova](mobile-engagement-cordova-get-started.md)

本主题演示如何使用 Azure 移动合作来了解您的应用程序的使用情况和分段用 Cordova 开发移动应用程序用户发送推式通知。

在本教程中，我们将创建一个空白 Cordova 应用程序使用 Mac 和集成移动服务 SDK。 它收集基本分析数据并接收推式通知使用 Android 的 iOS 和 Google 云消息 (GCM) 苹果推送通知系统 (APNS)。 我们会将此部署到 iOS 或 Android 设备进行测试。 

> [AZURE.IMPORTANT] 若要完成本教程，您必须为活动 Azure 帐户。 如果您没有帐户，您可以在几分钟创建免费的试用帐户。 有关详细信息，请参阅<a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fwww.windowsazure.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Azure 免费试用版</a>。

本教程要求如下︰

+ XCode，您可以从安装 Mac 应用程序商店 （用于部署到 iOS）
+ [Android SDK 和仿真程序](http://developer.android.com/sdk/installing/index.html)（用于部署到 Android）
+ 推送通知可以在 APNS 从苹果开发人员中心获取的证书 (.p12)
+ 您可以获取从 Google 开发人员控制台的 GCM 的 GCM 项目编号
+ [移动服务 Cordova 插件](https://www.npmjs.com/package/cordova-plugin-ms-azure-mobile-engagement)

> [AZURE.NOTE] 在[Github](https://github.com/Azure/azure-mobile-engagement-cordova) Cordova 插件，可以找到源代码和自述文件

##<a id="setup-azme"></a>设置移动服务为您的应用程序的

1. 登录到 Azure 管理门户，然后单击**+ 新建**在屏幕的底部。

2. 单击**应用程序服务**，然后**移动的服务**，然后**创建**。

    ![][1]

3. 在显示的弹出窗口，输入以下信息︰

    ![][2]

    - **应用程序名称**︰ 您的应用程序的名称。 
    - **平台**: （选择**iOS**或**Android**取决于部署 Cordova 应用程序） 应用程序的目标平台
    - **应用程序资源名称**︰ 将通过 Api 和 Url 可访问此应用程序的名称。 
    - **位置**︰ 在其中承载此应用程序和应用程序收集的数据中心。
    - **集合**︰ 选择以前创建的集合，或选择新建收藏集。
    - **收藏集名称**︰ 表示您的应用程序的组。 这还可以确保所有应用程序都将允许聚合的计算的度量值组中。 您应该使用您的公司名称或部门这里的话。

4. 选择您刚创建的**应用程序**选项卡中的应用程序。

5. 单击以便显示的连接设置以放入您的 SDK 集成在您的移动应用程序中**的连接信息**。

    ![][3]

6. 将**连接字符串**复制-这是您需要来标识此应用程序在应用程序代码中的并从您的电话应用程序的移动合作与交流。

    ![][4]

##<a id="connecting-app"></a>将您的应用程序连接到移动服务后端

本教程介绍了"基本的集成，"这是最小的设置需要收集数据，并发送推式通知。 

我们将与 Cordova 展示集成创建一个基本的应用程序︰

###创建一个新 Cordova 项目

1. 启动在 Mac 计算机上的*终端*窗口并键入以下内容会从默认的模板创建新 Cordova 项目︰

        $ cordova create azme-cordova com.mycompany.myapp
        $ cd azme-cordova

> [AZURE.IMPORTANT] 确保最终用于部署您的 iOS 应用程序的发布配置文件使用 com.mycompany.myapp 作为应用程序 id。  

2. 执行以下步骤来配置**iOS**的项目并在 iOS 模拟器中运行它︰

        $ cordova platform add ios 
        $ cordova run ios

3. 执行以下步骤来配置为**Android**的项目并在 Android 模拟器中运行它︰

        $ cordova platform add android
        $ cordova run android

4.  添加 Cordova 控制台插件。 

        $ cordova plugin add cordova-plugin-console 

###将您的应用程序连接到移动服务后端

1. 同时提供了变量的值，以配置插件安装 Azure 移动接洽 Cordova 插件︰

        cordova plugin add cordova-plugin-ms-azure-mobile-engagement    
            --variable AZME_IOS_COLLECTION=<iOSAppCollectionName>.device.mobileengagement.windows.net
            --variable AZME_IOS_SDKKEY=... 
            --variable AZME_IOS_APPID=... 
            --variable AZME_IOS_REACH_ICON=... (icon name WITH extension) 
            --variable AZME_ANDROID_COLLECTION=<AndroidAppCollectionName>.device.mobileengagement.windows.net
            --variable AZME_ANDROID_SDKKEY=...
            --variable AZME_ANDROID_APPID=...
            --variable AZME_ANDROID_REACH_ICON=... (icon name WITHOUT extension)       
            --variable AZME_ANDROID_GOOGLE_PROJECT_NUMBER=... (From your Google Cloud console for sending push notifications) 
            --variable AZME_REDIRECT_URL=... (URL scheme which triggers the app for deep linking)
            --variable AZME_ENABLE_LOG=true|false

    例如︰ 

        cordova plugin add cordova-plugin-ms-azure-mobile-engagement   
            --variable AZME_IOS_COLLECTION=apps-Collection.device.mobileengagement.windows.net
            --variable AZME_IOS_SDKKEY=26dxxxxxxxxxxxxb794 
            --variable AZME_IOS_APPID=piyxxxxxx
            --variable AZME_IOS_REACH_ICON=icon.png 
            --variable AZME_ANDROID_COLLECTION=apps-Collection.device.mobileengagement.windows.net
            --variable AZME_ANDROID_SDKKEY=c3d296xxxxxxxxxxc6540
            --variable AZME_ANDROID_APPID=piyxxxxxxx
            --variable AZME_ANDROID_REACH_ICON=icon   
            --variable AZME_ANDROID_GOOGLE_PROJECT_NUMBER=393xxxxxxx718
            --variable AZME_REDIRECT_URL=myapp 
            --variable AZME_ENABLE_LOG=true

> [AZURE.TIP] 应用程序标识、 SDKKey 和集合可从中检索连接字符串**Endpoint={YourAppCollection.Domain};SdkKey = {YourSDKKey};应用程序标识 = {YourAppId}** 

##<a id="monitor"></a>启用实时监视

1. 在 Cordova 项目-编辑**www/js/index.js**添加接到到移动合作一次*deviceReady*事件声明新的活动。

         onDeviceReady: function() {
                app.receivedEvent('deviceready');
                AzureEngagement.startActivity("myPage",{});
            }

2. 运行该应用程序︰
        
    - **对于 iOS**
    
        在`Terminal`窗口中启动您的新的模拟器实例中的应用程序通过执行以下︰

            cordova run ios

    - **Android 的**
        
        在`Terminal`窗口启动您在新的仿真程序实例的应用程序通过执行以下︰

            cordova run android

3. 您可以看到控制台日志中的以下︰

        [Engagement] Agent: Session started
        [Engagement] Agent: Activity 'myPage' started
        [Engagement] Connection: Established
        [Engagement] Connection: Sent: appInfo
        [Engagement] Connection: Sent: startSession
        [Engagement] Connection: Sent: activity name='myPage'

###请确保您的应用程序连接的实时监控

本节说明如何确保您的应用程序连接到移动服务后端通过移动服务活动的实时监控功能。

1. 导航到您的移动服务门户

    从 Azure 门户网站，确保您是在应用程序中我们正在使用此项目，然后单击底部的**接洽**按钮上︰

    ![][6]

2. 您将停放在设置页中您**接洽门户**中为您的应用程序。 从没有单击**监视器**选项卡上所示︰

    ![][7]

3. 如果正确配置您的应用程序在仿真程序中运行，则您将看到您的应用程序正在运行时，实时记录会话︰

    ![][8]

##<a id="integrate-push"></a>推送通知和消息传递应用程序中启用

移动服务允许您与您的用户使用推送通知和 app 市场活动的上下文中的消息传递进行交互。 本模块调用到达移动服务门户中。
以下各节将安装您的应用程序来接收它们。

###配置移动服务推凭据

若要允许移动接洽，以您的名义发送推送通知，您需要允许其访问您的苹果 iOS 证书或 GCM 服务器 API 密钥。 
    
1. 导航到您的移动服务门户。 确保您是在应用程序中我们正在使用此项目，然后单击底部的**接洽**按钮上︰
    
    ![][10]
    
2. 您将在项目门户中停放在设置页中。 从**本机推**部分的有单击︰
    
    ![][11]

3. 配置 iOS 证书/GCM 服务器 API 密钥

    **[] iOS**

    一。 选择您.p12，将其上传并键入您的密码︰
    
    ![][12]

    **[] Android**

    一。 请单击**API 密钥**的 GCM 设置部分中前面的编辑图标，如下所示︰
        
    ![][20]
    
    b。 在弹出窗口中，粘贴 GCM 服务器的密钥，然后单击**确定**。
    
    ![][21]

###启用推式通知，Cordova 应用程序中

编辑**www/js/index.js**添加到移动服务来请求推式通知和声明处理程序的调用︰

     onDeviceReady: function() {
            app.receivedEvent('deviceready');
            AzureEngagement.registerForPushNotification();
            AzureEngagement.onOpenURL(function(_url) { alert(_url); });
            AzureEngagement.startActivity("myPage",{});
        }

###运行应用程序

**[] iOS**

1. 我们将使用 XCode 来构建和部署设备测试推式通知，由于 iOS 只允许推式通知到实际设备上的应用程序。 转到在其中创建 Cordova 项目的位置和导航到**...\platforms\ios**的位置。 打开 XCode 中的本机.xcodeproj 文件。 
    
2. 生成并部署到 iOS 设备使用的帐户已设置的配置文件包含证书刚上载到移动合作门户网站和应用程序 Id Cordova 应用程序创建 Cordova 应用程序时提供的匹配一个。 您可以检出中的*包标识符*您**资源\*-info.plist**在 XCode 来匹配它的文件。 

3. 指出应用程序请求权发送通知您设备上，您将看到标准 iOS 弹出式菜单。 授予的权限。 

**[] Android**

仿真程序只可用于根据 GCM 通知支持 Android 模拟器上运行 Android 的应用程序。 

    cordova run android

##<a id="send"></a>将通知发送到您的应用程序

现在，我们将创建一个简单的推式通知活动将发送推送到您的设备上运行的应用程序︰

1. 导航到移动服务门户中的范围选项卡

2. 单击创建推入市场的**新产品发布**

    ![][13]

3. 提供的输入来创建您的客户活动︰

    ![][14]

    -   提供有关市场活动的名称。 
    -   **[] Android**选择作为*系统通知***交货类型** - *简单*
    -   选择的交货时间 
        -   对于**iOS** : *"不只应用程序"*
        -   **Android**的: *"任何时间"*
        
        这是功能某些文本的简单推通知类型。
    -   在通知文本，键入第一次将推式的第一行的标题
    -   然后键入您的消息将第二行中的

4. 滚动，并在内容部分中选择**仅通知**

    ![][15]

5. [可选]您还可以提供一个操作的 URL。 请确保它使用配置插件的**AZME 重定向 URL**变量如*myapp://test*时提供的 URL 方案。  

5. 在完成向下重新设置可能最基本的市场，现在滚动和**创建**市场活动以将其保存。
    
    ![][16]

6. 最后**激活**市场活动
    
    ![][17]

7. 作为此次活动的一部分，现在应该在设备或仿真程序上看到推式通知。 

##<a id="next-steps"></a>下一步行动
[Cordova 移动服务 SDK 提供的所有方法的概述](https://github.com/Azure/azure-mobile-engagement-cordova)

<!-- URLs. -->
[移动服务 iOS SDK]: http://go.microsoft.com/?linkid=9864553

<!-- Images. -->
[1]: ./media/mobile-engagement-cordova-get-started/create-mobile-engagement-app.png
[2]: ./media/mobile-engagement-cordova-get-started/create-azme-popup.png
[3]: ./media/mobile-engagement-cordova-get-started/app-main-page-select-connection-info.png
[4]: ./media/mobile-engagement-cordova-get-started/app-connection-info-page.png
[6]: ./media/mobile-engagement-cordova-get-started/engage-button.png
[7]: ./media/mobile-engagement-cordova-get-started/clic-monitor-tab.png
[8]: ./media/mobile-engagement-cordova-get-started/monitor.png
[10]: ./media/mobile-engagement-cordova-get-started/engage-button.png
[11]: ./media/mobile-engagement-cordova-get-started/engagement-portal.png
[12]: ./media/mobile-engagement-cordova-get-started/native-push-settings.png
[13]: ./media/mobile-engagement-cordova-get-started/new-announcement.png
[14]: ./media/mobile-engagement-cordova-get-started/campaign-first-params.png
[15]: ./media/mobile-engagement-cordova-get-started/campaign-content.png
[16]: ./media/mobile-engagement-cordova-get-started/campaign-create.png
[17]: ./media/mobile-engagement-cordova-get-started/campaign-activate.png
[18]: ./media/mobile-engagement-cordova-get-started/engage-button.png
[19]: ./media/mobile-engagement-cordova-get-started/engagement-portal.png
[20]: ./media/mobile-engagement-cordova-get-started/native-push-settings.png
[21]: ./media/mobile-engagement-cordova-get-started/api-key.png

测试

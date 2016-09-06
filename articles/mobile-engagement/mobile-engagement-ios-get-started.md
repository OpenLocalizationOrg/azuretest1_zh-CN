---
ms.openlocfilehash: 33391d7491c0efd93aaa4d10e4e190d7ab250574
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="入门中目标 C Io Azure 移动服务"
    description="了解如何使用 Azure 移动接洽 iOS 应用程序的分析和推式通知。"
    services="mobile-engagement"
    documentationCenter="Mobile"
    authors="MehrdadMzfr"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="hero-article"
    ms.date="08/05/2015"
    ms.author="MehrdadMzfr" />

# 开始使用 Azure 移动合作目标 c 的 iOS 应用程序的

> [AZURE.SELECTOR]
- [通用的 Windows](mobile-engagement-windows-store-dotnet-get-started.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-get-started.md)
- [iOS |Obj C](mobile-engagement-ios-get-started.md)
- [iOS |Swift](mobile-engagement-ios-swift-get-started.md)
- [Android](mobile-engagement-android-get-started.md)
- [Cordova](mobile-engagement-cordova-get-started.md)

本主题演示如何使用 Azure 移动合作来了解您的应用程序的使用情况并为分段的 iOS 应用程序用户发送推式通知。
在本教程中，您将创建一个空白的 iOS 应用程序能够收集基本数据和接收推式通知使用苹果推送通知系统 (APNS)。 完成本教程后，您将能够广播到所有设备或特定于目标的用户根据其设备属性的推式通知。

本教程演示如何使用移动服务的简单广播的方案。 请务必按照下一个教程以查看如何使用移动服务来满足特定用户和组的设备。

本教程要求如下︰

+ XCode，您可以从您的 MAC 应用程序商店安装
+ [移动服务 iOS SDK]
+ 通知您可以获取的证书 (.p12) 推入您 Apple 开发人员中心

学完本教程是为 iOS 应用程序的所有其他移动服务教程的先决条件。

> [AZURE.IMPORTANT] 学完本教程是为 iOS 应用程序，所有其他移动服务教程的先决条件，要完成它，必须有一个活动的 Azure 帐户。 如果您没有帐户，您可以在几分钟创建免费的试用帐户。 有关详细信息，请参阅<a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fwww.windowsazure.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Azure 免费试用版</a>。

<!--
##<a id="register"></a>Enable Apple Push Notification Service

[WACOM.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]
-->

##<a id="setup-azme"></a>为您的应用程序设置移动服务

1. 登录到 Azure 的门户，然后单击**+ 新建**在屏幕的底部。

2. 单击**服务应用程序**，单击**移动服务**，然后单击**创建**。

    ![][7]

3. 在弹出窗口中显示，请输入以下信息︰

   ![][8]

   - **应用程序名称**︰ 键入您的应用程序的名称。 随意使用任何字符。
   - **平台**︰ 选择应用程序的目标平台 (**iOS**) （如果您的应用程序以多个平台为目标，请重复本教程中为每个平台）。
   - **应用程序资源名称**︰ 这是依据将是通过 Api 和 Url 可访问此应用程序的名称。 您必须只使用常规 URL 字符。 自动生成的名称应提供强大的基础。 您还应追加要避免任何名称冲突，因为该名称必须是唯一的平台名称。
   - **位置**︰ 选择在其中承载此应用程序 （和更重要的是其集合） 的数据中心。
   - **集合**︰ 如果您已经创建了一个应用程序，选择以前创建的**集合**;否则，请选择**新的收藏集**。
   - **收藏集名称**︰ 这表示您的应用程序的组。 它还可以确保所有应用程序都允许聚合的计算的度量值组中。 您应该使用您的公司名称或部门这里的话。

4. 选择您刚创建的**应用程序**选项卡中的应用程序。

5. 单击以显示的连接设置以放入您的 SDK 集成在您的移动应用程序中的**连接信息**。

    ![][10]

6. 将**连接字符串**复制-这是您需要确定此应用程序在应用程序代码中的并从您的电话应用程序连接到移动服务。

    ![][11]

##<a id="connecting-app"></a>将您的应用程序连接到移动服务后端

本教程介绍了"基本集成"，这是收集数据和发送推式通知所需的最小集。 移动服务 iOS SDK 文档中找不到完整集成文档。

我们将使用 XCode 来演示集成创建基本应用程序。

###创建一个新的 iOS 项目

如果您已有一个应用程序，并熟悉 iOS 开发，可以跳过此步骤。

1. 启动 Xcode 并在弹出窗口中，选择**创建新的 Xcode 项目**。

    ![][12]

2. 选择**一个查看应用程序**，，然后单击**下一步**。

    ![][14]

3. 填写**产品名称**、**组织名称**和**组织标识符**。 请确保您已在**语言**框中选择**目标 C** 。

    ![][13]

Xcode 创建演示应用程序，我们将移动服务集成到其中。

###将您的应用程序连接到移动服务后端

1. 下载[移动式接洽 iOS SDK]。
2. 提取。.tar.gz 文件到您的计算机中的文件夹。
3. 用鼠标右键单击该项目，然后选择**将文件添加到**。

    ![][17]

4. 导航到文件夹解压缩 SDK，选择`EngagementSDK`文件夹，然后按**确定**。

    ![][18]

5. 打开**生成阶段**选项卡，并在**链接二进制与库**菜单中添加框架如下所示︰

    ![][19]

6. 返回到 Azure 门户应用程序的**连接信息**页面中，复制连接字符串。

    ![][11]

7. 打开您在您的应用程序委托实现文件并添加下面的代码行。

        #import "EngagementAgent.h"

8. 现在将粘贴中的连接字符串`didFinishLaunchingWithOptions`委托。

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
            [...]
            [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];
            [...]
        }

##<a id="monitor"></a>启用实时监视

要开始发送数据，并确保用户处于活动状态，您必须发送到移动服务后端的至少一个屏幕 （活动）。

- 打开**ViewController.h**文件导入**EngagementViewController.h**，和用**ViewController**接口的超级类替换**EngagementViewController**，如下所示︰

![][22]

###确保您的应用程序使用实时监视连接

本节说明如何确保您的应用程序连接到移动服务后端通过移动服务的实时监控功能。

1. 导航到您的移动服务门户

    从 Azure 门户网站，请确保您是在应用程序中我们使用此项目，并且然后单击底部的**接洽**按钮︰

    ![][26]

2. 您将停放在**设置**页面服务门户中为您的应用程序。 据此，请单击**监视器**选项卡︰

    ![][30]

3. 显示器是可以实时地将启动您的应用程序中显示任何设备︰

    ![][31]

4. Xcode，在重新启动您的应用程序在模拟器中或在已连接的设备。

5. 它是否正常运行，您现在应该看到一个会话监视器中 ！

**祝贺您 ！** 在本教程中的第一步成功，有的应用程序用于连接到移动服务后端和已发送数据。

6. 单击模拟器将带回的会话数量在监视器中为 0，如上所示的**主页**按钮。

    ![][33]

##<a id="integrate-push"></a>启用推式通知和消息传递应用程序中

移动服务允许您与您的用户交互和使用推式通知和 app 市场活动的上下文中的消息到达。 本模块调用到达移动服务门户中。
以下各节将您的应用程序设置为接收它们。

### 使您的应用程序接收静默推送通知

[AZURE.INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]  


### 覆盖范围将库添加到项目

1. 右键单击您的项目。
2. 选择**将文件添加到**。
3. 浏览到解压缩 SDK 文件夹。
4. 选择`EngagementReach`文件夹。
5. 单击**添加**。

### 修改您的应用程序代理

1. 在实现文件的顶部，导入服务达到模块。

        #import "AEReachModule.h"

2. **应用程序︰ didFinishLaunchingWithOptions**方法内, 到达模块并将其传递到您现有的服务初始化行︰

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
            AEReachModule * reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
            [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
            [...]
            return YES;
        }

###使您的应用程序接收 APNS 推送通知

1. 向**应用程序︰ didFinishLaunchingWithOptions**方法中添加以下行︰

        if ([application respondsToSelector:@selector(registerUserNotificationSettings:)]) {
            [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert) categories:nil]];
            [application registerForRemoteNotifications];
        }
        else {

            [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
        }

2. 添加**应用程序︰ didRegisterForRemoteNotificationsWithDeviceToken**方法，如下所示︰

        - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken
        {
            [[EngagementAgent shared] registerDeviceToken:deviceToken];
        }

3. 添加的**didReceiveRemoteNotification:fetchCompletionHandler**方法，如下所示︰

        - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
        {
            [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
        }

###推式证书授予移动服务访问

若要允许移动接洽，以您的名义发送推式通知，您需要允许其访问您的证书。 这是通过配置和移动服务门户网站输入您的证书。 请确保您获得.p12 证书苹果文档中所述。

1. 导航到您的移动服务门户。 确保您是在应用程序中我们使用此项目，并且然后单击底部的**接洽**按钮︰

    ![][26]

2. 您将在项目门户中停放在**设置**页中。 据此，请单击要上载 p12 证书的**本机推**部分︰

    ![][27]

3. 选择您 p12，上载它，然后键入您的密码︰

    ![][28]

4. 现在添加您设置的配置文件并生成您的应用程序的目标设备。

您已经全部设置。 现在，我们将会验证您正确执行这一基本集成。

##<a id="send"></a>将通知发送到您的应用程序

现在，我们将创建简单的推式通知市场活动将发送推到我们的应用程序︰

1. 导航到移动服务门户中的**到达**选项卡。

2. 单击**新通知**来创建推式通知市场。

    ![][35]

3. 设置市场活动的第一个字段︰

    ![][36]

    -   使用您喜欢的任何名称命名您的市场活动。
    -   作为**应用程序仅从**选择的**交货时间**︰ 这是一些文本的特点简单苹果推送通知类型。
    -   在通知文本，键入第一次将推式的第一行的标题内容。
    -   然后键入您的消息将第二行的。


4. 向下滚动，然后在**内容**部分中，选择"仅通知"。

    ![][37]

5. 在完成设置最基本的市场活动的可能。 现在再次强调，向下滚动，然后创建您的市场活动，以保存它 ！
![][38]

6. 最后步骤︰ 激活您的市场活动。
![][39]

7. 您应该看到在设备中的推式通知 ！

<!-- URLs. -->
[移动服务 iOS SDK]: http://go.microsoft.com/?linkid=9864553
[移动服务 Android SDK 文档]: http://go.microsoft.com/?linkid=9874682

<!-- Images. -->
[7]: ./media/mobile-engagement-ios-get-started/create-mobile-engagement-app.png
[8]: ./media/mobile-engagement-ios-get-started/create-azme-popup.png
[10]: ./media/mobile-engagement-ios-get-started/app-main-page-select-connection-info.png
[11]: ./media/mobile-engagement-ios-get-started/app-connection-info-page.png
[12]: ./media/mobile-engagement-ios-get-started/xcode-new-project.png
[13]: ./media/mobile-engagement-ios-get-started/xcode-project-props.png
[14]: ./media/mobile-engagement-ios-get-started/xcode-simple-view.png
[17]: ./media/mobile-engagement-ios-get-started/xcode-add-files.png
[18]: ./media/mobile-engagement-ios-get-started/xcode-select-engagement-sdk.png
[19]: ./media/mobile-engagement-ios-get-started/xcode-build-phases.png
[22]: ./media/mobile-engagement-ios-get-started/xcode-view-controller.png
[26]: ./media/mobile-engagement-ios-get-started/engage-button.png
[27]: ./media/mobile-engagement-ios-get-started/engagement-portal.png
[28]: ./media/mobile-engagement-ios-get-started/native-push-settings.png
[30]: ./media/mobile-engagement-ios-get-started/clic-monitor-tab.png
[31]: ./media/mobile-engagement-ios-get-started/monitor.png
[33]: ./media/mobile-engagement-ios-get-started/monitor-0.png
[35]: ./media/mobile-engagement-ios-get-started/new-announcement.png
[36]: ./media/mobile-engagement-ios-get-started/campaign-first-params.png
[37]: ./media/mobile-engagement-ios-get-started/campaign-content.png
[38]: ./media/mobile-engagement-ios-get-started/campaign-create.png
[39]: ./media/mobile-engagement-ios-get-started/campaign-activate.png

测试

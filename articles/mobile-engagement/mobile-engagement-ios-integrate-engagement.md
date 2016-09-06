---
ms.openlocfilehash: 3e052163580edf53bba00a42116e862c6a3f360d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="Azure 移动接洽 iOS SDK 集成"
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

#如何在 iOS 集成服务

> [AZURE.SELECTOR]
- [Windows 世界](mobile-engagement-windows-store-integrate-engagement.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-integrate-engagement.md)

本过程描述了最简单的方法来激活服务活动的分析和监视 iOS 应用程序中的功能。

> [AZURE.IMPORTANT] 接洽 SDK 要求 iOS6 +: 部署目标的应用程序必须至少 iOS 6。

以下步骤是不足以激活计算有关用户、 会话、 活动、 系统崩溃和 Technicals 的所有统计信息所需的日志报告。 必须手动使用服务 API 完成计算事件、 错误和作业等其他统计数据所需的日志报告 （由于这些统计信息是依赖的应用程序，请参阅[如何使用高级的移动服务标记在 iOS 应用程序的 API](mobile-engagement-ios-use-engagement-api.md) 。

##嵌入到 iOS 项目接洽 SDK

添加到 iOS 项目接洽 SDK︰ 在 Xcode，右键单击项目并选择**"添加文件到..."** ，然后选择`EngagementSDK`文件夹。

合作需要附加的架构工作︰ 在项目资源管理器中，打开项目窗格并选择正确的目标。 然后，打开**"构建阶段"**选项卡，然后在**"链接二进制与库"**菜单中，将添加这些框架︰

> -   `AdSupport.framework` -设置为链接 `Optional`
> -   `SystemConfiguration.framework`
> -   `CoreTelephony.framework`
> -   `CFNetwork.framework`
> -   `CoreLocation.framework`
> -   `libxml2.dylib`

> [AZURE.NOTE] 可以删除 AdSupport 框架。 工作需要收集 IDFA 此框架。 但是，可以禁用 IDFA 集合\<ios sdk 接洽 idfa\>遵守新的苹果策略有关此 id。

##初始化服务 SDK

您需要修改您的应用程序代理︰

-   在实现文件的顶部，导入服务代理︰

        [...]
        #import "EngagementAgent.h"

-   初始化方法内合作 '**applicationDidFinishLaunching:**或**应用程序︰ didFinishLaunchingWithOptions:**:

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
          [...]
          [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];
          [...]
        }

##基本报告

### 推荐的方法︰ 重载您`UIViewController`类

为了激活接洽，以计算用户、 会话、 活动、 系统崩溃和技术统计数据所需的所有日志的报告，您可以只是使所有您`UIViewController`子类继承`EngagementViewController`类 (相同规则的`UITableViewController` -\> `EngagementTableViewController`)。

**而无需接洽︰**

    #import <UIKit/UIKit.h>

    @interface Tab1ViewController : UIViewController<UITextFieldDelegate> {
      UITextField* myTextField1;
      UITextField* myTextField2;
    }

    @property (nonatomic, retain) IBOutlet UITextField* myTextField1;
    @property (nonatomic, retain) IBOutlet UITextField* myTextField2;

**与合作︰**

    #import <UIKit/UIKit.h>
    #import "EngagementViewController.h"

    @interface Tab1ViewController : EngagementViewController<UITextFieldDelegate> {
      UITextField* myTextField1;
      UITextField* myTextField2;
    }

    @property (nonatomic, retain) IBOutlet UITextField* myTextField1;
    @property (nonatomic, retain) IBOutlet UITextField* myTextField2;

### 备选方法︰ 调用`startActivity()`手动

如果您不能或不想重载您`UIViewController`的类，而是可以通过调用来启动您的活动`EngagementAgent`的直接方法。

> [AZURE.IMPORTANT] 自动调用 SDK iOS`endActivity()`时关闭该应用程序的方法。 因此，*强烈*建议您调用是`startActivity`方法，只要该活动的用户的更改，并*从不*调用`endActivity`方法，因为调用此方法会强制结束当前会话。

##报告位置

苹果的服务条款不允许应用程序使用位置跟踪用于统计目的只。 因此，它建议只有当您的应用程序还使用位置跟踪的另一个原因时，才启用位置报告。

从 iOS 8，您必须提供您的应用程序如何使用通过您的应用程序的 Info.plist 文件中设置的键[NSLocationWhenInUseUsageDescription]或[NSLocationAlwaysUsageDescription]的字符串的位置服务的说明。 如果希望在后台服务和报告位置，添加 NSLocationAlwaysUsageDescription 的关键。 在所有其他情况下，添加 NSLocationWhenInUseUsageDescription 的关键。

### 惰性的区域位置报告

惰性的区域位置报告，报告国家、 区域和与设备相关联的位置。 这种类型的位置报告只使用网络位置 （基于单元格 ID 或 WIFI）。 设备区域中每会话的最大一次报告。 从未使用 GPS，，因此这种类型的位置报告很少 （更不必说 no） 对电池的影响。

报告的区域用于计算地理统计信息的用户、 会话、 事件和错误。 他们也可以用作访问活动中的条件。 这要归功于[设备的 API]，可以检索报告设备的最后一个已知的区域。

启用惰性区位置报告，请在初始化服务代理后添加以下行︰

    - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
    {
      [...]
      [[EngagementAgent shared] setLazyAreaLocationReport:YES];
      [...]
    }

### 实时位置报告

实时位置报告，报告的纬度和经度与设备相关联。 默认情况下，这种类型的位置报告只使用网络位置 （基于单元格 ID 或 WIFI），并报告才处于活动状态时 （即在会话期间） 在前台运行应用程序。

实时的位置是*没有*用来计算统计信息。 它们的唯一用途是允许使用的实时地理围墙\<范围的听众-geofencing\>在到达市场的条件。

启用实时位置报告，请在初始化服务代理后添加以下行︰

    [[EngagementAgent shared] setRealtimeLocationReport:YES];

#### GPS 基于报告

默认情况下，实时位置报告只使用基于网络的位置。 若要启用基于 GPS 位置 （它们是更精确），添加︰

    [[EngagementAgent shared] setFineRealtimeLocationReport:YES];

#### 背景报告

默认情况下，实时位置报告后才可用 （例如在会话期间） 在前台运行应用程序。 若要启用报告还以背景的形式，添加︰

    [[EngagementAgent shared] setBackgroundRealtimeLocationReport:YES withLaunchOptions:launchOptions];

> [AZURE.NOTE] 当应用程序在后台运行，仅基于位置的网络报告，即使您启用 GPS。

此函数实现将调用[startMonitoringSignificantLocationChanges] ，当您的应用程序转到后台。 请注意，它将自动重新启动应用程序到背景如果到达新位置事件。

##高级报告

或者，如果您想要应用程序特定事件、 错误和作业报告，您需要通过方法使用服务 API`EngagementAgent`类。 此类的对象可以通过调用来检索`[EngagementAgent shared]`静态方法。

服务 API 允许使用所有服务活动的高级功能和详细的方式在 iOS 上使用服务 API (如下所示的技术文档，以及`EngagementAgent`类)。

##禁用 IDFA 回收

默认情况下，合作将使用[IDFA]来唯一地标识用户。 但是，如果您没有使用该应用程序的其他地方广告，您可能会被拒绝的应用程序商店审查过程。 可以通过添加预处理器宏禁用 IDFA 收集`ENGAGEMENT_DISABLE_IDFA`pch 文件中 (或在`Build Settings`应用程序)。 这将确保没有任何参考`ASIdentifierManager`，`advertisingIdentifier`或`isAdvertisingTrackingEnabled`应用程序生成中。

在**prefix.pch**文件中的集成︰

    #define ENGAGEMENT_DISABLE_IDFA
    ...

您可以验证 IDFA 集合被正确禁用应用程序中通过检查服务测试日志。 请参阅集成测试\<ios 的 sdk-服务-测试-idfa\>文档以了解更多信息。

##禁用日志报告

### 使用方法调用

如果您需要合作，以停止发送日志，您可以调用︰

    [[EngagementAgent shared] setEnabled:NO];

此调用是持久︰ 它使用`NSUserDefaults`来存储信息。

您可以启用日志报告再次调用同一个函数`YES`。

### 设置包中集成

而不是调用此函数，您还可以将集成此设置直接在您现有的`Settings.bundle`文件。 该字符串`engagement_agent_enabled`必须用作首选项标识符，它必须关联到一个切换开关 (`PSToggleSwitchSpecifier`)。

下面的示例对`Settings.bundle`演示如何实现它︰

    <dict>
        <key>PreferenceSpecifiers</key>
        <array>
            <dict>
                <key>DefaultValue</key>
                <true/>
                <key>Key</key>
                <string>engagement_agent_enabled</string>
                <key>Title</key>
                <string>Log reporting enabled</string>
                <key>Type</key>
                <string>PSToggleSwitchSpecifier</string>
            </dict>
        </array>
        <key>StringsTable</key>
        <string>Root</string>
    </dict>

<!-- URLs. -->
[设备 API]: http://go.microsoft.com/?linkid=9876094
[NSLocationWhenInUseUsageDescription]:https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26
[NSLocationAlwaysUsageDescription]:https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW18
[startMonitoringSignificantLocationChanges]:http://developer.apple.com/library/IOs/#documentation/CoreLocation/Reference/CLLocationManager_Class/CLLocationManager/CLLocationManager.html#//apple_ref/occ/instm/CLLocationManager/startMonitoringSignificantLocationChanges
[IDFA]:https://developer.apple.com/library/ios/documentation/AdSupport/Reference/ASIdentifierManager_Ref/ASIdentifierManager.html#//apple_ref/occ/instp/ASIdentifierManager/advertisingIdentifier

测试

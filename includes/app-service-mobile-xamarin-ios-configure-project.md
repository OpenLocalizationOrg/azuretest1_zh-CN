---
ms.openlocfilehash: e8e4552d53edbb4f1aabfa91bc23e2ce83bf3532
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
###在 Xamarin Studio 中

1. 在 Xamarin.Studio，打开**Info.plist**，并使用您在前面创建的 ID 更新**包标识符**。

    ![][121]

2. 向下滚动到**后台模式**并检查**启用后台模式**和**远程通知**框。 

    ![][122]

3. 双击打开**项目选项**解决方案面板中的项目。

4.  选择**包签名的 iOS**在**生成**并选择相应**的身份**和**资源调配配置文件**您刚刚为此项目设置。 

    ![][120]

    这样可以确保项目进行代码签名使用新的配置文件。 官方的 Xamarin 设备置备文档，请参阅[Xamarin 设备资源调配]。

### 在 Visual Studio 中

1. 在 Visual Studio 中，用鼠标右键单击该项目，然后单击**属性**。

3. 在属性页中， **iOS 应用程序**选项卡，并使用您在前面创建的 ID 更新**标识符**。

    ![][123]

4. 在**iOS 包签名**选项卡中，选择相应**的身份**和**资源调配配置文件**您刚刚为此项目设置。 

    ![][124]

    这样可以确保项目进行代码签名使用新的配置文件。 官方的 Xamarin 设备置备文档，请参阅[Xamarin 设备资源调配]。

[120]:./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-20.png
[121]:./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-21.png
[122]:./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-22.png
[123]:./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-23.png
[124]:./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-24.png

[Xamarin 设备置备]: http://developer.xamarin.com/guides/ios/getting_started/installation/device_provisioning/
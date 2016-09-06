---
ms.openlocfilehash: 6ffdea893f3a6d284f7c386fe4d2ccb6932cf711
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
1. 通过单击的 Android Studio 工具栏上的图标或单击**工具**打开 Android SDK 经理 -> **Android** -> **SDK 管理器**菜单上。 在项目中查找使用 Android SDK 的目标版本，打开它，并选择**Google 的 Api**，如果尚未安装。

2. 向下滚动到**其他方案**，展开它，并选择**Google 播放服务**，如下所示。 单击**安装程序包**。 请注意 SDK 的路径，在下面的步骤中使用。 

    ![](./media/notification-hubs-android-get-started/notification-hub-create-android-app4.png)


3. 打开应用程序目录中的**build.gradle**文件。

    ![](./media/mobile-services-android-get-started-push/android-studio-push-build-gradle.png)

4. 添加*依赖项*下的这行︰ 

        compile 'com.google.android.gms:play-services-base:6.5.87'

5. 在*defaultConfig*，改为*minSdkVersion* 9。
 
6. 单击工具栏中的**同步项目 Gradle 文件**图标。

7. 打开**AndroidManifest.xml** ，然后将此标记添加到*应用程序*的标记。

        <meta-data android:name="com.google.android.gms.version"
            android:value="@integer/google_play_services_version" />
 





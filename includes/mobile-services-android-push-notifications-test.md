---
ms.openlocfilehash: ccb7638c70da94e56282117f9f4475bb0443ef37
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

###设置测试的 Android 模拟器
在仿真程序中运行此应用程序时，请确保您使用 Android 虚拟设备 (AVD) 支持 Google 的 Api。

> [AZURE.IMPORTANT] 若要接收推式通知，您必须设置一个 Google 帐户 Android 虚拟设备上 （在仿真器中，导航到**设置**，然后单击**添加帐户**）。 此外，还要确保仿真程序连接到互联网。

1. 从**工具**，单击**打开 Android 仿真程序管理器**，选择您的设备，然后单击**编辑**。

    ![Android 的虚拟设备管理器](./media/mobile-services-android-push-notifications-test/notification-hub-create-android-app7.png)

2. **Google 的 Api**中选择**目标**，然后单击**确定**。

    ![编辑 Android 的虚拟设备](./media/mobile-services-android-push-notifications-test/notification-hub-create-android-app8.png)

3. 在顶部的工具栏上，单击**运行**，然后选择应用程序。 此启动仿真程序，并运行该应用程序。

  该应用程序从 GCM 检索*registrationId* ，并通知中心注册。

###插入一个新项，生成通知。

1. 在应用程序中，键入有意义的文本，例如，_新的移动服务任务_，然后单击**添加**按钮。

2. 从屏幕打开以查看通知的设备的通知中心的顶部向下刷。

    ![通知中心查看通知](./media/mobile-services-android-push-notifications-test/notification-area-received.png)
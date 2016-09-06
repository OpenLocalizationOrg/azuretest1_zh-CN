---
ms.openlocfilehash: c666fa6cc5ee6e55f538785a8a841b08de20300e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---


1. 通过单击**窗口**顶部的工具栏上的 Eclipse 打开 Android SDK 管理器。 查找在项目属性中指定的 Android sdk 的目标版本，打开它，然后选择**Google 的 Api**。

2. 向下滚动到**其他方案**，展开它，并选择**Google 播放服务**，如下所示。 单击**安装程序包**。 请注意 SDK 的路径，在下面的步骤中使用。 重新启动 Eclipse。

    ![](./media/notification-hubs-android-get-started/notification-hub-create-android-app4.png)


3. 将 Google 播放服务 SDK 安装在您的项目。 在 Eclipse 中，单击**文件**，然后**导入**。 选择**Android**，然后**到工作区中现有的 Android 代码**，然后单击**下一步**。 单击**浏览**，导航到 Android SDK 的路径 (通常是在一个名为文件夹中`adt-bundle-windows-x86_64`内包含 Eclipse 的文件夹)，然后转到`\extras\google\google_play_services\libproject`子文件夹，并那里选择 google 播放服务 lib 文件夹中，然后单击**确定**。 检查**复制到工作区中的项目**复选框，然后单击**完成**。

    ![](./media/mobile-services-android-get-started-push/mobile-eclipse-import-Play-library.png)

4. 接下来您必须引用您刚导入，从项目的 Google 播放服务 SDK 库。 

5. 在**包资源管理器**中右键单击项目并选择*属性*。
 
6. 在属性窗口中，选择左边的 Android。

    ![](./media/mobile-services-android-get-started-push/mobile-google-set-project-properties.png)


7. 在**项目的生成目标**，确保`Google APIs x86`(或`Google APIs`，这取决于您的开发平台) 选择相应的 SDK 级别。

 
8. 在**库**部分中，选择**添加**，选择 Google 播放服务项目 (*google 播放服务 lib*) 并单击**确定**。

9. **应用**单击然后单击**确定**。




---
ms.openlocfilehash: 3f6b27fcdf2825ce0b7c13e743ce315f64faad54
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
本教程的最后阶段是生成并运行您的新应用程序。

1. 浏览到保存压缩的项目文件的位置，然后展开到 Android Studio 项目目录中的文件在您的计算机上。

2. 打开 Android Studio。 如果您正在处理一个项目，它出现，关闭项目 (文件 = > 关闭项目)。

3. 选择**打开一个现有的 Android Studio 项目**，浏览到项目的位置，然后单击**确定。** 

    ![](./media/mobile-services-android-get-started/android-studio-import-project.png)

4. 在左侧的**项目资源管理器**窗口中，确保*项目*选项卡已选中，然后打开**应用程序**、 **src**、 **java**并双击**ToDoactivity**，

    ![](./media/mobile-services-android-get-started/Android-Studio-quickstart.png)


5. 如果您下载 SDK 版本 2.0，您需要使用 Url 和您的移动服务密钥更新的代码︰
    -   在**TodoActivity.java**中找到**OnCreate**方法和定位实例化移动服务客户端的代码。 该代码是在上图中可见。
    -   "MobileServiceUrl"替换为您的移动服务实际的 Url。
    -   "AppKey"替换为您的移动服务密钥。
    -   更多详细信息请参考教程[中添加到现有应用程序的移动服务](../articles/mobile-services/mobile-services-android-get-started-data.md)。 

6. 从**运行**菜单上，单击**运行**在 Android 模拟器启动项目。

    > [AZURE.IMPORTANT] 若要能够在 Android 模拟器上运行该项目，您必须定义至少一个 Android 虚拟设备 (AVD)。 AVD 管理器用于创建和管理这些设备。

7. 在应用程序中，键入有意义的文本，例如_完成本教程_，，然后单击**添加**。

    ![][10]

    将 POST 请求发送到 Azure 中承载新的移动服务。 从请求的数据插入到 TodoItem 表中。 在表中存储的项目由手机信息服务，以及数据显示在列表中。

    > [AZURE.NOTE] 您可以查看您的移动服务来查询、 插入 ToDoActivity.java 文件中找到的数据访问的代码。

8. 回在管理门户中，单击**数据**选项卡，然后单击**TodoItems**表。

    ![](./media/mobile-services-android-get-started/mobile-data-tab1.png)

    这允许您浏览该应用程序插入表的数据。

    ![](./media/mobile-services-android-get-started/mobile-data-browse.png)


<!-- Images. -->
[0]: ./media/mobile-services-android-get-started/mobile-quickstart-completed-android.png
[6]: ./media/mobile-services-android-get-started/mobile-portal-quickstart-android.png
[7]: ./media/mobile-services-android-get-started/mobile-quickstart-steps-android.png
[8]: ./media/mobile-services-android-get-started/Android-Studio-quickstart.png
[10]: ./media/mobile-services-android-get-started/mobile-quickstart-startup-android.png
[11]: ./media/mobile-services-android-get-started/mobile-data-tab.png
[12]: ./media/mobile-services-android-get-started/mobile-data-browse.png
[14]: ./media/mobile-services-android-get-started/android-studio-import-project.png
[15]: ./media/mobile-services-android-get-started/mobile-services-import-android-project.png

<!-- URLs. -->
[添加到现有应用程序的移动服务]: ../articles/mobile-services/mobile-services-android-get-started-data.md
[开始使用身份验证]: ../articles/mobile-services/mobile-services-android-get-started-users.md
[开始使用推式通知]: ../articles/mobile-services/mobile-services-javascript-backend-android-get-started-push.md
[Android SDK]: https://go.microsoft.com/fwLink/p/?LinkID=280125
[Android 的 Studio]: https://developer.android.com/sdk/index.html
[移动服务 Android SDK]: https://go.microsoft.com/fwLink/p/?LinkID=266533

[管理门户]: https://manage.windowsazure.com/
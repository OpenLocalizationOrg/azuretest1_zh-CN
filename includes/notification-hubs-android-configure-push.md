---
ms.openlocfilehash: 1ed5e99991d91e4dabf39333c08c0c84dcc024e8
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---


1. 登录到[Azure 管理门户](https://manage.windowsazure.com/)，然后单击**+ 新建**在屏幕的底部。

2. 单击**应用程序服务**，再**服务总线**，然后**通知中心**，然后**快速创建**。

3. 通知中心为键入一个名称，选择您需要的区域，然后单击**创建新的通知中心**。

    ![集线器属性设置通知](./media/notification-hubs-android-configure-push/notification-hub-create-from-portal2.png)

4. 在**服务总线**上，单击您刚创建的 (通常为***通知中心名称*-ns**) 的命名空间。

5. 在您的名称空间，请单击**通知集线器**标签您顶部，和然后单击刚创建的通知中心。

6. 单击顶部的**配置**选项卡上，输入您在上一节中，获得的**API 密钥**值，然后单击**保存**。

    ![在配置选项卡中设置 GCM API 密钥](./media/notification-hubs-android-configure-push/notification-hub-configure-android.png)

7. 选择**仪表板**选项卡的顶部，然后单击**视图连接字符串**。 请注意两个连接字符串。

通知中心现已配置为使用 GCM，并且必须对两者的连接字符串，注册您的应用程序来接收通知并发送推式通知。

---
ms.openlocfilehash: 653a569ea2d14f2f11bdbb99caa7a10aa7405b14
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

（可选） 可以与您本地计算机或虚拟机上运行，才能发布到 Azure 的移动服务测试推式通知。 若要执行此操作，必须设置通知中心由您的应用程序的 web.config 文件中的信息。 此信息仅用于运行时本地连接到通知中心;发布到 Azure 时，它将被忽略。

1. 重新在您的移动服务**推入**选项卡上，单击**通知中心**链接。

    ![](./media/mobile-services-dotnet-backend-configure-local-push/link-to-notification-hub.png)

    这会定位到由您的移动服务通知中心。

2. 在通知中心页中，记下名称的通知网络集线器，然后单击**视图连接字符串**。

    ![](./media/mobile-services-dotnet-backend-configure-local-push/notification-hub-page.png)

3. 在**访问连接信息**，复制**DefaultFullSharedAccessSignature**连接字符串。

    ![](./media/mobile-services-dotnet-backend-configure-local-push/notification-hub-connection-string.png)

4. 在 Visual Studio 中的移动服务项目中，打开 Web.config 文件服务，并在**connectionStrings**中，用前一步骤中的连接字符串连接字符串替换为**MS_NotificationHubConnectionString** 。

5. 在**appSettings**，替换的通知中心名称**MS_NotificationHubName**应用程序设置的值。

现在，移动服务项目配置为在本地运行时连接到 Azure 中的通知中心。 请注意，一定要用相同的通知中心名称和连接字符串作为门户，因为这些 Web.config 项目设置 Azure 中运行时门户网站设置覆盖。
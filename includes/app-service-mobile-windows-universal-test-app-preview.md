---
ms.openlocfilehash: 1d64f8c1bdc1f09ec9261a7469b3613719682657
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

请返回到 Visual Studio 并选择共享代码客户端应用程序项目 (它类似命名为`<your app name>.Shared`)

1. 展开的**App.xaml**文件，然后打开**App.xaml.cs**文件。 找到的声明`MobileService`使用本地主机 URL 初始化成员。 注释掉此声明 (与`CTRL+K,CTRL+C`) 和取消注释该声明 (`CTRL+K,CTRL+U`) 连接到您的托管服务︰

        // This MobileServiceClient has been configured to communicate with your local
        // test project for debugging purposes.
        //public static MobileServiceClient MobileService = new MobileServiceClient(
        //    "http://localhost:58454"
        //);

        // This MobileServiceClient has been configured to communicate with the Azure Mobile Service and
        // Azure Gateway using the application key. You're all set to start working with your Mobile Service!
        public static MobileServiceClient MobileService = new MobileServiceClient(
            "https://mymobileapp-code.azurewebsites.net",
            "https://myresourcegroupgateway.azurewebsites.net/Microsoft.Azure.AppService.ApiApps.Gateway",
            "XXXX-APPLICATION-KEY-XXXXX"
        );

2. 按 F5 键以重新生成该项目并启动 Windows 应用商店应用程序，这应该是默认启动项目。

2. 在应用程序中，在**插入 TodoItem**文本框中，键入有意义的文本，例如，*完成本教程*，，然后单击**保存**。

    ![](./media/app-service-mobile-windows-universal-test-app-preview/mobile-quickstart-startup.png)

    这将 POST 请求发送到 Azure 中承载新的移动应用程序后端。

3. 停止调试并到 Windows Phone 存储应用程序更改默认启动 Windows 通用解决方案中的项目 (右击`<your app name>.WindowsPhone`项目，然后单击**设为启动项目**)，然后再次按 F5。

    ![](./media/app-service-mobile-windows-universal-test-app-preview/mobile-quickstart-completed-wp8.png)

    注意到 Windows 应用程序启动后，从移动应用程序加载上一步中保存的数据。


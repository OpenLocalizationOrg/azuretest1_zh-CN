---
ms.openlocfilehash: d9048fdf6c4639532a5e8b4cb0c22b7d73feba03
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
7. 右击**解决方案**资源管理器 API 的应用程序项目并选择**发布**以打开发布对话框中。 应预先选择您在前面创建的发布配置文件。 

9. 单击**发布**着手部署过程。 

    ![API 的应用程序部署](./media/app-service-api-pub-web-deploy/26-5-deployment-success-v3.png)

    **Azure 服务活动应用程序**窗口将显示部署进度。 

    ![Azure 应用程序服务活动窗口的状态通知](./media/app-service-api-pub-web-deploy/26-5-deployment-success-v4.png)

    在部署过程中，Visual Studio 将自动尝试重新启动*网关*。 网关是一个 web 应用程序处理的所有 API 应用程序中的资源组中，管理功能，必须重新启动才能识别 API 的应用程序的 API 定义或*apiapp.json*文件中的更改。 
 
    如果您使用其他方法部署 API 的应用程序，或者如果 Visual Studio 未能重新启动网关，您可能需要手动重新启动该网关。 以下步骤说明如何做到这一点。

1. 在浏览器中，转到[Azure 预览门户](https://portal.azure.com)。 

2. 导航至**应用程序 API**刀片式服务器 API 应用程序部署。

    有关**API 的应用程序**刀片式服务器，以及如何找到它的信息，请参阅[管理 API 的应用程序](../articles/app-service-api/app-service-api-manage-in-portal.md)。

4. 单击**网关**链接。

3. 在**网关**刀片式服务器，单击**重新启动**。

    ![](./media/app-service-api-pub-web-deploy/restartgateway.png)

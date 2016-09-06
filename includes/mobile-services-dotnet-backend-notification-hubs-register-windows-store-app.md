---
ms.openlocfilehash: d1362ae9cb1060851d6d68a21a3254e95bef215b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---


1. 如果尚未注册您的应用程序，导航到 Windows 应用商店应用程序的开发人员中心在[提交应用程序页]，使用您的 Microsoft 帐户，登录，然后单击**应用程序名称**。

    ![](./media/mobile-services-dotnet-backend-notification-hubs-register-windows-store-app/mobile-services-submit-win8-app.png)

2. 在**应用程序名**中键入您的应用程序的名称，请单击**保留应用程序名称**，然后单击**保存**。

    ![](./media/mobile-services-dotnet-backend-notification-hubs-register-windows-store-app/mobile-services-win8-app-name.png)

    这将创建新的 Windows 应用商店注册为您的应用程序。

3. 在 Visual Studio，打开 Windows 应用商店应用程序创建的项目，当您完成本教程**开始使用移动服务**。

4. 在解决方案资源管理器中右击该项目，单击**存储**，然后单击**将相关联的应用程序与存储...**。 

    ![](./media/mobile-services-dotnet-backend-notification-hubs-register-windows-store-app/mobile-services-store-association.png)

    此时将显示向导**将应用程序与 Windows 应用商店**。

5. 在向导中，单击**登录**，然后再使用您的 Microsoft 帐户登录。

6. 选择您在步骤 2 中注册该应用程序，请单击**下一步**，，然后单击**关联**。

    ![](./media/mobile-services-dotnet-backend-notification-hubs-register-windows-store-app/mobile-services-select-app-name.png)

    将所需的 Windows 应用商店注册信息添加到应用程序清单。    

7. （可选）重复步骤 4-6 还注册 Windows Phone 存储通用的 Windows 应用程序的项目。

8. 在新应用程序的 Windows 开发人员中心页面，单击**服务**。 

    ![](./media/mobile-services-dotnet-backend-notification-hubs-register-windows-store-app/mobile-services-win8-edit-app.png) 

9. 在服务页中单击**Azure 移动服务**下的**Live 服务站点**。

    ![](./media/mobile-services-javascript-backend-register-windows-store-app/mobile-services-win8-edit2-app.png)

10. 单击**验证您的服务**，请记下**客户的机密**和**包的安全标识符 (SID)**的值。 

    ![](./media/mobile-services-dotnet-backend-notification-hubs-register-windows-store-app/mobile-services-win8-app-push-auth.png)

    > [AZURE.NOTE] 客户的机密和包 SID 是重要的安全凭据。 不要与任何人共享这些机密信息或将其与您的应用程序一起分发。 

11. 登录到[Azure 管理门户]，单击**移动服务**，然后单击应用程序。

    ![](./media/mobile-services-dotnet-backend-notification-hubs-register-windows-store-app/mobile-services-selection.png)

12. 单击**推送**选项卡并输入**客户的机密**和从在步骤 4 中的 WNS 获得**包的 SID**值然后单击**保存**。 

    ![](./media/mobile-services-dotnet-backend-notification-hubs-register-windows-store-app/mobile-push-tab.png)

    >[AZURE.NOTE]在门户中的**推**选项卡中设置增强推式通知的 WNS 凭据时，它们会被共享与通知集线器配置您的应用程序的通知中心。

<!-- URLs. -->
[提交应用程序页]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[Azure 的管理门户]: https://manage.windowsazure.com/

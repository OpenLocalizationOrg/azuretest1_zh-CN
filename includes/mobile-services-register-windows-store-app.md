---
ms.openlocfilehash: aecf97f57123a9ec714ceef20a0625beaf87a3e1
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

1. 如果尚未注册您的应用程序，导航到 Windows 应用商店应用程序的开发人员中心在[提交应用程序页]，使用您的 Microsoft 帐户，登录，然后单击**应用程序名称**。

    ![](./media/mobile-services-register-windows-store-app/mobile-services-submit-win8-app.png)

2. 选择**创建新的应用程序通过保留一个唯一的名称**并单击**继续**，然后在**应用程序名**中键入您的应用程序的名称单击**保留应用程序名称**，，然后单击**保存**。

    ![](./media/mobile-services-register-windows-store-app/mobile-services-win8-app-name.png)

    这将创建新的 Windows 应用商店注册为您的应用程序。

3. 在 Visual Studio 中，打开您完成本教程[开始使用移动服务]时创建的项目。

4. 在解决方案资源管理器中，右击 Windows 应用商店应用程序项目，单击**存储**，然后单击**将相关联的应用程序与存储...**。 

    ![](./media/mobile-services-register-windows-store-app/mobile-services-store-association.png)

    此时将显示向导**将应用程序与 Windows 应用商店**。

5. 在向导中，单击**登录**，然后再登录与您的 Microsoft 帐户，选择您在步骤 2 中注册该应用程序，单击**下一步**，，然后单击**关联**。

    将所需的 Windows 应用商店注册信息添加到应用程序清单。   

6. （可选）通用的 Windows 应用程序中，请重复步骤 4 和 5 为 Windows Phone 存储项目。 

6. 在新应用程序的 Windows 开发人员中心页面，单击**服务**。 

    ![](./media/mobile-services-register-windows-store-app/mobile-services-win8-edit-app.png) 

7. 在服务页中单击**Azure 移动服务**下的**Live 服务站点**。

    ![](./media/mobile-services-register-windows-store-app/mobile-services-win8-edit2-app.png) 

8. 单击**API 设置**选择启用**移动或桌面客户端应用程序**，提供移动服务 URL 作为**目标域**，提供的值`https://<mobile_service>.azure-mobile.net/login/microsoftaccount/`在**重定向的 URL**，然后单击**保存**。

    ![](./media/mobile-services-register-windows-store-app/mobile-services-win8-app-push-auth-2.png)

9. 在**应用程序设置**中，记下**客户机 ID**、**客户的机密**和**包的安全标识符 (SID)**的值。 

    ![](./media/mobile-services-register-windows-store-app/mobile-services-win8-app-push-auth.png)

    >[AZURE.NOTE]客户的机密和包 SID 是重要的安全凭据。 不要与任何人共享这些机密信息或将其与您的应用程序一起分发。

10. 登录到[Azure 管理门户]，单击**移动服务**，然后单击应用程序。

11. 单击**标识**选项卡，输入**客户的机密**和从在步骤 4 中的 WNS 获得**包的 SID**值，然后单击**保存**。

    ![](./media/mobile-services-register-windows-store-app/mobile-push-tab.png)

13. 单击**标识**选项卡。 注意到前一步骤中已设置**客户机密**和**包 SID**值。 输入您先前所做的请注意，然后单击**保存**的**客户机 ID** 。

    ![](./media/mobile-services-register-windows-store-app/mobile-services-identity-tab.png)
 
现在您就可以使用 Microsoft 帐户进行身份验证，在您的应用程序。  

<!-- Anchors. -->

<!-- Images. -->
 

<!-- URLs. -->
[开始使用移动服务]: /develop/mobile/tutorials/get-started/#create-new-service
[提交应用程序页]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[Azure 的管理门户]: https://manage.windowsazure.com/

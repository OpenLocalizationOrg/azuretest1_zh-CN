---
ms.openlocfilehash: af4e5d041fbd7f26960797e603621f9185c0858c
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
1. [如何配置您的移动应用程序使用 Azure Active Directory]主题按照注册 Azure Active Directory 租户您移动应用程序的后端。

2. 定位到**Active Directory**在[Azure 的管理门户]

   ![](./media/app-service-mobile-adal-register-app/app-service-navigate-aad.png)

3. 选择您的目录，然后选择**应用程序**选项卡的顶部。 若要创建新的应用程序注册的底部，单击**添加**。 

4. 单击**添加我的公司正在开发的应用程序**。

5. 在添加应用程序向导中，您的应用程序输入一个**名称**并单击**本机客户端应用程序**类型。 然后单击以继续。

6. 在**重定向 URI**框中，输入 / 登录/完成应用程序的服务网关的终结点。 此值应与 https://contoso.azurewebsites.net/login/done 类似。

7. 本机应用程序添加后，单击**配置**选项卡。 复制**客户机 ID**。 您将稍后需要它。

8. 向下滚动页面到**到其他应用程序的权限**部分，单击**添加应用程序**。

9. 搜索以前注册的 web 应用程序并单击加号图标。 再单击检查关闭对话框。

10. 在刚添加的新项，打开**授予权限**下拉列表并选择**访问 （应用程序名）**。 然后单击**保存**

   ![](./media/app-service-mobile-adal-register-app/aad-native-client-add-permissions.png)

您的应用程序现在 AAD 中，以便用户可以登录使用 AAD 单一登录配置。

[Azure 的管理门户]: https://manage.windowsazure.com/
[如何使用 Azure Active Directory 配置您的移动应用程序]: ../articles/app-service-how-to-configure-active-directory-authentication-preview.md

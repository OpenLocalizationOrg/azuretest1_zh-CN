---
ms.openlocfilehash: ba78002e878d045c48e96e4359b75771d38eab14
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
## <a name="register-app-aad"></a>Azure Active Directory 中注册您的客户端应用程序

1. 定位到**Active Directory**在[Azure 管理门户]中，然后单击您的目录。

   ![](./media/mobile-services-dotnet-adal-register-client/mobile-services-select-aad.png)

2. 单击**应用程序**选项卡的顶部，然后单击**添加**到应用程序。 

   ![](./media/mobile-services-dotnet-adal-register-client/mobile-services-aad-applications-tab.png)

3. 单击**添加我的公司正在开发的应用程序**。

4. 在添加应用程序向导中，您的应用程序输入一个**名称**并单击**本机客户端应用程序**类型。 然后单击以继续。

   ![](./media/mobile-services-dotnet-adal-register-client/mobile-services-native-selection.png)

5. 在**重定向 URI**框中，输入 / 登录/完成您的移动服务的终结点。 此值应与 https://todolist.azure-mobile.net/login/done 类似。

   ![](./media/mobile-services-dotnet-adal-register-client/mobile-services-native-redirect-uri.png)

6. 单击**配置**选项卡为本机应用程序和复制**客户机 ID**。 您将稍后需要它。

   ![](./media/mobile-services-dotnet-adal-register-client/mobile-services-native-client-id.png)

7. 向下滚动页面到**到其他应用程序的权限**部分，单击**添加应用程序**按钮。 **其他**从选择显示菜单和搜索 todo。 单击**任务列表**，以将其添加先前注册的移动服务，单击选中标记为完成。 手机信息服务应用程序授予访问权限。 然后单击**保存**

   ![](./media/mobile-services-dotnet-adal-register-client/mobile-services-native-add-permissions.png)

您的移动服务现已配置中 AAD 接收来自您的应用程序的单一登录方式进行登录。


[Azure 的管理门户]: https://manage.windowsazure.com/
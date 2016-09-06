---
ms.openlocfilehash: 27cc8b53dd4d69d0aa158e23ed8ab44befc4e681
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
## <a name="register-mobile-service-aad"></a>Azure Active Directory 中注册您的移动服务


在本节中将 Azure Active Directory 中注册您的移动服务和配置权限以允许单一登录进行模拟。

1. 按照[如何注册 Azure Active Directory 与]主题 Azure 活动目录注册应用程序。

2. 在[Azure 管理门户]中，返回到 Azure Active Directory 扩展和活动目录上单击

3. 单击**应用程序**选项卡，然后单击应用程序。

4. 单击**管理清单**。 然后单击**下载清单**并保存到本地目录的应用程序清单。

   ![](./media/mobile-services-dotnet-adal-register-service/mobile-services-aad-app-manage-manifest.png)

5. 使用 Visual Studio 中打开应用程序清单文件。 在顶部的文件查找的应用程序权限，则该行如下所示︰

        "oauth2Permissions": [],

    这一行替换为以下应用程序权限并保存该文件。

        "oauth2Permissions": [
            {
                "adminConsentDescription": "Allow the application access to the mobile service",
                "adminConsentDisplayName": "Have full access to the mobile service",
                "id": "b69ee3c9-c40d-4f2a-ac80-961cd1534e40",
                "isEnabled": true,
                "origin": "Application",
                "type": "User",
                "userConsentDescription": "Allow the application full access to the mobile service on your behalf",
                "userConsentDisplayName": "Have full access to the mobile service",
                "value": "user_impersonation"
            }
        ],

6. 在 Azure 管理门户中，单击**管理清单**的一次应用程序，并单击**上载清单**。  浏览到刚刚更新的应用程序清单的位置，并将上载清单。

<!-- URLs. -->
[如何在 Azure 的 Active Directory 中注册]: ../articles/mobile-services/mobile-services-how-to-register-active-directory-authentication.md
[Azure 的管理门户]: https://manage.windowsazure.com/
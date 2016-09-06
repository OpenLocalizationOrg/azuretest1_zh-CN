---
ms.openlocfilehash: d49f99afc05993d94698ad3284c7fd97ff291976
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

说明和下面的屏幕抓图应用于测试 Windows 应用商店客户机，但可以进行测试，这在任何其他 Azure 移动服务所支持的平台上。 

1. 在 Visual Studio 中，运行客户端应用程序并尝试使用名为 Dave 我们创建的用户帐户进行身份验证。 

    ![](./media/mobile-services-aad-rbac-test-app/dave-login.png)

2. Dave 不包含销售组的成员身份。 因此基于角色的访问检查将拒绝访问的表操作。 关闭客户端应用程序。

    ![](./media/mobile-services-aad-rbac-test-app/unauthorized.png)

3. 在 Visual Studio，再次运行客户端应用程序。 这次与我们创建名为 Bob 的用户帐户进行身份验证。

    ![](./media/mobile-services-aad-rbac-test-app/bob-login.png)

4. Bob 有销售组的成员身份。 因此，基于角色的访问检查，可对表操作的访问。

    ![](./media/mobile-services-aad-rbac-test-app/success.png)




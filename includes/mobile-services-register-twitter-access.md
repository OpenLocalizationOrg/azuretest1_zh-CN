---
ms.openlocfilehash: d8eae18e367321ae3750ebc4528a0f34d02e703d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---


新的 Twitter v1.1 Api 要求您的应用程序在访问资源之前进行身份验证。 首先，您需要获取使用 OAuth 2.0 请求的访问权限所需的凭据。 然后，您将安全地存储它们在应用程序设置为您的移动服务。

1. 如果您还没有这样做，完成主题中的步骤<a href="../articles/mobile-services/mobile-services-how-to-register-twitter-authentication.md/" target="_blank">移动服务中注册 Twitter 登录您应用程序</a>。 
  
    Twitter 将生成可用于访问 1.1 版 Twitter Api 所需的凭据。 您可以从使用 Twitter，开发人员网站获得这些凭据。 

2. 定位到<a href="http://go.microsoft.com/fwlink/p/?LinkId=268300" target="_blank">使用 Twitter 开发</a>网站，登录使用 Twitter 帐户凭据，并选择您的 Twitter 应用程序。

3. 在应用程序的**密钥和访问令牌**选项卡，记下以下值︰

    + **使用者密钥**
    + **使用者密码**
    + **访问令牌**
    + **访问令牌密钥**

4. 登录到[Azure 管理门户网站]上，单击**移动服务**，然后单击移动服务。

5. 单击**标识**选项卡，输入的**使用者密钥**和**用户机密**值来自 Twitter，并单击**保存**。 

    ![](./media/mobile-services-register-twitter-access/mobile-identity-tab-twitter-only.png)

2. 单击**配置**选项卡，向下滚动到**的应用程序设置**和输入的**名称**/**值**对从 Twitter 网站，获得以下各项然后单击**保存**。

    + `TWITTER_ACCESS_TOKEN`
    + `TWITTER_ACCESS_TOKEN_SECRET`

    ![](./media/mobile-services-register-twitter-access/mobile-schedule-job-app-settings.png)

    这将 Twitter 访问令牌存储应用程序设置中。 访问凭据还会存储在应用程序设置，加密和您可在访问这些服务器脚本而无需将它们硬编码脚本文件中的**标识**选项卡上的消费凭据，一样。 有关详细信息，请参阅[应用程序设置]。

<!-- URLs. -->
[移动服务服务器脚本引用]: http://go.microsoft.com/fwlink/?LinkId=262293
[Azure 的管理门户]: https://manage.windowsazure.com/
[移动服务中注册 Twitter 登录应用程序]: ../articles/mobile-services/mobile-services-how-to-register-twitter-authentication.md
[Twitter 的开发人员]: http://go.microsoft.com/fwlink/p/?LinkId=268300
[应用程序设置]: http://msdn.microsoft.com/library/azure/b6bb7d2d-35ae-47eb-a03f-6ee393e170f7
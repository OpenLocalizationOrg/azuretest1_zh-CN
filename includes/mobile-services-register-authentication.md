---
ms.openlocfilehash: 4ad46bdf9b978f49b76c4c1ffa4c03b1986a3443
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

1. 单击 Azure 门户网站 >**移动服务**> 移动服务 >**仪表板**，并进行**移动服务 URL**值的记录。

2. 将您的应用程序注册一个或多个下列身份验证提供程序︰
   * [Google](mobile-services-how-to-register-google-authentication.md)
   * [Facebook](mobile-services-how-to-register-facebook-authentication.md)
   * [Twitter](mobile-services-how-to-register-twitter-authentication.md)
   * [Microsoft](mobile-services-how-to-register-microsoft-authentication.md)
   * [Azure 的 Active Directory](mobile-services-how-to-register-active-directory-authentication.md)中。 
   
   请记下客户端的标识和客户端提供程序生成的机密值。 不要分发或共享的客户端的机密。

3. 在 Azure 的门户中，单击**移动服务**> 移动服务 >**标识**> 您标识提供程序的设置，然后输入的客户端 ID 和密钥值，您的提供商。 
 
您现在已经配置您的应用程序和移动服务要使用的身份验证提供程序。 您想要支持每个其他标识提供程序，可能还可以重复所有这些步骤。

    > [AZURE.IMPORTANT] 验证您已经标识提供程序的开发人员网站上设置正确重定向 URI。 上述每个提供程序的链接说明中所述，重定向 URI 可能不同.NET 的后端服务与 JavaScript 后端服务。 配置错误重定向 URI 可能会导致不正常显示的登录屏幕，并以意想不到的方式出现故障的应用程序。

---
ms.openlocfilehash: 401c941bcd5ce364c01b8df702203a02c9953c5d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

1. 在 Visual Studio 解决方案资源管理器中，右击该服务项目并单击**调试**上下文菜单下的**启动新实例**。

    Visual Studio 会打开默认 web 页为您的服务。 默认情况下，Visual Studio 承载您在 IIS Express 本地的移动应用程序后端。

2. 用鼠标右键单击任务栏图标的 IIS Express 在 Windows 任务栏上，确认已启动您移动应用程序的后端。

     ![验证在任务栏中的移动服务](./media/mobile-services-dotnet-backend-test-local-service-api-documentation/iis-express-tray.png)

3. 在您的.NET 后端的开始页上，单击**试试出**。

    这将显示 API 文档页中，您可以使用测试移动应用程序。

    >[AZURE.NOTE]身份验证不是在本地运行时访问此页所需要的。 Azure 中运行时，您必须作为密码 （无用户名） 提供的应用程序键才能访问此网页。

4. 单击**获取表/TodoItem**的链接。

    ![](./media/mobile-services-dotnet-backend-test-local-service-api-documentation/service-api-documentation-page.png)
    
    这显示 API 获取响应页。

5. 单击**尝试一下**，然后单击**发送**。
 
    ![](./media/mobile-services-dotnet-backend-test-local-service-api-documentation/service-try-this-out-get-todoitems.png)

    这将 GET 请求发送到本地的移动应用程序端，TodoItem 表中返回的所有行。 因为表生成器通过初始值设定项，两个 TodoItem 对象返回响应消息的正文中。 初始值设定项的详细信息，请参阅[如何到.NET 后端移动服务数据模型更改](../articles/mobile-services-dotnet-backend-how-to-use-code-first-migrations.md)。

    ![](./media/mobile-services-dotnet-backend-test-local-service-api-documentation/service-try-this-out-get-response.png)
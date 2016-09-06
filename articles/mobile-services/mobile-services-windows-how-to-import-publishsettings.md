---
ms.openlocfilehash: 1f1ef77e07b3d3e43d53ae00cc2477b824df8afc
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="导入您在 Visual Studio 2013年发布设置文件 |Microsoft Azure"
    description="了解如何导入订阅发布 Visual Studio 2013 Azure 移动服务应用程序的设置文件。"
    documentationCenter=""
    services="mobile-services"
    manager="dwrede"
    editor=""
    authors="ggailey777"/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="08/18/2015" 
    ms.author="glenga"/>

# 导入您的订阅发布设置文件在 Visual Studio 2013年

您可以创建移动服务之前，必须从 Azure 订购到 Visual Studio 发布设置文件导入。 这使 Visual Studio 替您连接到 Azure。  

>[AZURE.NOTE] 从 Visual Studio 2013年更新 2 开始，不再需要使用发布设置文件。 Visual Studio 就能够直接连接到 Azure 使用您提供的凭据。

1. 在 Visual Studio 2013，打开解决方案资源管理器，右键单击该项目然后单击**添加**，然后选择**连接服务...**。

    ![添加连接的服务](./media/mobile-services-windows-how-to-import-publishsettings/mobile-add-connected-service.png)

2. 在服务管理器对话框中，单击**创建服务...**，然后选择**&lt;导入...&gt;**从在对话框中创建移动服务的**订阅**。  

    ![从 VS 2013 创建新的移动服务](./media/mobile-services-windows-how-to-import-publishsettings/mobile-create-service-from-vs2013.png)

3. 在导入 Azure 的预订，请单击**下载订阅文件**，到 Azure 帐户登录 （如果需要），当浏览器请求以保存该文件，请单击**保存**。

    ![下载 VS 中的订阅文件](./media/mobile-services-windows-how-to-import-publishsettings/mobile-import-azure-subscription.png)

    > [AZURE.NOTE] 登录窗口将显示在浏览器中，这可能是 Visual Studio 窗口的背后。 请记住要记的下载的.publishsettings 文件的保存位置。 如果您的项目尚未连接到 Azure 订购，您可以跳过此步骤。

4. 单击**浏览**，导航到保存的.publishsettings 文件的位置，选择该文件，然后单击**打开**，然后再**导入**。

    ![导入 VS 中的订阅](./media/mobile-services-windows-how-to-import-publishsettings/mobile-import-azure-subscription-2.png)

    Visual Studio 中导入连接到 Azure 订购所需的数据。 当您的订阅已具有一个或多个现有的移动服务时，将显示服务名称。

    > [AZURE.NOTE] 导入后的发布设置，请考虑删除下载的.publishsettings 文件，因为它包含可供其他人访问您的帐户的信息。 如果您打算将其保存在其他连接的应用程序项目中使用，保护该文件。

<!-- Anchors. -->

<!-- Images. -->
[1]: ./media/mobile-services-how-to-register-microsoft-authentication/mobile-services-live-connect-add-app.png
[2]: ./media/mobile-services-how-to-register-microsoft-authentication/mobile-live-connect-app-api-settings.png
<!-- URLs. -->
[为使用实时连接的 Windows 应用商店应用程序的单一登录]: /develop/mobile/how-to-guides/register-for-single-sign-on/
[提交应用程序页]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[我的应用程序]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[开始使用移动服务]: /develop/mobile/tutorials/get-started/
[开始使用身份验证]: /develop/mobile/tutorials/get-started-with-users-dotnet/
[开始使用推式通知]: /develop/mobile/tutorials/get-started-with-push-dotnet/
[授权用户使用的脚本]: /develop/mobile/tutorials/authorize-users-in-scripts-dotnet/
[JavaScript 和 HTML]: /develop/mobile/tutorials/get-started-with-users-js/

[Azure 的管理门户]: https://manage.windowsazure.com/

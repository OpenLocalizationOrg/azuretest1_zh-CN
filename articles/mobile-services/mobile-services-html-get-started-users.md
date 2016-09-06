---
ms.openlocfilehash: 3b3864a3089e7fb3d9576d990e078d2dbeca1956
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="将身份验证添加到 HTML/JavaScript 应用程序 |Microsoft Azure" 
    description="了解如何使用移动服务通过多种身份提供程序，包括 Google、 Facebook、 Twitter，以及 Microsoft 帐户 HTML 应用程序的用户进行身份验证。" 
    services="mobile-services" 
    documentationCenter="" 
    authors="ggailey777" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-multiple" 
    ms.devlang="javascript" 
    ms.topic="article" 
    ms.date="07/21/2015" 
    ms.author="glenga"/>

# 添加到您的移动服务应用程序的身份验证 

[AZURE.INCLUDE [mobile-services-selector-get-started-users](../../includes/mobile-services-selector-get-started-users.md)]

本主题演示如何在 Azure 移动服务从 HTML 应用程序，包括 PhoneGap 或 Cordova 应用程序的用户进行身份验证。  在本教程中，您添加到快速入门项目使用标识提供程序支持移动服务的身份验证。 后被成功通过身份验证和授权的移动服务，将显示用户 ID 值。  

本教程基于移动服务快速入门。 此外第一次必须完成本教程[开始使用移动服务]。 

##<a name="register"></a>注册您的应用程序进行身份验证和配置移动服务

[AZURE.INCLUDE [mobile-services-register-authentication](../../includes/mobile-services-register-authentication.md)] 

##<a name="permissions"></a>限制对已验证身份的用户的权限

[AZURE.INCLUDE [mobile-services-restrict-permissions-javascript-backend](../../includes/mobile-services-restrict-permissions-javascript-backend.md)] 


3. 在应用程序目录中，启动**服务器**子文件夹中的以下命令文件之一。

    + **启动 windows**（Windows 计算机） 
    + **发布 mac.command** （Mac OS X 计算机）
    + **发布 linux.sh** （Linux 计算机）

    >[AZURE.NOTE]在 Windows 计算机上，键入`R`PowerShell 当要求您确认您想要运行的脚本。 Web 浏览器可能会警告您不运行脚本，因为它从互联网下载。 这种情况下，您必须请求浏览器继续加载该脚本。

    这将启动 web 服务器上本地计算机新的应用程序的宿主。

2. 打开 URL <a href="http://localhost:8000/" target="_blank">http://localhost:8000 /</a> web 浏览器中启动该应用程序。 

    无法加载数据。 这是因为该应用程序试图访问移动服务作为未经身份验证的用户，但_TodoItem_表现在要求进行身份验证。

3. （可选）打开 web 浏览器的脚本调试程序，然后重新加载该页面。 确认拒绝访问，则会发生错误。 

接下来，您将更新应用程序允许身份验证之前从移动服务中请求的资源。

##<a name="add-authentication"></a>向应用程序添加验证

>[AZURE.NOTE]因为在弹出菜单执行登录，您应调用**login**方法从按钮的 click 事件。 否则，许多浏览器都将取消显示登录窗口。

1. 打开项目文件 index.html，找到 H1 元素并在其下添加下面的代码段︰

        <div id="logged-in">
            You are logged in as <span id="login-name"></span>.
            <button id="log-out">Log out</button>
        </div>
        <div id="logged-out">
            You are not logged in.
            <button>Log in</button>
        </div>

    这使您可以从页面登录到移动服务。

2. 在 app.js 文件中，找到代码在调用 refreshTodoItems 函数中，该文件的最底部的行，并将其替换为以下代码︰ 
    
        function refreshAuthDisplay() {
            var isLoggedIn = client.currentUser !== null;
            $("#logged-in").toggle(isLoggedIn);
            $("#logged-out").toggle(!isLoggedIn);

            if (isLoggedIn) {
                $("#login-name").text(client.currentUser.userId);
                refreshTodoItems();
            }
        }

        function logIn() {
            client.login("facebook").then(refreshAuthDisplay, function(error){
                alert(error);
            });
        }

        function logOut() {
            client.logout();
            refreshAuthDisplay();
            $('#summary').html('<strong>You must login to access data.</strong>');
        }

        // On page init, fetch the data and set up event handlers
        $(function () {
            refreshAuthDisplay();
            $('#summary').html('<strong>You must login to access data.</strong>');          
            $("#logged-out button").click(logIn);
            $("#logged-in button").click(logOut);
        });

    这将创建一组函数来处理身份验证过程。 通过使用 Facebook 登录，用户进行身份验证。 如果您正在使用 Facebook 不标识提供程序，更改传递给**login**方法上面到下面的一个值︰ *microsoftaccount*、 *facebook*、*使用 twitter*、 *google*或*aad*。

    >[AZURE.IMPORTANT]在 PhoneGap 应用程序中，还必须向项目中添加了以下插件︰
    ><ul><li><code>phonegap plugin add https://git-wip-us.apache.org/repos/asf/cordova-plugin-device.git</code></li>
    ><li><code>phonegap plugin add https://git-wip-us.apache.org/repos/asf/cordova-plugin-inappbrowser.git</code></li></ul>

9. 返回到浏览器运行您的应用程序的位置，并刷新网页。 

       When you are successfully logged-in, the app should run without errors, and you should be able to query Mobile Services and make updates to data.

    >[AZURE.NOTE]当您使用 Internet Explorer 时，您可能会收到错误登录后︰ <code>Cannot reach window opener. It may be on a different Internet Explorer zone</code>。 这是因为弹出窗口从本地主机 （内联网） 运行在不同的安全区域中 (internet)。 这只会影响在使用本地主机的开发过程中的应用程序。 作为一种变通方法，打开**Internet 选项**的**安全**选项卡上单击**本地 Intranet**、 单击**站点**，禁用**自动检测到内部网的网络**。 请记住，若要更改此设置完成后，重新测试。

## <a name="next-steps"> </a>下一步行动

在下一步的教程中，[有脚本的授权用户]，将采用基于身份验证的用户的移动服务提供的用户 ID 值，并使用它来筛选移动服务返回的数据。 了解更多关于如何使用 HTML/JavaScript 中[移动服务 HTML/JavaScript 帮助概念参考]的移动服务

<!-- Anchors. -->
[注册您的应用程序进行身份验证和配置移动服务]: #register
[限制为经过身份验证的用户的表权限]: #permissions
[向应用程序添加验证]: #add-authentication
[下一步行动]:#next-steps

<!-- Images. -->

[4]: ./media/mobile-services-html-get-started-users/mobile-services-selection.png
[5]: ./media/mobile-services-html-get-started-users/mobile-service-uri.png
[13]: ./media/mobile-services-html-get-started-users/mobile-identity-tab.png
[14]: ./media/mobile-services-html-get-started-users/mobile-portal-data-tables.png
[15]: ./media/mobile-services-html-get-started-users/mobile-portal-change-table-perms.png

<!-- URLs. -->
[开始使用移动服务]: mobile-services-html-get-started.md
[有关数据入门]: mobile-services-html-get-started-data.md
[授权用户使用的脚本]: mobile-services-javascript-backend-service-side-authorization.md

[Azure 的管理门户]: https://manage.windowsazure.com/
[移动服务帮助 HTML/JavaScript 的概念引用]: mobile-services-html-how-to-use-client-library.md
 
测试

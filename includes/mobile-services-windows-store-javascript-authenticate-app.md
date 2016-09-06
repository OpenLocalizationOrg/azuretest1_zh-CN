---
ms.openlocfilehash: c9d4c9ba60d2777a2a1fbee62f47baf30c6560da
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---


1. 打开项目文件 default.js 和**应用程序中。OnActivated**方法重载，上次调用**refreshTodoItems**方法替换为以下代码︰ 
    
        var userId = null;

        // Request authentication from Mobile Services using a Facebook login.
        var login = function () {
            return new WinJS.Promise(function (complete) {
                client.login("facebook").done(function (results) {
                    userId = results.userId;
                    refreshTodoItems();
                    var message = "You are now logged in as: " + userId;
                    var dialog = new Windows.UI.Popups.MessageDialog(message);
                    dialog.showAsync().done(complete);
                }, function (error) {
                    userId = null;
                    var dialog = new Windows.UI.Popups
                        .MessageDialog("An error occurred during login", "Login Required");
                    dialog.showAsync().done(complete);
                });
            });
        }            

        var authenticate = function () {
            login().then(function () {
                if (userId === null) {

                    // Authentication failed, try again.
                    authenticate();
                }
            });
        }

        authenticate();

    这将创建一个成员变量来存储当前用户和一个方法来处理身份验证过程。 通过使用 Facebook 登录，用户进行身份验证。 如果您正在使用 Facebook 不标识提供程序，更改传递给的值<strong>登录</strong>上面的方法，为下列情况之一︰ _microsoftaccount_，_使用 twitter_， _google_或_windowsazureactivedirectory_。

    >[AZURE.NOTE]如果您注册您的 Windows 应用商店应用程序软件包信息移动服务，则应调用<a href="http://go.microsoft.com/fwlink/p/?LinkId=322050" target="_blank">登录</a>通过提供的值的方法<strong>为</strong>的<em>useSingleSignOn</em>参数。 如果不这样做，您的用户将仍显示登录提示登录调用每次。

2. 按 F5 键运行应用程序和登录到您选定的标识提供程序使用该应用程序。 

    成功登录后，应用程序应该运行错误，并应能为移动服务的查询，并对数据进行更新。
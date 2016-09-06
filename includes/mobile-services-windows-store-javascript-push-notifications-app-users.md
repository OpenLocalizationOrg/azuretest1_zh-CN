---
ms.openlocfilehash: ec7b79b6a66feb2e8b92ab86616c0cd4254fecdd
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

接下来，您需要更改推式通知已注册，以确保之后注册尝试对用户进行身份验证的方式。 客户端应用程序更新取决于在其中实现推式通知的方式。

###使用 Visual Studio 2013年更新 2 或更高版本中的强制通知向导

在此方法中，向导生成新的 push.register.js 和 service.js 文件在您的项目。

1. 在解决方案资源管理器中的 Visual Studio，打开 push.register.js 项目文件和注释出或删除对**addEventListener**的调用。 

2. 在 default.js 项目文件中，用下面的代码替换现有的**登录**函数︰
 
        // Request authentication from Mobile Services using a Facebook login.
        var login = function () {
            return new WinJS.Promise(function (complete) {
                client.login("facebook").done(function (results) {
                    userId = results.userId;
                    // Request a push notification channel.
                    Windows.Networking.PushNotifications
                        .PushNotificationChannelManager
                        .createPushNotificationChannelForApplicationAsync()
                        .then(function (channel) {
                            // Register for notifications using the new channel
                            client.push.registerNative(channel.uri);
                        });
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

###手动启用推式通知      

在此方法中，您的注册码教程中直接添加到 default.js 的项目文件。

1. 在解决方案资源管理器中的 Visual Studio，打开 default.js 项目文件，并在**onActivated**事件处理程序中查找调用**createPushNotificationChannelForApplicationAsync**函数，如下所示的代码行︰

        // Request a push notification channel.
        Windows.Networking.PushNotifications
            .PushNotificationChannelManager
            .createPushNotificationChannelForApplicationAsync()
            .then(function (channel) {
                // Register for notifications using the new channel
                client.push.registerNative(channel.uri);
            }); 
 
2. 这行代码移动到**登录**功能，对**refreshTodoItems**的调用前，使**登录**功能如下所示︰
 
        // Request authentication from Mobile Services using a Facebook login.
        var login = function () {
            return new WinJS.Promise(function (complete) {
                client.login("facebook").done(function (results) {
                    userId = results.userId;
                    // Request a push notification channel.
                    Windows.Networking.PushNotifications
                        .PushNotificationChannelManager
                        .createPushNotificationChannelForApplicationAsync()
                        .then(function (channel) {
                            // Register for notifications using the new channel
                            client.push.registerNative(channel.uri);
                        });
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

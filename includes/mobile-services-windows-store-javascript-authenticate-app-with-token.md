---
ms.openlocfilehash: 8985b91542b1910a57bcad59a9b5238a38139b9e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

前面的示例显示一个标准登录，这需要客户端联系身份标识提供程序和移动服务每次启动该应用程序。 不只是此方法效率很低，可以运行到使用率的相关问题应许多客户尝试启动您的应用程序在同一时间。 更好的方法是缓存移动服务返回的授权标记，然后尝试使用基于提供程序的登录之前，使用此第一。

>[AZURE.NOTE]可以缓存的移动服务颁发无论使用的客户端管理或托管服务的身份验证令牌。 本教程使用托管服务的身份验证。

1. 在 default.js 项目文件中使用下面的代码替换现有的**登录**函数︰

        var credential = null;
        var vault = new Windows.Security.Credentials.PasswordVault();

        // Request authentication from Mobile Services using a Facebook login.
        var login = function () {
            return new WinJS.Promise(function (complete) {
                client.login("facebook").done(function (results) {
                    // Create a credential for the returned user.
                    credential = new Windows.Security.Credentials
                        .PasswordCredential("Facebook", results.userId,
                        results.mobileServiceAuthenticationToken);
                    vault.add(credential);
                    userId = results.userId;

                    refreshTodoItems();
                    var message = "You are now logged in as: " + userId;
                    var dialog = new Windows.UI.Popups.MessageDialog(message);
                    dialog.showAsync().done(complete);
                }, function (error) {
                    var dialog = new Windows.UI.Popups
                        .MessageDialog("An error occurred during login", "Login Required");
                    dialog.showAsync().done(complete);
                });
            });
        }

2. 用以下代码替换现有的**身份验证**功能︰

        var authenticate = function () {
            // Try to get a stored credential from the PasswordVault.                
            try{
                credential = vault.findAllByResource("Facebook").getAt(0);
            }
            catch (error) {
                // This is expected when there's no stored credential.
            }
            
            if (credential) {
                // Set the user from the returned credential.   
                credential.retrievePassword();
                client.currentUser = {
                    "userId": credential.userName,
                    "mobileServiceAuthenticationToken": credential.password
                };

                // Try to return an item now to determine if the cached credential has expired.
                todoTable.take(1).read()
                            .done(function () {
                                refreshTodoItems();
                            }, function (error) {
                                if (error.request.status === 401) {
                                    login(credential, vault).then(function () {
                                        if (!credential) {

                                            // Authentication failed, try again.
                                            authenticate();
                                        }
                                    });
                                }                                   
                            });
            } else {

                login().then(function () {
                    if (!credential) {
                        // Authentication failed, try again.
                        authenticate();
                    }
                });
            }
        }

    在此版本的**验证**，该应用程序试图使用存储在**PasswordVault**中的凭据来访问移动服务。 一个简单的查询发送验证存储的标记尚未过期。 当返回 401 时，正则基于提供程序的登录尝试。 常规符号中也执行时不存储的凭据。

3. 两次重新启动该应用程序。

    请注意，第一个启动时，在与提供程序的符号再次需要。 但是，在第二个重新启动使用缓存的凭据并登录被绕过。 
---
ms.openlocfilehash: 99b5d450de20cf867aaa2e49b1e0e890436bbbc1
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

前面的示例显示一个标准登录，这需要客户端应用程序启动每次联系身份标识提供程序和应用程序服务。 不只是此方法效率很低，可以运行到使用率的相关问题应许多客户尝试启动您的应用程序在同一时间。 更好的方法是缓存返回您的应用程序服务的授权标记，然后尝试使用基于提供程序的登录之前，使用此第一。 

>[AZURE.NOTE]无论您是否使用客户端管理或托管服务的身份验证应用程序服务所颁发的令牌可以缓存。 本教程使用托管服务的身份验证。

1. 在 MainPage.xaml.cs 项目文件中添加下面的**using**语句︰

        using System.Linq;      
        using Windows.Security.Credentials;

2. **AuthenticateAsync**方法替换为以下代码︰

        private async System.Threading.Tasks.Task AuthenticateAsync()
        {
            string message;
            // This sample uses the Facebook provider.
            var provider = "Facebook";
              
            // Use the PasswordVault to securely store and access credentials.
            PasswordVault vault = new PasswordVault();
            PasswordCredential credential = null;

            while (credential == null)
            {
                try
                {
                    // Try to get an existing credential from the vault.
                    credential = vault.FindAllByResource(provider).FirstOrDefault();
                }
                catch (Exception)
                {
                    // When there is no matching resource an error occurs, which we ignore.
                }

                if (credential != null)
                {
                    // Create a user from the stored credentials.
                    user = new MobileServiceUser(credential.UserName);
                    credential.RetrievePassword();
                    user.MobileServiceAuthenticationToken = credential.Password;
                    
                    // Set the user from the stored credentials.
                    App.MobileService.CurrentUser = user;

                    try
                    {
                        // Try to return an item now to determine if the cached credential has expired.
                        await App.MobileService.GetTable<TodoItem>().Take(1).ToListAsync();
                    }
                    catch (MobileServiceInvalidOperationException ex)
                    {                        
                        if (ex.Response.StatusCode == System.Net.HttpStatusCode.Unauthorized)
                        {
                            // Remove the credential with the expired token.
                            vault.Remove(credential);
                            credential = null;
                            continue;
                        }
                    }
                }
                else
                {
                    try
                    {
                        // Login with the identity provider.
                        user = await App.MobileService
                            .LoginAsync(provider);                        

                        // Create and store the user credentials.
                        credential = new PasswordCredential(provider,
                            user.UserId, user.MobileServiceAuthenticationToken);
                        vault.Add(credential);
                    }
                    catch (MobileServiceInvalidOperationException ex)
                    {
                        message = "You must log in. Login Required";
                    }
                }
                message = string.Format("You are now logged in - {0}", user.UserId);
                var dialog = new MessageDialog(message);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }

    在此版本的**AuthenticateAsync**，该应用程序试图使用存储在**PasswordVault**中的凭据来访问移动应用程序。 一个简单的查询发送验证存储的标记尚未过期。 当返回 401 时，正则基于提供程序的登录尝试。 常规符号中也执行时不存储的凭据。

    >[AZURE.NOTE]此应用程序登录过程中，对已到期令牌进行测试，但令牌到期后可能会出现身份验证该应用程序正在使用。 处理与到期令牌相关的授权错误的解决方案，请参阅[缓存和处理在 Azure 移动服务到期的令牌管理 SDK](http://blogs.msdn.com/b/carlosfigueira/archive/2014/03/13/caching-and-handling-expired-tokens-in-azure-mobile-services-managed-sdk.aspx)的张贴内容。 

3. 两次重新启动该应用程序。

    请注意，第一个启动时，在与提供程序的符号再次需要。 但是，在第二个重新启动使用缓存的凭据并登录被绕过。 
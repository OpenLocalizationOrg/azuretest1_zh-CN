---
ms.openlocfilehash: 8992392ebd3ec25465ebcb9f4a40bb624c81c341
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

1. 打开共享的项目文件 MainPage.cs，并将下面的代码段添加到主页类︰
    
        // Define a member variable for storing the signed-in user. 
        private MobileServiceUser user;

        // Define a method that performs the authentication process
        // using a Facebook sign-in. 
        private async System.Threading.Tasks.Task AuthenticateAsync()
        {
            while (user == null)
            {
                string message;
                try
                {
                    // Change 'MobileService' to the name of your MobileServiceClient instance.
                    // Sign-in using Facebook authentication.
                    user = await App.MobileService
                        .LoginAsync(MobileServiceAuthenticationProvider.Facebook);
                    message = 
                        string.Format("You are now signed in - {0}", user.UserId);
                }
                catch (InvalidOperationException)
                {
                    message = "You must log in. Login Required";
                }
                        
                var dialog = new MessageDialog(message);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }

    通过使用 Facebook 登录，该用户进行身份验证。 如果您使用的 Facebook 不标识提供程序，您的提供程序的值更改上面的**MobileServiceAuthenticationProvider**的值。

3. 注释出或删除现有的**OnNavigatedTo**方法重写中的**RefreshTodoItems**方法的调用。

    这可防止用户进行身份验证之前加载数据。

    >[AZURE.NOTE]成功从 Windows Phone 商店 8.1 应用程序进行身份验证，则必须调用 LoginAsync， **OnNavigated**方法被调用后之后的页**加载**事件已引发。 在本教程中，这是通过向应用程序添加**登录**按钮。

4. 将下面的代码段添加到主页类︰

        private async void ButtonLogin_Click(object sender, RoutedEventArgs e)
        {
            // Login the user and then load data from the mobile service.
            await AuthenticateAsync();

            // Hide the login button and load items from the mobile service.
            this.ButtonLogin.Visibility = Windows.UI.Xaml.Visibility.Collapsed;
            await RefreshTodoItems();
        }
        
5. 在 Windows 应用商店应用程序项目中，打开 MainPage.xaml 项目文件，并添加以下**按钮**元素之前的元素的定义的**保存**按钮︰

        <Button Name="ButtonLogin" Click="ButtonLogin_Click" 
                        Visibility="Visible">Sign in</Button>

6. 在 Windows Phone 应用程序项目中，添加以下**按钮**元素只是之前定义的**保存**按钮的元素︰

        <Button Grid.Row ="1" Grid.Column="1" Name="ButtonLogin" Click="ButtonLogin_Click" 
                        Visibility="Visible" Margin="10, 0, 0, 0">Sign in</Button> 

5. 打开共享的 App.xaml.cs 的项目文件，然后添加以下使用语句，如果不存在︰

        using Microsoft.WindowsAzure.MobileServices;  
 
6. 在 App.xaml.cs 项目文件中添加以下代码︰

        protected override void OnActivated(IActivatedEventArgs args)
        {
            // Windows Phone 8.1 requires you to handle the respose from the WebAuthenticationBroker.
            #if WINDOWS_PHONE_APP
            if (args.Kind == ActivationKind.WebAuthenticationBrokerContinuation)
            {
                // Completes the sign-in process started by LoginAsync.
                // Change 'MobileService' to the name of your MobileServiceClient instance. 
                App.MobileService.LoginComplete(args as WebAuthenticationBrokerContinuationEventArgs);
            }
            #endif

            base.OnActivated(args);
        }

    如果已存在的**OnActivated**方法，只需添加`#if...#endif`的代码块。

8. 按 F5 键以运行 Windows 应用商店应用程序，单击**登录**按钮，您选定的标识提供商登录到该应用程序。 

    成功登录后，应用程序应该运行错误，并应能为移动服务的查询，并对数据进行更新。

9. 用鼠标右键单击 Windows Phone 存储应用程序项目，请单击**设为启动项目**，然后重复上面的步骤来验证应用程序还可以正常运行 Windows Phone 商店。  

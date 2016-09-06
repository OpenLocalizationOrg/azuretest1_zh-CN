---
ms.openlocfilehash: 190454fb9f448b6c19b520487d166241a77e229a
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

接下来，您需要更改推式通知已注册，以确保之后注册尝试对用户进行身份验证的方式。 客户端应用程序更新取决于在其中实现推式通知的方式。

###使用添加推送通知向导在 Visual Studio 2013年更新 2 或更高版本

在此方法中，向导生成一个新的 push.register.cs 文件，您的项目中。

1. 在解决方案资源管理器中的 Visual Studio，打开的 app.xaml.cs 项目文件和**OnLaunched**事件处理程序注释出或删除调用**UploadChannel**方法。 

2. 打开 push.register.cs 项目文件和**UploadChannel**方法中，替换为以下代码︰

        public async static void UploadChannel()
        {
            var channel = 
                await Windows.Networking.PushNotifications.PushNotificationChannelManager
                .CreatePushNotificationChannelForApplicationAsync();
        
            try
            {
                // Create a native push notification registration.
                await App.MobileService.GetPush().RegisterNativeAsync(channel.Uri);             
        
            }
            catch (Exception exception)
            {
                HandleRegisterException(exception);
            }
        }

    这将确保进行注册使用相同的客户端实例具有经过身份验证的用户凭据。 否则，注册将失败，并未经授权 (401) 错误。

3. 打开共享的 MainPage.cs 的项目文件，并使用以下标记替换**ButtonLogin_Click**处理程序︰

        private async void ButtonLogin_Click(object sender, RoutedEventArgs e)
        {
            // Login the user and then load data from the mobile service.
            await AuthenticateAsync();
            todolistPush.UploadChannel();

            // Hide the login button and load items from the mobile service.
            this.ButtonLogin.Visibility = Windows.UI.Xaml.Visibility.Collapsed;
            await RefreshTodoItems();
        }

    这将确保在尝试推入注册之前发生的验证。

4.  在前面的代码替换生成的推类名 (`todolistPush`) 与向导，通常采用的格式生成的类的名称<code><em>mobile_service</em>Push</code>。 

###手动启用推式通知      

在此方法中，您的注册码教程中直接添加到 app.xaml.cs 的项目文件。

1. 在解决方案资源管理器中的 Visual Studio，打开的 app.xaml.cs 项目文件和**OnLaunched**事件处理程序注释出或删除对**InitNotificationsAsync**的调用。 
 
2. 更改从**InitNotificationsAsync**方法的可访问性`private`到`public`并添加`static`修饰符。 

3. 打开共享的 MainPage.cs 的项目文件，并使用以下标记替换**ButtonLogin_Click**处理程序︰

        private async void ButtonLogin_Click(object sender, RoutedEventArgs e)
        {
            // Login the user and then load data from the mobile service.
            await AuthenticateAsync();
            App.InitNotificationsAsync();

            // Hide the login button and load items from the mobile service.
            this.ButtonLogin.Visibility = Windows.UI.Xaml.Visibility.Collapsed;
            await RefreshTodoItems();
        }
    
    这将确保在尝试推入注册之前发生的验证。
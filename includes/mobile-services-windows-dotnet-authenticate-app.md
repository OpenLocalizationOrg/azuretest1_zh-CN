---
ms.openlocfilehash: 8dd33668a4e1b3c5e3e279f9a59f6c20e2dbefb9
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

1. 打开项目文件 MainPage.xaml.cs，然后添加以下 using 语句︰

        using Windows.UI.Popups;

2. 将下面的代码段添加到主页类︰
    
        private MobileServiceUser user;
        private async System.Threading.Tasks.Task AuthenticateAsync()
        {
            while (user == null)
            {
                string message;
                try
                {
                    user = await App.MobileService
                        .LoginAsync(MobileServiceAuthenticationProvider.Facebook);
                    message = 
                        string.Format("You are now logged in - {0}", user.UserId);
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

    这将创建一个成员变量来存储当前用户和一个方法来处理身份验证过程。 通过使用 Facebook 登录，用户进行身份验证。 如果您使用的 Facebook 不标识提供程序，您的提供程序的值更改上面的**MobileServiceAuthenticationProvider**的值。

3. 替换现有的**OnNavigatedTo**方法重写以下方法调用新的**身份验证**方法︰

        protected override async void OnNavigatedTo(NavigationEventArgs e)
        {
            await AuthenticateAsync();
            RefreshTodoItems();
        }
        
4. 按 F5 键运行应用程序和登录到您选定的标识提供程序使用该应用程序。 

    成功登录后，应用程序应该运行错误，并应能为移动服务的查询，并对数据进行更新。
---
ms.openlocfilehash: d2aeda414299d266195f213e080911a381d95b94
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

接下来，您必须更改推式通知已注册，以确保之后注册尝试对用户进行身份验证的方式。 

1. 在解决方案资源管理器中的 Visual Studio，打开的 app.xaml.cs 项目文件和**Application_Launching**事件处理程序注释出或删除调用**AcquirePushChannel**方法。 
 
2. 更改从**AcquirePushChannel**方法的可访问性`private`到`public`并添加`static`修饰符。 

3. 打开 MainPage.xaml.cs 项目文件并替换的**OnNavigatedTo**方法重写为以下︰

        protected override async void OnNavigatedTo(NavigationEventArgs e)
        {
            await AuthenticateAsync();            
            App.AcquirePushChannel();
            RefreshTodoItems();
        }
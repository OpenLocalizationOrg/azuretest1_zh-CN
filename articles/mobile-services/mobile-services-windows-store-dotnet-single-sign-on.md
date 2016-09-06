---
ms.openlocfilehash: 4b15176306ca220534f68bce436f5be850827213
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="对您 Windows 应用商店应用程序实时连接进行身份验证" 
    description="学习如何使用实时连接单个登录在 Azure 移动服务从 Windows 应用商店应用程序。" 
    services="mobile-services" 
    documentationCenter="windows" 
    authors="ggailey777" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/08/2015" 
    ms.author="glenga"/>

# 验证您的 Windows 应用商店应用程序与客户端使用 Microsoft 帐户托管身份验证

[AZURE.INCLUDE [mobile-services-selector-single-signon](../../includes/mobile-services-selector-single-signon.md)]  

##概述
本主题演示如何获取 Microsoft 帐户使用实时 SDK 从通用的 Windows 应用程序的身份验证令牌。 然后可以使用此标记使用 Azure 移动服务的用户进行身份验证。  在本教程中，您添加到现有项目中使用实时 SDK Microsoft 帐户身份验证。 当成功通过身份验证登录的用户按名称受到欢迎并显示用户 ID 值。  

>[AZURE.NOTE]本教程演示使用导向客户端身份验证和实时 SDK 的好处。 这使您可以更轻松地与您已登录的用户的身份验证的移动服务。 您还可以请求其他作用域以使您的应用程序来访问资源，如 OneDrive。 
>导向服务的身份验证提供更加通用的经验，并支持多个身份验证提供程序。 导向服务身份验证的详细信息，请参阅主题[添加到您的应用程序的身份验证](mobile-services-javascript-backend-windows-universal-dotnet-get-started-users.md)。 

本教程要求如下︰

+ [实时的 SDK]
+ Microsoft Visual Studio 2013年更新 3 或更高版本。
+ 此外第一次必须完成教程[开始使用移动服务](mobile-services-javascript-backend-windows-store-dotnet-get-started.md)或[添加到现有应用程序的移动服务]。

##注册您的应用程序使用 Microsoft 帐户进行身份验证

若要能够对用户进行身份验证，您必须注册您的应用程序开发人员中心中的 Microsoft 科目。 然后，您必须与您的移动服务连接此登记。 请完成以下主题创建 Microsoft 帐户注册，并将其连接到您的移动服务中的步骤。

+ [注册您的应用程序使用 Microsoft 帐户登录](mobile-services-how-to-register-microsoft-authentication.md)

##<a name="permissions"></a>限制对已验证身份的用户的权限

接下来需要限制对资源，在这种情况下的*TodoItems*表格，以确保它只能访问已登录的用户访问。

[AZURE.INCLUDE [mobile-services-restrict-permissions-windows](../../includes/mobile-services-restrict-permissions-windows.md)] 

##<a name="add-authentication"></a>向应用程序添加验证

最后，您将添加实时 SDK 并使用它在您的应用程序中的用户进行身份验证。

1. 在**解决方案资源管理器**中右击解决方案，然后选择**管理 NuGet 程序包**。

2. 在左窗格中，选择**联机**类别， **LiveSDK**搜索，单击**安装** **Live SDK**包、 选择所有项目，然后接受许可协议。 

    这向解决方案中添加实时 SDK。

3. 打开共享的项目文件 MainPage.xaml.cs，然后添加以下 using 语句︰

        using Microsoft.Live;        

4. 将下面的代码段添加到**主页**类︰
    
        private LiveConnectSession session;
        private async System.Threading.Tasks.Task AuthenticateAsync()
        {

            // Get the URL the mobile service.
            var serviceUrl = App.MobileService.ApplicationUri.AbsoluteUri;

            // Create the authentication client using the mobile service URL.
            LiveAuthClient liveIdClient = new LiveAuthClient(serviceUrl);

            while (session == null)
            {
                // Request the authentication token from the Live authentication service.
                // The wl.basic scope is requested.
                LiveLoginResult result = await liveIdClient.LoginAsync(new string[] { "wl.basic" });
                if (result.Status == LiveConnectSessionStatus.Connected)
                {
                    session = result.Session;

                    // Get information about the logged-in user.
                    LiveConnectClient client = new LiveConnectClient(session);
                    LiveOperationResult meResult = await client.GetAsync("me");

                    // Use the Microsoft account auth token to sign in to Mobile Services.
                    MobileServiceUser loginResult = await App.MobileService
                        .LoginWithMicrosoftAccountAsync(result.Session.AuthenticationToken);

                    // Display a personalized sign-in greeting.
                    string title = string.Format("Welcome {0}!", meResult.Result["first_name"]);
                    var message = string.Format("You are now logged in - {0}", loginResult.UserId);
                    var dialog = new MessageDialog(message, title);
                    dialog.Commands.Add(new UICommand("OK"));
                    await dialog.ShowAsync();
                }
                else
                {
                    session = null;
                    var dialog = new MessageDialog("You must log in.", "Login Required");
                    dialog.Commands.Add(new UICommand("OK"));
                    await dialog.ShowAsync();
                }
            }
        }
    
    这将创建一个成员变量来存放当前实时连接会话和一个方法来处理身份验证过程。 

    >[AZURE.NOTE]您应尝试从服务请求新的标记之前使用缓存的实时 authentiction 令牌或移动服务授权令牌。 如果您不执行此操作，则可能必须使用-相关的问题应许多客户尝试同时启动您的应用程序。 有关如何缓存此标记的示例，请参阅[开始使用身份验证](../mobile-services-windows-store-dotnet-get-started-users.md#tokens)

5. 注释出或删除现有的**OnNavigatedTo**方法重写中的**RefreshTodoItems**方法的调用。

    这可防止用户进行身份验证之前加载数据。 接下来，您将添加一个按钮以启动登录过程。

6. 将下面的代码段添加到主页类︰

        private async void ButtonLogin_Click(object sender, RoutedEventArgs e)
        {
            // Login the user and then load data from the mobile service.
            await AuthenticateAsync();

            // Hide the login button and load items from the mobile service.
            this.ButtonLogin.Visibility = Windows.UI.Xaml.Visibility.Collapsed;
            await RefreshTodoItems();
        }
        
7. 在 Windows 应用商店应用程序项目中，打开 MainPage.xaml 项目文件，并添加以下**按钮**元素之前的元素的定义的**保存**按钮︰

        <Button Name="ButtonLogin" Click="ButtonLogin_Click" 
                        Visibility="Visible">Sign in</Button>

8. 对于 Windows Phone 存储应用程序项目，重复上面的步骤，但这一次在**TitlePanel**中，**文本块**元素的后面添加**按钮**。
        
9. 按 F5 键以与 Microsoft 客户动态连接到运行的应用程序和符号。 

    成功登录后，应用程序应该运行错误，并应能为移动服务的查询，并对数据进行更新。

10. 用鼠标右键单击 Windows Phone 存储应用程序项目，请单击**设为启动项目**，然后重复上面的步骤来验证应用程序还可以正常运行 Windows Phone 商店。

现在，由一个您已注册的身份提供程序进行身份验证的任何用户都可以访问*TodoItem*表。 为更好地保护用户的数据，还必须实现授权。 为此您获得给定用户的、 用来确定何种级别的访问权限的用户 ID，用户应该具有给定资源。

## <a name="next-steps"> </a>下一步行动

在下一步的教程中，[有脚本的授权用户]，将采用基于身份验证的用户的移动服务提供的用户 ID 值，并使用它来筛选移动服务返回的数据。 有关如何使用其他标识提供程序进行身份验证的信息，请参阅[开始使用身份验证]。 了解更多关于如何使用.NET 中[移动服务.NET 帮助概念参考]的移动服务

<!-- Anchors. -->
[注册您的应用程序进行身份验证和配置移动服务]: #register
[限制为经过身份验证的用户的表权限]: #permissions
[向应用程序添加验证]: #add-authentication
[下一步行动]:#next-steps

<!-- Images. -->

<!-- URLs. -->
[提交应用程序页]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[我的应用程序]: http://go.microsoft.com/fwlink/p/?LinkId=262039 

[添加到现有应用程序的移动服务]: ../mobile-services-windows-store-dotnet-get-started-data.md
[开始使用身份验证]: ../mobile-services-windows-store-dotnet-get-started-users.md
[授权用户使用的脚本]: mobile-services-javascript-backend-service-side-authorization.md

[Azure 的管理门户]: https://manage.windowsazure.com/
[移动服务.NET 帮助概念参考]: mobile-services-windows-dotnet-how-to-use-client-library.md
[实时的 SDK]: http://go.microsoft.com/fwlink/p/?LinkId=262253
 
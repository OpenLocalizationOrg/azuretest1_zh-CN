---
ms.openlocfilehash: c626074212ee5b4f02f10164fc34588ba41206f0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="开始使用身份验证 (Xamarin.iOS) 的移动服务"
    description="了解如何为 Xamarin.iOS Azure 移动服务应用程序中使用身份验证。"
    documentationCenter="xamarin"
    services="mobile-services"
    manager="dwrede"
    authors="lindydonna"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/18/2015"
    ms.author="donnam"/>

# 添加到您的移动服务应用程序的身份验证

[AZURE.INCLUDE [mobile-services-selector-get-started-users](../../includes/mobile-services-selector-get-started-users.md)]

本主题演示如何在 Azure 移动服务从您的应用程序的用户进行身份验证。  在本教程中，您添加到快速入门项目使用标识提供程序支持移动服务的身份验证。 后被成功通过身份验证和授权的移动服务，将显示用户 ID 值。  

本教程将引导您完成以下基本步骤，以便您的应用程序中的身份验证︰

1. [注册您的应用程序进行身份验证和配置移动服务]
2. [限制为经过身份验证的用户的表权限]
3. [向应用程序添加验证]

本教程基于移动服务快速入门。 此外第一次必须完成本教程[开始使用移动服务]。

学完本教程需要[Xamarin.iOS]、 XCode 6.0 和 iOS 7.0 或更高版本。

##<a name="register"></a>注册您的应用程序进行身份验证和配置移动服务

[AZURE.INCLUDE [mobile-services-register-authentication](../../includes/mobile-services-register-authentication.md)]

##<a name="permissions"></a>限制对已验证身份的用户的权限


[AZURE.INCLUDE [mobile-services-restrict-permissions-javascript-backend](../../includes/mobile-services-restrict-permissions-javascript-backend.md)]


3. 在 Xcode，打开创建[移动服务入门]教程完成的项目。

4. 按下**运行**按钮以生成项目，然后启动应用程序在 iPhone 仿真程序;验证应用程序启动后引发未处理的异常状态代码 401 （未经授权）。

    这是因为该应用程序试图访问移动服务作为未经身份验证的用户，但_TodoItem_表现在要求进行身份验证。

接下来，您将更新应用程序验证用户身份之前从移动服务中请求的资源。

##<a name="add-authentication"></a>向应用程序添加验证

1. 打开**ToDoService**项目文件，并添加以下变量

        // Mobile Service logged in user
        private MobileServiceUser user;
        public MobileServiceUser User { get { return user; } }

2. 然后添加一个名为**身份验证**到**ToDoService**的新方法定义为︰

        private async Task Authenticate(MonoTouch.UIKit.UIViewController view)
        {
            try
            {
                user = await client.LoginAsync(view, MobileServiceAuthenticationProvider.MicrosoftAccount);
            }
            catch (Exception ex)
            {
                Console.Error.WriteLine (@"ERROR - AUTHENTICATION FAILED {0}", ex.Message);
            }
        }

    > [AZURE.NOTE] 如果您使用的标识提供程序不是 Microsoft 帐户，更改传递给**LoginAsync**的上面到下面的一个值︰ _Facebook_、_使用 Twitter_， _Google_或_WindowsAzureActiveDirectory_。

3. 将**ToDoItem**表的请求从**ToDoService**构造函数移到名为**CreateTable**的新方法︰

        private async Task CreateTable()
        {
            // Create an MSTable instance to allow us to work with the ToDoItem table
            todoTable = client.GetSyncTable<ToDoItem>();
        }

4. 创建名为**LoginAndGetData**定义为一个新异步公共方法︰

        public async Task LoginAndGetData(MonoTouch.UIKit.UIViewController view)
        {
            await Authenticate(view);
            await CreateTable();
        }

5. **TodoListViewController**在重写**ViewDidAppear**方法并将它定义为找到下面。 此记录在用户如果**ToDoService**还没有句柄添加用户︰

        public override async void ViewDidAppear(bool animated)
        {
            base.ViewDidAppear(animated);

            if (QSToDoService.DefaultService.User == null)
            {
                await QSToDoService.DefaultService.LoginAndGetData(this);
            }

            if (QSToDoService.DefaultService.User == null)
            {
                // TODO:: show error
                return;
            }

            RefreshAsync();
        }
6. 删除**TodoListViewController.ViewDidLoad**到**RefreshAsync**的原始调用。

7. 按**运行**按钮以生成项目，启动该应用程序在 iPhone 的仿真程序，然后登录与您选定的标识提供程序。

    成功登录后，应用程序应该运行错误，并应能为移动服务的查询，并对数据进行更新。

## 获取已完成的示例
下载[已完成的示例项目]。 请务必 Azure 设置更新的**applicationURL**和**applicationKey**变量。

## <a name="next-steps"></a>下一步行动

在下一步的教程中，[有脚本的授权用户]，将采用基于身份验证的用户的移动服务提供的用户 ID 值，并使用它来筛选移动服务返回的数据。

<!-- Anchors. -->
[注册您的应用程序进行身份验证和配置移动服务]: #register
[限制为经过身份验证的用户的表权限]: #permissions
[向应用程序添加验证]: #add-authentication
[下一步行动]:#next-steps

<!-- Images. -->
[4]: ./media/partner-xamarin-mobile-services-ios-get-started-users/mobile-services-selection.png
[5]: ./media/partner-xamarin-mobile-services-ios-get-started-users/mobile-service-uri.png
[13]: ./media/partner-xamarin-mobile-services-ios-get-started-users/mobile-identity-tab.png
[14]: ./media/partner-xamarin-mobile-services-ios-get-started-users/mobile-portal-data-tables.png
[15]: ./media/partner-xamarin-mobile-services-ios-get-started-users/mobile-portal-change-table-perms.png

<!-- URLs. TODO:: update completed example project link with project download -->
[提交应用程序页]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[我的应用程序]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[对于 Windows live SDK]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[开始使用移动服务]: /develop/mobile/tutorials/get-started-xamarin-ios
[有关数据入门]: /develop/mobile/tutorials/get-started-with-data-xamarin-ios
[开始使用身份验证]: /develop/mobile/tutorials/get-started-with-users-xamarin-ios
[开始使用推式通知]: /develop/mobile/tutorials/-get-started-with-push-xamarin-ios
[授权用户使用的脚本]: /develop/mobile/tutorials/authorize-users-in-scripts-xamarin-ios

[Azure 的管理门户]: https://manage.windowsazure.com/
[已完成的示例项目]: http://go.microsoft.com/fwlink/p/?LinkId=331328
[Xamarin.iOS]: http://xamarin.com/download

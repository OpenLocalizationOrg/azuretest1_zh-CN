---
ms.openlocfilehash: f9c9e444fe58d4fc1abdaa9b1ea6b7cf6d83ae9a
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="开始使用 Xamarin iOS 中的移动应用程序的身份验证" 
    description="了解如何使用移动应用程序可通过多种标识提供程序，包括 AAD、 Google、 Facebook、 Twitter，以及 Microsoft Xamarin iOS 应用程序的用户进行身份验证。" 
    services="app-service\mobile" 
    documentationCenter="xamarin" 
    authors="mattchenderson" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-xamarin-ios" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/27/2015" 
    ms.author="mahender"/>

# 添加到您的 Xamarin.iOS 应用程序的身份验证

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]
&nbsp;  
[AZURE.INCLUDE [app-service-mobile-note-mobile-services-preview](../../includes/app-service-mobile-note-mobile-services-preview.md)]

本主题演示如何从客户端应用程序的应用程序服务移动应用程序的用户进行身份验证。 在本教程中，您将添加身份验证到快速入门项目使用标识提供程序所支持的应用程序服务。 后被成功通过身份验证和授权的移动应用程序，将显示用户 ID 值。

本教程基于移动应用程序快速入门。 此外第一次必须完成本教程[创建一个 Xamarin.iOS 应用程序]。 如果不使用下载快速入门服务器项目，您必须向项目添加身份验证扩展包。 有关服务器扩展软件包的详细信息，请参阅[使用.NET 后端服务器用于 Azure 的移动应用程序的 SDK](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)。 

##<a name="create-gateway"></a>创建应用程序的服务网关

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-gateway-preview](../../includes/app-service-mobile-dotnet-backend-create-gateway-preview.md)] 

##<a name="register"></a>注册您的应用程序进行身份验证和配置应用程序服务

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)] 

##<a name="permissions"></a>限制对已验证身份的用户的权限

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)] 

&nbsp;&nbsp;4.在 Visual Studio 或 Xamarin Studio，运行在设备或仿真程序上的客户端项目。 验证应用程序启动后引发未处理的异常状态代码 401 （未经授权）。 到控制台的调试程序，则记录该故障。 因此在 Visual Studio 中，您应该看到输出窗口中的失败。

&nbsp;&nbsp;这种未经授权的错误是因为该应用程序试图访问您作为未经身份验证的用户的移动应用程序后端。 *TodoItem*表现在要求进行身份验证。

接下来，您将更新客户端应用程序请求资源从移动应用程序的后端与身份验证的用户。

##<a name="add-authentication"></a>向应用程序添加验证

在本节中，您将修改该应用程序在显示数据之前显示登录屏幕。 当应用程序启动时，它将不连接到您的应用程序的服务并不会显示任何数据。 第一次，之后用户执行刷新势，将出现登录屏幕;成功登录后将显示的 todo 项列表。

1. 在客户端项目中，打开**QSTodoService.cs**文件，然后添加以下使用声明和成员声明为 QSTodoService:


        // Logged in user
        private MobileServiceUser user; 
        public MobileServiceUser User { get { return user; } }

2. 添加`using`语句 UIKit 并添加新的方法以下面的定义命名为**QSTodoService**的**身份验证**︰

    ```
        using UIKit;
    ```

        public async Task Authenticate(UIViewController view)
        {
            try
            {
                user = await client.LoginAsync(view, MobileServiceAuthenticationProvider.Facebook);
            }
            catch (Exception ex)
            {
                Console.Error.WriteLine (@"ERROR - AUTHENTICATION FAILED {0}", ex.Message);
            }
        }

    >[AZURE.NOTE] 如果您使用的标识提供程序以外的 Facebook，更改传递给**LoginAsync**的上面到下面的一个值︰ _MicrosoftAccount_，_使用 Twitter_， _Google_或_WindowsAzureActiveDirectory_。

3. 打开**QSTodoListViewController.cs**。 修改**ViewDidLoad**去接近结束对**RefreshAsync()**的调用方法的定义︰

        public override async void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            todoService = QSTodoService.DefaultService;
           await todoService.InitializeStoreAsync ();

           RefreshControl.ValueChanged += async (sender, e) => {
                await RefreshAsync ();
           }

            // Comment out the call to RefreshAsync
            // await RefreshAsync ();
        }


4. 修改方法**RefreshAsync**来进行身份验证并显示登录屏幕，如果**用户**属性为 null。 在下面的代码顶部的方法定义︰

        // start of RefreshAsync method
        if (todoService.User == null) {
            await QSTodoService.DefaultService.Authenticate (this);
            if (todoService.User == null) {
                Console.WriteLine ("couldn't login!!");
                return;
            }
        }
        // rest of RefreshAsync method
    
5. 在 Visual Studio 或 Xamarin Studio 中连接到 Xamarin 生成主机在您运行目标设备或仿真程序客户端项目的 Mac 上。 验证该应用程序显示任何数据。 

    拉出将导致登录屏幕显示的项的列表中向下执行刷新笔势。 一旦您已成功输入有效的凭据，应用程序将显示的 todo 项列表，您可以对数据进行更新。

 
<!-- URLs. -->
[提交应用程序页]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[我的应用程序]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[创建一个 Xamarin.iOS 应用程序]: app-service-mobile-dotnet-backend-xamarin-ios-get-started-preview.md

[Azure 的管理门户]: https://portal.azure.com
 

测试

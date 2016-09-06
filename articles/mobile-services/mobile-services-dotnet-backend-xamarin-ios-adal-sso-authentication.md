---
ms.openlocfilehash: e8f20c04661255b505a6ce1ea22dfd051a703a4a
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="验证您使用 Active Directory 验证库单一登录 (Xamarin.iOS) 的应用程序 |Microsoft Azure" 
    description="了解如何为单一登录 ADAL 与 Xamarin.iOS 应用程序中的身份验证用户。" 
    documentationCenter="xamarin" 
    authors="mattchenderson" 
    manager="dwrede" 
    editor="dwrede" 
    services="mobile-services"/>

<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-xamarin-ios" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="06/19/2015" 
    ms.author="mahender"/>

# 对您的应用程序使用 Active Directory 验证库单一登录进行身份验证

[AZURE.INCLUDE [mobile-services-selector-adal-sso](../../includes/mobile-services-selector-adal-sso.md)]

##概述

在本教程中，您将添加身份验证到快速入门项目使用活动目录身份验证库。 

若要能够对用户进行身份验证，必须注册 Azure 活动目录 (AAD) 与您的应用程序。 这是两个步骤。 首先，必须注册您的移动服务，并公开它的权限。 第二，您必须注册您的 Xamarin.iOS 应用程序并授予其访问权限，这些权限


>[AZURE.NOTE] 本教程旨在帮助您更好地了解如何移动服务使您能够执行单一登录应用程序 Xamarin.iOS Azure Active Directory 验证。 如果这是您首次使用移动服务体验，完成本教程[开始使用移动服务]。

##先决条件

本教程要求如下︰

* XCode 4.5 和 iOS 6.0 （或更高版本） 
* 与[Xamarin Studio]在 OS X 上的[Xamarin 扩展]Visual Studio
* [开始使用移动服务]或[数据入门]教程完成。
* Microsoft Azure 移动服务 SDK
* [活动目录身份验证库的 iOS 的 Xamarin 绑定]。

[AZURE.INCLUDE [mobile-services-dotnet-adal-register-service](../../includes/mobile-services-dotnet-adal-register-service.md)]

[AZURE.INCLUDE [mobile-services-dotnet-adal-register-client](../../includes/mobile-services-dotnet-adal-register-client.md)]

##配置为要求身份验证的移动服务

[AZURE.INCLUDE [mobile-services-restrict-permissions-dotnet-backend](../../includes/mobile-services-restrict-permissions-dotnet-backend.md)]

##将验证代码添加到客户端应用程序

1. 活动目录身份验证库 Xamarin 绑定添加到 Xamarin.iOS 项目。 在 Visual Studio 2013，右键单击**引用**，然后选择**添加引用**。 然后浏览到您的绑定库并单击**添加**。 请务必从 ADAL 源添加序列图像板。

2. QSTodoService 类中添加以下项︰ 

        private MobileServiceUser user;
        public MobileServiceUser User { get { return user; } }

        public async Task Authenticate()
        {
            var aadAuthority = @"<INSERT-AUTHORITY-HERE>";
            var aadResource = @"<INSERT-RESOURCE-URI-HERE>";
            var aadClientId = @"<INSERT-CLIENT-ID-HERE>";
            var aadRedirect = @"<INSERT-REDIRECT-URI-HERE>";

            ADAuthenticationError error;
            var aadContext = ADAuthenticationContext.AuthenticationContextWithAuthority(aadAuthority, out error);
            if (error != null)
            {
                throw new InvalidOperationException("Error creating the AAD context: " + error.ErrorDetails);
            }

            var tcs = new TaskCompletionSource<string>();
            aadContext.AcquireTokenWithResource(aadResource, aadClientId, new NSUrl(aadRedirect), result =>
                {
                    if (result.Status == ADAuthenticationResultStatus.SUCCEEDED)
                    {
                        tcs.SetResult(result.AccessToken);
                    }
                    else
                    {
                        string errorDetails;
                        if (result.Status == ADAuthenticationResultStatus.USER_CANCELLED)
                        {
                            errorDetails = "Authentication cancelled by user";
                        }
                        else
                        {
                            errorDetails = result.Error.ErrorDetails;
                        }

                        tcs.SetException(new InvalidOperationException("Error during authentication: " + errorDetails));
                    }
                });
            string accessToken = await tcs.Task;

            var token = new JObject(new JProperty("access_token", accessToken));

            try
            {
                user = await this.client.LoginAsync(MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, token);
            }
            catch (Exception ex)
            {
                Console.Error.WriteLine (@"ERROR - AUTHENTICATION FAILED {0}", ex.Message);
            }
        }

6. 在代码中的`AuthenticateAsync`上面，方法与组织资源调配您的应用程序的名称替换**插入的主管机构-这里**，格式应为 https://login.windows.net/tenant-name.onmicrosoft.com。 此值可以复制从[Azure 管理门户]中将 Azure Active Directory 中的域选项卡中。

7. 在代码中的`AuthenticateAsync`上面的方法替换为您的移动服务**的应用程序 ID URI** **插入资源 URI 这里**。 如果您遵循[如何注册 Azure Active Directory 与]主题您的应用程序 ID URI 应类似于 https://todolist.azure-mobile.net/login/aad。

8. 在代码中的`AuthenticateAsync`上面，方法从本机客户端应用程序中复制的客户机 ID 替换**插入客户端 ID 这里**。

9. 在代码中的`AuthenticateAsync`上面的方法替换**插入重定向 URI 这里**/ 登录/完成您的移动服务的终结点。 这应该是类似于 https://todolist.azure-mobile.net/login/done。


3. 在 QSTodoListViewController 中，通过添加以下代码之前调用 RefreshAsync(); 修改**ViewDidLoad**

        if (QSTodoService.DefaultService.User == null)
        {
            await QSTodoService.DefaultService.Authenticate();
        }

##测试客户端使用的身份验证

1. 从运行菜单上，单击运行以启动该应用程序 
2. 针对将 Azure Active Directory，您将收到登录提示。  
3. 该应用程序进行身份验证并返回 todo 项。

   ![](./media/mobile-services-dotnet-backend-xamarin-ios-adal-sso-authentication/mobile-services-app-run.png)



<!-- URLs. -->
[有关数据入门]: mobile-services-ios-get-started-data.md
[开始使用移动服务]: mobile-services-dotnet-backend-xamarin-ios-get-started.md
[如何在 Azure 的 Active Directory 中注册]: mobile-services-how-to-register-active-directory-authentication.md
[Azure 的管理门户]: https://manage.windowsazure.com/
[活动目录身份验证库的 iOS 的 Xamarin 绑定]: https://github.com/AzureADSamples/NativeClient-Xamarin-iOS
[Xamarin 扩展名]: http://xamarin.com/visual-studio
[Xamarin Studio]: http://xamarin.com/download 
测试t

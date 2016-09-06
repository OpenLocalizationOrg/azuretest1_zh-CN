---
ms.openlocfilehash: 3f7545ede47c010c616e500ce83a56c3affa91b0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="开始使用在 Xamarin Android 的移动应用程序的身份验证" 
    description="了解如何使用移动应用程序可通过多种标识提供程序，包括 AAD、 Google、 Facebook、 Twitter，以及 Microsoft Xamarin Android 应用程序的用户进行身份验证。" 
    services="app-service\mobile" 
    documentationCenter="xamarin" 
    authors="mattchenderson" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-xamarin-android" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/27/2015" 
    ms.author="mahender"/>

# 添加到您的 Xamarin.Android 应用程序的身份验证

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]
&nbsp;  
[AZURE.INCLUDE [app-service-mobile-note-mobile-services-preview](../../includes/app-service-mobile-note-mobile-services-preview.md)]

本主题演示如何从客户端应用程序的移动应用程序的用户进行身份验证。 在本教程中，您添加到快速入门项目使用标识提供程序支持的 Azure 移动应用程序身份验证。 后被成功通过身份验证和授权移动应用程序中，将显示用户 ID 值。

本教程基于移动应用程序快速入门。 此外第一次必须完成本教程[创建一个 Xamarin.Android 应用程序]。 如果不使用下载快速入门服务器项目，您必须向项目添加身份验证扩展包。 有关服务器扩展软件包的详细信息，请参阅[使用.NET 后端服务器用于 Azure 的移动应用程序的 SDK](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)。 

##<a name="create-gateway"></a>创建应用程序的服务网关

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-gateway-preview](../../includes/app-service-mobile-dotnet-backend-create-gateway-preview.md)] 

##<a name="register"></a>注册您的应用程序进行身份验证和配置应用程序服务

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)] 

##<a name="permissions"></a>限制对已验证身份的用户的权限

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)] 

<ol start="4">
<li><p>在 Visual Studio 或 Xamarin Studio 中，运行在设备或仿真程序上的客户端项目。 验证应用程序启动后引发未处理的异常状态代码 401 （未经授权）。</p>
   
    <p>这是因为该应用程序试图访问您作为未经身份验证的用户的移动应用程序后端。 <em>TodoItem</em>表现在要求进行身份验证。</p></li>
</ol>

接下来，您将更新客户端应用程序请求资源从移动应用程序的后端与身份验证的用户。

##<a name="add-authentication"></a>向应用程序添加验证

1. **TodoActivity**类中添加以下属性︰

            private MobileServiceUser user;

2. 将下面的方法添加到**TodoActivity**类中︰ 

            private async Task Authenticate()
            {
                try
                {
                    user = await client.LoginAsync(this, MobileServiceAuthenticationProvider.Facebook);
                    CreateAndShowDialog(string.Format("you are now logged in - {0}", user.UserId), "Logged in!");
                }
                catch (Exception ex)
                {
                    CreateAndShowDialog(ex, "Authentication failed");
                }
            }

    这将创建一个新方法来验证用户的身份。 上面的示例代码中的用户是通过使用 Facebook 的登录身份验证。 对话框用于显示一次经过身份验证的用户 ID。 

    > [AZURE.NOTE] 如果您正在使用 Facebook 不标识提供程序，更改传递给**LoginAsync**的上面到下面的一个值︰ _MicrosoftAccount_，_使用 Twitter_， _Google_或_WindowsAzureActiveDirectory_。

3. 在**OnCreate**方法中，实例化的代码之后添加以下代码行`MobileServiceClient`对象。

        // Create the Mobile Service Client instance, using the provided
        // Mobile Service URL, Gateway URL and key
        client = new MobileServiceClient (applicationURL, gatewayURL, applicationKey);
        
        await Authenticate(); // Added for authentication

    此调用启动身份验证过程，并等待它以异步方式。


4. 在 Visual Studio 或 Xamarin Studio，在设备或仿真程序上运行的客户端项目并登录到您选定的标识提供程序。 

    成功登录后，应用程序将显示您的登录 id 和托多项的列表，您可以对数据进行更新。


<!-- URLs. -->
[提交应用程序页]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[我的应用程序]: http://go.microsoft.com/fwlink/p/?LinkId=262039

[创建一个 Xamarin.Android 应用程序]: app-service-mobile-dotnet-backend-xamarin-android-get-started-preview.md

[Azure 的管理门户]: https://portal.azure.com
 

测试

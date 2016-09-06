---
ms.openlocfilehash: 326a9181b4c5f96a5caae4b055f78dee975d19f7
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="身份验证与 Active Directory 验证库单一登录 （Windows 应用商店） 您的应用程序 |Microsoft Azure"
    description="了解如何为单一登录 ADAL 与 Windows 应用商店应用程序中的身份验证用户。"
    documentationCenter="windows"
    authors="wesmc7777"
    manager="dwrede"
    editor=""
    services="mobile-services"/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/18/2015" 
    ms.author="wesmc"/>

# 对您的应用程序使用 Active Directory 验证库单一登录进行身份验证

[AZURE.INCLUDE [mobile-services-selector-adal-sso](../../includes/mobile-services-selector-adal-sso.md)]

##概述

在本教程中，您添加到快速启动项目使用活动目录身份验证库来支持[客户端直接登录操作](http://msdn.microsoft.com/library/azure/jj710106.aspx)使用 Azure Active Directory 身份验证。 若要支持使用 Azure Active Directory[服务直接登录操作](http://msdn.microsoft.com/library/azure/dn283952.aspx)，开始[添加到您的移动服务应用程序的身份验证](../mobile-services-dotnet-backend-windows-store-dotnet-get-started-users.md)本教程。

若要能够对用户进行身份验证，必须注册 Azure 活动目录 (AAD) 与您的应用程序。 这是两个步骤。 首先，必须注册您的移动服务，并公开它的权限。 第二，您必须注册您的 Windows 应用商店应用程序并授予其访问权限，这些权限


>[AZURE.NOTE] 本教程旨在帮助您更好地了解如何移动服务使您能够执行单一登录 Windows 应用商店应用程序使用[客户端定向登录操作](http://msdn.microsoft.com/library/azure/jj710106.aspx)Azure Active Directory 验证。 如果这是您首次使用移动服务体验，完成本教程[开始使用移动服务]。


##先决条件

本教程要求如下︰

* Visual Studio 2013 年 Windows 8.1 上运行。
* [开始使用移动服务]或[数据入门]教程完成。
* Microsoft Azure 移动服务 SDK NuGet 程序包
* 活动目录身份验证库 NuGet 程序包

[AZURE.INCLUDE [mobile-services-dotnet-adal-register-service](../../includes/mobile-services-dotnet-adal-register-service.md)]

##在 Azure 的 Active Directory 中注册您的应用程序

Azure Active Directory 中注册该应用程序，您必须将其关联到 Windows 应用商店，并有包安全标识符 (SID) 应用程序。 包 SID 获取注册 Azure Active Directory 中的本机应用程序设置。


###新的存储应用程序名称相关联的应用程序

1. 在 Visual Studio 中，右键单击应用程序客户端项目，然后单击**存储**和**相关联的应用程序与存储**

    ![][1]

2. 登录到您的开发人员中心帐户。

3. 请输入您想要保留的应用程序，然后单击**保留**的应用程序名。

    ![][2]

4. 选择新的应用程序名称，然后单击**下一步**。

5. 请单击**关联**将应用程序与存储区名称相关联。


###获取包的 SID 对于您的应用程序。

现在，您需要检索 SID 将与本机应用程序设置配置您的软件包。

1. 登录到您的[Windows 开发人员中心仪表板]并单击应用程序中的**编辑**。

    ![][3]

2. 然后单击**服务**

    ![][4]

3. 然后单击**Live 服务站点**。

    ![][5]

4. 从页面顶端复制您的软件包的 SID。

    ![][6]

###创建本机应用程序注册

1. 定位到**Active Directory**在[Azure 管理门户]中，然后单击您的目录。

    ![][7]

2. 单击**应用程序**选项卡的顶部，然后单击**添加**到应用程序。

    ![][8]

3. 单击**添加我的公司正在开发的应用程序**。

4. 在添加应用程序向导中，您的应用程序输入一个**名称**并单击**本机客户端应用程序**类型。 然后单击以继续。

    ![][9]

5. 在**重定向 URI**中，将 App 包装箱 SID 先复制粘贴，然后单击以完成本机应用程序注册。

    ![][10]

6. 单击**配置**选项卡为本机应用程序和复制**客户机 ID**。 您将稍后需要它。

    ![][11]

7. 向下滚动到**到其他应用程序的权限**部分页面并授予到前面注册移动服务应用程序的完全访问权限。 然后单击**保存**

    ![][12]

您的移动服务现已配置中 AAD 接收来自您的应用程序的单一登录方式进行登录。



##配置为要求身份验证的移动服务

[AZURE.INCLUDE [mobile-services-restrict-permissions-dotnet-backend](../../includes/mobile-services-restrict-permissions-dotnet-backend.md)]

##将验证代码添加到客户端应用程序

1. 打开您的 Windows 客户端应用程序项目中存储 Visual Studio。

[AZURE.INCLUDE [mobile-services-dotnet-adal-install-nuget](../../includes/mobile-services-dotnet-adal-install-nuget.md)]

4. 在 Visual Studio 的解决方案资源管理器窗口中，打开 MainPage.xaml.cs 文件，然后添加以下使用语句。

        using Windows.UI.Popups;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Newtonsoft.Json.Linq;


5. 将下面的代码添加到主页类声明的`AuthenticateAsync`方法。

        private MobileServiceUser user;
        private async Task AuthenticateAsync()
        {
            string authority = "<INSERT-AUTHORITY-HERE>";
            string resourceURI = "<INSERT-RESOURCE-URI-HERE>";
            string clientID = "<INSERT-CLIENT-ID-HERE>";
            while (user == null)
            {
                string message;
                try
                {
                  AuthenticationContext ac = new AuthenticationContext(authority);
                  AuthenticationResult ar = await ac.AcquireTokenAsync(resourceURI, clientID, (Uri) null);
                  JObject payload = new JObject();
                  payload["access_token"] = ar.AccessToken;
                  user = await App.MobileService.LoginAsync(MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
                  message = string.Format("You are now logged in - {0}", user.UserId);
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

6. 在代码中的`AuthenticateAsync`上面，方法与组织资源调配您的应用程序的名称替换**插入的主管机构-这里**，格式应为 https://login.windows.net/tenant-name.onmicrosoft.com。 此值可以复制从[Azure 管理门户]中将 Azure Active Directory 中的域选项卡中。

7. 在代码中的`AuthenticateAsync`上面的方法替换为您的移动服务**的应用程序 ID URI** **插入资源 URI 这里**。 如果您遵循[如何注册 Azure Active Directory 与]主题您的应用程序 ID URI 应类似于 https://todolist.azure-mobile.net/login/aad。

8. 在代码中的`AuthenticateAsync`上面，方法从本机客户端应用程序中复制的客户机 ID 替换**插入客户端 ID 这里**。

9. 在 Visual Studio 的解决方案资源管理器窗口中，打开客户端项目中的 Package.appxmanifest 文件。 单击**功能**选项卡并实现**企业级应用程序**和**专用网络 （客户端和服务器）**。 保存该文件。

    ![][14]

10. 在 MainPage.cs 文件中，更新`OnNavigatedTo`事件处理程序来调用`AuthenticateAsync`，如下所示的方法。

        protected override async void OnNavigatedTo(NavigationEventArgs e)
        {
            await AuthenticateAsync();
            await RefreshTodoItems();
        }


##测试客户端使用的身份验证

1. 在 Visual Studio，运行客户端应用程序。
2. 针对将 Azure Active Directory，您将收到登录提示。  
3. 该应用程序进行身份验证并返回 todo 项。

    ![][15]




<!-- Images -->
[0]: ./media/mobile-services-windows-store-dotnet-adal-sso-authenticate/mobile-services-aad-app-manage-manifest.png
[1]: ./media/mobile-services-windows-store-dotnet-adal-sso-authentication/mobile-services-vs-associate-app.png
[2]: ./media/mobile-services-windows-store-dotnet-adal-sso-authentication/mobile-services-vs-reserve-store-appname.png
[3]: ./media/mobile-services-windows-store-dotnet-adal-sso-authentication/mobile-services-store-app-edit.png
[4]: ./media/mobile-services-windows-store-dotnet-adal-sso-authentication/mobile-services-store-app-services.png
[5]: ./media/mobile-services-windows-store-dotnet-adal-sso-authentication/mobile-services-live-services-site.png
[6]: ./media/mobile-services-windows-store-dotnet-adal-sso-authentication/mobile-services-store-app-package-sid.png
[7]: ./media/mobile-services-windows-store-dotnet-adal-sso-authentication/mobile-services-select-aad.png
[8]: ./media/mobile-services-windows-store-dotnet-adal-sso-authentication/mobile-services-aad-applications-tab.png
[9]: ./media/mobile-services-windows-store-dotnet-adal-sso-authentication/mobile-services-native-selection.png
[10]: ./media/mobile-services-windows-store-dotnet-adal-sso-authentication/mobile-services-native-sid-redirect-uri.png
[11]: ./media/mobile-services-windows-store-dotnet-adal-sso-authentication/mobile-services-native-client-id.png
[12]: ./media/mobile-services-windows-store-dotnet-adal-sso-authentication/mobile-services-native-add-permissions.png
[14]: ./media/mobile-services-windows-store-dotnet-adal-sso-authentication/mobile-services-package-appxmanifest.png
[15]: ./media/mobile-services-windows-store-dotnet-adal-sso-authentication/mobile-services-app-run.png

<!-- URLs. -->
[如何在 Azure 的 Active Directory 中注册]: mobile-services-how-to-register-active-directory-authentication.md
[Azure 的管理门户]: https://manage.windowsazure.com/
[有关数据入门]: ../mobile-services-dotnet-backend-windows-store-dotnet-get-started-data.md
[开始使用移动服务]: mobile-services-dotnet-backend-windows-store-dotnet-get-started.md
[Windows 开发人员中心的仪表板]: http://go.microsoft.com/fwlink/p/?LinkID=266734
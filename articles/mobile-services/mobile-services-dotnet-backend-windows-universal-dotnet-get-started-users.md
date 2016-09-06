---
ms.openlocfilehash: cf886985b703d40672d8974e846872875b650d9b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="将身份验证添加到您的 Windows 8.1 通用应用程序 |Microsoft Azure" 
    description="了解如何使用移动服务通过使用各种标识提供程序，包括 Google、 Facebook、 Twitter，以及 Microsoft Windows 8.1 通用应用程序的用户进行身份验证。" 
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
    ms.date="07/01/2015" 
    ms.author="glenga"/>

# 添加到您的移动服务应用程序的身份验证 

[AZURE.INCLUDE [mobile-services-selector-get-started-users](../../includes/mobile-services-selector-get-started-users.md)]

## 概述

本主题演示如何在 Azure 移动服务从通用的 Windows 应用程序的用户进行身份验证。 在本教程中，您添加到快速入门项目使用标识提供程序支持移动服务的身份验证。 后被成功通过身份验证和授权的移动服务，将显示用户 ID 值。

本教程基于移动服务快速入门。 此外第一次必须完成[移动服务入门]教程或[添加到现有应用程序的移动服务](mobile-services-dotnet-backend-windows-universal-dotnet-get-started-data.md)。 

>[AZURE.NOTE]本教程展示了如何使用 Windows 应用商店和 Windows Phone 商店 8.1 的应用程序中的用户导向服务器身份验证。 Windows Phone 8.0 或 Windows Phone Silverlight 8.1 的应用程序，请参阅此版本的[开始使用移动服务中的身份验证](mobile-services-dotnet-backend-windows-phone-get-started-users.md)。 前景色定向客户端的身份验证信息，请参阅[登录 Google，Microsoft 和 Azure 移动服务与 Facebook Sdk](http://azure.microsoft.com/blog/2014/10/27/logging-in-with-google-microsoft-and-facebook-sdks-to-azure-mobile-services/)。

##<a name="register"></a>注册您的应用程序进行身份验证和配置移动服务

[AZURE.INCLUDE [mobile-services-register-authentication](../../includes/mobile-services-register-authentication.md)] 

[AZURE.INCLUDE [mobile-services-dotnet-backend-aad-server-extension](../../includes/mobile-services-dotnet-backend-aad-server-extension.md)] 

##<a name="permissions"></a>限制对已验证身份的用户的权限

[AZURE.INCLUDE [mobile-services-restrict-permissions-dotnet-backend](../../includes/mobile-services-restrict-permissions-dotnet-backend.md)] 

&nbsp;&nbsp;6.在 Visual Studio 中，用鼠标右键单击该任务列表应用程序的 Windows 应用商店项目，然后单击**设为启动项目**。

&nbsp;&nbsp;7.在共享项目中，打开 App.xaml.cs 项目文件，请的定义的[MobileServiceClient](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.aspx)中，请确保它被配置为连接到运行在 Azure 的移动服务。

>[AZURE.NOTE]当使用 Visual Studio 工具来将您的应用程序连接到移动服务时，该工具将生成两套**MobileServiceClient**定义，另一个用于每个客户端平台。 这是通过统一简化生成的代码的好时机`#if...#endif`包装成单个解包定义这两个版本的应用程序所使用的**MobileServiceClient**定义。 您不必这样做，当您从 Azure 管理门户下载快速启动应用程序。

&nbsp;&nbsp;8.按 F5 键以运行 Windows 应用商店应用程序，并验证应用程序启动后引发未处理的异常状态代码 401 （未经授权）。
   
&nbsp;&nbsp;这是因为该应用程序试图访问移动服务作为未经身份验证的用户，但*TodoItem*表现在要求进行身份验证。

接下来，您将更新应用程序验证用户身份之前从移动服务中请求的资源。

##<a name="add-authentication"></a>向应用程序添加验证

[AZURE.INCLUDE [mobile-services-windows-universal-dotnet-authenticate-app](../../includes/mobile-services-windows-universal-dotnet-authenticate-app.md)] 

>[AZURE.NOTE]如果您注册您的 Windows 应用商店应用程序软件包信息移动服务，则应调用<a href="http://go.microsoft.com/fwlink/p/?LinkId=311594" target="_blank">LoginAsync</a> ，通过提供**真正**的*useSingleSignOn*参数值的方法。 如果不这样做，您的用户将继续调用登录方法每次显示登录提示。

##<a name="tokens"></a>在客户端上存储的授权标记

[AZURE.INCLUDE [mobile-services-windows-store-dotnet-authenticate-app-with-token](../../includes/mobile-services-windows-store-dotnet-authenticate-app-with-token.md)] 


## <a name="next-steps"> </a>下一步行动

在下一步的教程中，[服务端的移动服务用户授权，][授权用户提供的脚本]，将采取基于身份验证的用户的移动服务提供的用户 ID 值，并使用它来筛选移动服务返回的数据。 

##请参见

+ [增强的用户功能](http://azure.microsoft.com/blog/2014/10/02/custom-login-scopes-single-sign-on-new-asp-net-web-api-updates-to-the-azure-mobile-services-net-backend/)<br/>
您可以通过在.NET 后端调用**ServiceUser.GetIdentitiesAsync()**方法维护标识提供程序通过移动服务中的其他用户数据。 

+ [移动服务.NET 帮助概念参考]<br/>了解有关如何使用.NET 客户端移动服务的详细信息。


<!-- Anchors. -->
[注册您的应用程序进行身份验证和配置移动服务]: #register
[限制为经过身份验证的用户的表权限]: #permissions
[向应用程序添加验证]: #add-authentication
[存储在客户端的身份验证令牌]: #tokens
[下一步行动]:#next-steps


<!-- URLs. -->
[提交应用程序页]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[我的应用程序]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[对于 Windows live SDK]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[为使用实时连接的 Windows 应用商店应用程序的单一登录]: mobile-services-windows-store-dotnet-single-sign-on.md
[开始使用移动服务]: mobile-services-dotnet-backend-windows-store-dotnet-get-started.md
[有关数据入门]: ../mobile-services-dotnet-backend-windows-store-dotnet-get-started-data.md
[开始使用身份验证]: ../mobile-services-dotnet-backend-windows-store-dotnet-get-started-users.md
[开始使用推式通知]: ../mobile-services-dotnet-backend-windows-store-dotnet-get-started-push.md
[授权用户使用的脚本]: ../mobile-services-dotnet-backend-windows-store-dotnet-authorize-users-in-scripts.md
[JavaScript 和 HTML]: ../mobile-services-dotnet-backend-windows-store-javascript-get-started-users.md

[Azure 的管理门户]: https://manage.windowsazure.com/
[移动服务.NET 帮助概念参考]: mobile-services-windows-dotnet-how-to-use-client-library.md
[注册为 Microsoft 身份验证 Windows 应用商店应用程序软件包]: ../mobile-services-how-to-register-store-app-package-microsoft-authentication.md
 

测试

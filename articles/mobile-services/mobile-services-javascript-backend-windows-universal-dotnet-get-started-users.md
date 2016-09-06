---
ms.openlocfilehash: c87f1c53ba56a721e892c374be2c60d6b1fe6b8a
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="开始使用身份验证 （Windows 应用商店） |Microsoft Azure" 
    description="了解如何使用移动服务通过多种身份提供程序，包括 Google、 Facebook、 Twitter，以及 Microsoft Windows 应用商店应用程序的用户进行身份验证。" 
    services="mobile-services" 
    documentationCenter="windows" 
    authors="ggailey777" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-store" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="06/14/2015" 
    ms.author="glenga"/>

# 添加到您的移动服务应用程序的身份验证

[AZURE.INCLUDE [mobile-services-selector-get-started-users](../../includes/mobile-services-selector-get-started-users.md)]      

本主题演示如何在 Azure 移动服务从通用的 Windows 应用程序的用户进行身份验证。 在本教程中，您添加到快速入门项目使用标识提供程序支持移动服务的身份验证。 后被成功通过身份验证和授权的移动服务，将显示用户 ID 值。

本教程基于移动服务快速入门。 此外第一次必须完成[移动服务入门]教程或[添加到现有应用程序的移动服务](mobile-services-javascript-backend-windows-universal-dotnet-get-started-data.md)。 

>[AZURE.NOTE]本教程演示了如何在 Windows 应用商店和 Windows Phone 商店 8.1 的应用程序中的用户进行身份验证。 Windows Phone 8.0 或 Windows Phone Silverlight 8.1 的应用程序，请参阅此版本的[开始使用移动服务中的身份验证](mobile-services-windows-phone-get-started-users.md)。

>本教程演示如何使用不同的身份提供程序服务管理身份验证流程。 此方法可以很容易地配置，并且支持多个提供程序。 若要改为使用 Windows 应用商店应用程序中使用实时 SDK 的客户端管理流程，请参阅主题[验证您的 Windows 应用商店应用程序与客户端使用 Microsoft 帐户托管身份验证](mobile-services-windows-store-dotnet-single-sign-on.md)。 

##<a name="register"></a> 注册您的应用程序进行身份验证和配置移动服务

[AZURE.INCLUDE [mobile-services-register-authentication](../../includes/mobile-services-register-authentication.md)] 

##<a name="permissions"></a> 限制对已验证身份的用户的权限

[AZURE.INCLUDE [mobile-services-restrict-permissions-windows](../../includes/mobile-services-restrict-permissions-windows.md)] 
 
>[AZURE.NOTE] 当使用 Visual Studio 工具来将您的应用程序连接到移动服务时，该工具将生成两套**MobileServiceClient**定义，另一个用于每个客户端平台。 这是通过统一简化生成的代码的好时机`#if...#endif`包装成单个解包定义这两个版本的应用程序所使用的**MobileServiceClient**定义。 您不需要这样做，如果您从 Azure 管理门户下载快速启动应用程序。

##<a name="add-authentication"></a> 向应用程序添加验证

[AZURE.INCLUDE [mobile-services-windows-universal-dotnet-authenticate-app](../../includes/mobile-services-windows-universal-dotnet-authenticate-app.md)] 

现在，由您信任的身份提供方进行身份验证的任何用户都可以访问*TodoItem*表。 为更好地保护用户的数据，还必须实现授权。 为此您获得给定用户的、 用来确定何种级别的访问权限的用户 ID，用户应该具有给定资源。

##<a name="tokens"></a>在客户端上存储的授权令牌

[AZURE.INCLUDE [mobile-services-windows-store-dotnet-authenticate-app-with-token](../../includes/mobile-services-windows-store-dotnet-authenticate-app-with-token.md)] 

## <a name="next-steps"> </a>下一步行动

在下一步的教程中，[服务端的移动服务用户的授权](mobile-services-javascript-backend-service-side-authorization.md)，将采用基于身份验证的用户的移动服务提供的用户 ID 值，并使用它来筛选移动服务返回的数据。 

##请参见

+ [增强的用户功能](http://go.microsoft.com/fwlink/p/?LinkId=506605)<br/>
您可以通过移动服务中标识提供程序维护的服务器脚本中调用**user.getIdentities()**函数的其他用户数据。 

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

[开始使用移动服务]: mobile-services-javascript-backend-windows-store-dotnet-get-started.md
[有关数据入门]: ../mobile-services-javascript-backend-windows-store-dotnet-get-started-data.md
[开始使用身份验证]: ../mobile-services-javascript-backend-windows-store-dotnet-get-started-users.md
[开始使用推式通知]: ../mobile-services-javascript-backend-windows-store-dotnet-get-started-push.md
[授权用户使用的脚本]: ../mobile-services-windows-store-dotnet-authorize-users-in-scripts.md
[JavaScript 和 HTML]: mobile-services-windows-store-javascript-get-started-users.md

[Azure 的管理门户]: https://manage.windowsazure.com/
[移动服务.NET 帮助概念参考]: mobile-services-windows-dotnet-how-to-use-client-library.md
[注册为 Microsoft 身份验证 Windows 应用商店应用程序软件包]: ../mobile-services-how-to-register-store-app-package-microsoft-authentication.md
 
测试

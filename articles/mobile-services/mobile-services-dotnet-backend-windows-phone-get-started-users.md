---
ms.openlocfilehash: 156d8a8f6ea2baafd44b01626f93dbf150091af9
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="将身份验证添加到您的 Windows Phone Silverlight 应用程序 |Microsoft Azure" 
    description="了解如何使用移动服务的 Windows Phone Silverlight 应用程序通过一系列身份提供程序，包括 Google、 Facebook、 Twitter，以及 Microsoft 帐户的用户进行身份验证。" 
    services="mobile-services" 
    documentationCenter="windows" 
    authors="ggailey777" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="07/21/2015" 
    ms.author="glenga"/>

# 添加到您的移动服务应用程序的身份验证

[AZURE.INCLUDE [mobile-services-selector-get-started-users](../../includes/mobile-services-selector-get-started-users.md)]

## 概述

本主题演示如何从您的 Windows Phone 应用程序在 Azure 移动服务中的用户进行身份验证。 在本教程中，您添加到快速入门项目使用标识提供程序支持移动服务的身份验证。 后被成功通过身份验证和授权的移动服务，将显示用户 ID 值。

>[AZURE.NOTE] 本主题支持仅 Windows Phone 8.0 和 Windows Phone 8.1 Silverlight 的应用程序。 如果您有 Windows Phone 商店 8.1 或通用的 Windows 应用程序，请按照[本主题的通用的 Windows 应用程序版本](mobile-services-dotnet-backend-windows-universal-dotnet-get-started-users.md)。

本教程基于移动服务快速入门。 此外第一次必须完成本教程[开始使用移动服务]。 

## 注册您的应用程序进行身份验证和配置移动服务

[AZURE.INCLUDE [mobile-services-register-authentication](../../includes/mobile-services-register-authentication.md)] 

[AZURE.INCLUDE [mobile-services-dotnet-backend-aad-server-extension](../../includes/mobile-services-dotnet-backend-aad-server-extension.md)] 

##  限制对已验证身份的用户的权限

[AZURE.INCLUDE [mobile-services-restrict-permissions-dotnet-backend](../../includes/mobile-services-restrict-permissions-dotnet-backend.md)] 

&nbsp;&nbsp;6.在 Visual Studio 中，打开您的客户端应用程序项目并确保在 App.xaml.cs， **MobileServiceClient**的实例配置为使用云到移动服务的 URL。

&nbsp;&nbsp;7.按 F5 键以运行此快速入门基于的应用程序;验证应用程序启动后引发未处理的异常状态代码 401 （未经授权）。 当应用程序尝试访问移动服务作为未经身份验证的用户，但*TodoItem*表现在要求进行身份验证时，将发生这种情况。 

接下来，您将更新应用程序验证用户身份之前从移动服务中请求的资源。

## 向应用程序添加验证

[AZURE.INCLUDE [mobile-services-windows-phone-authenticate-app](../../includes/mobile-services-windows-phone-authenticate-app.md)]

## 在客户端上存储的授权标记

[AZURE.INCLUDE [mobile-services-windows-phone-authenticate-app-with-token](../../includes/mobile-services-windows-phone-authenticate-app-with-token.md)] 

##  下一步行动

在下一步的教程中，[服务端的移动服务用户授权，][授权用户提供的脚本]，将采取基于身份验证的用户的移动服务提供的用户 ID 值，并使用它来筛选移动服务返回的数据。 了解更多关于如何使用.NET 中[移动服务.NET 帮助概念参考]的移动服务

<!-- Anchors. -->


<!-- URLs. -->
[提交应用程序页]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[我的应用程序]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[对于 Windows live SDK]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[为 Windows Phone 通过实时连接的应用程序的单一登录]: mobile-services-windows-phone-single-sign-on.md
[开始使用移动服务]: ../mobile-services-dotnet-backend-windows-phone-get-started.md
[有关数据入门]: mobile-services-dotnet-backend-windows-phone-get-started-data.md
[开始使用身份验证]: mobile-services-dotnet-backend-windows-phone-get-started-users.md
[开始使用推式通知]: mobile-services-dotnet-backend-windows-phone-get-started-push.md
[授权用户使用的脚本]: ../mobile-services-dotnet-backend-windows-phone-authorize-users-in-scripts.md
[JavaScript 和 HTML]: ../mobile-services-dotnet-backend-windows-store-javascript-get-started-users.md

[Azure 的管理门户]: https://manage.windowsazure.com/
[移动服务.NET 帮助概念参考]: mobile-services-windows-dotnet-how-to-use-client-library.md
[注册为 Microsoft 身份验证 Windows 应用商店应用程序软件包]: ../mobile-services-how-to-register-store-app-package-microsoft-authentication.md
 
测试

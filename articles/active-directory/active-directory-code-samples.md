---
ms.openlocfilehash: c5f5b2bd9c52276ddd2441306abe753f024b8c03
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="Azure 的 Active Directory 代码示例"
   description="Azure Active Directory 代码示例，按方案组织的索引。"
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="msmbaldwin"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="07/30/2015"
   ms.author="mbaldwin"/>

# Azure 的 Active Directory 代码示例

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

可以使用 Microsoft Azure 活动目录 (AD Azure) 要添加到您的 web 应用程序和 web Api 的身份验证和授权。 本节提供指向代码示例，向您展示它如何完成，您可以使用您的应用程序中的代码段。 在代码示例页上，您可以找到详细的阅读的我的要求、 安装和设置帮助的主题。 并对代码注释来帮助您了解的关键部分。

要了解每个样本类型的基本情况，请参阅 Azure AD 身份验证方案。

导致我们在 GitHub 上的示例︰ [Microsoft Azure 活动目录示例和文档](https://github.com/AzureADSamples)。

## 为 Web 应用程序的 web 浏览器

这些示例演示如何编写一个 web 应用程序指示用户的浏览器来使它们登录到 Azure 的广告。



| 语言/平台 | 示例 | 说明
| ----------------- | ------ | -----------
| C# /.NET | [WebApp-OpenIDConnect-最低](http://github.com/AzureADSamples/WebApp-OpenIDConnect-DotNet) | 使用 OpenID 连接 （ASP.Net OpenID 连接 OWIN 中间件） 从 Azure AD 租户的用户进行身份验证。
| C# /.NET | [WebApp MultiTenant OpenIdConnect 最低](https://github.com/AzureADSamples/WebApp-MultiTenant-OpenIdConnect-DotNet) | 多租户使用 OpenID 连接 （ASP.Net OpenID 连接 OWIN 中间件） 来验证用户的身份从多个 Azure AD 承租人的.NET MVC web 应用程序。
| C# /.NET | [WebApp-WSFederation-最低](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet) | 使用 WS 联合身份验证 （ASP.Net WS 联合身份验证 OWIN 中间件） 来验证用户的身份从 Azure AD 租户。






## 单页应用程序 (SPA)

此示例演示如何编写使用 Azure 广告保护的单页应用程序。  

| 语言/平台 | 示例 | 说明
| ----------------- | ------ | -----------
| JavaScript、 C# /.NET | [SinglePageApp 最低](https://github.com/AzureADSamples/SinglePageApp-DotNet) | 使用 JavaScript 和 Azure AD ADAL 安全使用 ASP.NET web API 后端实现一个基于 AngularJS 的单页应用程序。


## 本机应用程序与 Web API

以下代码示例说明如何生成本机客户端应用程序调用 web Api 的 Azure AD 的保护。 他们使用[Azure AD 身份验证库 (ADAL)](http://go.microsoft.com/fwlink/?LinkID=258232)和[Azure 广告 OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx)。

| 语言/平台 | 示例 | 说明
| ----------------- | ------ | -----------
| Javascript | [NativeClient-MultiTarget-Cordova](https://github.com/AzureADSamples/NativeClient-MultiTarget-Cordova) | 使用 Apache Cordova 的 ADAL 插件生成的 Apache Cordova 应用程序用于调用一个 web API 和使用 Azure 广告进行身份验证。
| C# /.NET | [NativeClient 最低](http://github.com/AzureADSamples/NativeClient-DotNet) | 使用 Azure 广告受保护的.NET WPF 应用程序的调用 web API。
| C# /.NET | [NativeClient WindowsStore](http://github.com/AzureADSamples/NativeClient-WindowsStore) | Azure AD 的 Windows 应用商店应用程序调用 web API 都得到保护。
| C# /.NET | [NativeClient WebAPI-MultiTenant WindowsStore](https://github.com/AzureADSamples/NativeClient-WebAPI-MultiTenant-WindowsStore) | 多租户 web 使用 Azure AD 安全 API 调用 Windows 应用商店应用程序。
| C# /.NET | [-最低的 WebAPI OnBehalfOf](http://github.com/AzureADSamples/WebAPI-OnBehalfOf-DotNet) | 调用 web API，从而获取令牌代表原始的用户，然后使用该标记来调用其他 web API 的本机客户端应用程序。
| C# /.NET | [NativeClient WindowsPhone8.1](https://github.com/AzureADSamples/NativeClient-WindowsPhone8.1) | Windows 应用商店应用程序的调用 web API 的 Windows Phone 8.1 受到由 Azure 的广告。
| ObjC | [NativeClient Io](http://github.com/AzureADSamples/NativeClient-iOS) | 调用 web API 的 iOS 应用程序身份验证要求 Azure 的广告。
| C# /.NET | [-最低的 WebAPI ManuallyValidateJwt](https://github.com/AzureADSamples/WebAPI-ManuallyValidateJwt-DotNet) | 本机客户端包含的应用程序逻辑可以处理而不是使用 OWIN 中间件在 web API，JWT 令牌。
| C# / Xamarin | [NativeClient-Xamarin-Android](https://github.com/AzureADSamples/NativeClient-Xamarin-Android) | Android 库 Xamarin 绑定到本机 Azure AD 身份验证库 (ADAL)。
| C# / Xamarin | [NativeClient-Xamarin-iOS](http://github.com/AzureADSamples/NativeClient-Xamarin-iOS) | IOS Xamarin 绑定到本机 Azure AD 身份验证库 (ADAL)。
| C# / Xamarin | [NativeClient-MultiTarget-最低](http://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet) | Xamarin 项目针对五个平台和调用 web API 都受到由 Azure 的广告。
| C# /.NET | [NativeClient 无头最低](http://github.com/AzureADSamples/NativeClient-Headless-DotNet) | 本机应用程序，执行非交互式身份验证的调用 web API 被安全通过 Azure 的广告。



## Web 应用程序与 Web API

这些代码示例显示如何使用[OAuth 2.0 在 Azure 的广告](https://msdn.microsoft.com/library/azure/dn645545.aspx)来生成 web 应用程序调用 web Api 的 Azure AD 的保护。

| 语言/平台 | 示例 | 说明
| ----------------- | ------ | -----------
| C# /.NET | [WebApp WebAPI OpenIDConnect 最低](http://github.com/AzureADSamples/WebApp-WebAPI-OpenIDConnect-DotNet) | 调用 web API 使用已登录的用户的权限。
|  C# /.NET | [WebApp 的 WebAPI-OAuth2-AppIdentity-最低](http://github.com/AzureADSamples/WebApp-WebAPI-OAuth2-AppIdentity-DotNet) | 调用应用程序的权限的 web API。
| C# /.NET | [WebApp 的 WebAPI-OAuth2-用户身份的最低](http://github.com/AzureADSamples/WebApp-WebAPI-OAuth2-UserIdentity-Dotnet) | [在 Azure 广告 OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) 2.0 授权添加现有 web 应用程序，以便它可以调用 web API。
| JavaScript | [WebAPI Nodejs](http://github.com/AzureADSamples/WebAPI-Nodejs) | 设置 API 保护 Azure 广告与集成在一起的 REST API 服务。 包括具有 Web API 的 Node.js 服务器。
| C# /.NET | [WebApp 的 WebAPI-MultiTenant-OpenIdConnect-最低](https://github.com/AzureADSamples/WebApp-WebAPI-MultiTenant-OpenIdConnect-DotNet) |  多租户 MVC web 应用程序使用 OpenID 连接 （ASP.Net OpenID 连接 OWIN 中间件） 来验证用户的身份从 Azure AD 租户。 使用授权代码来调用图形 API。

## 服务器或后台程序到 Web API 的应用程序

这些代码示例显示如何构建一个守护程序或服务器的应用程序，通过使用[Azure AD 身份验证库 (ADAL)](http://go.microsoft.com/fwlink/?LinkID=258232)和[Azure 广告 OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx)从 web API 获取资源。

| 语言/平台 | 示例 | 说明
| ----------------- | ------ | -----------
| C# /.NET | [守护程序最低](http://github.com/AzureADSamples/Daemon-DotNet) | 控制台应用程序调用 web API。 客户端凭据是一个密码。
| C# /.NET | [守护程序-CertificateCredential-最低](http://github.com/AzureADSamples/Daemon-CertificateCredential-DotNet) | 调用 web API 一个控制台应用程序。 客户端凭据是证书。


## Azure 广告图形 API 调用

这些代码示例显示如何构建应用程序调用 Azure 广告图形 API，用于读取和写入目录数据。

| 语言/平台 | 示例 | 说明
| ----------------- | ------ | -----------
| Java | [-Java 的 WebApp GraphAPI](http://github.com/AzureADSamples/WebApp-GraphAPI-Java) | 使用图形 API 访问 Azure 的广告目录数据 web 应用程序。
| PHP | [-PHP 的 WebApp GraphAPI](http://github.com/AzureADSamples/WebApp-GraphAPI-PHP) | 使用图形 API 访问 Azure 的广告目录数据 web 应用程序。
| C# /.NET | [WebApp-GraphAPI-最低](http://github.com/AzureADSamples/WebApp-GraphAPI-DotNet) | 使用图形 API 访问 Azure 的广告目录数据 web 应用程序。
| C# /.NET | [ConsoleApp-GraphAPI-最低](https://github.com/AzureADSamples/ConsoleApp-GraphAPI-DotNet) | 此控制台应用程序演示常见读取和写入调用图形 API，并演示如何执行用户许可证分配和更新用户的缩略图图片和链接。
| C# /.NET | [ConsoleApp GraphAPI DiffQuery 最低](https://github.com/AzureADSamples/ConsoleApp-GraphAPI-DiffQuery-DotNet) | 使用图形 API 中差异的查询在 Azure AD 租户获得定期更改用户对象一个控制台应用程序。
| C# /.NET | [WebApp GraphAPI DirectoryExtensions 最低](https://github.com/AzureADSamples/WebApp-GraphAPI-DirectoryExtensions-DotNet) | MVC 应用程序中使用图形 API 查询来生成简单的公司的组织结构图。
| PHP | [WebApp GraphAPI DirectoryExtensions PHP](https://github.com/AzureADSamples/WebApp-GraphAPI-DirectoryExtensions-PHP) | 调用图形 API 来注册扩展，然后读取、 更新和删除扩展属性中的值的 PHP 应用程序。


## 授权

这些代码示例显示如何使用 Azure 广告进行授权。

| 语言/平台 | 示例 | 说明
| ----------------- | ------ | -----------
| C# /.NET | [WebApp-GroupClaims-最低](https://github.com/AzureADSamples/WebApp-GroupClaims-DotNet) | 执行基于角色的访问控制 (RBAC) 与 Azure AD 集成在应用程序中使用 Azure Active Directory 组声明。
| C# /.NET | [WebApp-RoleClaims-最低](https://github.com/AzureADSamples/WebApp-RoleClaims-DotNet) | 执行基于角色的访问控制 (RBAC) 与 Azure AD 集成在应用程序中使用 Azure Active Directory 应用程序角色。


## 传统的演练

这些演练使用稍微比较老的技术，但仍可能会感兴趣。

| 语言/平台 | 示例 | 说明
| ----------------- | ------ | -----------
| C# /.NET | [在 Microsoft Azure AD 应用程序基于角色和基于 ACL 的授权](http://go.microsoft.com/fwlink/?LinkId=331694) | 在 Azure 广告与集成的应用程序执行基于角色的授权 (RBAC) 和基于 ACL 的授权。
| C# /.NET |  [AAL REST 服务为 Windows 应用商店应用程序的身份验证](http://go.microsoft.com/fwlink/?LinkId=330605) |  使用[Azure AD 身份验证库 (ADAL)](http://go.microsoft.com/fwlink/?LinkID=258232) (以前称为 AAL) 为 Windows 存储测试添加到 Windows 应用商店应用程序的用户身份验证功能。
| C# /.NET | [通过浏览器对话框的 AAD 的 ADAL-到 REST 服务的本机应用程序的身份验证](http://go.microsoft.com/fwlink/?LinkId=259814) |  使用[Azure AD 身份验证库 (ADAL)](http://go.microsoft.com/fwlink/?LinkID=258232)添加到 WPF 客户端用户身份验证功能。
| C# /.NET | [通过浏览器对话框的 ACS ADAL-到 REST 服务的本机应用程序的身份验证](http://code.msdn.microsoft.com/AAL-Native-App-to-REST-de57f2cc) |  使用[Azure AD 身份验证库 (ADAL)](http://go.microsoft.com/fwlink/?LinkID=258232)和[访问控制服务 2.0 (ACS)](http://msdn.microsoft.com/library/azure/hh147631.aspx)添加到 WPF 客户端用户身份验证功能。
| C# /.NET | [ADAL 的服务器到服务器的身份验证](http://go.microsoft.com/fwlink/?LinkId=259816) | 使用[Azure AD 身份验证库 (ADAL)](http://go.microsoft.com/fwlink/?LinkID=258232)来确保安全服务调用从服务器端进程 MVC4 Web API REST 服务。
| C# /.NET | [添加登录到 Web 应用程序使用 Azure 的广告](https://msdn.microsoft.com/library/azure/dn151790.aspx) | .NET 应用程序配置为 web 单一登录对 Azure 广告企业目录执行。
| C# /.NET | [开发 Web 的多租户应用程序使用 Azure 的广告](https://msdn.microsoft.com/library/azure/dn151789.aspx) | 使用 Azure 广告添加到单一登录和目录访问功能的一个.NET 应用程序跨多个组织的工作。
JAVA | [Azure 广告图形 API 的 Java 示例应用程序](http://go.microsoft.com/fwlink/?LinkId=263969) | 从 Azure AD 访问目录数据使用 Graph API。
PHP | [PHP 的 Azure 广告图形 API 的示例应用程序](http://code.msdn.microsoft.com/PHP-Sample-App-For-Windows-228c6ddb) | 从 Azure AD 访问目录数据使用 Graph API。
| C# /.NET | [Azure 广告图形 API 的示例应用程序](http://go.microsoft.com/fwlink/?LinkID=262648) | 从 Azure AD 访问目录数据使用 Graph API。
| C# /.NET | [Azure 广告图差异查询的示例应用程序](http://go.microsoft.com/fwlink/?LinkId=275398) | 使用图形 API 中差异的查询在 Azure AD 租户获得定期更改用户对象。
| C# /.NET | [用于集成 Azure 广告的多租户云应用程序的示例应用程序](http://go.microsoft.com/fwlink/?LinkId=275397) | 将多租户应用程序集成到 Azure 的广告。
| C# /.NET | [保护 Windows 应用商店应用程序和其他 Web 服务使用 Azure AD （预览）](https://msdn.microsoft.com/library/azure/dn169448.aspx) | 创建一个简单的 web API 资源和 Windows 应用商店客户端应用程序使用 Azure 的 AD 和[Azure AD 身份验证库 (ADAL)](http://go.microsoft.com/fwlink/?LinkID=258232)。
| C# /.NET| [使用图形 API 来查询 Azure 的广告](https://msdn.microsoft.com/library/azure/dn151791.aspx) | 配置 Microsoft.NET 应用程序以使用 Azure 广告图形 API 来访问 Azure AD 客户端目录中的数据。

## 请参见

##### 其他资源


[Azure 广告图形 API 帮助器库](http://code.msdn.microsoft.com/Windows-Azure-AD-Graph-API-a8c72e18)

[开发现代应用程序使用 OAuth 和 Active Directory 联合身份验证服务](http://msdn.microsoft.com/library/dn633593.aspx)

测试

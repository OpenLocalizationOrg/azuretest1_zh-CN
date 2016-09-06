---
ms.openlocfilehash: 30afd3c968b222c8b550994685e53ece06f6a142
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="要开始使用活动目录身份验证-发生了什么变化"
    description="描述 Visual Studio 中的 Azure Active Directory 项目出了什么问题"
    services="active-directory"
    documentationCenter=""
    authors="patshea123"
    manager="douge"
    editor="tglee"/>

<tags
    ms.service="active-directory"
    ms.workload="web"
    ms.tgt_pltfrm="vs-what-happened"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/22/2015"
    ms.author="patshea123"/>

# 我的项目发生了什么变化？

> [AZURE.SELECTOR]
> - [入门教程](vs-active-directory-dotnet-getting-started.md)
> - [发生了什么事](vs-active-directory-dotnet-what-happened.md)



##已添加的引用

###NuGet 程序包的引用

- `Microsoft.IdentityModel.Protocol.Extensions`
- `Microsoft.Owin`
- `Microsoft.Owin.Host.SystemWeb`
- `Microsoft.Owin.Security`
- `Microsoft.Owin.Security.Cookies`
- `Microsoft.Owin.Security.OpenIdConnect`
- `Owin`
- `System.IdentityModel.Tokens.Jwt`

###.NET 引用

- `Microsoft.IdentityModel.Protocol.Extensions`
- `Microsoft.Owin`
- `Microsoft.Owin.Host.SystemWeb`
- `Microsoft.Owin.Security`
- `Microsoft.Owin.Security.Cookies`
- `Microsoft.Owin.Security.OpenIdConnect`
- `Owin`
- `System.IdentityModel`
- `System.IdentityModel.Tokens.Jwt`
- `System.Runtime.Serialization`

##已添加代码

###将代码文件添加到项目

身份验证启动类，`App_Start/Startup.Auth.cs`已添加到您的项目包含启动 Azure AD 身份验证逻辑。 此外，控制器类，Controllers/AccountController.cs 已添加了包含`SignIn()`和`SignOut()`的方法。 最后，将分部视图`Views/Shared/_LoginPartial.cshtml`添加了包含登录/注销操作链接。

###启动代码添加到您的项目

如果您的项目中已经启动类，**配置**方法更新，以包括对`ConfigureAuth(app)`。 否则，启动类被添加到您的项目。

###您的 app.config 或 web.config 具有新配置值

添加了下面的配置项。
    <pre>
    `<appSettings>
        <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
        <add key="ida:AADInstance" value="https://login.microsoftonline.com/" />
        <add key="ida:Domain" value="The selected Azure AD Domain" />
        <add key="ida:TenantId" value="The Id of your selected Azure AD Tenant" />
        <add key="ida:PostLogoutRedirectUri" value="Your project start page" />
    </appSettings>` </pre>

###Azure 活动目录 (AD) 的应用程序创建
Azure 的广告应用程序向导中选择的目录中创建。

##如果我签*禁用单独的用户帐户身份验证*，什么其他更改我的项目？
NuGet 将包引用被移除，并且已删除的文件和备份。 这取决于您的项目的状态时，可能需要手动删除其他参考资料或文件，或者修改相应的代码。

###NuGet 将包引用删除 （对于那些存在）

- `Microsoft.AspNet.Identity.Core`
- `Microsoft.AspNet.Identity.EntityFramework`
- `Microsoft.AspNet.Identity.Owin`

###代码文件备份和删除 （对于那些存在）

以下每个文件的备份，并从项目中移除。 备份文件都位于备份文件夹内的项目目录的根目录。

- `App_Start\IdentityConfig.cs`
- `Controllers\ManageController.cs`
- `Models\IdentityModels.cs`
- `Models\ManageViewModels.cs`

###代码文件的备份 （对于那些存在）

以下每个文件备份之前被替换。 备份文件都位于备份文件夹内的项目目录的根目录。

- `Startup.cs`
- `App_Start\Startup.Auth.cs`
- `Controllers\AccountController.cs`
- `Views\Shared\_LoginPartial.cshtml`

##如果我签*读取目录数据*，哪些其他更改我的项目？

已添加更多参考。

###NuGet 程序包的其他参考

- `EntityFramework`
- `Microsoft.Azure.ActiveDirectory.GraphClient`
- `Microsoft.Data.Edm`
- `Microsoft.Data.OData`
- `Microsoft.Data.Services.Client`
- `Microsoft.IdentityModel.Clients.ActiveDirectory`
- `System.Spatial`

###其他.NET 参考

- `EntityFramework`
- `EntityFramework.SqlServer`
- `Microsoft.Azure.ActiveDirectory.GraphClient`
- `Microsoft.Data.Edm`
- `Microsoft.Data.OData`
- `Microsoft.Data.Services.Client`
- `Microsoft.IdentityModel.Clients.ActiveDirectory`
- `Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms`
- `System.Spatial`

###其他代码文件添加到项目

增加了两个文件来支持令牌缓存︰ `Models\ADALTokenCache.cs` ， `Models\ApplicationDbContext.cs`。  添加一个额外的控制器和视图来说明使用 Azure 图形 Api 的访问用户配置文件信息。  这些文件是`Controllers\UserProfileController.cs`， `Views\UserProfile\Index.cshtml`。

###其他启动代码添加到您的项目

在`startup.auth.cs`文件中，一个新`OpenIdConnectAuthenticationNotifications`对象被添加到`Notifications`的成员`OpenIdConnectAuthenticationOptions`。  这是启用接收 OAuth 代码和交换它的访问令牌。

###向 web.config 或 app.config 进行了其他更改

添加了以下的额外的配置项。
    <pre>
    `<appSettings>
        <add key="ida:ClientSecret" value="Your Azure AD App's new client secret" />
    </appSettings>` </pre>

已添加了下列配置节和连接字符串。
    <pre>
    `<configSections>
        <!-- For more information on Entity Framework configuration, visit http://go.microsoft.com/fwlink/?LinkID=237468 -->
        <section name="entityFramework" type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=6.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" requirePermission="false" />
    </configSections>
    <connectionStrings>
        <add name="DefaultConnection" connectionString="Data Source=(localdb)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\aspnet-[AppName + Generated Id].mdf;Initial Catalog=aspnet-[AppName + Generated Id];Integrated Security=True" providerName="System.Data.SqlClient" />
    </connectionStrings>
    <entityFramework>
        <defaultConnectionFactory type="System.Data.Entity.Infrastructure.LocalDbConnectionFactory, EntityFramework">
          <parameters>
            <parameter value="mssqllocaldb" />
          </parameters>
        </defaultConnectionFactory>
        <providers>
          <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
        </providers>
    </entityFramework>`</pre>


###Azure 活动目录应用程序已更新
Azure 活动目录应用程序已更新以包括*读取目录数据*权限和其他键创建它然后被用作*ida: ClientSecret*在`web.config`文件。

[了解更多有关 Azure Active Directory](http://azure.microsoft.com/services/active-directory/)

测试

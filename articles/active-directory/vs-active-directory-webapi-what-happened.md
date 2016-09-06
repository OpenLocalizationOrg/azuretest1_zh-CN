---
ms.openlocfilehash: 8ad277329f47018cecc735ea71c0e0f27a8e163c
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle=""
    description="描述更改的内容在 Visual Studio 项目中运行 Azure Active Directory 向导之后"
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
    ms.author="patshea"/>

# 我的项目发生了什么变化？

> [AZURE.SELECTOR]
> - [入门教程](vs-active-directory-webapi-getting-started.md)
> - [发生了什么事](vs-active-directory-webapi-what-happened.md)

##已添加的引用

###NuGet 程序包的引用

- `Microsoft.Owin`
- `Microsoft.Owin.Host.SystemWeb`
- `Microsoft.Owin.Security`
- `Microsoft.Owin.Security.ActiveDirectory`
- `Microsoft.Owin.Security.Jwt`
- `Microsoft.Owin.Security.OAuth`
- `Owin`
- `System.IdentityModel.Tokens.Jwt`

###.NET 引用

- `Microsoft.Owin`
- `Microsoft.Owin.Host.SystemWeb`
- `Microsoft.Owin.Security`
- `Microsoft.Owin.Security.ActiveDirectory`
- `Microsoft.Owin.Security.Jwt`
- `Microsoft.Owin.Security.OAuth`
- `Owin`
- `System.IdentityModel.Tokens.Jwt`

##代码更改

###将代码文件添加到项目

身份验证启动类， **App_Start/Startup.Auth.cs**已添加到您的项目包含启动 Azure AD 身份验证逻辑。

###启动代码添加到您的项目

如果您的项目中已经启动类，**配置**方法更新，以包括对`ConfigureAuth(app)`。 否则，启动类被添加到您的项目。


###您的 app.config 或 web.config 文件具有新配置值。

添加了下面的配置项。
```
    `<appSettings>
            <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
            <add key="ida:Tenant" value="Your selected Azure AD Tenant" />
            <add key="ida:Audience" value="The App ID Uri from the wizard" />
    </appSettings>` 
```

###Azure AD 应用程序创建

Azure 的广告应用程序向导中选择的目录中创建。

[了解更多有关 Azure Active Directory](http://azure.microsoft.com/services/active-directory/)

##如果我签*禁用单独的用户帐户身份验证*，什么其他更改我的项目？
NuGet 将包引用被移除，并且已删除的文件和备份。 这取决于您的项目的状态时，可能需要手动删除其他参考资料或文件，或者修改相应的代码。

###NuGet 将包引用删除 （对于那些存在）

- `Microsoft.AspNet.Identity.Core`
- `Microsoft.AspNet.Identity.EntityFramework`
- `Microsoft.AspNet.Identity.Owin`

###代码文件备份和删除 （对于那些存在）

以下每个文件的备份，并从项目中移除。 备份文件都位于备份文件夹内的项目目录的根目录。

- `App_Start\IdentityConfig.cs`
- `Controllers\AccountController.cs`
- `Controllers\ManageController.cs`
- `Models\IdentityModels.cs`
- `Providers\ApplicationOAuthProvider.cs`

###代码文件的备份 （对于那些存在）

以下每个文件备份之前被替换。 备份文件都位于备份文件夹内的项目目录的根目录。

- `Startup.cs`
- `App_Start\Startup.Auth.cs`

##如果我签*读取目录数据*，哪些其他更改我的项目？

###向 web.config 或 app.config 进行了其他更改

添加了以下的额外的配置项。

```
    `<appSettings>
        <add key="ida:Password" value="Your Azure AD App's new password" />
    </appSettings>` 
```

###Azure 活动目录应用程序已更新
Azure 活动目录应用程序已更新以包括*读取目录数据*权限和其他键创建它然后被用作*ida︰ 密码*在`web.config`文件。

[了解更多有关 Azure Active Directory](http://azure.microsoft.com/services/active-directory/)

测试

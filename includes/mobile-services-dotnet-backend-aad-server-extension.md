---
ms.openlocfilehash: e36cfd96055eef26cb68d5ca019415a23c46fb47
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
### （可选）配置.NET Azure Active Directory 的移动服务

>[AZURE.NOTE] 这些步骤是可选的因为它们只应用于 Azure Active Directory 的登录提供程序。

1. 安装[WindowsAzure.MobileServices.Backend.Security NuGet 程序包](https://www.nuget.org/packages/WindowsAzure.MobileServices.Backend.Security)。

2. 在 Visual Studio 中展开 App_Start，并打开 WebApiConfig.cs。 添加以下`using`顶部的语句︰

        using Microsoft.WindowsAzure.Mobile.Service.Security.Providers;

3. 在 WebApiConfig.cs 中，添加下面的代码`Register`方法时，紧随其后`options`实例化︰

        options.LoginProviders.Remove(typeof(AzureActiveDirectoryLoginProvider));
        options.LoginProviders.Add(typeof(AzureActiveDirectoryExtendedLoginProvider));

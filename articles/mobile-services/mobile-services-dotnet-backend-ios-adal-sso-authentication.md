---
ms.openlocfilehash: dee5b63446039df48ab5a84e954202e999daa10b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="验证您使用活动目录身份验证库单一登录 (iOS) 的应用程序 |Microsoft Azure"
    description="了解如何为单一登录 ADAL 与 iOS 应用程序中的身份验证用户。"
    documentationCenter="ios"
    authors="mattchenderson"
    manager="dwrede"
    editor=""
    services="mobile-services"/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="08/18/2015" 
    ms.author="mahender"/>

# 对您的应用程序使用 Active Directory 验证库单一登录进行身份验证

[AZURE.INCLUDE [mobile-services-selector-adal-sso](../../includes/mobile-services-selector-adal-sso.md)]

##概述

在本教程中，您将添加身份验证到快速入门项目使用活动目录身份验证库。

若要能够对用户进行身份验证，必须注册 Azure 活动目录 (AAD) 与您的应用程序。 这是两个步骤。 首先，必须注册您的移动服务，并公开它的权限。 第二，您必须注册您的 iOS 应用程序并授予其访问权限，这些权限


>[AZURE.NOTE] 本教程旨在帮助您更好地了解如何移动服务使您能够执行单一登录 Azure Active Directory 验证的 iOS 应用程序。 如果这是您首次使用移动服务体验，完成本教程[开始使用移动服务]。


##先决条件


本教程要求如下︰

* XCode 4.5 和 iOS 6.0 （或更高版本）
* [开始使用移动服务]或[数据入门]教程完成。
* Microsoft Azure 移动服务 SDK
* [对于 iOS 的活动目录身份验证库]

[AZURE.INCLUDE [mobile-services-dotnet-adal-register-client](../../includes/mobile-services-dotnet-adal-register-client.md)]

##配置为要求身份验证的移动服务

[AZURE.INCLUDE [mobile-services-restrict-permissions-dotnet-backend](../../includes/mobile-services-restrict-permissions-dotnet-backend.md)]

##将验证代码添加到客户端应用程序

1. 下载[的 iOS 的活动目录身份验证库]并将其包括在您的项目。 请务必从 ADAL 源添加序列图像板。

2. QSTodoListViewController，包括使用以下 ADAL:

        #import "ADALiOS/ADAuthenticationContext.h"

2. 然后添加下面的方法︰

        - (void) loginAndGetData
        {
            MSClient *client = self.todoService.client;
            if (client.currentUser != nil) {
                return;
            }

            NSString *authority = @"<INSERT-AUTHORITY-HERE>";
            NSString *resourceURI = @"<INSERT-RESOURCE-URI-HERE>";
            NSString *clientID = @"<INSERT-CLIENT-ID-HERE>";
            NSString *redirectURI = @"<INSERT-REDIRECT-URI-HERE>";

            ADAuthenticationError *error;
            ADAuthenticationContext *authContext = [ADAuthenticationContext authenticationContextWithAuthority:authority error:&error];
            NSURL *redirectUri = [[NSURL alloc]initWithString:redirectURI];

            [authContext acquireTokenWithResource:resourceURI clientId:clientID redirectUri:redirectUri completionBlock:^(ADAuthenticationResult *result) {
                if (result.tokenCacheStoreItem == nil)
                {
                    return;
                }
                else
                {
                    NSDictionary *payload = @{
                        @"access_token" : result.tokenCacheStoreItem.accessToken
                    };
                    [client loginWithProvider:@"windowsazureactivedirectory" token:payload completion:^(MSUser *user, NSError *error) {
                        [self refresh];
                    }];
                }
            }];
        }


6. 在代码中的`loginAndGetData`上面，方法与组织资源调配您的应用程序的名称替换**插入的主管机构-这里**，格式应为 https://login.windows.net/tenant-name.onmicrosoft.com。 此值可以复制从[Azure 管理门户]中将 Azure Active Directory 中的域选项卡中。

7. 在代码中的`loginAndGetData`上面的方法替换为您的移动服务**的应用程序 ID URI** **插入资源 URI 这里**。 如果您遵循[如何注册 Azure Active Directory 与]主题您的应用程序 ID URI 应类似于 https://todolist.azure-mobile.net/login/aad。

8. 在代码中的`loginAndGetData`上面，方法从本机客户端应用程序中复制的客户机 ID 替换**插入客户端 ID 这里**。

9. 在代码中的`loginAndGetData`上面的方法替换**插入重定向 URI 这里**/ 登录/完成您的移动服务的终结点。 这应该是类似于 https://todolist.azure-mobile.net/login/done。


3. 在 QSTodoListViewController 中，修改`ViewDidLoad`通过替换`[self refresh]`如下︰

        [self loginAndGetData];

##测试客户端使用的身份验证

1. 从产品菜单上，单击运行以启动该应用程序
2. 针对将 Azure Active Directory，您将收到登录提示。  
3. 该应用程序进行身份验证并返回 todo 项。

   ![](./media/mobile-services-dotnet-backend-ios-adal-sso-authentication/mobile-services-app-run.png)



<!-- URLs. -->
[有关数据入门]: mobile-services-ios-get-started-data.md
[开始使用移动服务]: mobile-services-dotnet-backend-ios-get-started.md
[如何在 Azure 的 Active Directory 中注册]: mobile-services-how-to-register-active-directory-authentication.md
[Azure 的管理门户]: https://manage.windowsazure.com/
[对于 iOS 的活动目录身份验证库]: https://github.com/MSOpenTech/azure-activedirectory-library-for-ios

测试

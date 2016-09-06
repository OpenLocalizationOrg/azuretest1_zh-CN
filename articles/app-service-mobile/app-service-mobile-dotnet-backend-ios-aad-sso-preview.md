---
ms.openlocfilehash: ed4d942bf7084a302334ad0e014cec35177a1c1d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
        pageTitle="IOS 应用程序使用 Azure 活动目录单一登录的用户进行身份验证"
        description="了解如何登录到活动目录身份验证库的 iOS 应用程序的用户。"
        documentationCenter="Mobile"
        authors="mattchenderson"
        services="app-service\mobile"
        manager="dwrede" />

<tags ms.service="app-service-mobile"
ms.workload="mobile"
ms.tgt_pltfrm="mobile-ios"
ms.devlang="objective-c"
ms.topic="article"
ms.date="05/19/2015"
ms.author="mahender"
ms.test="test-value1"/>

# Azure Active Directory 单一登录添加到您的 iOS 应用程序

[AZURE.INCLUDE [app-service-mobile-selector-aad-sso](../../includes/app-service-mobile-selector-aad-sso.md)]

[AZURE.INCLUDE [app-service-mobile-note-mobile-services-preview](../../includes/app-service-mobile-note-mobile-services-preview.md)]

在本教程中，您添加到快速启动项目使用活动目录身份验证库 (ADAL) 身份验证。 根据[添加到您的应用程序的身份验证]指南中的介绍，还可以只是通过移动应用程序 SDK，启用使用较少的配置身份验证。 在 ADAL 上使用本主题为最终用户提供更多集成的身份验证体验，ADAL 提供了更丰富的功能，用于访问其他 AAD 受保护的资源。

若要能够使用 ADAL 的用户进行身份验证，您必须注册 Azure 活动目录 (AAD) 租户应用程序。 这是两个步骤。 首先，必须将应用程序服务注册，并公开它的权限。 第二，您必须注册您的 iOS 应用程序并授予其访问权限，这些权限。

本教程要求如下︰

* XCode 4.5 和 iOS 6.0 （或更高版本）
* [移动应用程序入门]教程完成。
* Microsoft Azure 移动服务 SDK
* [对于 iOS 的活动目录身份验证库]

##<a name="review"></a>查看服务器项目配置 （可选）

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-enable-auth-preview](../../includes/app-service-mobile-dotnet-backend-enable-auth-preview.md)]

## <a name="register-application"></a>Azure Active Directory 中注册应用程序

[AZURE.INCLUDE [app-service-mobile-adal-register-app](../../includes/app-service-mobile-adal-register-app.md)]

## <a name="require-authentication"></a>应用程序配置为要求身份验证

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

## <a name="add-adal"></a>添加到活动目录身份验证库的引用

1. 下载[的 iOS 的活动目录身份验证库]。

2. Xcode 导航区中选择您的项目，单击**文件**并选择**将文件添加到...**。 导航到您下载库并选择**ADALiOS.xcodeproj**。

3. 再次选择您的项目并打开**生成设置**选项卡。 导航到**链接**部分中，添加`-ObjC`于**其他链接器标志**。

4. 选择**生成阶段**选项卡。 在**目标依赖项**下添加`ADALiOS`。

5. 在**链接二进制与库**，添加`libADALiOS.a`。

您现在能够引用项目中的活动目录身份验证库。

## <a name="add-authentication-code"></a>将验证代码添加到客户端应用程序

2. QSTodoListViewController，包括使用以下 ADAL:

        #import "ADALiOS/ADAuthenticationContext.h"

3. 然后添加下面的方法︰

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

4. 在代码中的`loginAndGetData`上面，方法与组织资源调配您的应用程序的名称替换**插入的主管机构-这里**，格式应为 https://login.windows.net/tenant-name.onmicrosoft.com。 此值可以复制从[Azure 管理门户]中将 Azure Active Directory 中的域选项卡中。

5. 在代码中的`loginAndGetData`上面的方法为您的移动应用程序使用**应用程序 ID URI**替换**插入资源 URI 这里**。 如果您遵循[如何配置您的移动应用程序使用 Azure Active Directory]主题您的应用程序 ID URI 应类似于 https://contosogateway.azurewebsites.net/login/aad。

6. 在代码中的`loginAndGetData`上面，方法从本机客户端应用程序中复制的客户机 ID 替换**插入客户端 ID 这里**。

7. 在代码中的`loginAndGetData`上面的方法替换**插入重定向 URI 这里**/ 登录/完成您的应用程序的服务网关的终结点。 这应该是类似于 https://contosogateway.azurewebsites.net/login/done。

8. 在 QSTodoListViewController 中，修改`viewDidLoad`通过替换`[self refresh]`如下︰

        [self loginAndGetData];

## <a name="test-client"></a>测试客户端使用的身份验证

1. 从产品菜单上，单击运行以启动该应用程序
2. 针对将 Azure Active Directory，您将收到登录提示。  
3. 该应用程序进行身份验证并返回 todo 项。

<!-- URLs. -->
[如何使用 Azure Active Directory 配置您的移动应用程序]: app-service-mobile-how-to-configure-active-directory-authentication-preview.md
[Azure 的管理门户]: https://manage.windowsazure.com/
[对于 iOS 的活动目录身份验证库]: https://github.com/MSOpenTech/azure-activedirectory-library-for-ios
 [开始使用移动应用程序]: app-service-mobile-dotnet-backend-ios-get-started-preview.md
 [添加到您的应用程序的身份验证]: app-service-mobile-dotnet-backend-ios-get-started-users-preview.md

测试

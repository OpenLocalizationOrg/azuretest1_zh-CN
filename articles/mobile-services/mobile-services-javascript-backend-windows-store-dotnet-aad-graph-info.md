---
ms.openlocfilehash: af0edf200c0791f077cd1f88a3b4598109f9a38e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="访问 Azure Active Directory 图形信息 （Windows 应用商店） |Microsoft Azure" 
    description="了解如何访问 Windows 应用商店应用程序中使用图形 API 的 Azure Active Directory 信息。" 
    documentationCenter="windows" 
    authors="wesmc7777" 
    manager="dwrede" 
    editor="" 
    services="mobile-services"/>

<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="multiple" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="06/18/2015" 
    ms.author="wesmc"/>

# 访问 Azure Active Directory 图表信息


[AZURE.INCLUDE [mobile-services-selector-aad-graph](../../includes/mobile-services-selector-aad-graph.md)]
##概述

像其他标识提供程序提供移动服务，Azure 活动目录 (AAD) 提供程序还支持一个丰富的图形 API，可以用于以编程方式访问该目录。 在本教程中，您更新任务列表应用程序进行个性化设置返回附加的用户信息从使用[REST API，图表]目录中检索通过身份验证的用户的应用程序体验。

Azure 广告图形 API 的详细信息，请参阅[Azure 活动目录图表团队博客]。 

>[AZURE.NOTE] 本教程的目的是扩展身份验证使用 Azure Active Directory 的知识。 预计完成使用 Azure Active Directory 身份验证提供程序[将身份验证添加到您的应用程序]教程。 本教程将继续更新 TodoItem 应用程序[添加到您的应用程序的身份验证]本教程中使用。



##先决条件 

在开始本教程之前，您必须已经完成这些移动服务指南︰

+ [添加到您的应用程序的身份验证]<br/>应做事项列表示例应用程序中添加登录要求。

+ [自定义 API 教程]<br/>演示如何调用一个自定义的 API。 



##在 AAD 中生成的应用程序注册为访问键


期间[将添加到您的应用程序的身份验证]教程中，您创建集成的应用程序注册完成[注册使用 Azure 活动目录登录]步骤。 在本节中，您生成密钥用于读取目录信息的集成应用程序的客户端 id。 

[AZURE.INCLUDE [mobile-services-generate-aad-app-registration-access-key](../../includes/mobile-services-generate-aad-app-registration-access-key.md)]



##创建自定义的 GetUserInfo 的 API

在本节中，您将创建将使用[REST API，图]从 AAD 检索有关用户的其他信息的 GetUserInfo 自定义 API。

如果您从未使用过自定义的 Api 的移动服务，则应考虑完成此部分之前查看[自定义 API 教程]。

1. 在[Azure 管理门户]中，创建新的移动服务 GetUserInfo 自定义的 API，**只经过身份验证的**用户设置权限的 get 方法。

    ![][0]

2. 打开脚本编辑器的新的 GetUserInfo API 和以下变量到顶部的脚本。

        var appSettings = require('mobileservice-config').appSettings;
        var tenant_domain = appSettings.AAD_TENANT_DOMAIN;
        var clientID = appSettings.AAD_CLIENT_ID;
        var key = appSettings.AAD_CLIENT_KEY;



3. 添加的以下定义`getAADToken`函数。

        function getAADToken(callback) {
            var req = require("request");
            var options = {
                url: "https://login.windows.net/" + tenant_domain + "/oauth2/token?api-version=1.0",
                method: 'POST',
                form: {
                    grant_type: "client_credentials",
                    resource: "https://graph.windows.net",
                    client_id: clientID,
                    client_secret: key
                }
            };
            req(options, function (err, resp, body) {
                if (err || resp.statusCode !== 200) callback(err, null);
                else callback(null, JSON.parse(body).access_token);
            });
        }

    此函数在给定*tenant_domain*、 集成应用程序*的客户端 id*和应用程序*键*，提供用于读取目录信息图形访问令牌。

4. 添加以下`getUser`函数使用 Graph API 返回用户的信息。

        function getUser(access_token, objectId, callback) {
            var req = require("request");
            var options = {
                url: "https://graph.windows.net/" + tenant_domain + "/users/" + objectId + "?api-version=1.0",
                method: 'GET',
                headers: {
                    "Authorization": "Bearer " + access_token
                }
            };
            req(options, function (err, resp, body) {
                if (err || resp.statusCode !== 200) callback(err, null);
                else callback(null, JSON.parse(body));
            });
        };

    此函数是[获取用户]终结点的图形 REST API，精简包装。 它使用关系图的访问令牌来获取用户信息，使用用户的对象 id。

5. 更新导出`get`，如下所示方法返回用户使用其他函数的信息。

        exports.get = function (request, response) {
            if (request.user.level == 'anonymous') {
                response.send(statusCodes.UNAUTHORIZED, null);
                return;
            }
            var errorHandler = function (err) {
                console.log(err);
                response.send(statusCodes.INTERNAL_SERVER_ERROR, err);
            };
            request.user.getIdentities({
                success: function (identities) {
                    var objectId = identities.aad.oid;
                    getAADToken(function (err, access_token) {
                        if (err) errorHandler(err);
                        else getUser(access_token, objectId, function (err, user_info) {
                            if (err) errorHandler(err);
                            else {
                                console.log('GetUserInfo: ' + JSON.stringify(user_info, null, 4));
                                response.send(statusCodes.OK, user_info);
                              }
                            });
                    });
                },
                error: errorHandler
            });
        };


##更新应用程序使用 GetUserInfo


在本节中，您将更新`AuthenticateAsync`您实现了在 [开始使用身份验证] 教程中调用自定义的 API，并从 AAD 返回有关用户的其他信息的方法。 

[AZURE.INCLUDE [mobile-services-aad-graph-info-update-app](../../includes/mobile-services-aad-graph-info-update-app.md)]


 


##测试应用程序

[AZURE.INCLUDE [mobile-services-aad-graph-info-test-app](../../includes/mobile-services-aad-graph-info-test-app.md)]




##下一步行动

在下一步的教程中，[基于角色的访问控制与中移动服务 AAD]，您将使用基于角色的访问控制 Azure 活动目录 (AAD) 的才允许访问检查组成员资格。 



<!-- Images -->
[0]: ./media/mobile-services-javascript-backend-windows-store-dotnet-aad-graph-info/create-getuserinfo.png


<!-- URLs. -->
[添加到您的应用程序的身份验证]: ../mobile-services-windows-store-dotnet-get-started-users.md
[如何在 Azure 的 Active Directory 中注册]: mobile-services-how-to-register-active-directory-authentication.md
[Azure 的管理门户]: https://manage.windowsazure.com/
[自定义 API 教程]: mobile-services-windows-store-dotnet-call-custom-api.md
[存储服务器脚本]: mobile-services-store-scripts-source-control.md
[注册使用 Azure 活动目录登录]: mobile-services-how-to-register-active-directory-authentication.md
[Graph API]: http://msdn.microsoft.com/library/azure/hh974478.aspx
[REST API，关系图]: http://msdn.microsoft.com/library/azure/hh974478.aspx
[获取用户]: http://msdn.microsoft.com/library/azure/dn151678.aspx
[基于角色的访问控制与中移动服务 AAD]: mobile-services-javascript-backend-windows-store-dotnet-aad-rbac.md
[Azure 的 Active Directory 图团队博客]: http://go.microsoft.com/fwlink/?LinkId=510536 
测试t

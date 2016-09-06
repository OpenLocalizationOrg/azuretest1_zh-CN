---
ms.openlocfilehash: a59036f0a9d005f1bc10eca39a3b9c5f9fc352ae
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="经过验证的通用 Windows 应用程序用户发送推式通知。" 
    description="了解如何从 Azure 移动服务向通用 Windows C# 应用程序的特定用户发送推式通知。" 
    services="mobile-services,notification-hubs" 
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
    ms.date="07/22/2015" 
    ms.author="glenga"/>

# 对经过身份验证的用户发送推式通知

[AZURE.INCLUDE [mobile-services-selector-push-users](../../includes/mobile-services-selector-push-users.md)]

##概述
本主题演示如何向任何已注册的设备上通过验证的用户发送推式通知。 与前面的[外推式通知到您的应用程序]教程，本教程更改您的移动服务，需要对用户进行身份验证，客户端可以使用推式通知的通知中心注册之前。 注册也修改添加标记基于所分配的用户 id。 最后，更新服务器脚本，以将通知发送到所有注册到身份验证的用户而不是只。

本教程将引导您完成以下过程︰

1. [更新服务登记要求身份验证]
2. [更新应用程序之前注册登录]
3. [测试应用程序]
 
本教程支持 Windows 应用商店和 Windows Phone 存储的应用程序。

##先决条件 

在开始本教程之前，您必须已经完成这些移动服务指南︰

+ [添加到您的应用程序的身份验证]<br/>应做事项列表示例应用程序中添加登录要求。

+ [将推式通知添加到您的应用程序]<br/>通过使用通知集线器配置任务列表示例应用程序用于推式通知。 

完成这两个教程后，可以防止注册为从您的移动服务的推式通知进行未经身份验证的用户。

##<a name="register"></a>更新服务要求身份验证注册

[AZURE.INCLUDE [mobile-services-javascript-backend-push-notifications-app-users](../../includes/mobile-services-javascript-backend-push-notifications-app-users.md)] 

&nbsp;&nbsp;5.用下面的代码替换该插入函数，然后单击**保存**:

    function insert(item, user, request) {
    // Define a payload for the Windows Store toast notification.
    var payload = '<?xml version="1.0" encoding="utf-8"?><toast><visual>' +    
    '<binding template="ToastText01"><text id="1">' +
    item.text + '</text></binding></visual></toast>';

    // Get the ID of the logged-in user.
    var userId = user.userId;       

    request.execute({
        success: function() {
            // If the insert succeeds, send a notification to all devices 
            // registered to the logged-in user as a tag.
                push.wns.send(userId, payload, 'wns/toast', {
                success: function(pushResponse) {
                    console.log("Sent push:", pushResponse);
                    request.respond();
                    },              
                    error: function (pushResponse) {
                            console.log("Error Sending push:", pushResponse);
                        request.respond(500, { error: pushResponse });
                        }
                    });
                }
            });
    }

&nbsp;&nbsp;此插入脚本使用用户 ID 标记要发送到所有已登录的用户创建的 Windows 应用商店应用程序登记的推式通知 （带有插入的项的文本）。

##<a name="update-app"></a>更新应用程序之前注册登录

[AZURE.INCLUDE [mobile-services-windows-store-dotnet-push-notifications-app-users](../../includes/mobile-services-windows-store-dotnet-push-notifications-app-users.md)] 

##<a name="test"></a>测试应用程序

[AZURE.INCLUDE [mobile-services-windows-test-push-users](../../includes/mobile-services-windows-test-push-users.md)] 

<!-- Anchors. -->
[更新服务登记要求身份验证]: #register
[更新应用程序之前注册登录]: #update-app
[测试应用程序]: #test
[下一步行动]:#next-steps


<!-- URLs. -->
[添加到您的应用程序的身份验证]: ../mobile-services-windows-store-dotnet-get-started-users.md
[将推式通知添加到您的应用程序]: ../mobile-services-javascript-backend-windows-store-dotnet-get-started-push.md

[Azure 的管理门户]: https://manage.windowsazure.com/
 
测试

---
ms.openlocfilehash: a7a74ead20264473fc3fb49c802e959c5346b1ed
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="向经过身份验证的用户 (通用 Windows 8.1) 发送推式通知 |Microsoft Azure" 
    description="了解如何使用 Azure 移动服务向运行 Windows 8.1 通用应用程序特定身份验证的用户发送推式通知。" 
    services="mobile-services,notification-hubs" 
    documentationCenter="windows" 
    authors="ggailey777" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="07/01/2015" 
    ms.author="glenga"/>

# 对经过身份验证的用户发送推式通知

[AZURE.INCLUDE [mobile-services-selector-push-users](../../includes/mobile-services-selector-push-users.md)]

##概述

本主题演示如何向任何已注册的设备上经过身份验证的用户发送推式通知。 与前面[推式通知][开始使用推式通知]的教程，本教程中更改您的移动服务，需要对用户进行身份验证，客户端可以使用推式通知的通知中心注册之前。 注册也修改添加标记基于所分配的用户 id。 最后，更新的服务器代码，以将通知发送到所有注册到身份验证的用户而不是只。
 
本教程支持 Windows 应用商店和 Windows Phone 存储的应用程序。

##先决条件 

在开始本教程之前，您必须已经完成这些移动服务指南︰

+ [开始使用身份验证]<br/>应做事项列表示例应用程序中添加登录要求。

+ [开始使用推式通知]<br/>通过使用通知集线器配置任务列表示例应用程序用于推式通知。 

完成这两个教程后，可以防止注册为从您的移动服务的推式通知进行未经身份验证的用户。

##<a name="register"></a>更新服务要求身份验证注册

[AZURE.INCLUDE [mobile-services-dotnet-backend-push-notifications-app-users](../../includes/mobile-services-dotnet-backend-push-notifications-app-users.md)] 

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
[开始使用身份验证]: ../mobile-services-dotnet-backend-windows-store-dotnet-get-started-users.md
[开始使用推式通知]: ../mobile-services-dotnet-backend-windows-store-dotnet-get-started-push.md

[Azure 的管理门户]: https://manage.windowsazure.com/

 
测试

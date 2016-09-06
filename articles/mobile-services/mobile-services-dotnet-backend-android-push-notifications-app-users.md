---
ms.openlocfilehash: 1d2969801545a51e72b9587ac62b416bd04f43d5
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="对经过身份验证的用户发送推式通知"
    description="了解如何向特定发送推式通知"
    services="mobile-services,notification-hubs"
    documentationCenter="android"
    authors="wesmc7777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="notification-hubs"
    ms.devlang="java"
    ms.topic="article"
    ms.date="06/16/2015" 
    ms.author="wesmc"/>

# 对经过身份验证的用户发送推式通知

[AZURE.INCLUDE [mobile-services-selector-push-users](../../includes/mobile-services-selector-push-users.md)]

##概述

本主题演示如何向任何已注册的设备上通过验证的用户发送推式通知。 与前面[推式通知][开始使用推式通知]的教程，本教程中更改您的移动服务，需要对用户进行身份验证，客户端可以使用推式通知的通知中心注册之前。 注册也修改添加标记基于所分配的用户 id。 最后，更新的服务器代码，以将通知发送到所有注册到身份验证的用户而不是只。


本教程将支持 Android 应用程序。

##先决条件

在开始本教程之前，您必须已经完成这些移动服务指南︰

+ [添加到您的移动服务应用程序的身份验证]<br/>应做事项列表示例应用程序中添加登录要求。

+ [开始使用推式通知]<br/>通过使用通知集线器配置任务列表示例应用程序用于推式通知。

完成这两个教程后，可以防止注册为从您的移动服务的推式通知进行未经身份验证的用户。

##更新服务要求身份验证注册

[AZURE.INCLUDE [mobile-services-dotnet-backend-push-notifications-app-users](../../includes/mobile-services-dotnet-backend-push-notifications-app-users.md)]

##更新应用程序之前注册登录

[AZURE.INCLUDE [mobile-services-android-push-notifications-app-users](../../includes/mobile-services-android-push-notifications-app-users.md)]

##测试应用程序

[AZURE.INCLUDE [mobile-services-android-test-push-users](../../includes/mobile-services-android-test-push-users.md)]


<!---##Next steps

In the next tutorial, [Service-side authorization of Mobile Services users](mobile-services-javascript-backend-service-side-authorization.md), you will take the user ID value provided by Mobile Services based on an authenticated user and use it to filter the data returned by Mobile Services. Learn more about how to use Mobile Services with .NET in [Mobile Services .NET How-to Conceptual Reference]-->


<!-- URLs. -->
[添加到您的移动服务应用程序的身份验证]: mobile-services-dotnet-backend-android-get-started-users.md
[开始使用推式通知]: mobile-services-dotnet-backend-android-get-started-push.md

[Azure 的管理门户]: https://manage.windowsazure.com/
[移动服务.NET 帮助概念参考]: /develop/mobile/how-to-guides/work-with-net-client-library

测试

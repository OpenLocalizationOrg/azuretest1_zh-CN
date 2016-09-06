---
ms.openlocfilehash: 53bc60b24cad877ff9540447c9b61c3069dd17d7
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="开始使用移动服务用于 Xamarin Android 的应用程序 |Microsoft Azure"
    description="了解如何使用 Azure 移动服务和通知集线器于 Xamarin Android 应用程序发送推式通知"
    services="mobile-services"
    documentationCenter="xamarin"
    authors="ggailey777"
    manager="dwrede"
    editor="mollybos"/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/18/2015" 
    ms.author="glenga"/>

# 向您的移动服务应用程序添加推式通知

[AZURE.INCLUDE [mobile-services-selector-get-started-push](../../includes/mobile-services-selector-get-started-push.md)]

本主题演示如何使用 Azure 移动服务向 Xamarin.Android 应用程序发送推式通知。 在本教程中，您将添加使用 Google 云消息 (GCM)[开始使用移动服务]项目服务的推式通知。 完成后，您的移动服务将发送推式通知每当插入记录。

本教程将引导您完成以下基本步骤启用推式通知︰

1. [启用消息的 Google 云](#register)
2. [配置移动服务](#configure)
3. [配置项目以获得推式通知](#configure-app)
4. [将推式通知代码添加到您的应用程序](#add-push)
5. [插入数据接收通知](#test)

本教程要求如下︰

+ 活动的 Google 帐户。
+ [Google 云消息传递客户端组件]。 在本教程中，您将添加该组件。

您应该已经安装的[Xamarin.Android]和[Azure 移动服务][Azure 移动服务组件]组件安装在将项目从[开始使用移动服务]完成。

##<a id="register"></a>启用消息的 Google 云

[AZURE.INCLUDE [Enable GCM](../../includes/mobile-services-enable-google-cloud-messaging.md)]

##<a id="configure"></a>配置移动服务发送推送请求

[AZURE.INCLUDE [mobile-services-android-configure-push](../../includes/mobile-services-android-configure-push.md)]

##<a id="update-server"></a>更新移动服务发送推式通知

[AZURE.INCLUDE [mobile-services-dotnet-backend-android-push-update-service](../../includes/mobile-services-dotnet-backend-android-push-update-service.md)]

##<a id="configure-app"></a>配置推式通知的现有项目

[AZURE.INCLUDE [mobile-services-xamarin-android-push-configure-project](../../includes/mobile-services-xamarin-android-push-configure-project.md)]

##<a id="add-push"></a>将推式通知代码添加到您的应用程序

[AZURE.INCLUDE [mobile-services-xamarin-android-push-add-to-app](../../includes/mobile-services-xamarin-android-push-add-to-app.md)]

##<a name="test-app"></a>测试针对手机信息发布服务应用程序

通过直接附加 Android 使用 USB 电缆，电话或通过在仿真程序中使用的虚拟设备，可以测试该应用程序。

###<a id="local-testing"></a> 启用推式通知进行本地测试

[AZURE.INCLUDE [mobile-services-dotnet-backend-configure-local-push](../../includes/mobile-services-dotnet-backend-configure-local-push.md)]

[AZURE.INCLUDE [mobile-services-android-push-notifications-test](../../includes/mobile-services-android-push-notifications-test.md)]

<!-- URLs. -->
[开始使用移动服务]: mobile-services-dotnet-backend-xamarin-android-get-started.md


[Google 云消息传递客户端组件]: http://components.xamarin.com/view/GCMClient/
[Xamarin.Android]: http://xamarin.com/download/
[Azure 的移动服务组件]: http://components.xamarin.com/view/azure-mobile-services/

测试

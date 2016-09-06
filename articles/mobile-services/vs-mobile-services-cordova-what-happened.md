---
ms.openlocfilehash: c3bf74f4351bc82ec98b427b30174c8f9c70d810
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="" 
    description="介绍 Cordova 在 Azure 手机信息服务项目出了什么问题" 
    services="mobile-services" 
    documentationCenter="" 
    authors="patshea123" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="NA" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="08/12/2015" 
    ms.author="patshea"/>

# 我的项目发生了什么变化？

> [AZURE.SELECTOR]
> - [入门教程](vs-mobile-services-cordova-getting-started.md)
> - [发生了什么事](vs-mobile-services-cordova-what-happened.md)

##添加引用

已启用所有多设备混合应用程序中包含的 Azure 移动服务客户端插件。
  
##连接字符串值的移动服务

在下， `services\mobileServices\settings`， **MobileServiceClient**具有一个新的 JavaScript (.js) 文件生成包含选定的移动服务的应用程序的 URL 和应用程序键。 该文件包含初始化的移动服务客户端对象，类似于下面的代码。

    var mobileServiceClient;
    document.addEventListener("deviceready", function() {
            mobileServiceClient = new WindowsAzure.MobileServiceClient(
            "<your mobile service name>.azure-mobile.net",
            "<insert your key>"
        );

[了解有关移动服务的详细信息](http://azure.microsoft.com/documentation/services/mobile-services/) 

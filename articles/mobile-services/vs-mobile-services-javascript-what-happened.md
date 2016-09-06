---
ms.openlocfilehash: 321279f6b8c2acc5103a8f1fb69dc634c667f8a8
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="" 
    description="描述 Visual Studio 中的 Azure 移动服务项目出了什么问题" 
    services="mobile-services" 
    documentationCenter="" 
    authors="patshea123" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="NA" 
    ms.devlang="JavaScript" 
    ms.topic="article" 
    ms.date="07/02/2015" 
    ms.author="patshea"/>

# 我的项目发生了什么变化？

> [AZURE.SELECTOR]
> - [入门教程](vs-mobile-services-javascript-getting-started.md)
> - [发生了什么事](vs-mobile-services-javascript-what-happened.md)

###我的项目发生了什么变化？

#####NuGet 程序包添加

安装**WindowsAzure.MobileServices.WinJS** NuGet 程序包，包括 Azure 移动服务库中的`js\MobileServices.js`文件。
  
#####连接字符串值的移动服务 

在`services\mobileServices\settings`文件夹中，与**MobileServiceClient**新的 JavaScript (.js) 文件生成包含选定的移动服务的应用程序的 URL 和应用程序键。  


#####添加到 default.html 引用

引用到`MobileServices.js`，设置文件添加到`default.html`。  


#####添加连接的服务文件

在服务文件夹中添加连接的服务配置文件。



 

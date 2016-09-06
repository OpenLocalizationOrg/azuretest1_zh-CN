---
ms.openlocfilehash: 6a95157def692b7fe62c95001e235829633a7fa9
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="" 
    description="描述 Visual Studio 中的 Azure 移动服务.NET 项目中发生的事情" 
    services="mobile-services" 
    documentationCenter="" 
    authors="patshea123" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="NA" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="07/02/2015" 
    ms.author="patshea"/>

# 我的项目发生了什么变化？

> [AZURE.SELECTOR]
> - [入门教程](vs-mobile-services-dotnet-getting-started.md)
> - [发生了什么事](vs-mobile-services-dotnet-what-happened.md)

###我的项目发生了什么变化？

#####添加引用

Azure 移动服务 NuGet 程序包添加到您的项目。 因此，以下.NET 引用已添加到项目中︰

- `Microsoft.WindowsAzure.Mobile`
- `Microsoft.WindowsAzure.Mobile.Ext`
- `Newtonsoft.Json`
- `System.Net.Http.Extensions`
- `System.Net.Http.Primitives` 

#####连接字符串值的移动服务

在 App.xaml.cs 文件中，选定的移动服务的应用程序 URL 与应用程序键创建一个**MobileServiceClient**对象。 

####添加的移动服务项目

如果在连接服务提供商创建了.NET 的移动服务，然后创建移动服务项目的值，并将添加到解决方案中。


[了解有关移动服务的详细信息](http://azure.microsoft.com/documentation/services/mobile-services/) 

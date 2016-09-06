---
ms.openlocfilehash: 4b781e4cebf81a11047a6c47e1ca2cb7dba2e17d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="从 Windows 应用商店的客户端的移动服务调用一个自定义的 API"
    description="了解如何定义一个自定义的 API，然后从 Windows 应用商店应用程序使用 Azure 移动服务调用它。"
    services="mobile-services"
    documentationCenter="windows"
    authors="ggailey777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/16/2015" 
    ms.author="glenga"/>

# 从客户端调用一个自定义的 API

[AZURE.INCLUDE [mobile-services-selector-call-custom-api](../../includes/mobile-services-selector-call-custom-api.md)]

本主题演示如何从 Windows 应用商店应用程序调用一个自定义的 API。 自定义的 API 使您能够定义自定义终结点公开服务器功能的不映射到插入、 更新、 删除或读取操作。 通过使用自定义的 API，您可以更好地控制消息，包括读取和设置 HTTP 消息头定义非 JSON 消息正文格式。

本主题中创建的自定义 API 使您能够发送一个 POST 请求，在已完成的标志设置为`true`的表中的所有 todo 项。 如果没有此自定义的 API，客户端必须发送单个请求来更新表中每个 todo 项标志。

本教程基于移动服务快速入门。 在开始本教程之前，您必须首先完成[移动服务入门]或[添加到现有应用程序的移动服务]。

## <a name="define-custom-api"></a>定义自定义的 API

[AZURE.INCLUDE [mobile-services-create-custom-api](../../includes/mobile-services-create-custom-api.md)]

[AZURE.INCLUDE [mobile-services-windows-store-dotnet-call-custom-api](../../includes/mobile-services-windows-store-dotnet-call-custom-api.md)]


## 下一步行动

本主题介绍了如何使用**InvokeApiAsync**方法来调用您的 Windows 应用商店应用程序从一个非常简单的自定义 API。 若要了解有关使用**InvokeApiAsync**方法的详细信息，请参阅发布[自定义 API 在 Azure 移动服务](http://blogs.msdn.com/b/carlosfigueira/archive/2013/06/19/custom-api-in-azure-mobile-services-client-sdks.aspx)。  

另外，还要考虑找出更多的以下移动服务主题︰

* [移动服务服务器脚本引用]
  <br/>了解有关创建自定义的 Api 的详细信息。

* [在源代码管理存储服务器脚本]
  <br/> 了解如何使用源代码控制功能可以更容易地和安全地制定并发布 API 的自定义脚本代码。

<!-- Anchors. -->
[定义自定义的 API]: #define-custom-api
[更新应用程序以调用自定义的 API]: #update-app
[测试应用程序]: #test-app
[下一步行动]: #next-steps

<!-- URLs. -->
[移动服务服务器脚本引用]: http://go.microsoft.com/fwlink/?LinkId=262293
[开始使用移动服务]: mobile-services-javascript-backend-windows-store-dotnet-get-started.md
[添加到现有应用程序的移动服务]: mobile-services-javascript-backend-windows-universal-dotnet-get-started-data.md
[定义一个自定义的 API，支持定期通知]: mobile-services-windows-store-dotnet-create-pull-notifications.md
[在源代码管理存储服务器脚本]: mobile-services-store-scripts-source-control.md

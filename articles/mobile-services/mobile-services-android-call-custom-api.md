---
ms.openlocfilehash: eab917210ffe91ed73001890d001068bd5249414
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    writer="ricksal"
    pageTitle="从 Android 的客户端调用一个自定义的 API |Microsoft Azure"
    description="了解如何定义一个自定义的 API，然后调用它从一个 Android 的应用程序，使用 Azure 移动服务。"
    services="mobile-services"
    documentationCenter="android"
    authors="RickSaling"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="06/16/2015"
    ms.author="ricksal"/>

# 从客户端调用一个自定义的 API

[AZURE.INCLUDE [mobile-services-selector-call-custom-api](../../includes/mobile-services-selector-call-custom-api.md)]

本主题演示如何从一个 Android 的应用程序调用一个自定义的 API。 自定义的 API 使您能够定义自定义终结点公开服务器功能的不映射到插入、 更新、 删除或读取操作。 通过使用自定义的 API，您可以更好地控制消息，包括读取和设置 HTTP 消息头定义非 JSON 消息正文格式。

本主题中创建自定义的 API，您可以发送一个 POST 请求，*完成*标志设置为`true`的移动服务的表中的所有 todo 项。 如果没有此自定义的 API，客户端必须发送单个请求来更新表中每个 todo 项标志。

会将此功能添加到应用程序时您完成[开始使用移动服务]或[数据入门]教程创建。


>[AZURE.NOTE] 如果您想要查看已完成的应用程序的源代码，请转<a href="https://github.com/RickSaling/mobile-services-samples/tree/futures/CallCustomApi/Android" target="_blank">在此处</a>。

##先决条件

[AZURE.INCLUDE [mobile-services-android-prerequisites](../../includes/mobile-services-android-prerequisites.md)]

## <a name="define-custom-api"></a>定义自定义的 API

[AZURE.INCLUDE [mobile-services-create-custom-api](../../includes/mobile-services-create-custom-api.md)]


[AZURE.INCLUDE [mobile-services-android-call-custom-api](../../includes/mobile-services-android-call-custom-api.md)]


## 下一步行动

本主题介绍了如何使用**invokeApi**方法来调用 Android 应用程序从一个非常简单的自定义 API。 若要了解有关使用**invokeApi**方法的详细信息，请参阅发布[自定义 API 在 Azure 移动服务](http://blogs.msdn.com/b/carlosfigueira/archive/2013/06/19/custom-api-in-azure-mobile-services-client-sdks.aspx)。  

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
[移动服务 Android SDK]: http://go.microsoft.com/fwlink/p/?LinkID=280126
[移动服务服务器脚本引用]: http://go.microsoft.com/fwlink/?LinkId=262293
[我的应用程序的仪表板]: http://go.microsoft.com/fwlink/?LinkId=262039
[开始使用移动服务]: mobile-services-android-get-started.md
[有关数据入门]: mobile-services-android-get-started-data.md
[开始使用身份验证]: mobile-services-android-get-started-users.md
[开始使用推式通知]: ../mobile-services-android-get-started-push.md

[在源代码管理存储服务器脚本]: mobile-services-store-scripts-source-control.md

测试

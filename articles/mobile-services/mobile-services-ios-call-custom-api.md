---
ms.openlocfilehash: 9b3545a805f789f75ade10e60a6887ce3552a543
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="如何从 iOS 客户端调用一个自定义的 API"
    description="了解如何定义一个自定义的 API，然后调用它从 iOS 应用程序使用 Azure 移动服务。"
    services="mobile-services"
    documentationCenter="ios"
    authors="krisragh"
    writer="krisragh"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="06/16/2015"
    ms.author="krisragh"/>

# 如何调用自定义 API 从 iOS 客户端 （JavaScript 后端）

[AZURE.INCLUDE [mobile-services-selector-call-custom-api](../../includes/mobile-services-selector-call-custom-api.md)]

本主题演示如何从 iOS 应用程序调用一个自定义的 API。 自定义的 API，可以定义自定义终结点服务器的功能，但它不会不映射到数据库插入、 更新、 删除或读取操作。 通过使用自定义的 API，您可以更好地控制消息，包括 HTTP 标头和正文格式。

## <a name="define-custom-api"></a>定义自定义的 API

[AZURE.INCLUDE [mobile-services-create-custom-api](../../includes/mobile-services-create-custom-api.md)]

[AZURE.INCLUDE [mobile-services-ios-call-custom-api](../../includes/mobile-services-ios-call-custom-api.md)]

## 下一步行动

本主题介绍了如何使用**invokeApi**方法来从 iOS 应用程序调用一个相当简单的自定义 API。 若要了解有关使用**invokeApi**方法的详细信息，请参阅发布[自定义 API 在 Azure 移动服务](http://blogs.msdn.com/b/carlosfigueira/archive/2013/06/19/custom-api-in-azure-mobile-services-client-sdks.aspx)。  

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
[Windows 推式通知和实时连接]: http://go.microsoft.com/fwlink/?LinkID=257677
[移动服务服务器脚本引用]: http://go.microsoft.com/fwlink/?LinkId=262293
[我的应用程序的仪表板]: http://go.microsoft.com/fwlink/?LinkId=262039
[移动服务快速入门]: mobile-services-ios-get-started.md
[开始使用移动服务]: mobile-services-ios-get-started.md
[有关数据入门]: mobile-services-ios-get-started-data.md
[开始使用身份验证]: mobile-services-ios-get-started-users.md
[开始使用推式通知]: ../mobile-services-ios-get-started-push.md
[在源代码管理存储服务器脚本]: mobile-services-store-scripts-source-control.md

测试

---
ms.openlocfilehash: 50dda264e7994505660510248840326f39d51dc8
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
     pageTitle="如何启用的 Azure 的内容传递网络 (CDN)" 
     description="本主题演示如何启用 Azure 内容传递网络 (CDN)。" 
     services="cdn" 
     documentationCenter="" 
     authors="zhangmanling" 
     manager="dwrede" 
     editor=""/>
<tags 
     ms.service="cdn" 
     ms.workload="media" 
     ms.tgt_pltfrm="na" 
     ms.devlang="na" 
     ms.topic="article" 
     ms.date="09/01/2015" 
     ms.author="mazha"/>



#如何启用的 Azure 的内容传递网络 (CDN)  

CDN 可以启用您通过 Azure 管理门户的来源的。 当前可用的来源类型包括︰ 存储、 云服务的 Web 应用程序。 您还可以为 Azure 媒体服务流终结点启用 CDN。 一旦启用了 CDN 终结点您原点，所有公开可用的对象有资格 CDN 边缘缓存。

请注意，现在您还可以创建自定义的来源，它并没有将 Azure。

##若要创建新的 CDN 终结点  

1.  登录到[Azure 的管理门户](http://manage.windowsazure.com/)。
2.  在导航窗格中单击**CDN**。
3.  在功能区上，单击**新建**。 在**新建**对话框中，选择**应用程序服务**，然后**CDN**，然后**快速创建**。
4.  在**源类型**下拉列表中选择来源类型列表类型的窗体可用的来源。
    
    将**原始 URL**的下拉列表中显示可用的源 Url 的列表。
    

    ![createnew][createnew]

    如果选择**自定义源**，您可以输入一个自定义的原始 URL。 它没有为 Azure 的起始地址。

    ![customorigin][customorigin]

    >[AZURE.NOTE] 目前只有 HTTP 支持的来源，您必须使用介质服务扩展启用 Azure CDN 流终结点 Azure 媒体服务。
    
5.  单击**创建**按钮创建新的终结点。


>[AZURE.NOTE] 创建终结点的配置将立即不可用;可能需要多达 60 分钟的登记通过 CDN 网络传播。 通过 CDN 可用内容之前，请立即使用 CDN 的域名用户可能会收到状态代码 400 （错误请求）。

##请参见
[如何将内容传递网络 (CDN) 内容映射到自定义的域](cdn-map-content-to-custom-domain.md)

[createnew]: ./media/cdn-create-new-endpoint/cdn-create-new-account.png

[customorigin]: ./media/cdn-create-new-endpoint/cdn-custom-origin.png
 
测试

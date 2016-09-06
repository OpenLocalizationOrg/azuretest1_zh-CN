---
ms.openlocfilehash: 37ea3d9c4784d8ef177127f8cafdd9f70110fcce
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="配置 IIS Express 本地测试移动服务"
    description="了解如何配置 IIS Express 允许连接到本地的移动服务项目进行测试。"
    authors="ggailey777"
    manager="dwrede"
    services="mobile-services"
    documentationCenter=""
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="06/16/2015"
    ms.author="glenga"/>

# 本地 web 服务器配置为允许连接到本地的移动服务

Azure 创建您的移动设备的移动服务使服务在使用受支持的.NET 语言之一的 Visual Studio 中，然后将其发布到 Azure。 为您的移动服务使用.NET 后端的主要好处之一是能够运行、 测试和调试之前将它发布到 Azure 的移动服务在本地计算机或虚拟机上。 

若要能够使用模拟器，虚拟机上或在单独的工作站上运行的客户端进行本地测试移动服务，您需要配置的本地 Web 服务器和主机计算机允许连接到工作站的 IP 地址和端口。 本主题演示如何配置 IIS Express 在启用连接到您本地承载的移动服务。

[AZURE.INCLUDE [mobile-services-how-to-configure-iis-express](../../includes/mobile-services-how-to-configure-iis-express.md)]

测试

---
ms.openlocfilehash: 2a6f4e019dafaf5220729d55719be85d1c9416e5
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="在 Azure 应用程序服务使用流量管理器中配置 web 应用程序自定义域名" 
    description="使用自定义域名的 Azure 应用程序服务中包括用于负载平衡的流量管理器的 web 应用程序。" 
    services="app-service\web" 
    documentationCenter="" 
    authors="MikeWasson" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/18/2015" 
    ms.author="mwasson"/>

#在 Azure 应用程序服务使用流量管理器中配置 web 应用程序自定义域名

[AZURE.INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

[AZURE.INCLUDE [intro](../../includes/custom-dns-web-site-intro-traffic-manager.md)]

本文提供了有关自定义域名使用 Azure 应用程序服务使用通信管理器进行负载平衡的一般说明。

[AZURE.INCLUDE [tmwebsitefooter](../../includes/custom-dns-web-site-traffic-manager-notes.md)]

[AZURE.INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]

<a name="understanding-records"></a>
## 了解 DNS 记录

[AZURE.INCLUDE [understandingdns](../../includes/custom-dns-web-site-understanding-dns-traffic-manager.md)]

<a name="bkmk_configsharedmode"></a>
## 配置 web 应用程序的标准模式

[AZURE.INCLUDE [modes](../../includes/custom-dns-web-site-modes-traffic-manager.md)]

<a name="bkmk_configurecname"></a>
## 添加您自定义的域的 DNS 记录


> [AZURE.NOTE] 如果已购买到 Azure 应用程序服务 Web 应用程序的域则跳过步骤，请参阅最后一步︰<a href="/documentation/articles/custom-dns-web-site-buydomains-web-app" title="Web Apps" class="current">购买的 Web 应用程序的域</a>文章。 


要将您自定义的域与在 Azure 应用程序服务 web 应用程序相关联，您必须添加一个新条目在 DNS 表中为您自定义的域通过购买从您的域名的域注册所提供的工具。 使用以下步骤查找并使用 DNS 的工具。

1. 登录到您的帐户，您的域注册，然后查找用于管理 DNS 记录的页。 查找链接或站点标记为**域名**、 **DNS**或**名称服务器管理**的区域。 经常会发现该页面的链接可查看您的帐户信息，然后查看诸如**我的域**的链接。

4. 一旦找到管理页为您的域名，寻找一个链接，使您可以编辑的 DNS 记录。 这可能作为一个**区域文件**， **DNS 记录**，或**高级**配置链接列出。

    * 页将很可能有几条记录已创建，例如，条目关联**@**或\*与域停车页。 它还可能包含常见的子域例如**www**的记录。
    * 页面将会提及的**CNAME 记录**，或提供下拉列表以选择记录类型。 它可能会也提到其他记录，如**记录**和**MX 记录**。 在某些情况下，将由其他如**别名记录**名称调用 CNAME 记录。
    * 该页面还将字段允许您**映射**到一个**主机名**或**域名**到另一个域名。

5. 虽然每个注册的具体情况各不相同，一般情况下您*从*映射 （如**contoso.com**，) 您自定义的域的名称*与*流量管理器域名 (**contoso.trafficmanager.net**) 用于您的 web 应用程序。

6. 添加或修改 DNS 记录注册完成后，一旦保存更改。

<a name="enabledomain"></a>
## 启用通信量管理器

[AZURE.INCLUDE [modes](../../includes/custom-dns-web-site-enable-on-traffic-manager.md)]

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]
 
 

测试

---
ms.openlocfilehash: 840705371c64be00c142cd3f588449b5c7b8d6ce
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="在 Azure 应用程序服务中配置自定义域名"
    description="了解如何在 Azure 应用程序服务 web 应用程序中使用自定义的域名。"
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
    ms.date="07/13/2015"
    ms.author="mwasson"/>

# 在 Azure 应用程序服务中配置自定义域名

> [AZURE.SELECTOR]
- [购买的 Web 应用程序的域](custom-dns-web-site-buydomains-web-app.md)
- [Web 应用程序与外部域](web-sites-custom-domain-name.md)
- [Web 应用程序使用通信管理器](web-sites-traffic-manager-custom-domain-name.md)
- [GoDaddy](web-sites-godaddy-custom-domain-name.md)

[AZURE.INCLUDE [websites-cloud-services-css-guided-walkthrough](../../includes/websites-cloud-services-css-guided-walkthrough.md)]

创建 web 应用程序时，Azure 将其赋给一个 azurewebsites.net 的子域中。 例如，如果您的 web 应用程序名为**contoso**，URL 是**contoso.azurewebsites.net**。 Azure 还会指定一个虚拟 IP 地址。

用于生产的 web 应用程序，您可能希望用户看到自定义域名。 本文解释如何保留或使用[应用程序服务 Web 应用程序](http://go.microsoft.com/fwlink/?LinkId=529714)配置自定义的域。 

[AZURE.INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]


## 概述

如果您的域名已经有，或您希望保留域从其他域注册，这里是使 web 应用程序自定义域名的一般步骤︰

1. 保留您的域名。 本文不介绍该过程。 有许多可供选择的域注册。 当您注册时，其网站将引导您完成此过程。
1. 创建映射到 Azure 的 web 应用程序的域的 DNS 记录。
1. 添加在[Azure 门户网站](http://go.microsoft.com/fwlink/?LinkId=529715)的域名。

在此基本的轮廓，有需要考虑的具体情况︰

- 根域的映射。 根域是您保留的域注册的域。 例如， **contoso.com**。
- 将映射到一个子域。 例如， **blogs.contoso.com**。  您可以映射到不同的 web 应用程序的不同的子域。
- 将映射到一个通配符。 例如， ** \*。 contoso.com**。 一个通配符条目应用到您的域的所有子域。

[AZURE.INCLUDE [modes](../../includes/custom-dns-web-site-modes.md)]


## DNS 记录类型

域名系统 (DNS) 使用数据记录将域名映射到 IP 地址。 有几种类型的 DNS 记录。 对于 web 应用程序中，您将创建*一个*记录或*CNAME*记录。

- A **（地址）**记录将域名映射为 IP 地址。
- **CNAME （规范名称）**记录将域名映射到另一个域的名称。 DNS 就会使用第二个名称来查找地址。 用户仍会看到他们的浏览器中的第一个域名。 例如，可以映射到 contoso.com * &lt;yourwebapp&gt;*。 azurewebsites.net。

IP 地址更改，CNAME 条目仍然有效，而必须更新 A 记录。 但是，某些域注册机构不允许通配符域或根域的 CNAME 记录。 在这种情况下，您必须使用一个 A 记录。

> [AZURE.NOTE] 如果删除和重新创建 web 应用程序，或更改网站的 IP 地址可能会更改应用程序模式后释放。


## 找到的虚拟 IP 地址

如果您正在创建 CNAME 记录，请跳过此步骤。 若要创建一个记录，您需要您的 web 应用程序的虚拟 IP 地址。 若要获取 IP 地址︰

1.  在浏览器中，打开[Azure 门户](https://portal.azure.com)。
2.  单击左侧的页的**浏览**选项。
3.  请单击**Web 应用程序**刀片式服务器。
4.  单击您的 web 应用程序的名称。
5.  在**概要**页中，单击**所有设置**。
6.  单击**自定义的域和 SSL**。 
7.  在**自定义的域和 SSL**刀片式服务器，请单击**将外部域"**。 IP 地址位于底部的这一部分。

## 创建的 DNS 记录

登录到您的域注册和使用他们的工具中添加一个记录或 CNAME 记录。 每个注册 web 应用程序稍有不同，但下面是一些一般的指导原则。

1.  用于管理 DNS 记录到的页面。 查找链接或站点标记为**域名**、 **DNS**或**名称服务器管理**的区域。 经常会发现以下链接可查看您的帐户信息，然后查看诸如**我的域**的链接。
2.  找到管理页，寻找可以添加或编辑 DNS 记录的链接。 这可能作为一个**区域文件**， **DNS 记录**，或**高级**配置链接列出。

该页面可能还会列出一个单独，或其他记录和 CNAME 记录提供一个下拉列表中选择记录类型。 此外，它可能使用其他名称的记录类型，例如， **IP 地址记录**而不是记录或**别名记录**而不是 CNAME 记录。  通常登记员创建某些记录，因此可能已记录为常见的子域，例如**www**的根域。

当您创建或编辑记录时，这些字段将允许您将域名映射到 IP 地址 （A 记录） 或另一个域 （CNAME 记录）。 CNAME 记录，您将*从*映射您自定义的域*为*azurewebsites.net 子域。

在许多的注册工具，将只需键入您的域不是整个域名的子域部分。 此外可以使用许多工具 ' @' 是指根域。 例如︰

<table cellspacing="0" border="1">
  <tr>
    <th>主机</th>
    <th>记录类型</th>
    <th>IP 地址或 URL</th>
  </tr>
  <tr>
    <td>@</td>
    <td>（地址）</td>
    <td>127.0.0.1</td>
  </tr>
  <tr>
    <td>www</td>
    <td>CNAME （别名）</td>
    <td>contoso.azurewebsites.net</td>
  </tr>
</table>

假定该自定义域名称 'contoso.com'，这将创建以下记录︰

- 映射到 127.0.0.1 **contoso.com** 。
- 映射到**contoso.azurewebsites.net** **www.contoso.com** 。

>[AZURE.NOTE] 您可以使用 Azure DNS 以便为您的 web 应用程序承载所需域记录。 配置自定义的域，和 Azure DNS 中创建您的记录，请参见[web 应用程序创建自定义的 DNS 记录](../dns-web-sites-custom-domain)。 

<a name="awverify" />
## 创建一个 awverify 记录 （只记录）

如果您创建一个记录，web 应用程序还需要特殊的 CNAME 记录，用于验证您拥有您正在尝试使用的域。 此 CNAME 记录必须具有以下形式。

- *A 记录，如果映射的根域或一个通配符域︰*从**awverify 创建映射的 CNAME 记录。&lt;yourdomain&gt; ** **awverify 到。&lt;yourwebappname&gt;。 azurewebsites.net**。  例如，如果用于**contoso.com**A 记录，创建**awverify.contoso.com**的 CNAME 记录。
- *如果 A 记录映射特定子域︰*从**awverify 创建映射的 CNAME 记录。&lt;子域&gt;** **awverify 到。&lt;yourwebappname&gt;。 azurewebsites.net**。 例如，如果用于**blogs.contoso.com**A 记录，创建**awverify.blogs.contoso.com**的 CNAME 记录。

您的 web 应用程序的访问者看不到 awverify 的子域;它只是 Azure 来验证您的域。

## 使您的 web 应用程序的域名称

[AZURE.INCLUDE [modes](../../includes/custom-dns-web-site-enable-on-web-site.md)]

>[AZURE.NOTE] 如果您想要怎样的 Azure 帐户之前开始使用 Azure 应用程序服务，请转到[尝试应用程序服务](http://go.microsoft.com/fwlink/?LinkId=523751)，立即可以在此创建短期的初学者 web 应用程序在应用程序服务。 没有信用卡，所需;没有承诺。


## 下一步行动

有关详细信息，请参阅︰[开始使用 Azure DNS](../dns/dns-getstarted-create-dnszone)和[委托到 Azure DNS 的域](../dns/dns-domain-delegation) 

## 会发生什么变化
* 有关更改网站为应用程序服务的指南，请参阅︰ [Azure 应用程序服务，并对现有的 Azure 服务及其影响](http://go.microsoft.com/fwlink/?LinkId=529714)
* 旧的门户与新门户的更改的指南，请参阅︰[用于预览门户导航的引用](http://go.microsoft.com/fwlink/?LinkId=529715)

<!-- Anchors. -->
[概述]: #overview
[DNS 记录类型]: #dns-record-types
[找到的虚拟 IP 地址]: #find-the-virtual-ip-address
[创建的 DNS 记录]: #create-the-dns-records
[使您的 web 应用程序的域名称]: #enable-the-domain-name-on-your-web-app

<!-- Images -->
[子域]: media/web-sites-custom-domain-name/azurewebsites-subdomain.png
 

测试

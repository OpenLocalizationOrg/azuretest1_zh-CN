---
ms.openlocfilehash: e46a5f1cc33fc8b3d515c164afa4d4053e44df20
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="如何提供内部应用程序安全地远程访问"
    description="介绍如何使用 Azure AD 应用程序代理服务器可提供内部应用程序安全地远程访问。"
    services="active-directory"
    documentationCenter=""
    authors="rkarlin"
    manager="terrylan"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/07/2015"
    ms.author="rkarlin"/>

# 如何提供内部应用程序安全地远程访问

要为具有所有种类的设备--远程用户提供了访问托管，托管;平板电脑、 智能手机和笔记本电脑。 并对各种资源提供安全访问可能会很复杂。 在最近几年，反向代理服务器提供安全的远程访问的常用方式，但它们需要都难以保证安全且难以进行高可用性的防火墙后面。

## 在云环境中的安全远程访问
在现代的云环境中，我们采用远程访问到 Microsoft Azure Active Directory 中使用应用程序代理服务器的下一级。 应用程序代理是 Azure 广告作为一种服务，即它易于部署和使用的功能。 它集成了 Azure Active Directory，所使用的 Office 365 的标识相同的平台。

## 什么是 Azure 活动目录应用程序代理服务器？
应用程序代理服务器提供了单点登录和加强远程访问安全的 web 应用程序承载内部，如 SharePoint 网站和 Outlook Web Access 等。 现在可以访问内部部署 web 应用程序相同的方式在 Azure Active Directory，您 SaaS 应用程序而无需使用 VPN 或更改网络基础结构。 应用程序代理，您可以发布应用程序。 员工可以登录到您的应用程序，从家庭，在他们自己的设备上，并通过此基于云的代理服务器进行身份验证。

## 它是如何工作的？
### 启用访问
应用程序代理服务器的工作原理是安装超薄 Windows Server 服务调用在您的网络的接口。 连接器不需要打开任何入站的端口，您不必在 DMZ 中放置任何内容。 如果您有大量的流量到您的应用程序，您可以添加多个连接器，并且该服务将会自动的负载平衡。 连接器是无状态的从根据需要云纳入一切。 当用户访问应用程序远程，从任何设备，他通过 Azure Active Directory 进行身份验证并获取访问该应用程序。 

### 单一登录
Azure 的广告应用程序代理服务器提供单一登录 (SSO) 应用程序使用 IWA 或声明感知应用程序。 如果您的应用程序使用集成 Windows 身份验证，则应用程序代理模拟用户使用 Kerberos 约束委派提供 SSO。 因为用户已经经过 Azure Active Directory 实现声明感知应用程序的 SSO。

## 如何开始
请确保您有 Azure AD 基本或特优订阅和 Azure 的广告目录作为全局管理员。 您还需要目录管理员和用户访问应用程序的 Azure 广告基本或特优许可证。 看一下这里的详细信息。 

### 获取对内部应用程序的开始启用远程访问
设置应用程序代理两个步骤完成︰

1. [启用应用程序代理服务器和配置连接器](active-directory-application-proxy-enable.md)<br>
2. [发布应用程序](active-directory-application-proxy-publish.md)的快速而简单向导用于获取您的内部部署的应用程序已发布并可访问远程。

## 下一步是什么？
还有更多可以执行与应用程序代理︰


- [发布应用程序使用您自己的域名](https://msdn.microsoft.com/library/azure/mt210927.aspx)
- [启用单一登录](https://msdn.microsoft.com/library/azure/dn879065.aspx)
- [使用声明感知应用程序](https://msdn.microsoft.com/library/azure/mt210926.aspx)
- [启用条件访问](https://msdn.microsoft.com/library/azure/dn931796.aspx)


### 了解有关应用程序代理
- [看一看此处我们联机帮助](https://msdn.microsoft.com/library/azure/dn768219.aspx)
- [签出应用程序代理日志](http://blogs.technet.com/b/applicationproxyblog/)
- [观看我们的视频频道 9 ！](http://channel9.msdn.com/events/Ignite/2015/BRK3864)

## 其他资源
* [作为一个组织注册 azure](../sign-up-organization.md)
* [Azure 的标识](../fundamentals-identity.md)

测试

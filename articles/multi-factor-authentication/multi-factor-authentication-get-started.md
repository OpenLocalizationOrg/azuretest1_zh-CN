---
ms.openlocfilehash: a5ad05b98096665068ec4810b53856153a8c4254
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="Azure 的多因素身份验证-快速入门" 
    description="选择最适合您问什么上午试图保护我和在哪里找到我的用户的多因素身份验证 secutiry 解决方案。  然后，选择云，MFA 服务器或 AD FS。" 
    services="multi-factor-authentication" 
    documentationCenter="" 
    authors="billmath" 
    manager="stevenpo" 
    editor="curtland"/>

<tags 
    ms.service="multi-factor-authentication" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/24/2015" 
    ms.author="billmath"/>

#为您选择的多因素安全解决方案

因为有几个不同特色的 Azure 多因素身份验证，我们必须确定几项事情为了弄清哪一个版本是使用正确的密码。  这些事项包括︰

-   [我要保护什么](#what-am-i-trying-to-secure)
-   [用户位于何处](#where-are-the-users-located)

以下各节将确定每个提供指导。

## 我要保护什么？

为了确定正确的多因素身份验证解决方案，首先我们必须先回答问题的是什么您想要使用的身份验证的第二个方法来保护。  它是在 Azure 应用程序？  或者是它的远程访问系统为例。  通过确定我们努力确保安全，我们将看到的多因素身份验证需要启用回答。  



您试图保护什么| 在云环境中的多因素身份验证|多因素身份验证服务器 
------------- | :-------------: | :-------------: |
第一方 Microsoft 应用程序|* |* |
Saas 应用程序的应用程序库中|* |* |
通过 Azure 应用程序代理广告发布的 IIS 应用程序|* |* |
IIS 应用程序不能通过 Azure 应用程序代理广告发布 | |* |
如 RDG VPN 的远程访问| |* |



## 用户位于何处

接下来，取决于用户的位置所在，我们可以确定适合的解决方案，无论它是在云环境中的多因素身份验证或者内部部署使用 MFA 服务器使用。



用户位置| 解决方案
------------- | :------------- | 
Azure 的活动目录| 在云环境中的多因素身份验证|
Azure 的 AD 和本地广告使用 AD FS 联合身份验证| 云中的 MFA 和 MFA 服务器都是可用的选项 
Azure 的 AD 和本地广告使用目录同步，Azure AD 同步，Azure AD 连接的任何密码同步|云中的 MFA 和 MFA 服务器都是可用的选项 
Azure 的 AD 和本地广告目录同步，Azure AD 同步，Azure AD 连接的使用密码同步|在云环境中的多因素身份验证
内部部署 Active Directory|多因素身份验证服务器

下表是一个比较的功能，是与在云环境中的多因素身份验证和多因素身份验证服务器。

 | 在云环境中的多因素身份验证 | 多因素身份验证服务器
------------- | :-------------: | :-------------: |
第二个因素为移动应用程序通知 | ● | ● |
第二个因素为移动应用程序验证代码 | ● | ●
第二个因素作为电话联络 | ● | ● 
第二个因素作为单向 SMS | ● | ●
第二个因素作为双向 SMS |  | ● 
第二个因素作为硬件令牌 |  | ● 
对于不支持 MFA 的客户端应用程序密码 | ● |  
管理控制身份验证方法 |  | ● 
PIN 模式 |  | ●
欺诈警报 | ● | ●
MFA 报告 | ● | ● 
一次性的旁路 | ● | ● 
自定义问候语的电话联络 | ● | ● 
可自定义来电显示的电话联络 | ● | ● 
信任的 Ip | ● | ● 
挂起 MFA 记忆设备 （公共预览） | ● |  
条件接收 | ● | ● 
高速缓存 | ● | ● 

现在，我们就能确定是否使用云多因素身份验证或 MFA 服务器内部，我们可以开始设置和使用 Azure 多因素身份验证。   **选择代表您的方案的图标 ！**

<center>




[![云](./media/multi-factor-authentication-get-started/cloud2.png)](multi-factor-authentication-get-started-cloud.md)  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[![Proofup](./media/multi-factor-authentication-get-started/server2.png)](multi-factor-authentication-get-started-server.md) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</center>

**其他资源**

* [对于用户](multi-factor-authentication-end-user.md)
* [在 MSDN 上的 azure 多因素身份验证](https://msdn.microsoft.com/library/azure/dn249471.aspx) 

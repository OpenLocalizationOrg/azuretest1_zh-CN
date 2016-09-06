---
ms.openlocfilehash: 0e86294bb26f22717573c3f93adae6af6c1776dd
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="Azure 多因素身份验证和 Active Directory 联合身份验证服务入门" 
    description="这是介绍如何开始使用 Azure MFA 和 AD FS Azure 的多因素身份验证页。" 
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
    ms.topic="article" 
    ms.date="08/24/2015" 
    ms.author="billmath"/>

# Azure 多因素身份验证和 Active Directory 联合身份验证服务入门



<center>![云](./media/multi-factor-authentication-get-started-adfs/adfs.png)</center>

如果您的组织已联合使用 Azure Active Directory 使用 AD FS 将内部部署 Active Directory，使用 Azure 多因素身份验证以下两个选项都可用。

- 安全使用 Azure 多因素身份验证或 Active Directory 联合身份验证服务的云资源 
- 安全云资源和内部资源使用 Azure 多因素身份验证服务器 

下表总结了保护与 Azure 多因素身份验证和 AD FS 资源之间的身份验证体验

|身份验证体验-基于浏览器的应用程序|身份验证体验-基于非浏览器应用程序
:------------- | :------------- | :------------- |
保护 Azure 广告资源使用 Azure 多因素身份验证的安全 |<li>身份验证的第一个因素执行内部使用 AD FS。</li> <li>第二个因素是基于电话方法利用云身份验证执行的。</li>|最终用户可以使用应用程序密码进行登录。
使用 Active Directory 联合身份验证服务的固定 Azure 广告资源 |<li>身份验证的第一个因素执行内部使用 AD FS。</li><li>第二个因素是执行内部部署考虑索赔。</li>|最终用户可以使用应用程序密码进行登录。

与联盟用户的应用程序密码的注意事项︰ 

- 应用程序密码使用云身份验证进行验证，因此绕过联合体。 在设置应用程序密码只通常会使用联合身份验证。
- 内部部署客户端访问控制设置不会应用程序密码生效。
- 您为应用程序密码丢失内部身份验证日志记录功能。
- 帐户禁用/删除可能长达 3 小时的目录同步，延迟的应用程序的云标识密码禁用/删除。

设置 Azure 多因素身份验证或使用 AD FS Azure 多因素身份验证服务器的信息，请参阅以下︰

- [安全使用 Azure 多因素身份验证和 AD FS 的云资源](multi-factor-authentication-get-started-adfs-cloud.md)
- [使用 Windows Server 2012 R2 AD FS Azure 多因素身份验证服务器的安全云以及内部资源](multi-factor-authentication-get-started-adfs-w2k12.md)
- [安全云以及内部资源使用 AD FS 2.0 Azure 多因素身份验证服务器](multi-factor-authentication-get-started-adfs-adfs2.md)







 
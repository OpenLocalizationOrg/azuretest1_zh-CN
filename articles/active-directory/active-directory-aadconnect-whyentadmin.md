---
ms.openlocfilehash: 6d45b69015ee256c9199964be996d5fdebc5239f
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="为什么我们需要企业管理员帐户" 
    description="自定义设置说明。" 
    services="active-directory" 
    documentationCenter="" 
    authors="billmath" 
    manager="stevenpo" 
    editor="curtand"/>

<tags 
    ms.service="active-directory" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/24/2015" 
    ms.author="billmath"/>

# 为什么我们需要设置 Azure AD 连接时，连接到 AD DS 企业管理员帐户

下表显示企业管理员帐户是必需设置 Azure AD 连接的原因。

在以下情况下  | 说明 
------------- | ------------- |
快速设置和目录同步升级 | <li>快速设置，我们创建用于同步 （也称为 AD 连接器帐户） 的本地 Active Directory 帐户和分配正确的权限，用于同步和密码同步</li> <li>目录同步升级，我们重置当前配置 AD 接口帐户上的密码和配置新的 Azure AD 连接同步服务使用此帐户。 </li>



**其他资源**


* [在 Azure AD 连接的帐户和权限的详细信息](active-directory-aadconnect-account-summary.md)
* [Azure AD 连接的自定义安装](active-directory-aadconnect-get-started-custom.md)
* [在 MSDN 上的 azure AD 连接](active-directory-aadconnect.md) 

测试

---
ms.openlocfilehash: 15e37372b3393518046664ea7bd518308b8bcf26
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="为什么我们需要设置 Azure AD 连接的 Azure AD 全局管理员帐户" 
    description="自定义设置说明为什么我们需要一个全局管理员帐户。" 
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

# 为什么我们需要设置 Azure AD 连接的 Azure AD 全局管理员帐户

下表显示 Azure AD 全局管理员帐户是必需设置 Azure AD 连接的原因。

在以下情况下  | 说明 
------------- | ------------- |
快速设置和目录同步升级 | 我们 Azure 的广告目录中启用同步 （如果需要），并创建将持续同步操作 （Azure AD 接口帐户） 使用 Azure 的广告帐户。 
自定义设置 | 我们启用同步 Azure 的广告目录中，创建将用于日常同步操作 （Azure AD 连接器的帐户） 的 Azure 的广告帐户。  单一登录使用 AD FS 选项，我们可以阅读并在 Azure AD 配置联合身份验证属性。



**其他资源**


* [在 Azure AD 连接的帐户和权限的详细信息](active-directory-aadconnect-account-summary.md)
* [Azure AD 连接的自定义安装](active-directory-aadconnect-get-started-custom.md)

测试

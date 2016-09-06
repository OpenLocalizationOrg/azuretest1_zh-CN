---
ms.openlocfilehash: f8967692f5d8ab3c1ed5b4fe47e7993b7e631863
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="更改 Azure AD 连接的默认配置" 
    description="了解如何更改 Azure AD 连接的默认配置。" 
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

# 更改 Azure AD 连接的默认配置 


在大多数情况下的 Azure AD 连接的默认配置足以轻松地将您的内部目录扩展到云。  但是在某些情况下当您可能需要修改默认设置，并适合您组织的业务逻辑。  在这些情况下，您可以修改默认配置，但也有一些事情您需要执行此操作之前需要注意。

当需要更改默认的配置时，请执行以下操作︰

- 当您需要修改"框中"同步规则的属性流时，则不要更改它。 相反，创建一个新的同步规则具有更高优先级 （较低数值），其中包含您所需的属性流。
- 导出自定义同步规则使用同步规则编辑器。 这为您提供 PowerShell 脚本可用于在灾难恢复方案的情况下轻松地重新创建。
- 如果您需要更改作用域或中"框中"同步规则的连接设置，将此记录，并重新应用之后升级到较新版本的 Azure AD 连接的更改。 测试

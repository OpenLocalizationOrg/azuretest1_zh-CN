---
ms.openlocfilehash: 64b091ed88080fbf239951c85167022ebdf22bd3
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="Azure AD 连接的 SSL 证书要求" 
    description="使用 AD FS Azure AD 连接的 SSL 证书要求。" 
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

# Azure AD 连接的 SSL 证书要求

**要点︰**具有强烈建议在 AD FS 服务器场中的所有节点以及所有 Web 应用程序代理服务器使用相同的 SSL 证书。 

- 此证书必须 X509 证书。 
- 可以在测试实验室环境; 在联合服务器上使用自签名的证书但是，对于生产环境中，我们建议您从公共 CA 获取证书。 
    - 如果使用不受信任公共证书，确保每个 Web 应用程序代理服务器上安装的证书是受信任和所有联合服务器上本地服务器 
- 不支持基于 CryptoAPI 下一代 (CNG) 密钥和密钥存储提供程序的证书。  这意味着您必须使用基于 CSP （加密服务提供程序） 和不 KSP （密钥存储提供程序） 的证书。 
- 该证书的标识必须匹配的联合身份验证服务名 (例如，fs.contoso.com)。 
    - 标识是类型 dNSName 的任一主题备用名称 (SAN) 扩展，或者，如果没有 SAN 的条目，使用者名称指定为通用名称。  
    - 只要其中之一与联合身份验证服务名称相匹配，可以出现在证书中，SAN 的多个项。 
    - 如果您打算使用联接的工作场所，其他 SAN 是所需的值"enterpriseregistration"。 跟您的组织的用户主体名称 (UPN) 后缀，如 enterpriseregistration.contoso.com。 

支持通配符证书。  
 
测试

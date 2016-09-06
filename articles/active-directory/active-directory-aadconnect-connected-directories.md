---
ms.openlocfilehash: cbf9944fe5d6512a25e7c36db9a893a2bfb50bdd
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="使用 Azure AD 连接连接您的目录" 
    description="Azure AD 连接的自定义设置说明连接目录。" 
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



# 使用 Azure AD 连接连接您的目录

对于自定义设置，企业管理员帐户不需要连接到活动目录林。  该向导将接受域帐户或本地用户帐户。  但是，帐户将用作本地 AD 接口帐户，即，它是可以读取和写入同步目录信息的帐户。

这意味着您将需要分配其他权限来实现以下方案︰ 

方案  |权限
------------- | ------------- |
密码同步| <li>复制目录的更改。</li>  <li>复制目录更改所有。</li>
混合的 Exchange 部署|请参阅[Office 365 交换混合 AAD 同步回写属性和权限](https://msdn.microsoft.com/library/azure/dn757602.aspx#exchange)。
密码回写 | <li>更改密码</li><li>重置密码</li>
用户、 组和设备回写|写入权限的目录对象和属性，您想要回写
单一登录和 AD FS| 联合的服务器位于的域中的域管理员权限。





**其他资源**

* [在 Azure AD 连接的帐户和权限的详细信息](active-directory-aadconnect-account-summary.md)
* [密码同步的权限](https://msdn.microsoft.com/library/azure/dn757602.aspx#psynch)
* [交换的混合权限](https://msdn.microsoft.com/library/azure/dn757602.aspx#exchange)
* [权限的密码写回](https://msdn.microsoft.com/library/azure/dn757602.aspx#pwriteback)
* [Azure AD 连接的自定义安装](active-directory-aadconnect-get-started-custom.md)
* [在 MSDN 上的 azure AD 连接](active-directory-aadconnect.md)
 

测试

---
ms.openlocfilehash: 9b37058e55dbc922cee6a2ec347e349df7c4f7f1
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何 Azure AD 连接的工作原理" 
    description="了解如何 Azure AD 连接的工作原理。" 
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

# 如何 Azure AD 连接的工作原理



Azure 活动目录连接是三个主要部分组成。  它们是同步服务、 可选的 Active Directory 联合身份验证服务棋子和监视部分是完成使用[Azure AD 连接运行状况](https://msdn.microsoft.com/library/azure/dn906722.aspx)。


<center>![Azure AD 连接堆栈](./media/active-directory-aadconnect-how-it-works/AADConnectStack2.png)
</center>

- 同步-此部件的构成组件和此前作为目录同步和 AAD 同步发布功能。  这是负责创建用户和组的一部分。  它也是负责确保在您的内部环境中用户和组的信息匹配云。
- AD FS-这是 Azure AD 连接的可选部分，可用于混合环境中使用内部部署安装 AD FS 基础结构。  此部件可被用于由地址中包含诸如加入域 SSO，广告登录策略的实施以及智能卡或第三方 MFA 复杂部署到组织中。  有关配置 SSO 的附加信息[与单一登录上的目录同步](https://msdn.microsoft.com/library/azure/dn441213.aspx)。
- 运行状况监视-使用 AD FS Azure AD 连接状况可以提供可靠的复杂部署联合身份验证服务器的监视并提供在 Azure 门户中查看此活动的中心位置。  有关其他信息，请参阅[Azure 活动目录连接状况](https://msdn.microsoft.com/library/azure/dn906722.aspx)。


## 支持组件的 azure AD 连接

下面是每个必备项和支持 Azure AD 连接将在设置 Azure AD 连接的服务器安装的组件的列表。  此列表是基本的速成版安装。  如果您选择使用不同的 SQL Server 安装同步服务页上然后，下面列出的 SQL Server 2012年组件未安装。 

- Azure 的广告将 Azure AD 连接器连接
- Microsoft SQL Server 2012年命令行实用程序
- Microsoft SQL Server 2012年的本机客户端
- Microsoft SQL Server 2012年明示 LocalDB
- Windows PowerShell 的 azure 的活动目录模块
- Microsoft 在线服务登录助手面向 IT 专业人员
- Microsoft Visual C++ 2013年重新分发包




 

测试

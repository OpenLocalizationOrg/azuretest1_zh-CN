---
ms.openlocfilehash: 02da31a3ef3a7225a5b4aa0e79a7e3296daa19e3
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

<properties 
    pageTitle="将 Active Directory 配置为 Azure 的远程应用程序" 
    description="了解如何设置活动目录使用 Azure 远程应用程序。" 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/03/2015" 
    ms.author="elizapo" />



# 活动目录配置为 Azure 的远程应用程序


对于混合 Azure RemoteApp 的集合，您需要设置 Active Directory 域基础结构上部署和 Azure Active Directory 租户与目录集成 （和 （可选） 单一登录）。 此外，您需要在本地目录中创建一些 Active Directory 对象。 使用以下信息配置内部部署 Active Directory 和 Azure 的广告，并集成两个。

## 配置内部活动目录
通过配置内部活动目录开始。 您需要确定要使用，然后为远程应用程序创建 Active Directory 对象的域 UPN 后缀。 

没有 Active Directory 域服务对您的 Windows Server 2012 R2 环境尚未添加？ 签出[这些说明](https://technet.microsoft.com/library/cc731053.aspx)有关如何做到这一点。
### 检查并配置域后的 UPN 后缀
如果您的目录林中不使用 Azure AD 域的远程应用程序将使用匹配的 UPN 后缀，您需要添加它。 确定是否配置必要的后缀︰


1. 启动 Active Directory 用户和计算机。
2.  展开的域名，然后单击**用户**。
3.  右键单击**管理员**，然后再单击**属性**。
4.  在**帐户**选项卡上查看**用户登录名**字段中此域配置 UPN 名称。
5.  如果您看不匹配您想要用于远程应用程序集合的域名后缀，请执行以下操作︰
    1.  启动 Active Directory 域和信任关系。
    2.  **活动目录域和信任关系**，请用鼠标右键单击，然后单击**属性**。
    3.  查看列表中的后缀。
    4.  在框中，键入域名称的 FQDN，然后单击**添加**，然后选择**确定**。 示例︰ contoso.com。 

        不包括"@"在此处的后缀。

从现在起，在创建新用户时，您可以选择新后缀从用户登录名。 此外，您可以更改用户的属性中的帐户选项卡上的现有用户的后缀。

有关详细信息，请参阅[添加用户主体名称后缀](http://technet.microsoft.com/library/cc772007.aspx)。

### 为在 Active Directory 中的远程应用程序中创建对象
远程应用程序需要在您的内部部署 Active Directory 中的两个对象︰


- 通过将 RDSH 终结点加入到内部域提供 RemoteApp 程序域资源的访问权限的服务帐户。
- 组织单位 (OU) 来包含远程应用程序的计算机对象。 建议 （但不是要求） 使用 OU 的帐户和策略将使用与远程应用程序隔离。

使用以下信息创建每个对象。

#### 创建服务帐户︰


1. 启动 Active Directory 用户和计算机。
2.  右键单击**用户**，然后单击**新建 > 用户**。
3.  远程应用程序服务帐户输入一个用户名和密码。

    **注意︰**创建远程应用程序集时，您将需要此帐户信息。

#### 创建新组织单位 (OU)


1. 在 Active Directory 用户和计算机，右键单击您的域。 单击**新 > 单位**。
2. 添加到此新 OU 的远程应用程序创建的服务帐户。

    为此，查找您创建的服务帐户。 用鼠标右键单击，然后单击**移动**。 选择新的 OU，然后单击**确定**。


1. 远程应用程序服务帐户授予权力的添加和删除到该 OU 中的计算机︰
    1. 从 Active Directory 用户和计算机管理单元中，单击**视图 > 高级功能**。 将**安全**选项卡添加到属性信息。
    2. 远程应用程序服务 OU 中，用鼠标右键单击，然后单击**属性**。
    3. 在**安全**选项卡上，单击**添加**。 远程应用程序服务帐户的用户对象中，选择，然后单击**确定**。
    4. 单击**高级**。
    5. 在**权限**选项卡上选择的远程应用程序服务帐户，然后单击**编辑**。
    6. 在**适用于**字段中选择**该对象和所有子对象**。
    7. 在**权限**字段中，选择创建计算机对象和删除计算机对象旁边的**允许**，然后单击**确定**。 
    8. 单击**确定**在剩余的两个窗口。


## Azure 的 Active Directory 配置
现在，内部部署 Active Directory 设置了，将移动到 Azure 的广告。 您需要创建一个自定义的域相匹配为您的内部域的 UPN 域名后缀并设置目录集成。 混合集合支持进行同步 （使用类目录同步工具） 的 Azure Active Directory 帐户从 Windows 服务器的 Active Directory 部署;具体而言，或者使用密码同步选项同步与 Active Directory 联合身份验证服务 (AD FS) 联合身份验证配置同步。 

使用以下信息配置 Azure Active Directory


- [添加您的域](http://technet.microsoft.com/library/hh969247.aspx)– 使用此信息来添加域相匹配的内部部署 Active Directory 域的 UPN 域名后缀。
- [目录集成](http://technet.microsoft.com/library/jj573653.aspx)— 利用此信息来选择一个目录集成选项 –[与密码同步目录同步](http://technet.microsoft.com/library/dn441214.aspx)或[与联合身份验证的目录同步](http://technet.microsoft.com/library/dn441213.aspx)。

您还可以配置[多因素身份验证 (MFA)](http://technet.microsoft.com/library/dn249466.aspx)。

## 配置目录同步的问题？

如果您无法配置目录同步，请进行以下检查︰

- 使用最新版本的 Azure 目录同步工具 
-   在管理门户中，在**Active Directory-> 默认目录-> 域**，已经添加您的自定义域 (例如 mydomain.com) 并使其为主。
-   在**Active Directory-> 默认目录-> 用户**，您将添加的域 (例如 myAzureSyncUser@mydomain.com) 中的新用户。
-   在 Active Directory 域上添加新的域用户，并成为他的企业管理 (如 myDomainSyncUser@mydomain.com) 成员。

现在开始 Azure 目录同步工具， **myAzureSyncUser@mydomain.com**凭据用于第一个提示 （Microsoft Azure 活动目录管理员凭据） 和**myDomainSyncUser@mydomain.com**用于第二个提示。
 
---
ms.openlocfilehash: 546f90a20264ba3dc1c78284920000eee5c08df0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="Azure AD 连接︰ 用户帐户及权限 |Microsoft Azure"
   description="本主题介绍使用和创建帐户和所需的权限。"
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="stevenpo"
   editor=""/>

<tags
   ms.service="active-directory"  
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="12/16/2015"
   ms.author="andkjell;billmath"/>


# 帐户和 Azure AD 连接所需的权限

Azure AD 连接安装向导提供了两种不同的路径︰

- 在快速设置中，我们需要更多特权，使我们可以轻松地设置您的配置而无需创建用户或单独配置的权限。

- 自定义设置中我们为您提供更多的选择和选项，但有些情况下，您将需要确保您具有正确的权限您自己。

## 相关的文档
如果未读取有关[集成内部标识使用 Azure 活动目录](active-directory-aadconnect.md)的文档下, 表提供了指向相关主题的链接。

| 主题 |  |
| --------- | --------- |
| 安装使用快速设置 | [Azure AD 连接的快速安装](active-directory-aadconnect-get-started-express.md) |
| 安装使用自定义设置 | [Azure AD 连接的自定义安装](active-directory-aadconnect-get-started-custom.md) |
| 从目录同步升级 | [从 Azure AD 同步工具 （目录同步） 升级](active-directory-aadconnect-dirsync-upgrade-get-started.md) |


## 快速设置安装
在设置快速安装向导会询问企业管理员凭据，因此内部活动目录可以配置为使用 Azure AD 连接所需的权限。 如果要从目录同步升级的企业管理员凭据用于重置目录同步使用的帐户的密码。

向导页  | 收集的凭据 | 所需权限| 用于
------------- | ------------- |------------- |------------- |
N/A|运行安装向导的用户| 本地服务器的管理员联系| <li>创建将使用[同步引擎服务帐户](#azure-ad-connect-sync-service-account)作为本地帐户。
连接到 Azure 的广告| Azure 的广告目录凭据 | 在 Azure 广告中的全局管理员角色 | <li>启用在 Azure 的广告目录同步。</li>  <li>Azure 广告中将持续同步操作使用[Azure AD 帐户](#azure-ad-service-account)的创建。</li>
连接到 AD DS | 内部部署 Active Directory 凭据 | 企业管理员 (EA) 在 Active Directory 中组的成员| <li>在活动目录中创建一个[帐户](#active-directory-account)并授予它的权限。 这创建了帐户用于读取和写入过程中同步目录信息。</li>

### 企业管理员凭据
这些凭据仅在安装过程中使用，并在安装完成后将不会使用。 它是企业管理员，并不是域管理员，以确保在 Active Directory 中的权限可以设置中的所有域。

### 全局管理员凭据
这些凭据仅在安装过程中使用，并在安装完成后将不会使用。 它用于创建用于对 Azure AD 同步更改的[Azure AD 帐户](#azure-ad-service-account)。 该帐户还可以实现同步作为一项功能在 Azure 的广告。 所使用的帐户不能有 MFA 启用。

## 自定义设置安装
当使用用来连接到活动目录的帐户必须在安装前创建的自定义设置。

向导页  | 收集的凭据 | 所需权限| 用于
------------- | ------------- |------------- |-------------
N/A|运行安装向导的用户|<li>本地服务器的管理员联系</li><li>如果使用完整的 SQL Server，则用户必须在 SQL 系统管理员 (SA)</li>| 默认情况下，创建将使用[同步引擎服务帐户](#azure-ad-connect-sync-service-account)作为本地帐户。 如果管理员没有指定特定的帐户只能创建该帐户。
安装同步服务服务帐户选项 | 广告或本地用户帐户凭据 | 用户，权限将被授予由安装向导|如果管理员指定的帐户，该帐户用作同步服务的服务帐户。
连接到 Azure 的广告|Azure 的广告目录凭据| 在 Azure 广告中的全局管理员角色| <li>启用在 Azure 的广告目录同步。</li>  <li>Azure 广告中将持续同步操作使用[Azure AD 帐户](#azure-ad-service-account)的创建。</li>
连接您的目录|部署的每个目录林，将连接到 Azure AD 的 Active Directory 凭据 | 权限将取决于功能启用并[创建 AD DS 帐户](#create-the-ad-ds-account)，请参阅 |此帐户用于读取和写入过程中同步目录信息。
AD FS 服务器|对于列表中每个服务器，向导会收集凭据如果运行该向导的用户登录凭据不足，无法连接|域管理员|安装和配置 AD FS 服务器角色。
Web 应用程序代理服务器 |对于列表中每个服务器，向导会收集凭据如果运行该向导的用户登录凭据不足，无法连接|目标计算机上的本地管理员|安装和配置的 WAP 服务器角色。
代理服务器信任的凭据 |联合身份验证服务信任凭据 （凭据代理将使用注册 FS 从信任证书 |是 AD FS 服务器的本地管理员的域帐户|初始注册 FS WAP 信任证书。
AD FS 服务帐户页中，"使用域用户帐户选项"|AD 用户帐户凭据|域用户|提供的凭据的 AD 用户帐户将用作 AD FS 服务的登录帐户。

### 创建 AD DS 帐户
当安装 Azure AD 连接在您指定的帐户**连接您的目录**页必须存在于 Active Directory 并授予具有所需的权限。 安装向导将验证的权限以及所有问题将只找到同步过程。

所需的权限取决于可选功能，则启用。 如果您有多个域，则必须为林中的所有域授予权限。 如果您不启用这些功能的默认**域用户**权限将是足够的。

| 功能 | 权限 |
| ------ | ------ |
| 密码同步 | <li>复制目录的更改</li>  <li>复制目录更改所有 |
| 混合的 Exchange 部署 | 为用户、 组和联系人记录在[交换混合回写](active-directory-aadconnectsync-attributes-synchronized.md#exchange-hybrid-writeback)的属性的写入权限。 |
| 密码写回 | 属性为用户记录中[的密码管理入门](active-directory-passwords-getting-started.md#step-4-set-up-the-appropriate-active-directory-permissions)的写入权限。 |
| 设备写回 | 使用 PowerShell 脚本[设备写回](active-directory-aadconnect-get-started-custom-device-writeback.md)所述授予的权限。|
| 组的写回 | 读取、 创建、 更新和删除通讯组应该位于该 OU 中的组对象。|

## 升级
当从 Azure AD 连接的一个版本升级到新版本时，您将需要以下权限︰

| 主体 | 所需权限 | 用于 |
| ---- | ---- | ---- |
| 运行安装向导的用户 | 本地服务器的管理员联系 | 更新的二进制文件。 |
| 运行安装向导的用户 | ADSyncAdmins 的成员 | 对同步规则和其他配置进行更改。 |
| 运行安装向导的用户 | 如果使用的完整的 SQL 服务器︰ DBO （或类似） 的同步引擎数据库 | 进行数据库级别的更改，例如更新与新列的表。 |

## 有关创建帐户的详细信息

### 活动目录帐户

如果使用快速设置，然后将其要用于同步的 Active Directory 中创建帐户。 创建的帐户位于目录林根域，在用户容器中，将使用**MSOL_**作为前缀的名称。 使用长时间复杂密码未过期的情况下创建的帐户。 如果您有在您的域中的密码策略，请确保长和复杂的密码就会被允许为此帐户。

![AD 帐户](./media/active-directory-aadconnect-accounts-permissions/adsyncserviceaccount.png)

### Azure AD 连接同步服务帐户
安装向导创建两个本地服务帐户 （除非您指定该帐户以使用自定义设置中）。 前缀的**AAD_**用实际同步服务来作为运行的帐户。 如果您在域控制器上安装了 Azure AD 连接，在域中创建帐户。 如果在远程服务器上使用的 SQL 服务器，则必须在域中位于**AAD_**服务帐户。 带前缀的**AADSyncSched_**用于已计划的任务同步引擎运行的帐户。

![同步服务帐户](./media/active-directory-aadconnect-accounts-permissions/syncserviceaccount.png)

使用长时间复杂密码未过期的创建帐户。

对于同步引擎服务帐户，此帐户将用于 windows 存储加密密钥，因此应重置或更改此帐户的密码。

如果您使用完整的 SQL Server 服务帐户将同步引擎创建的数据库的 DBO。 该服务将无法按预期使用的任何其他权限。 此外将创建 SQL 登录名。

此外授予对文件、 注册表项和其他对象与同步引擎相关权限帐户。

### Azure AD 服务帐户
在 Azure 的广告客户将创建同步服务使用。 此帐户可通过其显示名称来识别。

![AD 帐户](./media/active-directory-aadconnect-accounts-permissions/aadsyncserviceaccount.png)

用户名称的第二部分，可以标识的服务器使用的帐户的名称。 在图片上的服务器名称是 FABRIKAMCON。 如果您有临时服务器，每个服务器都将自己的帐户。 在 Azure AD 中没有 10 个同步服务帐户的限制。

使用长时间复杂密码未过期的情况下，创建服务帐户。 它被授予**目录同步帐户**都只具有权限执行目录同步任务具有特殊作用。 这种特殊的内置角色不能被授予 Azure AD 连接向导和 Azure 门户将只显示此角色的**用户**帐户。

![广告客户角色](./media/active-directory-aadconnect-accounts-permissions/aadsyncserviceaccountrole.png)

## 下一步行动

了解有关[将您的内部标识使用 Azure Active Directory 集成](active-directory-aadconnect.md)。

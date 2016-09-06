---
ms.openlocfilehash: 325ca5cdee36054bd572e5c4e0e451861bf7b4ce
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="Azure AD 连接同步-实现密码同步 |Microsoft Azure"
    description="您提供的信息，您需要了解密码同步的工作原理，以及如何在您的环境中启用它。"
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="stevenpo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2015"
    ms.author="markusvi;andkjell"/>


# Azure AD 连接同步︰ 实现密码同步

使用密码同步时，您启用了用户使用相同的密码，它们用于登录到您的内部部署 Active Directory 以用于登录到 Azure Active Directory。

本主题的目的是为您提供的信息，您需要了解密码同步的工作原理，以及如何在您的环境中启用它。

## 什么是密码同步

密码同步是 Azure 活动目录连接同步服务 （Azure AD 连接同步） 同步到 Azure 活动目录 (AD Azure) 从内部活动目录的用户密码的功能。 此功能使您的用户可以登录到其 Azure Active Directory 服务 （如 Office 365、 Microsoft Intune 和 CRM Online） 使用相同的密码，因为它们用于登录到您的内部网络。

> [AZURE.NOTE] 有关配置为 FIPS 和密码同步的 Active Directory 域服务的详细信息，请参阅 FIPS 兼容系统中的密码同步失败。

## 密码同步的可用性

Azure Active directory 任何客户有资格运行密码同步。 请参见下面的密码同步和其他功能，如联合身份验证的兼容性信息。

## 密码同步的工作原理

密码同步是由 Azure AD 连接同步目录同步功能的延伸。 因此，此功能要求您在部署和将 Azure Active Directory 配置目录同步。

活动目录域服务的实际用户密码的哈希值表示形式存储密码。 密码哈希值不能用来登录到您的内部网络。 它还被设计，这样就能获得对用户明文密码的访问您可能无法还原。 若要同步密码，Azure AD 连接同步在内部部署 Active Directory 中提取用户的密码哈希。 其他安全处理应用于密码哈希之前就可以同步到 Azure 活动目录验证服务。 密码同步过程的实际数据流动相类似的如显示名称或电子邮件地址的用户数据同步。

密码同步频率高于其他特性的标准目录同步窗口。 密码同步在每用户的基础上，按时间顺序通常同步。 当用户的密码同步内部部署从广告到云，将覆盖现有的云密码。

第一次启用密码同步功能时，它会到 Azure Active Directory 从内部活动目录执行初始同步作用域中的所有用户的密码。 不能显式定义同步到云的密码的用户的集。 随后，由内部用户更改密码后，密码同步功能检测，并将同步更改的密码，通常在几分钟内。 密码同步功能自动重试失败的用户的密码同步。 如果同步密码的尝试过程中发生错误错误将记录在事件查看器中。

密码同步对当前登录的用户没有任何影响。 如果登录到云服务的用户也会更改后端密码，云服务会话将不间断地继续。 但是，一旦云服务要求用户重新进行身份验证，需要提供新密码。 此时，用户需要提供新的密码--最近同步在内部部署 Active Directory 中到云的密码。

## 安全注意事项

同步密码时，用户的密码的明文版本时均未公开密码同步功能，也不到 Azure 广告或任何相关的服务。

此外，也不需要在内部部署 Active Directory 以可逆加密格式存储密码。 摘要的 Active Directory 密码哈希值用于内部部署之间传输广告和 Azure 活动目录。 密码哈希值的摘要不能用于访问客户的内部环境中的资源。

## 密码策略注意事项

有两种类型的密码策略，受到了启用密码同步︰

1. 密码复杂性策略
2. 密码过期策略

### 密码复杂性策略

当启用密码同步时，配置内部部署 Active Directory 中的密码复杂性策略重写任何可能定义为同步的用户群中的复杂性策略。 这意味着在客户的内部活动目录环境中有效的任何密码可用于访问 Azure 的广告服务。


> [AZURE.NOTE] 在云环境中直接创建的用户的密码受到仍密码策略定义在云中。

### 密码过期策略

如果用户密码同步的作用域中，云帐户密码设置为"*永不过期*"。 这意味着用户的密码过期在内部环境中，可能会但他们可以继续登录到云服务使用此过期的密码。

将云密码更新下一次用户更改内部环境中的密码。


## 覆盖同步密码

管理员可以手动重置用户的密码使用 Azure 活动目录 PowerShell。

在这种情况下，新密码将覆盖该用户的同步的密码，在云环境中定义的所有密码策略将都应用于新的密码。

如果用户更改内部密码，新密码将同步到云中，和会覆盖手动更新的密码。


## 准备进行密码同步

租户可以启用密码同步之前，Azure Active Directory 租户必须启用目录同步。


## 启用密码同步

运行 Azure AD 连接配置向导时，您可以启用密码同步。

在**可选功能**对话框页面上，选择"**密码同步**"。

![可选功能][1]


> [AZURE.NOTE] 这一过程就会触发完全同步。 完全同步周期通常会超过其他的同步周期来完成。


## 管理密码同步

您可以监视通过事件日志正在运行 Azure AD 连接的计算机的密码同步的进度。


### 确定密码同步状态

您可以确定哪些用户成功有其密码同步通过查看的事件，符合下列条件︰

| 源| 事件 ID |
| --- | --- |
| 目录同步| 656|
| 目录同步| 657|

事件与事件 ID 656 提供处理的密码更改请求的报告︰

![事件 ID 656][2]

相应的事件 ID 657 与这些请求提供的结果︰

![事件 ID 657][3]

在事件中，由其定位和 DN 值标识受影响的对象。 为用户返回的 Get MsoUser cmdlet 的**ImmutableId**值对应的定位点值。

除了对象标识符中，**事件 ID 656**提供内部部署 Active Directory 中更改用户的密码的日期︰

![密码更改请求][4]

事件 ID 657 有结果字段以及源的对象标识符，以指示该用户对象的同步的状态。

已成功同步的密码在事件的事件 ID 657 表示成功的结果属性的值。 密码同步尝试失败，结果属性的值时失败︰

![密码更改结果][5]

### 触发所有密码完全的同步
如果您更改了筛选器的配置，则需要触发所有密码完全同步，因此现在在作用域中的用户将其密码同步。

    $adConnector = "<CASE SENSITIVE AD CONNECTOR NAME>"
    $aadConnector = "<CASE SENSITIVE AAD CONNECTOR NAME>"
    Import-Module adsync
    $c = Get-ADSyncConnector -Name $adConnector
    $p = New-Object Microsoft.IdentityManagement.PowerShell.ObjectModel.ConfigurationParameter “Microsoft.Synchronize.ForceFullPasswordSync”, String, ConnectorGlobal, $null, $null, $null
    $p.Value = 1
    $c.GlobalParameters.Remove($p.Name)
    $c.GlobalParameters.Add($p)
    $c = Add-ADSyncConnector -Connector $c
    Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $aadConnector -Enable $false
    Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $aadConnector -Enable $true


## 禁用密码同步

通过重新运行 Azure AD 连接配置向导中禁用密码同步。
按照向导的提示，当取消选择"密码同步"复选框。


> [AZURE.NOTE] 这一过程就会触发完全同步。 完全同步周期通常会超过其他的同步周期来完成。

运行配置向导之后，您的组织将不再进行同步密码。 新密码的更改将不会同步到云。 以前必须同步其密码的用户将能够继续登录的密码，直到它们手动更改他们的密码在云中。



## 其他资源

* [Azure AD 连接的同步︰ 自定义同步选项](active-directory-aadconnectsync-whatis.md)
* [与 Azure Active Directory 集成您的内部标识](active-directory-aadconnect.md)

<!--Image references-->
[1]: ./media/active-directory-aadsync-implement-password-synchronization/IC759788.png
[2]: ./media/active-directory-aadsync-implement-password-synchronization/IC662504.png
[3]: ./media/active-directory-aadsync-implement-password-synchronization/IC662505.png
[4]: ./media/active-directory-aadsync-implement-password-synchronization/IC662506.png
[5]: ./media/active-directory-aadsync-implement-password-synchronization/IC662507.png

测试

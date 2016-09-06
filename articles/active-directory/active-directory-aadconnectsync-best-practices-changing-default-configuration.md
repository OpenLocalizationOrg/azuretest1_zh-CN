---
ms.openlocfilehash: a4dbbf38f4ef127a9e28f08146bc424e5ed80baf
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="最佳实践更改默认配置 |Microsoft Azure"
    description="提供用于更改默认配置的 Azure AD 连接同步的最佳做法。"
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
    ms.date="08/25/2015"
    ms.author="markusvi;andkjell"/>


# Azure AD 连接同步︰ 最佳做法更改默认配置

本主题的目的是为了说明支持和不支持到 Azure AD 连接同步的更改。

通过 Azure AD 连接创建配置适用于大部分与 Azure AD 同步内部部署 Active Directory 的环境"按原样"。 但是，在某些情况下，有必要应用某些更改配置以满足特定需要或要求。

## 更改服务帐户
Azure AD 连接同步安装向导所创建的服务帐户下运行。 此服务帐户将使用同步的数据库存储加密密钥。 创建与 127 个字符长的密码和密码设置为永不过期。

- 它不**支持**更改或重新设置服务帐户的密码。 这样做将会破坏加密密钥，该服务将不能访问该数据库，能够启动。

## 对计划程序的更改
Azure AD 连接的同步设置同步标识数据每 3 小时一次。 在安装过程中会创建计划的任务，具有权限才能运行同步服务器的服务帐户下运行。

- 它不**支持**更改计划任务。 不知道该服务帐户的密码。 请参阅[更改服务帐户](#changes-to-the-service-account)
- 它不**支持**同步频率高于默认值 3 小时。

## 更改同步规则

虽然支持它要将更改应用于您 Azure AD 连接的同步配置，因为 Azure AD 连接同步应在尽可能接近装置应将它们应用时要小心。

以下是一份期望的行为︰

- 升级到较新版本的 Azure AD 连接后, 大多数设置都将重置回默认设置。
- 在应用升级后，到"框中"同步规则的更改将丢失。
- 删除"框中"同步过程升级到较新版本重新创建规则。
- 已升级到较新版本时，已创建的自定义同步规则仍未被修改。



当需要更改默认的配置时，请执行以下操作︰

- 当您需要修改"框中"同步规则的属性流时，则不要更改它。 相反，创建一个新的同步规则具有更高优先级 （较低数值），其中包含您所需的属性流。
- 导出自定义同步规则使用同步规则编辑器。 这为您提供 PowerShell 脚本可用于在灾难恢复方案的情况下轻松地重新创建。
- 如果您需要更改作用域或中"框中"同步规则的连接设置，将此记录，并重新应用后，升级到新版的 Azure AD 同步变化。



## 其他资源

* [Azure AD 连接同步︰ 自定义同步选项](active-directory-aadconnectsync-whatis.md)
* [与 Azure Active Directory 集成您的内部标识](active-directory-aadconnect.md)

<!--Image references-->

测试

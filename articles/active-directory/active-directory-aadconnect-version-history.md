---
ms.openlocfilehash: 02d69886b1b01f9fd863d1f626d424bda428c6fd
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="Azure AD 连接︰ 版本发布历史记录 |Microsoft Azure"
   description="本主题列出了 Azure AD 连接和 Azure AD 同步的所有版本"
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="stevenpo"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/24/2015"
   ms.author="andkjell"/>

# Azure AD 连接︰ 版本版本历史记录

Azure Active Directory 团队定期更新新特性和功能 Azure AD 连接。 不是所有的新增功能是适用于所有访问群体。

本文旨在帮助您跟踪已发布的版本并了解您是否需要更新到最新版本，或不。

## 1.0.8667.0
发布︰ 2015年 8 月

**新功能︰**

- 现在向所有的 Windows 服务器语言本地化 Azure AD 连接安装向导。
- 添加的支持帐户解锁时使用 Azure AD 密码管理。

**已解决的问题︰**

- 如果另一个用户继续安装而不是第一次启动安装的人会崩溃 azure AD 连接安装向导。
- 不，如果 Azure AD 连接无法彻底卸载 Azure AD 连接同步的上一安装，则可能要重新安装。
- 不能安装使用速成版安装，如果用户不是此目录林的根域中，或使用 Active directory 的非英语版本的 Azure AD 连接。
- 如果无法解决活动目录的用户帐户的 FQDN，则显示误导性的错误消息"无法提交架构"。
- 如果该向导更改了活动目录连接器上使用的帐户，向导将在后续运行失败。
- Azure AD 连接有时无法在域控制器上安装。
- 不能启用和禁用"转移模式"，如果已添加扩展属性。
- 密码写回失败在某些配置中由于错误的密码，在活动的目录连接器上。
- 如果 dn 属性筛选中使用，则无法升级目录同步。

## 1.0.8641.0
发布︰ 2015 6 月

**Azure AD 连接的初始版本。**

从 Azure AD 同步到 Azure AD 连接的更改的名称。

## 1.0.494.0501
发布︰ 2015 5 月

**新的要求︰**

- Azure AD 同步现在要求安装的.Net framework 版本 4.5.1。

**已解决的问题︰**

- 从 Azure 广告的密码写回失败，servicebus 连接错误。

## 1.0.491.0413
发布︰ 2015 4 月

**已解决的问题和改进︰**

- 如果启用了回收站，在目录林中有多个域 Active Directory 连接器不正确处理删除。
- Azure 活动目录连接器已改进的导入操作的性能。
- 当一组超过了成员资格限制 （默认情况下，限制设置为 50 k 对象），Azure Active Directory 中已删除的组。 新的行为是，组将保持，不会引发错误和任何新的成员资格更改会被导出。
- 如果具有相同 DN 的分阶段的删除已存在的连接器空间中无法设置一个新的对象。
- 某些对象标记为正在同步在增量同步期间，虽然转移的对象上没有变化。
- 强制密码同步还会删除首选的 DC 列表。
- CSExportAnalyzer 有一些对象状态的问题。

**新功能︰**

- 现在，联接可以连接到在 MV 中"ANY"对象类型。

## 1.0.485.0222
发布︰ 2015年 2 月

**改进︰**

- 改进的导入性能。

**已解决的问题︰**

- 密码同步采用属性筛选使用的 cloudFiltered 属性。 筛选的对象不再将密码同步作用域中。
- 在极少数情况下，拓扑结构在其中有很多域控制器，密码同步不起作用。
- Azure AD/Intune 已启用"停止-服务器"时从 Azure AD 接口导入后设备管理。
- 加入来自同一林中的多个域的外部安全主体 (Fsp) 导致不明确联接错误。

## 1.0.475.1202
发布︰ 2014 年 12 月

**新功能︰**

- 它现在支持使用基于属性筛选的密码同步。 有关更多详细信息，请参阅密码同步筛选。
- 到 AD 写回特性 msDS-ExternalDirectoryObjectID。 这增加了对 Office 365 提供应用程序使用 OAuth2 在线访问，支持并部署混合 Exchange 部署中的邮箱。

**固定的升级问题︰**

- 新版的签到助手是在服务器上可用。
- 自定义安装路径用于安装 Azure AD 同步。
- 无效的自定义联接条件阻止升级。

**其他修补程序︰**

- 对于 Office 专业人员加上固定模板。
- 固定的安装问题引起的以破折号开头的用户名称。
- 固定在第二次运行安装向导时丢失的 sourceAnchor 设置。
- 密码同步的固定的 ETW 跟踪

## 1.0.470.1023
发布︰ 2014年 10 月

**新功能︰**

- 密码同步从多个后端广告到 Azure 的广告。
- 本地化安装程序用户界面中的所有 Windows Server 语言。

**从 AADSync 1.0 GA 升级**

如果您已经安装的 Azure AD 同步，还有一个额外的步骤，您必须在您更改了任何现成的同步规则的情况下采取。 已升级到 1.0.470.1023 后释放的同步复制的规则进行修改。 对于每个修改同步规则执行以下操作︰

- 查找同步规则已修改并记下所做的更改。
- 删除同步规则。
- 查找新创建的 Azure AD 同步的同步规则，然后重新应用所做的更改。

**AD 帐户的权限**

AD 帐户必须被授予其他权限以便能够从 AD 中读取密码哈希值。 要授予的权限称为"复制目录更改"和"以所有形式复制目录更改"。 两个权限需要能够读取密码哈希值

## 1.0.419.0911
发布︰ 2014年 9 月

**首次发行的 Azure AD 同步。**

## 其他资源
[Azure AD 连接同步︰ 自定义同步选项](active-directory-aadconnectsync-whatis.md)

[与 Azure Active Directory 集成您的内部标识](active-directory-aadconnect.md)

测试

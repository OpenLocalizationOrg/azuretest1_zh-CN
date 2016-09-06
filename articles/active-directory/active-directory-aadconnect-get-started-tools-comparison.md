---
ms.openlocfilehash: 9af7dce6dae042d620c078ad57e690acf1d2d920
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="目录集成工具比较" 
    description="这是页面将为您提供与综合比较各种目录集成工具的表。" 
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

# 目录集成工具比较

目录集成工具具有多年增长和发展。  本文档是为了帮助提供这些工具的整合的视图和每个可用的功能进行比较。 

>[AZURE.NOTE] Azure AD 连接采用了组件和此前作为目录同步和 AAD 同步发布功能。 这些工具都不能再分别发布和所有未来的改进将纳入更新到 Azure AD 连接，以便您总是知道哪里可以获取最新的功能。 
>
>目前仍然支持目录同步，但在某些时候在未来它将被否决。 一旦它被弃用，它将只支持一段时间。  在此时间段内时支持的目录同步后将结束。


为每个表使用以下项。

● 现在 = 可用</br>
FR = 将来的发行版</br>
PP = 公共预览</br>


## 内部为云同步

| 功能  | Azure 的活动目录连接  | Azure 的活动目录同步服务 （AAD 同步） | Azure 的 Active Directory 同步工具 （目录同步）| Forefront 标识管理器 2010 R2 (FIM) |
| :-------- |:--------:|:--------:|:--------:|:--------:
| 连接到单个内部 AD 林 | ● | ● | ● | ● |
| 连接到多个内部 AD 林 |●  | ● |  | ● |
| 连接到多个使用内部部署 Exchange 的部门 | ● |  |  |  |
| 连接到内部部署单个 LDAP 目录 | FR |  |  | ● |
| 连接到多个内部 LDAP 目录 |FR  |  |  | ● |
| 连接到本地广告和本地 LDAP 目录 |FR  |  |  | ● |
| 连接到自定义的系统 （即 SQL、 Oracle、 MySQL 等） | FR |  |  | ● |
| 同步客户定义属性 （目录扩展） | ● |  |  |  |

## 内部同步到云

| 功能  | Azure 的活动目录连接  | Azure 的活动目录同步服务 | Azure 的 Active Directory 同步工具 （目录同步） | Forefront 标识管理器 2010 R2 (FIM) |
| :-------- |:--------:|:--------:|:--------:|:--------:
| 设备的写回 | ● |  | ● |  |
| 特性 （对于混合的 Exchange 部署） 的写回 | ● | ● | ● | ● |
| 用户和组对象的写回 |  ●|  | |  |
| 回写的 （从自助服务密码重置 (SSPR) 并更改密码） 的密码 |  ● | ● |  |  |



## 身份验证功能支持

| 功能  | Azure 的活动目录连接 | Azure 的活动目录同步服务 | Azure 的 Active Directory 同步工具 （目录同步） | Forefront 标识管理器 2010 R2 (FIM) |
| :-------- |:--------:|:--------:|:--------:|:--------:
| 对于单密码同步部署 AD 林 | ● | ● | ● |  |
| 多个密码同步部署 AD 林 |  ●| ● |  |  |
| 单一登录与联盟 | ● | ● | ● | ● |
| 写回的密码 （从 SSPR 和密码更改） |●  | ● |  |  |



## 设置和安装

| 功能  | Azure 的活动目录连接  | Azure 的活动目录同步服务 | Azure 的 Active Directory 同步工具 （目录同步） | Forefront 标识管理器 2010 R2 (FIM) |
| :-------- |:--------:|:--------:|:--------:|:--------:
| 支持的域控制器上安装 | ● | ● | ● |  |
| 支持使用 SQL Express 安装 | ● | ● | ● |  |
| 从目录同步可以方便地升级 |● |  |  |  |
| 本地化的 Windows 服务器语言 | FR | ● | ● |  |
| Windows Server 2008 和 Windows Server 2008 R2 支持 | ● 对于同步，不为联盟| ● | ●  | ● |
| 对 Windows Server 2012 和 Windows Server 2012 R2 支持 | ● | ● | ● | 仅 2012 |


## 筛选和配置

功能  | Azure 的活动目录连接 | Azure 的活动目录同步服务 | Azure 的 Active Directory 同步工具 （目录同步） | Forefront 标识管理器 2010 R2 (FIM)  
:-------- |:--------:|:--------:|:--------:|:--------:|
筛选域和组织单位 | ● | ● | ● | ●  
在选择对象属性值筛选 | ● | ● | ● | ● 
允许的最小特性集同步 (MinSync) | ● | ● |  |   
允许不同的服务模板将应用于属性流 |●  | ● |  |   
允许从 AD 流到 Azure AD 中删除属性 | ● | ● |  |  
允许属性流的高级自的定义 | ● | ● |  | ●  

测试

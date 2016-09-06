---
ms.openlocfilehash: e2188d49816cf1c1151a790f5b51efe788098914
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="使用方案和部署考虑事项 Azure 广告加入 |Microsoft Azure" 
    description="解释如何管理员可以设置 Azure 广告加入为其最终用户 （员工、 学生、 其他用户） 的主题。" 
    services="active-directory" 
    documentationCenter="" 
    authors="femila" 
    manager="stevenpo" 
    editor=""/>

<tags 
    ms.service="active-directory" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/02/2015" 
    ms.author="femila"/>

# 使用案例和 Azure AD 连接的部署注意事项 

## Azure AD 连接的使用情形
方案 1︰ 业务很大程度上在云中
--------------------------------------------------------
Azure AD 连接可以让您受益如果当前操作并为您在云或立即迁移到云的业务管理标识。 您可以使用在 Azure 的广告，以用于登录到 Windows 10 中创建帐户。 通过[第一个运行的经验 (FRX) 进程]((active-directory-azureadjoin-user-frx.md))或通过[设置体验](active-directory-azureadjoin-user-upgrade.md)的加入 Azure 广告，您的用户可以将他们的计算机加入到 Azure 的广告。  您的用户现在可以享受 SSO 访问其云资源例如 Office 365 在浏览器或 Office 应用程序中。 

方案 2︰ 教育机构 
----------------------------------------------------------------------------------
教育机构通常有两种用户类型︰ 教职员和学生。 参与试验的教职员被认为长术语组织的成员，需要为它们创建本地帐户。 但学生的较短期限的组织成员，因此可以管理 Azure AD 中所以目录比例可以推送到云环境而不是内部。 这些学生现在可以利用他们的 Azure 的广告帐户登录到 Windows 和 Office 应用程序中有权访问 Office 365 提供资源。 

方案 3︰ 零售企业
---------------------------------------------------------------------------------------
零售业务具有季节性的工作人员和长期雇员。 可以创建更长期限的全职员工，以及内部部署的帐户通常使用加入域的计算机。 但季节性工作人员的较短期限的组织成员，因此需要管理的用户许可证可以更轻松地来回移动。 在 Office 365 许可证云中创建这些用户允许这些用户才能登录到 Windows 和 Office 的 Azure 的广告帐户的应用程序维护其许可证的更具灵活性，一旦他们离开时的优势。 

方案 4︰ 其他方案
------------------------------------------------------------------------------------------
除上文所提及的情况下，也可以获益都有加入他们的设备加入到 Azure AD 简化加入体验，设备管理 Azure 做广告，因为自动 MDM 登记和单一登录到 Azure AD 用户和内部部署的资源。  


##Azure AD 连接的部署注意事项

Azure 广告直接加入公司所拥有的设备为用户启用
-----------------------------------------------------------------------------------------

企业可以向合作伙伴公司和组织提供云专用帐户。 然后提供这些合作伙伴能够轻松访问和单一登录其公司的应用程序和资源。 这种情况下是适用于使用他们的设备主要访问云中的 SaaS 应用程序依赖于 Azure AD 身份验证 Office 365 和资源的用户。

### 先决条件
**在企业级别 （管理员）**

*   使用 Azure Active Directory azure 订阅  

**在用户级别**

*   Windows 10 （专业人员和企业 Sku）

### 管理员任务
* [设置设备注册和 MFA](active-directory-azureadjoin-setup.md)

### 用户任务
* [设置 Azure AD 安装过程中使用新的 Windows 10 设备](active-directory-azureadjoin-user-frx.md)
* [从设置设置 Azure AD 的 Windows 10 设备](active-directory-azureadjoin-user-upgrade.md)
* [个人的 Windows 10 设备加入您的组织](active-directory-azureadjoin-personal-device.md)
  


## 在 Windows 10 您的组织中启用 BYOD
您可以设置您的用户和员工给用户他们个人的 Windows 设备访问公司的应用程序和资源。 您的用户可以添加个人 Windows 设备通过安全和兼容的方式访问工作资源及其 Azure 的广告帐户 （工作）。 

### 先决条件
**在企业级别 （管理员）**

*   Azure 广告订阅

**在用户级别**

*   Windows 10 （专业人员和企业 Sku）


### 管理员任务

* [设置设备注册和 MFA](active-directory-azureadjoin-setup.md)

### 用户任务
* [个人的 Windows 10 设备加入您的组织](active-directory-azureadjoin-personal-device.md)


## 其他信息
* [扩展到 Windows 10 设备通过 Azure 活动目录加入云功能](active-directory-azureadjoin-user-upgrade.md)
* [设置 Azure 广告加入](active-directory-azureadjoin-setup.md)

测试

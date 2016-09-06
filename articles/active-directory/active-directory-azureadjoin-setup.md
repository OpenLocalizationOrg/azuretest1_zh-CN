---
ms.openlocfilehash: 1fd9d2bf0057d4a51d8bcd7d071253d12c9ab48b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="为用户设置了 Azure 广告加入 |Microsoft Azure" 
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

# 您的组织中设置 Azure 广告加入

设置 Azure AD 连接之前，您需要同步用户对云的内部目录，或者在 Azure AD 中手动创建托管的帐户。 

同步到 Azure 广告上 prem 用户详细的说明介绍[集成内部标识使用 Azure 活动目录](active-directory-aadconnect.md)。


要手动创建和管理在 Azure AD 用户，是指[在 Azure AD 用户管理](https://msdn.microsoft.com/library/azure/hh967609.aspx)。

## 设置设备注册 
1. 在 Azure 门户以管理员身份登录。
2. 在左窗格中，选中 Active Directory。
3. 在**目录**选项卡上，选择您的目录。
4. 选择**配置**选项卡。
5. 滚动到部分**设备**。
6. 在**设备**选项卡上，设置下列各项︰  
   * **每个用户的设备的最大数目**︰ 选择的设备，用户可以在 Azure AD 中具有最大数量。  如果用户达到此配额，它们不能够添加额外的设备，直到删除一个或多个其现有设备。
   * **需要加入设备的多因素身份验证**︰ 启用时，用户应提供将其设备加入到 Azure AD 身份验证第二个因素。 多因素身份验证的详细信息，请参阅[入门 Azure 云环境中的多因素身份验证](multi-factor-authentication-get-started-cloud/)
   * **用户可能 Azure AD 连接设备**︰ 选择的用户和组，可以将设备加入到 Azure 的广告。
   * **在 Azure 广告加入设备上的其他管理员**︰ 使用 Azure AD 津贴或企业移动套件 (EMS)，您可以选择哪些用户被授予对该设备的本地管理员权限。 全局管理员和设备所有者默认情况下授予本地管理员权限。

<center>![](./media/active-directory-azureadjoin/active-directory-aadjoin-configure-devices.png) </center>
 
为用户设置了 Azure 广告加入后，它们可以连接到 Azure 广告通过他们公司或者个人的设备。 

以下是如何可以使您的用户设置 Azure 广告加入的三种情形︰

- 用户直接到 Azure AD 加入公司所拥有的设备
- 用户域将公司拥有的设备加入到内部部署 Active Directory 和延长到 Azure 的广告
- 用户的个人设备上添加到 Windows 工作帐户 

## 其他信息
* [扩展到 Windows 10 设备通过 Azure 活动目录加入云功能](active-directory-azureadjoin-user-upgrade.md)
* [了解使用方案 Azure 广告加入](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [设置 Azure 广告加入](active-directory-azureadjoin-setup.md)




测试

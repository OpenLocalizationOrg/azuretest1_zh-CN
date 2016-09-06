---
ms.openlocfilehash: c01309d684aa87416bd7a0b3f83a6d74195e480c
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="加入您的组织的个人设备 |Microsoft Azure"
    description="解释如何，用户可以注册 Windows 10 个人计算机到公司网络的主题。"
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

# 个人设备加入您的组织

Windows 10 设备加入您的组织
--------------------------------------------------------------------------------------------
1.  从**开始**菜单中，选择**设置**。
2.  选择**帐户**，然后单击**您的帐户**。
3.  单击**添加工作或学校帐户**，然后键入您组织的帐户中。
4.  您将再进入登录页为您的组织。 输入您的用户名和密码，然后单击**确定**。
5.  您然后将提示输入多因素身份验证质询。 这是可配置的 IT。
6.  Azure 广告然后将检查此用户/设备是否需要移动设备管理 (MDM) 注册。
7.  Windows 将在 Azure AD 公司的目录中注册该设备，然后注册 mdm。
8.  完成这些后，如果您托管的用户，Windows 将安装过程总结并采取通过自动登录屏幕的桌面用户。
9.  如果您是联盟的用户，您将转到 Windows 登录屏幕并需要输入您的凭据。

## 其他信息
* [扩展到 Windows 10 设备通过 Azure 活动目录加入云功能](active-directory-azureadjoin-user-upgrade.md)
* [了解使用情况和 Azure AD 连接的部署注意事项](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [设置 Azure 广告加入](active-directory-azureadjoin-setup.md)

测试

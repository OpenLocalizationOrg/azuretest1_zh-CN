---
ms.openlocfilehash: 2c842acd463eb883fff0ee2149457804eb645435
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="从设置设置 Azure AD 的 Windows 10 设备 |Microsoft Azure" 
    description="解释如何用户可以加入到设置菜单通过 Azure AD 的主题。" 
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

# 从设置设置 Azure AD 的 Windows 10 设备
如果您已经在使用 Windows 7 或 8，并且您的计算机升级到 Windows 10，您可以将其加入到 Azure 广告通过设置菜单。

加入到 Azure 的广告，从设置菜单
-----------------------------------------------------------------------------------------------

1. 从开始菜单中，单击设置超级按钮。
2. 从设置->**系统**->**有关**->**加入 Azure 的广告**
<center>
![](./media/active-directory-azureadjoin/active-directory-azureadjoin-settings.png) </center>

3. 单击**继续**Azure 广告加入消息窗口上。
<center>
![](./media/active-directory-azureadjoin/active-directory-azureadjoin-message.png) </center>
4. 提供您的登录凭据。 此登录体验将包括完整的身份验证所需的所有步骤。 如果联盟组织的一部分，您的管理员将提供您联合身份验证体验由您的组织。
<center>
![](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png) </center>
5. 如果您的组织已配置加入到 Azure AD 的多因素身份验证，必须先能够继续提供第二个因素。
6. 单击允许此设备管理中的**接受**屏幕。
7. 您应该看到该消息"您的设备现在加入到您的组织在 Azure 的广告"。


## 其他信息
* [扩展到 Windows 10 设备通过 Azure 活动目录加入云功能](active-directory-azureadjoin-user-upgrade.md)
* [了解使用情况和 Azure AD 连接的部署注意事项](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [设置 Azure 广告加入](active-directory-azureadjoin-setup.md)

测试

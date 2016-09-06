---
ms.openlocfilehash: da9cdfa3f1e4843c2b7172aa1cad9f85592d1b09
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="设置 Azure AD 安装过程中的新设备 |Microsoft Azure" 
    description="解释如何用户设置 Azure 广告加入在其首次运行体验过程的主题。" 
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

# 设置 Azure AD 安装过程中的新设备

在 Windows 10，最终用户可以将其设备加入到 Azure 广告中首次运行体验 (FRX)。 这将允许组织以分发给他们的员工或学生，塑封的设备或让他们选择他们自己的设备 (CYOD)。
如果您安装的专业或 Windows 10 企业 SKU，体验默认公司拥有的设备的设置。

若要将设备加入到 Azure 的广告
-----------------------------------------------------------------------

1. 后**前的准备工作**阶段，系统会提示安装向导。
2. 通过自定义其区域和语言选项开始，接受最终用户许可协议和上网。
<center>
![](./media/active-directory-azureadjoin/active-directory-azureadjoin-customize-region.png) </center> 
3. 选择要连接到 internet 的网络。
4. 如果是个人设备或公司拥有设备，请选择此选项︰
5. 请单击**此设备属于我的组织**。 这将启动 Azure 广告加入体验。 下面是您在 Windows 10 专业 SKU 中看到一个屏幕。 
<center>
![](./media/active-directory-azureadjoin/active-directory-azureadjoin-who-owns-pc.png) </center>

6.  输入您的组织提供给您的凭据。
<center>
![](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png) </center> 
7.  一旦您输入您的用户名，则匹配租户位于 Azure 的广告。 如果您是在联盟的域中，您将重定向到您的内部安全令牌服务 (STS) 服务器 (例如，AD FS)。 如果您在非联盟的域中的用户，您需要直接在 Azure 的承载广告的页面上输入您的凭据。 此外请参阅您的组织的徽标，并将支持文本，如果公司品牌被配置。
8.  您然后将提示输入多因素身份验证质询。 这是可配置的 IT。
9.  Azure 广告然后将检查此用户/设备是否需要移动设备管理 (MDM) 注册。 
10. Windows 将在 Azure AD 公司的目录中注册该设备，然后注册 mdm。
11. 完成这些后，如果您托管的用户，Windows 将安装过程总结并经过自动登录到桌面用户。
12. 如果您是联盟的用户，您停放在 Windows 登录屏幕上，必须输入您的凭据登录。

> [AZURE.NOTE] 不支持联接内部部署 Active Directory 域中 Windows 的全新体验。 因此，如果您打算将计算机加入到域您应该选择链接"设置与本地 Windows 帐户改为"。 如之前所做，然后可以加入从 PC 设置的域。

## 其他信息
* [了解使用方案 Azure 广告加入](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [设置 Azure 广告加入](active-directory-azureadjoin-setup.md)



测试

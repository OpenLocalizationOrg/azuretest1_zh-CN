---
ms.openlocfilehash: d844b4e25e538671f0ab9a46d0c24151aba91fd0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="设备自动注册了 Azure Active Directory for Windows Domain-Joined 设备 |Microsoft Azure"
    description="IT 管理员可以选择加入域的 Windows 设备注册 Azure 活动目录 (AD Azure) 自动地默默。"
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
    ms.date="08/14/2015"
    ms.author="femila"/>

# 与 Azure Active Directory for Windows Domain-Joined 设备的设备自动注册

作为 IT 管理员，您可以选择自动地默默注册 Azure 活动目录 (AD Azure) 加入域的 Windows 设备。 这很有用，如果您配置了基于设备条件访问策略与 Office365 的应用程序或应用程序管理内部 AD fs。 您可以了解有关设备注册方案通过阅读[Azure 活动目录设备注册概述](active-directory-conditional-access-device-registration-overview.md)。

自动设备注册 Azure Active Directory 仅供 Windows 7 和已加入到 Active Directory 域的 Windows 8.1 机。 这些是通常公司负责为信息工作者提供了的机器。

要重新注册您的域加入 Windows Azure 广告设备，请按照下面的先决条件。 一旦完成了各项前提条件，配置设备自动注册为您的域加入 Windows 设备。

## 自动设备注册的域加入 Windows Azure Active Directory 设备的先决条件

部署 AD FS 和连接到 Azure Active Directory 使用 Azure 活动目录连接
----------------------------------------------------------------------------------------------
1. 使用 Azure 活动目录连接部署 Windows Server 2012 R2 与 Active Directory 联合身份验证服务 (AD FS) 并设置使用 Azure Active Directory 的联盟关系。
2. 配置其他 Azure Active Directory 信赖方信任声明规则。
3. 打开 AD FS 管理控制台，然后定位到**AD FS**>**信任关系 > 信赖方信任**。 在 Microsoft Office 365 标识平台信赖方信任对象上右键单击并选择**编辑声明规则...**
4. 在**颁发转换规则**选项卡上，选择**添加规则**。
5. 从**声明规则**模板下拉列表框中选择**发送使用自定义规则的声明**。 选择**下一步**。
6. 键入的*Auth 方法声明规则***声明规则名称︰**文本框。
7. 键入中的下列声明规则**声明规则︰**文字框 ***

        c:[Type == "http://schemas.microsoft.com/claims/authnmethodsreferences"]
        => issue(claim = c);
    
8. 单击**确定**两次，以完成该对话框。

配置信赖方信任身份验证类引用其他 Azure 活动目录
-----------------------------------------------------------------------------------------------------
9. 在联合身份验证服务器上，打开 Windows PowerShell 命令窗口并键入︰


  `Set-AdfsRelyingPartyTrust -TargetName <RPObjectName> -AllowedAuthenticationClassReferences wiaormultiauthn`
   
其中<RPObjectName>是 Azure Active Directory 信赖方信任对象的信赖方对象名称。 此对象通常称为 Microsoft Office 365 标识平台。

AD FS 全局身份验证策略
-----------------------------------------------------------------------------
1. AD FS 全局主身份验证将策略配置为允许 Windows 集成身份验证的 Intranet （这是默认值）。


Internet Explorer 配置
------------------------------------------------------------------------------
1. 本地 intranet 安全区域您 Windows 的设备上，在 Internet Explorer 中配置下列设置︰
    * 只有一个证书时不提示进行客户证书选择︰**启用**
    * 允许脚本︰**启用**
    * 仅在 Intranet 区域中的自动登录︰**已选中**

这些是 Internet Explorer 本地 intranet 安全区域的默认设置。 您可以查看或管理这些设置在 Internet Explorer 中的通过导航到**Internet 选项** > **安全**> 本地 intranet > 自定义级别。 您还可以配置这些设置使用 Active Directory 组策略。

网络连接
-------------------------------------------------------------
加入域的 Windows 设备必须连接到 AD FS 和 Active Directory 域控制器自动注册 Azure 的广告。 这通常意味着该计算机必须连接到公司网络。 这可以包括有线的连接、 Wi-Fi 连接、 DirectAccess，或者 VPN。

##配置自动设备注册 Windows 7 和 Windows 8.1 加入域设备

配置自动设备注册 Windows 7 和 Windows 8.1 加入域设备使用下面的链接。 请确保在继续操作之前您已完成上述的前提条件。

* [配置自动设备注册为 Windows 8.1 加入域设备](active-directory-conditional-access-automatic-device-registration-windows8_1.md)

* [配置自动设备注册 Windows 7 的加入域设备](active-directory-conditional-access-automatic-device-registration-windows7.md)

附加注释
--------------------------------------------------------------------

设备注册 Azure 的广告与提供最广泛的设备功能。 Azure 广告设备注册，您可以注册个人 (BYOD) 移动设备和所拥有的公司，加入域的设备。 可以与这两个承载服务如 Office365 和服务管理内部使用 AD FS 使用设备。 

公司同时移动和传统设备或使用 Office365，Azure 的广告，或其他 Microsoft 服务应该在 Azure AD 使用 Azure 广告设备注册服务注册设备使用。如果您的公司不使用移动设备而不使用任何 Microsoft 服务，例如 Office365，Azure 的广告，或者 Microsoft Intune 改为主机仅在内部部署的应用程序，您可以选择在 Active Directory 中注册的设备使用 AD FS。 

您可以了解更多有关部署设备注册与 AD FS[此处](https://technet.microsoft.com/en-us/library/dn486831.aspx)。
测试

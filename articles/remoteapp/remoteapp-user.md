---
ms.openlocfilehash: 55301983f3b88f75424ee7f1008baf1d650aebdb
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="将用户添加到 Azure RemoteApp 集合" 
    description="了解如何将用户添加到 Azure RemoteApp 集合" 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/12/2015" 
    ms.author="elizapo" />

# 如何将用户添加到 Azure RemoteApp 集合

您的用户可以查看并在 Azure 远程应用程序中使用您的应用程序之前，您必须授予他们访问到您的收藏。 这是非常简单的过程︰**用户访问权**选项卡上，输入用户的帐户信息，然后单击复选标记。

您需要哪种帐户信息？ 这取决于集合的类型在您创建 （云或混合），无论您使用 Office 365 ProPlus 在该集合中。

## 受支持的用户标识

不同的集合类型 （与混合云） 支持用于访问应用程序中使用不同的用户标识。  

混合的远程应用程序的集合，您需要在内部部署版本和 Azure Active Directory 租户与目录集成设置 Active Directory 域基础结构 （和 （可选） 单一登录）。 此外，您需要在本地目录中创建一些 Active Directory 对象。  

RemoteApp 云集合，Azure 支持标识的活动目录的任何用户可以授予用户访问远程应用程序以包括 Microsoft 的帐户。  请参阅下表。 

Office 365 的用户是 Azure Active Directory 用户。 如果他们有 Azure Active Directory 混合，目录同步帐户，它们可被授予 RemoteApp 混合部署中的用户访问。   

您可以使用此表作为快速参考标识支持在您的收藏集以及 Active Directory 的要求是什么。

|用户帐户 |云   |混合|
|--------------|--------|------|
|Microsoft 帐户|     是|    否|
|Azure 活动目录 (AD Azure)| | | 
|Azure AD 云    |是    |否 |
|ADsync 密码同步  |是    |是    |
|无需密码同步 ADsync|  是 |否 |
|与 AD FS ADsync  |是    |是    |
|第三方 Azure 支持身份提供程序 （如 Ping）   |是    |是|   
|多因素身份验证    |是    |是    |

查看有关活动目录配置为远程应用程序的[详细信息](remoteapp-ad.md)。


> [AZURE.NOTE] Azure Active Directory 用户必须在与您的订阅的租户。 （您可以查看和修改您在门户中的**设置**选项卡上的订阅。 请参阅[更改 Azure Active Directory 租户使用远程应用程序](remoteapp-changetenant.md)的详细信息）。

## Office 365 ProPlus 用户帐户信息
如果您使用 Office 365 ProPlus 模板图像您集合*或*如果您创建使用 Office 365 的自定义图像，您只能添加具有 Office 365 订阅的默认域您的订阅的 Azure Active Directory 用户。 有关更多信息，请参见[使用 Azure RemoteApp 的 Office 365 提供](remoteapp-o365.md)。
 
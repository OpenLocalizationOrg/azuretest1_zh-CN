---
ms.openlocfilehash: e3b6eb7e5a65a1a43c94dbb2bbe7ce2d7769c75f
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="Azure 的 Active Directory 设备注册概述 |Microsoft Azure"
    description="是基于设备的条件访问方案的基础。 设备注册时，Azure 活动目录设备注册规定标识用来对设备进行身份验证，当用户登录时使用的设备。"
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

# Azure 的活动目录设备注册概述

活动目录设备注册 azure 是基于设备的条件访问方案的基础。 设备注册时，Azure 活动目录设备注册规定标识用来对设备进行身份验证，当用户登录时使用的设备。 经过身份验证的设备和属性的设备，然后可用来执行在云和内部承载的应用程序的条件访问策略。
再加上 Intune 之类的移动设备管理解决方案，Azure Active Directory 中的设备属性将更新有关该设备的其他信息。 这使您可以创建条件强制从设备访问的访问规则可以满足您对安全性和法规遵从性的标准。
活动目录设备注册 azure 是将 Azure Active Directory 中可用。 此项服务包括对 iOS 和 Android，支持设备。 利用 Azure 活动目录设备注册的个别方案可能会有更具体的要求和支持的平台。 继续阅读以了解有关目前提供给您的特定方案。

## 启用通过 Azure 活动目录设备注册的方案

Azure 广告设备注册可以看作各种方案的基础。 一般情况下，此项服务包括对 iOS 和 Android，支持设备。 利用 Azure 广告设备注册的个别方案可能会有更具体的要求和支持的平台。 这些方案如下所示︰ 条件访问应用程序承载内部︰ 您可以使用已注册的设备访问策略的应用程序被配置为使用 Windows Server 2012 R2 的 AD FS。 有关内部的条件访问设置的详细信息，请参阅设置内部条件访问使用 Azure 活动目录的设备注册。 

与 Microsoft Intune 条件 Office 365 提供应用程序的访问权限:: IT 管理员可以设置条件访问设备策略，以保护公司的资源，而同时允许信息工作者集中上兼容的设备来访问这些服务。 详细信息，请参阅 Office 365 服务条件访问设备策略

##设置 Azure 活动目录设备注册

Azure 活动目录设备注册服务有以下设置︰ 启用 Azure 广告设备注册 Azure 门户中的。

Windows 设备发现服务通过查找已知的 DNS 记录。 以便 Windows 7 和 Windows 8.1 设备能够发现并使用该服务，您必须配置您的公司 DNS。

可以查看和启用/禁用注册的 Azure Active Directory 中使用管理门户的设备。 

## 启用设备注册 Azure 的活动目录
下一节介绍如何启用 Azure 活动目录设备注册服务中的目录。
若要启用 Azure 活动目录设备注册服务
-------------------------------------------------------------
1. Azure 门户以管理员身份登录。
1. 在左窗格中，选中**Active Directory**。
1. 在**目录**选项卡上，选择您的目录。
1. 选择**配置**选项卡。
1. 滚动到部分**设备**。
1. 选择**所有****用户可能工作场所连接设备**。
1. 选择您要授权每个用户的设备的最大数目。

>[AZURE.NOTE]
>与 Microsoft Intune 或移动设备管理的 Office 365 的注册要求加入的工作区。 如果您已配置这些服务的任何一个，选择全部和无按钮被禁用。


默认情况下，服务不是允许双因素身份验证。 但是，双因素身份验证被建议注册设备时。

* 在此服务要求两因素身份验证，Azure Active Directory 中配置两因素身份验证提供程序和配置的多因素身份验证的用户帐户，因此必须到 Azure Active Directory 添加多因素身份验证，请参阅

* 如果您对 Windows Server 2012 R2 使用 AD FS，您必须配置 AD FS 中的双因素身份验证模块，请参阅使用 Active Directory 联合身份验证服务的多因素身份验证

## 配置 Azure 活动目录设备注册搜索
Windows 7 和 Windows 8.1 设备将会发现设备注册服务器通过组合设备注册服务器名是众所周知的用户帐户名。
您必须创建 DNS CNAME 记录指向与 Azure 活动目录设备注册服务相关联的 A 记录。 CNAME 记录必须使用已知的前缀 enterpriseregistration 跟您组织中的用户帐户所使用的 UPN 后缀。 如果您的组织使用多个 UPN 后缀，则必须在 DNS 中创建多条 CNAME 记录。

例如，如果使用在您的组织名为 @contoso.com 和 @region.contoso.com 两个 UPN 后缀，您将创建以下 DNS 记录。
 
| 项                                     | 类型  | 地址                            |
|-------------------------------------------|-------|------------------------------------|
| enterpriseregistration.contoso.com        | CNAME | enterpriseregistration.windows.net |
| enterpriseregistration.region.contoso.com | CNAME | enterpriseregistration.windows.net |

## 查看和管理设备 Azure Active Directory 中的对象
1. 从 Azure 管理员门户，可以查看、 阻止，并解锁设备。 被阻止的设备将不再有权的应用程序被配置为允许注册的设备。
1. Microsoft Azure 门户以管理员身份登录。
1. 在左窗格中，选中**Active Directory**。
1. 选择您的目录。
1. 选择**用户**选项卡。 然后选择用户可以查看他们的设备
1. 选择**设备**选项卡。
1. 从下拉菜单中选择**注册的设备**。
1. 您可以在此处查看阻止，或允许用户注册的设备。 

## 其他主题

您可以使用 Azure 广告设备注册注册 Windows 7 和加入 Windows 8.1 域设备。 以下主题提供了有关这些系统必备组件和 Windows 7 和 Windows 8.1 设备上配置设备注册所需的步骤的详细信息。
设备自动注册 Azure Active Directory 的 Windows 域加入设备 

测试

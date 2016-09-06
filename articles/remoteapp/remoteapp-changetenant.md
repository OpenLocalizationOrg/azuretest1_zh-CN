---
ms.openlocfilehash: 08cdd500e41f13feaf7889861289632723e8800f
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

<properties
    pageTitle="更改在 Azure RemoteApp Azure Active Directory 租户"
    description="了解如何更改与 Azure RemoteApp Azure Active Directory 租户"
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



# 更改在 Azure RemoteApp Azure Active Directory 租户

Azure RemoteApp 使用 Azure 活动目录 (AD Azure) 允许用户的访问权限。 可以使用仅 Azure AD 租户是 Azure 订阅关联。 在门户中的设置页面上，您可以查看相关联的订阅。 在订阅选项卡上查看列的目录。

> [AZURE.NOTE] 为成功执行此更改，首先从现有的 Azure Active Directory 租户所有 Azure RemoteApp 集合中删除所有用户。 要做到这一点，转到 Azure 门户，转到 Azure RemoteApp 选项卡打开每个 Azure 远程应用程序集合。 转到**用户**选项卡，删除属于您当前的 Azure Active Directory 租户的用户。 对所有现有的 Azure RemoteApp 集合重复。 如果不这样做，您将不能创建或修补程序集。

如果您想要使用一个不同的租户，则使用以下步骤以更改与您的订阅关联︰

1. 在门户中，删除任何 Azure AD 用户到 Azure RemoteApp 集合已向其授予访问权限。


2. 设置 （以前称为 Live ID） Microsoft 帐户作为服务管理员。 （在**设置-> 管理员**看起来）。
    1. 单击当前登录的用户在右上角角，然后单击**查看我的费用**。
    2. 选择您的订阅，然后单击**编辑订阅详细信息**。
    3. 进行必要的更改。



3. 退出门户，然后重新使用您在上一步中指定的 Microsoft 帐户登录。


4. 单击**新应用程序-> 服务-> 主目录-> 自定义创建-> 使用现有的目录**。 添加要与此订阅关联的 Azure AD 租户。


5. 在**设置-> 订阅**，选择您的订阅，然后单击**编辑目录**。 选择您要使用 Azure AD 租户。



您可以立即使用新的 Azure AD 租户来控制对 Azure 订阅的访问并在 Azure 远程应用程序中配置用户的访问权限。

---
ms.openlocfilehash: 54f322d133203af374938d886844da23a0db8735
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

<properties
    pageTitle="使用组来管理对 SaaS 应用程序的访问 |Microsoft Azure"
    description="如何使用 Azure AD 津贴或基本组分配访问权限的 SaaS 应用程序集成在一起 Azure 的广告。"
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na" 
    ms.topic="article"
    ms.date="08/10/2015"
    ms.author="femila"/>


# 使用组来管理对 SaaS 应用程序的访问

Azure AD 特优可以使用组来分配访问权限的 SaaS 应用程序与 Azure AD 集成在一起。 例如，如果您想要分配访问权限的市场部门使用五个不同的 SaaS 应用程序，可以创建一个包含的市场营销部门中的用户组，然后将该组分配给这些五种 SaaS 应用程序所需的市场营销部门。 通过这种方式可以节省时间，通过管理在一个地方的市场营销部门的成员资格。 用户然后分配到应用程序时被添加为成员的市场营销组，并有其分配时，删除从该应用程序从市场营销组中删除了。

与数百个应用程序，您可以添加从 Azure 的广告应用程序库中可以使用此功能。

**若要分配对 SaaS 应用程序组的访问权限**


1. 打开您所选择的浏览器并转到 Azure 管理门户。 在 Azure 管理门户中，发现在左侧的导航栏上的 Active Directory 扩展。 在**目录**选项卡下，单击您要将一个组的访问权限分配给 Saas 应用程序的目录。


2. 单击应用程序选项卡为您的目录。 从应用程序库，添加的应用程序上单击，然后单击用户和组选项卡上。

3. 在用户和组选项卡中开头为字段中，输入您要分配访问权限的组的名称，请单击右上角的复选标记。 您只需键入组的名称的第一部分。 然后，单击组突出显示它，然后单击**分配访问权限**按钮上时显示确认消息，请单击**是**。


4. 您还可以查看哪些用户分配给应用程序，直接或通过组成员资格。 若要执行此操作，为**所有用户**更改**显示下拉列表从组**。 此列表显示用户目录以及每个用户分配到应用程序中。 该列表还显示是否已分配的用户都被分配到应用程序直接 （工作分配类型显示为直接），或通过组成员身份 （工作分配类型显示为继承。）


> [AZURE.NOTE]
>只有在您已经启用了 Azure AD 津贴或 Azure AD 基本后，您将看到用户和组选项卡。

下面是一些将 Azure Active Directory 提供一些附加信息的主题

* [使用 Azure Active Directory 组管理对资源的访问](active-directory-manage-groups.md)

* [什么是 Azure 活动目录？](active-directory-whatis.md)

* [与 Azure Active Directory 集成您的内部标识](active-directory-aadconnect.md)

测试

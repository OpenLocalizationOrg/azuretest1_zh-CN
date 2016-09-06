---
ms.openlocfilehash: ba9e451e43803124f44056b66af98384701c2b84
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="创建一个简单的规则来配置动态成员身份组 |Microsoft Azure"
    description="说明如何创建一个简单的规则来配置动态组的成员身份。"
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""/>

<tags
    ms.service="active-directory" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/13/2015" 
    ms.author="femila"/>


# 创建一个简单的规则来配置动态组的成员身份

**要启用动态对于特定组的成员身份，请执行以下步骤︰**

1. 在 Azure 管理门户中，在**组**选项卡下，选择您想要编辑的组，然后在此组中的**配置**选项卡上，将**启用动态成员身份**开关设置为**是**。


2. 您现在可以设置简单的单一规则，将控制为此组功能如何动态成员身份的组。 请确保**中添加用户，**单选按钮处于选中状态，然后从下拉菜单中 （例如，部门、 职务等），选择用户属性 

3. 接下来，选择一个条件 （不等于、 等于、 不启动与、 开头，不包含、 包含、 不匹配，匹配），并最后指定选定的用户属性的值。 例如，如果某个组分配给一个 SaaS 应用程序和通过，由此设置规则启用动态此组的成员身份**中添加用户，**设置职务为 Equals(-eq) 名销售代表，其作业标题设置为销售代表，Azure 的广告目录内的所有用户将有权访问此 SaaS 应用程序。

下面是一些将 Azure Active Directory 提供一些附加信息的主题 

* [使用 Azure Active Directory 组管理对资源的访问](active-directory-manage-groups.md)

* [什么是 Azure 活动目录？](active-directory-whatis.md)

* [与 Azure Active Directory 集成您的内部标识](active-directory-aadconnect.md)


测试

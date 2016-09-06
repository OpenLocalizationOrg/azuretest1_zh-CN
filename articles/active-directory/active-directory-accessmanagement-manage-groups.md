---
ms.openlocfilehash: 5434d1bf99feea2d0eeed96caad94202b15bb3b6
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
< 属性

    pageTitle="Managing security groups in Azure Active Directory | Microsoft Azure"
    description="Covers how to sign up for Azure and first steps you can try with Azure AD."
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


#在 Azure Active Directory 管理安全组


##如何创建和管理安全组

**若要从 Azure 管理门户中创建组**

1. 在管理门户中，请单击活动目录和您单位的目录的名称，然后单击。
2. 单击**组**选项卡。
3. 在组页中，单击**添加组**。
4. 在**添加组**窗口中指定的名称和组的说明。
5. 使用 Office 365 帐户门户、 Windows Intune 帐户门户或 Azure 管理门户网站，订阅您的组织已根据哪些服务可以完成此任务。 有关使用入口管理 Azure 活动目录的详细信息，请参阅管理 Azure 的广告目录。

## 如何分配或安全组中删除用户

**若要向组中添加成员，从 Azure 管理门户**

1. 在管理门户中，请单击活动目录和您单位的目录的名称，然后单击。
2. 单击**组**选项卡。
3. 在**组**页中，单击您想要将成员添加到组的名称。 默认情况下，这将显示选定组的**成员**选项卡。
4. 在该组的页上，单击**添加成员**。
5. 在**添加成员**页中，单击用户或您想要添加作为此组的成员，请确保该名称添加到所选窗格中的组的名称。


**从 Azure 管理门户从组中删除成员**

1. 在管理门户中，请单击活动目录和您单位的目录的名称，然后单击。
2. 单击**组**选项卡。
3. 在组页中，单击要删除的成员的组的名称。
4. 在该组的页上，单击**成员**选项卡。
5. 在该组的页上，单击您想要删除此组，然后单击**删除**该成员的名称。
6. 请验证您要单击操作验证问题的答案为**是**从组中删除该成员。


##如何使用规则来动态管理安全组的成员
**要启用动态对于特定组的成员身份，请执行以下步骤︰**

1. 在 Azure 管理门户中，在**组**选项卡下，选择您想要编辑的组，然后在此组中的**配置**选项卡上，将**启用动态成员身份**开关设置为**是**。
2. 您现在可以设置简单的单一规则，将控制为此组功能如何动态成员身份的组。 请确保**添加用户，**单选按钮被选中，然后从下拉菜单中 （例如，部门、 职务等），选择用户属性 
3. 接下来，选择一个条件 （不等于、 等于、 不启动与、 开头，不包含、 包含、 不匹配，匹配），并最后指定选定的用户属性的值。
4. 例如，如果为组分配到 SaaS 应用程序 （有关详细信息，请参阅分配到 Azure 广告 SaaS 应用程序组访问） 并通过设置规则，从而使得添加用户启用动态此组的成员身份设置为职务，Equals(-eq) 销售代表，其作业标题设置为销售代表 Azure 的广告目录内的所有用户将有权访问此 SaaS 应用程序。

下面是一些将 Azure Active Directory 提供一些附加信息的主题 

* [使用 Azure Active Directory 组管理对资源的访问](active-directory-manage-groups.md)

* [什么是 Azure 活动目录？](active-directory-whatis.md)

* [将您的内部标识使用 Azure Active Directory 集成](active-directory-aadconnect.md)测试

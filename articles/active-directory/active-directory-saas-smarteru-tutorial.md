---
ms.openlocfilehash: 8fe572636547db40a2337bbc983826815681b324
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties pageTitle="教程︰ Azure Active Directory 集成与 SmarterU |Microsoft Azure" description="了解如何使用 SmarterU Azure Active Directory 以启用单一登录、 自动化资源调配，和更多。" services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="08/01/2015" ms.author="markvi" />
#教程︰ 与 SmarterU 的 Azure Active Directory 集成
>[AZURE.TIP]反馈，请单击[此处](http://go.microsoft.com/fwlink/?LinkId=524463)。
  
本教程的目的是显示 Azure 和 SmarterU 的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   SmarterU 租户
  
后学完本教程，您已分配给 SmarterU Azure AD 用户将能够到 SmarterU 公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](https://msdn.microsoft.com/library/dn308586)应用程序的单一登录
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 SmarterU
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-smarteru-tutorial/IC777320.png "Scenario")

##启用应用程序集成为 SmarterU
  
本部分的目的是概述如何启用应用程序集成为 SmarterU。

###若要启用应用程序集成为 SmarterU，请执行以下步骤︰

1.  在 Azure 管理门户中，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录](./media/active-directory-saas-smarteru-tutorial/IC700993.png "Active Directory")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-smarteru-tutorial/IC700994.png "Applications")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-smarteru-tutorial/IC749321.png "Add application")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![从 gallerry 中添加应用程序](./media/active-directory-saas-smarteru-tutorial/IC749322.png "Add an application from gallerry")

6.  在**搜索框**中，键入**SmarterU**。

    ![应用程序 fallery](./media/active-directory-saas-smarteru-tutorial/IC777321.png "Application fallery")

7.  在结果窗格中，选择**SmarterU**，，然后单击**完成**添加应用程序。

    ![SmarterU](./media/active-directory-saas-smarteru-tutorial/IC777322.png "SmarterU")

##配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 SmarterU。

###若要配置单一登录，请执行以下步骤︰

1.  在 Azure 广告门户中， **SmarterU**应用程序集成在页上，单击**配置单一登录**来打开**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-smarteru-tutorial/IC777323.png "Configure Single Sign-On")

2.  在**您想怎样来登录到 SmarterU 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-smarteru-tutorial/IC777324.png "Configure Single Sign-On")

3.  **配置单一登录在 SmarterU**页面，以下载您的元数据，请单击**下载元数据**，并且数据然后为本地文件**c:\\SmarterUMetaData.cer**。

    ![配置单一登录](./media/active-directory-saas-smarteru-tutorial/IC777325.png "Configure Single Sign-On")

4.  在不同的 web 浏览器窗口中，以管理员身份登录到 SmarterU 公司网站。

5.  在顶部的工具栏中，单击**帐户设置**。

    ![帐户设置](./media/active-directory-saas-smarteru-tutorial/IC777326.png "Account Settings")

6.  在帐户配置页，请执行以下步骤︰

    ![外部的授权](./media/active-directory-saas-smarteru-tutorial/IC777327.png "External Authorization")

    1.  选择**启用外部授权**。
    2.  在**主服务器登录控件**部分中，选择**SmarterU**选项卡。
    3.  在**用户默认登录**部分中，选择**SmarterU**选项卡。
    4.  选择**启用 Okta**。
    5.  复制下载元数据文件的内容，然后将其粘贴到**Okta 元数据**的文本框。
    6.  单击**保存**。

7.  在 Azure 广告门户，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-smarteru-tutorial/IC777328.png "Configure Single Sign-On")

##配置用户设置
  
为了使 Azure AD 用户能够登录到 SmarterU，他们必须到 SmarterU 配置。  
对于 SmarterU，资源调配是手动任务。

###若要设置用户帐户，请执行以下步骤︰

1.  登录到**SmarterU**租户。

2.  请转到**用户**。

3.  在用户部分中，执行以下步骤︰

    ![新用户](./media/active-directory-saas-smarteru-tutorial/IC777329.png "New User")

    1.  请单击**+ 用户**。
    2.  在下面的文本框中键入 Azure AD 用户帐户的相关的属性值︰**主要通过电子邮件发送**、**雇员 ID**、**密码**、**确认密码**、**给定名称**、**姓氏**。
    3.  单击**活动**。
    4.  单击**保存**。

>[AZURE.NOTE] 您可以使用任何其他 SmarterU 用户帐户创建工具或 Api 来设置 AAD 用户帐户由 SmarterU 提供。

##分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###要将用户分配到 SmarterU 中，执行以下步骤︰

1.  在 Azure 广告门户中，创建一个测试帐户。

2.  在**SmarterU**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-smarteru-tutorial/IC777330.png "Assign Users")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-smarteru-tutorial/IC767830.png "Yes")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](https://msdn.microsoft.com/library/dn308586)。
测试

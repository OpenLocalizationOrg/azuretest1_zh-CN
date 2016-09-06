---
ms.openlocfilehash: 1a139474e90ef50ed23870af50e04a867cea9a55
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties pageTitle="教程︰ Azure Active Directory 集成与 ABa Sainsburys 连接 |Microsoft Azure" description="了解如何使用 ABa Sainsburys 连接与 Azure Active Directory 以启用单一登录、 自动化资源调配，和更多。" services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="08/01/2015" ms.author="markvi" />
#教程︰ Azure Active Directory 集成与 ABa Sainsburys 连接
>[AZURE.TIP] 反馈，请单击[此处](http://go.microsoft.com/fwlink/?LinkId=615290)。

本教程的目的是显示 Azure 和 Aba Sainsburys 连接的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   Aba Sainsburys 连接-启用单一登录的订阅

后学完本教程，您已分配给连接 Aba Sainsburys Azure AD 用户将能够到在 Aba Sainsburys 连接公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](https://msdn.microsoft.com/library/dn308586)应用程序的单一登录

在本教程中介绍的方案由以下构造块组成︰

1.  启用对 Aba Sainsburys 连接的应用程序集成
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-aba-sainsburys-connect-tutorial/IC807723.png "Scenario")
##启用对 Aba Sainsburys 连接的应用程序集成

本部分的目的是概述如何启用的 Aba Sainsburys 连接的应用程序集成。

###若要启用应用程序集成的 Aba Sainsburys 连接，请执行以下步骤︰

1.  在 Azure 管理门户中，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录](./media/active-directory-saas-aba-sainsburys-connect-tutorial/IC700993.png "Active Directory")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-aba-sainsburys-connect-tutorial/IC700994.png "Applications")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-aba-sainsburys-connect-tutorial/IC749321.png "Add application")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![从 gallerry 中添加应用程序](./media/active-directory-saas-aba-sainsburys-connect-tutorial/IC749322.png "Add an application from gallerry")

6.  在**搜索框**中，键入**Aba Sainsburys 连接**。

    ![Aba Sainsburys 连接](./media/active-directory-saas-aba-sainsburys-connect-tutorial/IC807724.png "Aba Sainsburys Connect")

7.  在结果窗格中， **Aba Sainsburys 连接**，请选择，然后单击**完成**添加应用程序。

    ![Aba Sainsburys 连接](./media/active-directory-saas-aba-sainsburys-connect-tutorial/IC807725.png "Aba Sainsburys Connect")
##配置单一登录

本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 Aba Sainsburys 连接。

###若要配置单一登录，请执行以下步骤︰

1.  在 Azure 广告门户， **Aba Sainsburys 连接**应用程序集成在页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录](./media/active-directory-saas-aba-sainsburys-connect-tutorial/IC807726.png "Configure Single Sign-On")

2.  在**您想怎样 Aba Sainsburys 连接到登录的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-aba-sainsburys-connect-tutorial/IC807727.png "Configure Single Sign-On")

3.  在**配置应用程序设置**页中，请执行以下步骤︰

    ![配置应用程序设置](./media/active-directory-saas-aba-sainsburys-connect-tutorial/IC807728.png "Configure App Settings")

    1.  在**符号上 URL**文本框中，键入您为登录到 Aba Sainsburys 连接应用程序的用户使用的 URL (例如︰ *https://myaba.co.uk/client-access/sainsburys/saml.php*)。
    2.  单击**下一步**

4.  在**配置单一登录在 Aba Sainsburys 连接**页面上，要下载元数据，请单击**下载元数据**，然后将保存在您的计算机上的元数据文件。

    ![配置单一登录](./media/active-directory-saas-aba-sainsburys-connect-tutorial/IC807729.png "Configure Single Sign-On")

5.  将下载元数据文件发送给您连接 Aba Sainsburys 的支持团队。

    >[AZURE.NOTE] Aba Sainsburys 连接支持团队就不得不执行实际的 SSO 配置。
当您的订阅已启用 SSO，您将得到通知。

6.  在 Azure 广告门户，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-aba-sainsburys-connect-tutorial/IC807730.png "Configure Single Sign-On")
##配置用户设置

为了使 Azure AD 用户能够登录 Aba Sainsburys 连接到，它们必须 Aba Sainsburys 连接到配置。  
在 Aba Sainsburys 连接，用户帐户需要由您连接 Aba Sainsburys 的支持团队。

>[AZURE.NOTE] 您可以使用任何其他 Aba Sainsburys 连接用户帐户创建工具或 Api 提供的 Aba Sainsburys 连接设置 Azure 活动目录的用户帐户。

##分配用户

若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###若要将用户分配到 Aba Sainsburys 连接，请执行以下步骤︰

1.  在 Azure 广告门户中，创建一个测试帐户。

2.  在**Aba Sainsburys 连接**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-aba-sainsburys-connect-tutorial/IC807731.png "Assign Users")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-aba-sainsburys-connect-tutorial/IC767830.png "Yes")

如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](https://msdn.microsoft.com/library/dn308586)。

测试

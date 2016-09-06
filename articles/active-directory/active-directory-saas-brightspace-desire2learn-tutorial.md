---
ms.openlocfilehash: c7813ecca5ca8b1d6f3e7b4e3413654ae43b9269
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties pageTitle="教程︰ Azure Active Directory 集成与由 Desire2Learn Brightspace |Microsoft Azure" description="了解如何使用由 Desire2Learn Brightspace Azure Active Directory 以启用单一登录、 自动化资源调配，和更多。" services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="08/01/2015" ms.author="markvi" />
#教程︰ 使用由 Desire2Learn Brightspace 的 Azure 的 Active Directory 集成
>[AZURE.TIP]反馈，请单击[此处](http://go.microsoft.com/fwlink/?LinkId=550977)。

本教程的目的是显示 Azure 和由 Desire2Learn Brightspace 的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   通过 Desire2Learn 单一登录 Brightspace 启用订阅

后学完本教程，您已分配给由 Desire2Learn Brightspace Azure AD 用户将能够到单一登录到应用程序在您的 Brightspace Desire2Learn 公司网站 （服务提供程序初始化登录），或使用[接入面板简介](https://msdn.microsoft.com/library/dn308586)

在本教程中介绍的方案由以下构造块组成︰

1.  启用通过 Desire2Learn Brightspace 应用程序的集成
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798957.png "Scenario")
##启用通过 Desire2Learn Brightspace 应用程序的集成

本部分的目的是概述如何启用由 Desire2Learn Brightspace 应用程序的集成。

###若要启用由 Desire2Learn Brightspace 应用程序的集成，请执行以下步骤︰

1.  在 Azure 管理门户中，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录](./media/active-directory-saas-brightspace-desire2learn-tutorial/IC700993.png "Active Directory")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-brightspace-desire2learn-tutorial/IC700994.png "Applications")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-brightspace-desire2learn-tutorial/IC749321.png "Add application")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![从 gallerry 中添加应用程序](./media/active-directory-saas-brightspace-desire2learn-tutorial/IC749322.png "Add an application from gallerry")

6.  在**搜索框**中，键入**由 Desire2Learn Brightspace**。

    ![Apllication 库](./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798958.png "Apllication Gallery")

7.  在结果窗格中，选择**通过 Desire2Learn Brightspace**，，然后单击**完成**添加应用程序。

    ![通过 Desire2Learn Brightspace](./media/active-directory-saas-brightspace-desire2learn-tutorial/IC799321.png "Brightspace by Desire2Learn")
##配置单一登录

本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到由 Desire2Learn Brightspace。

###若要配置单一登录，请执行以下步骤︰

1.  在 Azure 广告门户中，**由 Desire2Learn Brightspace**应用程序集成在页上，单击**配置单一登录**来打开**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798959.png "Configure Single Sign-On")

2.  在**您想怎样用户能够登录到 Brightspace 的 Desire2Learn**页面上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798960.png "Configure Single Sign-On")

3.  在**配置应用程序 URL**页上，请执行以下步骤︰

    ![配置应用程序的 URL](./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798961.png "Configure App URL")

    1.  在**符号上 URL**文本框中，键入用户用于登录到您**的 Desire2Learn Brightspace**的 URL (例如︰ *https://partnershowcase.desire2learn.com/Shibboleth.sso/Login?entityID=https://sts.windows-ppe.net/5caf9349-fd93-4a74-b064-0070f65bfb49/&target=https%3A%2F%2Fpartnershowcase.desire2learn.com%2Fd2l%2FshibbolethSSO%2Faspinfo.asp*)。
    2.  单击**下一步**

4.  在**配置单一登录 Brightspace Desire2Learn 通过在**页面上，要下载元数据，请单击**下载元数据**，然后将元数据保存在您的计算机上。

    ![配置单一登录](./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798962.png "Configure Single Sign-On")

5.  将下载元数据文件发送到您的 Brightspace Desire2Learn 的支持团队。

    >[AZURE.NOTE] 您 Brightspace Desire2Learn 的支持团队由具有执行实际的 SSO 配置。
当您的订阅已启用 SSO，您将得到通知。

6.  在 Azure 广告门户，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798963.png "Configure Single Sign-On")
##配置用户设置

为了使 Azure AD 用户要登录到的 Desire2Learn Brightspace，它们必须被调配到 Brightspace Desire2Learn 通过。  
在由 Desire2Learn Brightspace，用户帐户需要由您 Brightspace Desire2Learn 的支持团队。

>[AZURE.NOTE] 您可以使用任何其他 Brightspace 的 Desire2Learn 用户帐户创建工具或 Api 提供的由 Desire2Learn Brightspace 设置 Azure 活动目录的用户帐户。

##分配用户

若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###若要将用户分配到的 Desire2Learn Brightspace，请执行以下步骤︰

1.  在 Azure 广告门户中，创建一个测试帐户。

2.  在**由 Desire2Learn Brightspace**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798964.png "Assign Users")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-brightspace-desire2learn-tutorial/IC767830.png "Yes")

如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](https://msdn.microsoft.com/library/dn308586)。

测试

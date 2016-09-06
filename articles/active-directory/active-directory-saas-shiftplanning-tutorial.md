---
ms.openlocfilehash: 18e0218795f8c6c6ca7ff3b6edd62fdc578170cb
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties pageTitle="教程︰ Azure Active Directory 集成与 ShiftPlanning |Microsoft Azure" description="了解如何使用 ShiftPlanning Azure Active Directory 以启用单一登录、 自动化资源调配，和更多。" services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="08/01/2015" ms.author="markvi" />
#教程︰ 与 ShiftPlanning 的 Azure Active Directory 集成
>[AZURE.TIP]反馈，请单击[此处](http://go.microsoft.com/fwlink/?LinkId=526797)。
  
本教程的目的是显示 Azure 和 ShiftPlanning 的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   ShiftPlanning-启用单一登录的订阅
  
后学完本教程，您已分配给 ShiftPlanning Azure AD 用户将能够到 ShiftPlanning 公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](https://msdn.microsoft.com/library/dn308586)应用程序的单一登录
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 ShiftPlanning
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-shiftplanning-tutorial/IC786612.png "Scenario")
##启用应用程序集成为 ShiftPlanning
  
本部分的目的是概述如何启用应用程序集成为 ShiftPlanning。

###若要启用应用程序集成为 ShiftPlanning，请执行以下步骤︰

1.  在 Azure 管理门户中，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录](./media/active-directory-saas-shiftplanning-tutorial/IC700993.png "Active Directory")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-shiftplanning-tutorial/IC700994.png "Applications")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-shiftplanning-tutorial/IC749321.png "Add application")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![从 gallerry 中添加应用程序](./media/active-directory-saas-shiftplanning-tutorial/IC749322.png "Add an application from gallerry")

6.  在**搜索框**中，键入**ShiftPlanning**。

    ![应用程序库](./media/active-directory-saas-shiftplanning-tutorial/IC786613.png "Application Gallery")

7.  在结果窗格中，选择**ShiftPlanning**，，然后单击**完成**添加应用程序。

    ![ShiftPlanning](./media/active-directory-saas-shiftplanning-tutorial/IC786614.png "ShiftPlanning")
##配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 ShiftPlanning。  
作为此过程的一部分，您将需要创建一个 base 64 编码的证书文件。  
如果您不熟悉此过程，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)

###若要配置单一登录，请执行以下步骤︰

1.  在 Azure 广告门户中， **ShiftPlanning**应用程序集成在页上，单击**配置单一登录**来打开**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-shiftplanning-tutorial/IC786615.png "Configure Single Sign-On")

2.  在**您想怎样来登录到 ShiftPlanning 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-shiftplanning-tutorial/IC786616.png "Configure Single Sign-On")

3.  在**配置应用程序 URL**页上，在**ShiftPlanning 号上的 URL**文本框中，键入您的 URL 使用以下模式"*https://company.shiftplanning.com/includes/saml/*"，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-shiftplanning-tutorial/IC786617.png "Configure App URL")

4.  在**配置单一登录 ShiftPlanning 在**页上，下载您的证书，单击**下载证书**，然后将保存在您的计算机上的证书文件。

    ![配置单一登录](./media/active-directory-saas-shiftplanning-tutorial/IC786618.png "Configure Single Sign-On")

5.  在不同的 web 浏览器窗口中，以管理员身份登录到**ShiftPlanning**公司网站。

6.  在顶部菜单中，单击**管理**。

    ![管理员](./media/active-directory-saas-shiftplanning-tutorial/IC786619.png "Admin")

7.  **集成**、 下单击**单一登录**。

    ![单一登录](./media/active-directory-saas-shiftplanning-tutorial/IC786620.png "Single Sign-On")

8.  在**单一登录**部分中，执行以下步骤︰

    ![单一登录](./media/active-directory-saas-shiftplanning-tutorial/IC786905.png "Single Sign-On")

    1.  选择**启用 SAML**。
    2.  选择**允许的密码登录**
    3.  在 Azure 的门户中，在**配置单一登录在 ShiftPlanning**对话框页复制的**远程登录 URL**值，，然后将其粘贴到**SAML 颁发者 URL**文本框。
    4.  在 Azure 的门户中，在**配置单一登录在 ShiftPlanning**对话框页面上，**远程注销 URL**值，复制，然后将其粘贴到**远程注销 URL**文本框。
    5.  创建您已下载的证书的**base 64 编码**的文件。  

        >[AZURE.TIP]有关详细信息，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)

    6.  在记事本中打开您的 base 64 编码的证书，将其内容复制到剪贴板，然后将其粘贴到**X.509 证书**文本框
    7.  单击**保存设置**。

9.  在 Azure 广告门户，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-shiftplanning-tutorial/IC786621.png "Configure Single Sign-On")
##配置用户设置
  
为了使 Azure AD 用户能够登录到 ShiftPlanning，他们必须到 ShiftPlanning 配置。  
对于 ShiftPlanning，资源调配是手动任务。

###若要设置用户帐户，请执行以下步骤︰

1.  以管理员身份登录到您的**ShiftPlanning**公司网站。

2.  单击**管理**。

    ![管理员](./media/active-directory-saas-shiftplanning-tutorial/IC786619.png "Admin")

3.  单击**人员**。

    ![竖线](./media/active-directory-saas-shiftplanning-tutorial/IC786623.png "Staff")

4.  在**操作**，单击**添加员工**。

    ![添加员工](./media/active-directory-saas-shiftplanning-tutorial/IC786624.png "Add Employees")

5.  在**添加职员**部分中，执行以下步骤︰

    ![为员工节省](./media/active-directory-saas-shiftplanning-tutorial/IC786625.png "Save Employees")

    1.  键入**名字**、**姓氏**和**电子邮件**有效 AAD 帐户，您希望提供到相关的文本框。
    2.  单击**保存员工**。

>[AZURE.NOTE]您可以使用任何其他 ShiftPlanning 用户帐户创建工具或 Api 来设置 AAD 用户帐户由 ShiftPlanning 提供。

##分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###要将用户分配到 ShiftPlanning 中，执行以下步骤︰

1.  在 Azure 广告门户中，创建一个测试帐户。

2.  在**ShiftPlanning**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-shiftplanning-tutorial/IC786626.png "Assign Users")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-shiftplanning-tutorial/IC767830.png "Yes")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](https://msdn.microsoft.com/library/dn308586)。
测试

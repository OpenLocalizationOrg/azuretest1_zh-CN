---
ms.openlocfilehash: e3d575d45f1d6b973a56358d9e6771c0954778b7
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties pageTitle="教程︰ Azure Active Directory 集成与按需 xMatters |Microsoft Azure" description="了解如何使用 Azure Active Directory xMatters 按需启用单一登录、 自动化资源调配，以及更多。" services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="08/01/2015" ms.author="markvi" />
#教程︰ Azure Active Directory 集成与 xMatters 位置
>[AZURE.TIP]反馈，请单击[此处](http://go.microsoft.com/fwlink/?LinkId=524330)。
  
本教程的目的是显示 Azure 和 xMatters 按需的集成。 在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   XMatters 按需租户
  
学完本教程后, 已分配给 xMatters 需 Azure AD 用户将能够到 xMatters 需公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](https://msdn.microsoft.com/library/dn308586)应用程序的单一登录
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用 xMatters 位置的应用程序的集成
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776788.png "Scenario")

##启用 xMatters 位置的应用程序的集成
  
本部分的目的是概述如何启用 xMatters 位置的应用程序集成。

###若要启用的 xMatters 位置的应用程序集成，请执行以下步骤︰

1.  在 Azure 管理门户中，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录](./media/active-directory-saas-xmatters-ondemand-tutorial/IC700993.png "Active Directory")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-xmatters-ondemand-tutorial/IC700994.png "Applications")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-xmatters-ondemand-tutorial/IC749321.png "Add application")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![从 gallerry 中添加应用程序](./media/active-directory-saas-xmatters-ondemand-tutorial/IC749322.png "Add an application from gallerry")

6.  在**搜索框**中，键入**xMatters 位置**。

    ![应用程序库](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776789.png "Application gallery")

7.  在结果窗格中，选择**XMatters 位置**，，然后单击**完成**添加应用程序。

    ![xMatters 按需](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776790.png "xMatters OnDemand")

##配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 XMatters 按需。

###若要配置单一登录，请执行以下步骤︰

1.  在 Azure 广告门户中， **XMatters 需**应用程序集成在页上，单击**配置单一登录**来打开**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776791.png "Configure single sign-on")

2.  **您想怎样用户能够登录到 XMatters 需**在页上，选择**Microsoft Azure AD 单一登录**，，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776792.png "Configure single sign-on")

3.  在**配置应用程序 URL**页上，在**XMatters 按需登录在 URL**文本框中，键入您的 URL 使用以下模式"*https://\<租户名称\>。XMattersOnDemandapp.com*"，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776793.png "Configure app URL")

4.  在**配置单一登录 XMatters 需在**页上，下载您的证书，单击**证书下载**，然后保存为本地证书文件**c:\\XMatters OnDemand.cer**。

    >[AZURE.IMPORTANT] 您需要转发到 xMatters 的支持团队的证书。 需要上载由 xMatters 的支持团队就可以定版单一登录配置证书。

    ![在配置单一登录](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776794.png "Configure single sign on")

5.  在不同的 web 浏览器窗口中，登录公司网站 XMatters 需以管理员身份。

6.  在顶部的工具栏中，单击**管理**，然后单击左边导航栏中的**公司的详细信息**。

    ![管理员](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776795.png "Admin")

7.  在**SAML 配置**页上，请执行以下步骤︰

    ![SAML 配置](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776796.png "SAML configuration")

    1.  选择**启用 SAML**。
    2.  在 Azure 的门户中，在**配置单一登录 XMatters 需在**对话框页面上，**标识提供程序 ID**值，复制，然后将其粘贴到**标识提供程序 ID**文本框。
    3.  在 Azure 的门户中，在**配置单一登录 XMatters 需在**对话框页面上，**单一登录服务 URL**值，复制，然后将其粘贴到**单个符号在 URL**文本框。
    4.  在 Azure 的门户中，在**配置单一登录 XMatters 需在**对话框页面上，复制**单个 Sign-Out 服务 URL**值，，然后将其粘贴到**单个注销 URL**文本框。
    5.  在公司的详细信息页上，在顶部，单击**保存更改**。
        ![公司的详细信息](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776797.png "Company details")

8.  在 Azure 广告门户，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![在配置单一登录](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776798.png "Configure single sign on")

##配置用户设置
  
为了使 Azure AD 用户能够登录到 XMatters 位置，他们必须到 XMatters 按需调配。  
对于 XMatters 需配备是手动任务。

###若要设置用户帐户，请执行以下步骤︰

1.  登录到**XMatters 需**租户。

2.  单击**用户**选项卡。

3.  单击**添加用户**。

    ![用户](./media/active-directory-saas-xmatters-ondemand-tutorial/IC781048.png "Users")

4.  选择**活动**。

5.  在**添加用户**部分中，执行以下步骤︰

    ![添加用户](./media/active-directory-saas-xmatters-ondemand-tutorial/IC781049.png "Add a User")

    1.  输入**用户 Id**、**名字**、**姓氏**、 您希望提供一个有效的 AAD 帐户的**站点**。
    2.  单击**保存**。

>[AZURE.NOTE] 您可以使用任何其他 XMatters 需用户帐户创建工具或 Api 提供的 XMatters 位置来设置 AAD 用户帐户。

##分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###若要将用户分配到 XMatters 位置，请执行以下步骤︰

1.  在 Azure 广告门户中，创建一个测试帐户。

2.  **XMatters 按需**应用程序集成在页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776799.png "Assign users")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-xmatters-ondemand-tutorial/IC767830.png "Yes")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](https://msdn.microsoft.com/library/dn308586)。
测试

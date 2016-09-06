---
ms.openlocfilehash: 073e6533f2b0a7cc1848a8467ca4e848fc602866
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties pageTitle="教程︰ Azure Active Directory 集成与 Samanage |Microsoft Azure" description="了解如何使用 Samanage Azure Active Directory 以启用单一登录、 自动化资源调配，和更多。" services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="08/01/2015" ms.author="markvi" />
#教程︰ 与 Samanage 的 Azure Active Directory 集成
>[AZURE.TIP]反馈，请单击[此处](http://go.microsoft.com/fwlink/?LinkId=522516)。
  
本教程的目的是显示 Azure 和 Samanage 的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   Samanage 租户
  
后学完本教程，您已分配给 Samanage Azure AD 用户将能够到 Samanage 公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](https://msdn.microsoft.com/library/dn308586)应用程序的单一登录
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 Samanage
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-samanage-tutorial/IC771705.png "Scenario")
##启用应用程序集成为 Samanage
  
本部分的目的是概述如何启用应用程序集成为 Samanage。

###若要启用应用程序集成为 Samanage，请执行以下步骤︰

1.  在 Azure 管理门户中，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录](./media/active-directory-saas-samanage-tutorial/IC700993.png "Active Directory")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-samanage-tutorial/IC700994.png "Applications")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-samanage-tutorial/IC749321.png "Add application")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![从 gallerry 中添加应用程序](./media/active-directory-saas-samanage-tutorial/IC749322.png "Add an application from gallerry")

6.  在**搜索框**中，键入**Samanage**。

    ![应用程序库](./media/active-directory-saas-samanage-tutorial/IC771707.png "Application gallery")

7.  在结果窗格中，选择**Samanage**，，然后单击**完成**添加应用程序。

    ![Samanage](./media/active-directory-saas-samanage-tutorial/IC771708.png "Samanage")
##配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 Samanage。  
作为此过程的一部分，您将需要创建一个 base 64 编码的证书文件。  
如果您不熟悉此过程，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)。

###若要配置单一登录，请执行以下步骤︰

1.  在 Azure 广告门户中， **Samanage**应用程序集成在页上，单击**配置单一登录**来打开**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-samanage-tutorial/IC771709.png "Configure single sign-on")

2.  在**您想怎样来登录到 Samanage 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![Microsoft Azure 广告单登录](./media/active-directory-saas-samanage-tutorial/IC771710.png "Microsoft Azure AD Single Sign-On")

3.  **配置应用程序 URL**页面上的**Samanage 号中 URL**文本框中，键入您的 URL 使用以下模式"*https://\<租户名称\>。 samanage.com*"，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-samanage-tutorial/IC771711.png "Configure App URL")

4.  在**配置单一登录 Samanage 在**页上，单击**下载证书**，然后将保存在您的计算机上的证书文件。

    ![配置单一登录](./media/active-directory-saas-samanage-tutorial/IC777613.png "Configure Single Sign-On")

5.  在不同的 web 浏览器窗口中，以管理员身份登录到 Samanage 公司网站。

6.  单击**仪表板**，在左侧的导航窗格中选择**设置**。

    ![仪表板](./media/active-directory-saas-samanage-tutorial/IC771712.png "Dashboard")

7.  **单一登录**，请单击。

    ![单一登录](./media/active-directory-saas-samanage-tutorial/IC771713.png "Single Sign-On")

8.  在**登录使用 SAML**对话框页上，执行下面的步骤，然后单击**保存更改**:

    1.  单击**启用单一登录使用 SAML**。
        ![使用 SAML 的登录](./media/active-directory-saas-samanage-tutorial/IC771719.png "Login using SAML")
    2.  在 Azure 的门户中，在**配置单一登录在 Samanage**对话框页面上，**标识提供程序 ID**值，复制，然后将其粘贴**标识提供程序 URL**文本框。
        ![配置单一登录](./media/active-directory-saas-samanage-tutorial/IC771720.png "Configure Single Sign-On")
    3.  在 Azure 的门户中，在**配置单一登录在 Samanage**对话框页面上的**远程登录 URL**值，复制，然后将其粘贴到**登录 URL**文本框。
    4.  在 Azure 的门户中，在**配置单一登录在 Samanage**对话框页复制**远程注销 URL**值，，然后将其粘贴到**注销 URL**文本框。
    5.  创建您已下载的证书的**base 64 编码**的文件。  

        >[AZURE.TIP] 有关详细信息，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)

    6.  在记事本中打开您的 base 64 编码的证书，将其内容复制到剪贴板，然后将其粘贴到**X.509 证书**文本框
    7.  单击**创建用户如果它们不存在于 Samanage**。
        ![Update](./media/active-directory-saas-samanage-tutorial/IC771722.png "Update")
    8.  单击**更新**。

9.  在 Azure 广告门户，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-samanage-tutorial/IC771723.png "Configure Single Sign-On")
##配置用户设置
  
为了使 Azure AD 用户能够登录到 Samanage，他们必须到 Samanage 配置。  
对于 Samanage，资源调配是手动任务。

###若要设置用户帐户，请执行以下步骤︰

1.  登录到**Samanage**租户。

2.  转到**仪表板\>安装程序**。

    ![安装](./media/active-directory-saas-samanage-tutorial/IC771724.png "Setup")

3.  单击**用户**选项卡

    ![用户](./media/active-directory-saas-samanage-tutorial/IC771725.png "Users")

4.  单击**新用户**。

    ![新用户](./media/active-directory-saas-samanage-tutorial/IC771726.png "New User")

5.  键入**电子邮件地址**和 Azure 的广告帐户所需设置，然后单击**创建用户**的**名称**。

    >[AZURE.NOTE]AAD 帐户持有者将收到一封电子邮件，点击链接以确认他们的帐户，再将变为活动状态。

    ![创建用户](./media/active-directory-saas-samanage-tutorial/IC771727.png "Creat User")

>[AZURE.NOTE]您可以使用任何其他 Samanage 用户帐户创建工具或 Api 来设置 AAD 用户帐户由 Samanage 提供。

##分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###要将用户分配到 Samanage 中，执行以下步骤︰

1.  在 Azure 广告门户中，创建一个测试帐户。

2.  在**Samanage**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-samanage-tutorial/IC771728.png "Assign users")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-samanage-tutorial/IC767830.png "Yes")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](https://msdn.microsoft.com/library/dn308586)。
测试

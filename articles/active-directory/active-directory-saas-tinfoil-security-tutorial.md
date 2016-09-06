---
ms.openlocfilehash: dc5379980d63275ec25728c3b7780b28eaea010f
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties pageTitle="教程︰ Azure Active Directory 集成锡箔安全性 |Microsoft Azure" description="了解如何使用 Azure Active Directory 锡箔安全启用单一登录、 自动化资源调配，和更多。" services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="08/01/2015" ms.author="markvi" />
#教程︰ Azure Active Directory 集成用锡箔安全
>[AZURE.TIP]反馈，请单击[此处](http://go.microsoft.com/fwlink/?LinkId=613055)。
  
本教程的目的是显示的 Azure 和锡箔安全集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   锡箔安全-启用单一登录的订阅
  
后学完本教程，您已分配给锡箔安全 Azure AD 用户将能够到锡箔安全公司网站 （标识提供程序启动登录），或使用[简介访问权限面板](https://msdn.microsoft.com/library/dn308586)应用程序的单一登录
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用锡箔安全的应用程序集成
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![配置单一登录](./media/active-directory-saas-tinfoil-security-tutorial/IC798965.png "Configure Single Sign-On")

##启用锡箔安全的应用程序集成
  
本部分的目的是概述如何启用锡箔安全的应用程序集成。

###若要启用锡箔安全的应用程序集成，请执行以下步骤︰

1.  在 Azure 管理门户中，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录](./media/active-directory-saas-tinfoil-security-tutorial/IC700993.png "Active Directory")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-tinfoil-security-tutorial/IC700994.png "Applications")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-tinfoil-security-tutorial/IC749321.png "Add application")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![从 gallerry 中添加应用程序](./media/active-directory-saas-tinfoil-security-tutorial/IC749322.png "Add an application from gallerry")

6.  在**搜索框**中，键入**锡箔安全**。

    ![应用程序库](./media/active-directory-saas-tinfoil-security-tutorial/IC798966.png "Application Gallery")

7.  在结果窗格中，选择**锡箔安全**，然后单击**完成**添加应用程序。

    ![锡箔安全](./media/active-directory-saas-tinfoil-security-tutorial/IC802771.png "Tinfoil Security")

##配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到锡箔安全。  
配置单一登录的锡箔安全要求您检索证书的指纹值。  
如果您不熟悉此过程，请参阅[如何检索证书的指纹值](http://youtu.be/YKQF266SAxI)。

###若要配置单一登录，请执行以下步骤︰

1.  在 Azure 广告门户中，**锡箔安全**应用程序集成在页上，单击**配置单一登录**来打开**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-tinfoil-security-tutorial/IC798967.png "Configure Single Sign-On")

2.  在**您想怎样用户能够登录到锡箔安全**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-tinfoil-security-tutorial/IC798968.png "Configure Single Sign-On")

3.  在**配置应用程序 URL**页上，在**锡箔安全回复 URL**文本框中，键入您锡箔安全断言使用者服务 (ACS) 的 URL (例如:"*https://www.tinfoilsecurity.com/saml/consume*"，然后单击**下一步**。

    >[AZURE.NOTE] 您应该能够从锡箔安全元数据 (https://www.tinfoilsecurity.com/saml/metadata) 获得 ACS URL。

    ![配置应用程序的 URL](./media/active-directory-saas-tinfoil-security-tutorial/IC798969.png "Configure App URL")

4.  在**配置单一登录在锡箔安全**页上，下载您的证书，单击**证书下载**，然后保存为本地证书文件**c:\\锡箔 Security.cer**。

    ![配置单一登录](./media/active-directory-saas-tinfoil-security-tutorial/IC798970.png "Configure Single Sign-On")

5.  在不同的 web 浏览器窗口中，以管理员身份登录到锡箔安全公司网站。

6.  在顶部工具栏上，单击**我的帐户**。

    ![仪表板](./media/active-directory-saas-tinfoil-security-tutorial/IC798971.png "Dashboard")

7.  单击**安全**。

    ![安全性](./media/active-directory-saas-tinfoil-security-tutorial/IC798972.png "Security")

8.  在**单一登录**配置页上，请执行以下步骤︰

    ![单一登录](./media/active-directory-saas-tinfoil-security-tutorial/IC798973.png "Single Sign-On")

    1.  选择**启用 SAML**。
    2.  单击**手动配置**。
    3.  在 Azure 的门户中，在**配置单一登录锡箔安全性**对话框页**SAML SSO URL**值，复制，然后将其粘贴到**SAML 张贴 URL**文本框。
    4.  从导出的证书，复制的**指纹**值，然后将其粘贴到文本框中**SAML 证书指纹**。  

        >[AZURE.TIP] 有关详细信息，请参阅[如何检索证书的指纹值](http://youtu.be/YKQF266SAxI)

    5.  复制**您的帐户 ID**。
    6.  单击**保存**。

9.  在 Azure 广告门户，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-tinfoil-security-tutorial/IC798974.png "Configure Single Sign-On")

10. 在顶部菜单中，单击**属性**以打开**SAML 令牌的属性**对话框。

    ![属性](./media/active-directory-saas-tinfoil-security-tutorial/IC795920.png "Attributes")

11. 若要添加所需的属性映射，请执行以下步骤︰

    ![属性](./media/active-directory-saas-tinfoil-security-tutorial/IC798975.png "Attributes")

    1.  单击**添加用户属性**。
    2.  在**属性名称**文本框中，键入**accountid**。
    3.  在**属性值**文本框中，粘贴在前面的部分中复制的帐户 ID 值。
    4.  单击**完成**。

12. 单击**应用更改**。

##配置用户设置
  
为了使 Azure AD 用户能够登录到锡箔安全，他们必须到锡箔安全配置。  
在锡箔安全资源调配是手动任务。

###若要获取用户设置，请执行以下步骤︰

1.  如果用户属于企业帐户，您需要联系锡箔安全支持团队以获取创建的用户帐户。

2.  如果用户是正则的锡箔安全 SaaS 用户，用户可以对任何用户的站点添加合作者。 这将触发一个进程发送到指定的电子邮件的邀请，以创建新的锡箔安全用户帐户。

>[AZURE.NOTE] 您可以使用任何其他锡箔安全用户帐户创建工具或 Api 提供的锡箔安全规定 AAD 用户帐户。

##分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###若要将用户分配到锡箔安全性，请执行以下步骤︰

1.  在 Azure 广告门户中，创建一个测试帐户。

2.  在**锡箔安全**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-tinfoil-security-tutorial/IC798976.png "Assign Users")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-tinfoil-security-tutorial/IC767830.png "Yes")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](https://msdn.microsoft.com/library/dn308586)。
测试

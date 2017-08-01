---
ms.openlocfilehash: 94fe907126ba671de6a3d884737c45c0f1fa38c6
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties pageTitle="教程︰ Azure Active Directory 集成与 NetDocuments |Microsoft Azure" description="了解如何使用 NetDocuments Azure Active Directory 以启用单一登录、 自动化资源调配，和更多。" services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="08/01/2015" ms.author="markvi" />
#教程︰ 与 NetDocuments 的 Azure Active Directory 集成
>[AZURE.TIP]反馈，请单击[此处](http://go.microsoft.com/fwlink/?LinkId=529696)。
  
本教程的目的是显示 Azure 和 NetDocuments 的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   NetDocuments 租户
  
后学完本教程，您已分配给 NetDocuments Azure AD 用户将能够到 NetDocuments 公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](https://msdn.microsoft.com/library/dn308586)应用程序的单一登录
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 NetDocuments
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-netdocuments-tutorial/IC795040.png "Scenario")
##启用应用程序集成为 NetDocuments
  
本部分的目的是概述如何启用应用程序集成为 NetDocuments。

###若要启用应用程序集成为 NetDocuments，请执行以下步骤︰

1.  在 Azure 管理门户中，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录](./media/active-directory-saas-netdocuments-tutorial/IC700993.png "Active Directory")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-netdocuments-tutorial/IC700994.png "Applications")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-netdocuments-tutorial/IC749321.png "Add application")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![从 gallerry 中添加应用程序](./media/active-directory-saas-netdocuments-tutorial/IC749322.png "Add an application from gallerry")

6.  在**搜索框**中，键入**NetDocuments**。

    ![应用程序库](./media/active-directory-saas-netdocuments-tutorial/IC795041.png "Application Gallery")

7.  在结果窗格中，选择**NetDocuments**，，然后单击**完成**添加应用程序。

    ![NetDocuments](./media/active-directory-saas-netdocuments-tutorial/IC795042.png "NetDocuments")
##配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 NetDocuments。  
NetDocuments 为单一登录配置需要检索证书的指纹值。  
如果您不熟悉此过程，请参阅[如何检索证书的指纹值](http://youtu.be/YKQF266SAxI)。

###若要配置单一登录，请执行以下步骤︰

1.  在 Azure 广告门户中， **NetDocuments**应用程序集成在页上，单击**配置单一登录**来打开**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-netdocuments-tutorial/IC795043.png "Configure Single Sign-On")

2.  在**您想怎样来登录到 NetDocuments 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-netdocuments-tutorial/IC795044.png "Configure Single Sign-On")

3.  在**配置应用程序 URL**页上，请执行以下步骤︰

    ![配置应用程序的 URL](./media/active-directory-saas-netdocuments-tutorial/IC795045.png "Configure App URL")

    1.  在**符号上 URL**文本框中，键入用户用于登录到您的 NetDocuments 应用程序 URL (例如:"*https://vault.netvoyage.com/neWeb2/docCent.aspx?whr=CA-JI1BG3H1*")。
    2.  在**NetDocuments 回复 URL**文本框中，键入您键入相同的值的**符号在 URL**文本框。  

        >[AZURE.NOTE]您可以找到正确的值结尾的**联合标识**对话框 （请参见步骤 9 的屏幕快照）。

    3.  单击**下一步**

4.  在**配置单一登录 NetDocuments 在**页上，下载您的证书，单击**下载证书**，然后将保存在您的计算机的本地证书文件。

    ![配置单一登录](./media/active-directory-saas-netdocuments-tutorial/IC795046.png "Configure Single Sign-On")

5.  在不同的 web 浏览器窗口中，以管理员身份登录到 NetDocuments 公司网站。

6.  转到**管理**。

7.  单击**添加和删除用户和组**。

    ![存储库](./media/active-directory-saas-netdocuments-tutorial/IC795047.png "Repository")

8.  单击**配置高级身份验证选项**。

    ![配置高级身份验证选项](./media/active-directory-saas-netdocuments-tutorial/IC795048.png "Configure advanced authentication options")

9.  在**联合标识**对话框中，请执行以下步骤︰

    ![联合的 Identitty](./media/active-directory-saas-netdocuments-tutorial/IC795049.png "Federated Identitty")

    1.  作为**联盟的标识服务器类型**，选择**Active Directory 联合身份验证服务**。
    2.  单击**选择文件**上载下载元数据文件。
    3.  单击**确定**。

10. 在 Azure 广告门户，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-netdocuments-tutorial/IC795050.png "Configure Single Sign-On")
##配置用户设置
  
为了使 Azure AD 用户能够登录到 NetDocuments，他们必须到 NetDocuments 配置。  
对于 NetDocuments，资源调配是手动任务。

###若要配置用户设置，请执行以下步骤︰

1.  以管理员身份唱歌到**NetDocuments**公司网站。

2.  在顶部菜单中，单击**管理**。

    ![管理员](./media/active-directory-saas-netdocuments-tutorial/IC795051.png "Admin")

3.  单击**添加和删除用户和组**。

    ![存储库](./media/active-directory-saas-netdocuments-tutorial/IC795047.png "Repository")

4.  在**电子邮件地址**文本框中，键入您要提供，并单击**添加用户**的有效 Azure Active Directory 帐户的电子邮件地址。

    ![电子邮件地址](./media/active-directory-saas-netdocuments-tutorial/IC795053.png "Email Address")

    >[AZURE.NOTE]Azure Active Directory 帐户持有人将获得包括确认该帐户之前将变为活动状态的链接的电子邮件。

>[AZURE.NOTE]您可以使用任何其他 NetDocuments 用户帐户创建工具或 Api 来设置 AAD 用户帐户由 NetDocuments 提供。

##分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###要将用户分配到 NetDocuments 中，执行以下步骤︰

1.  在 Azure 广告门户中，创建一个测试帐户。

2.  在**NetDocuments**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-netdocuments-tutorial/IC795054.png "Assign Users")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-netdocuments-tutorial/IC767830.png "Yes")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](https://msdn.microsoft.com/library/dn308586)。
测试
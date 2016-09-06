---
ms.openlocfilehash: 63ae400b44340045750aea6cbbc3c5fcd1970737
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties pageTitle="教程︰ Azure Active Directory 集成与 Projectplace |Microsoft Azure" description="了解如何使用 Projectplace Azure Active Directory 以启用单一登录、 自动化资源调配，和更多。" services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="08/01/2015" ms.author="markvi" />
#教程︰ 与 Projectplace 的 Azure Active Directory 集成
>[AZURE.TIP]反馈，请单击[此处](http://go.microsoft.com/fwlink/?LinkId=529080)。
  
本教程的目的是显示 Azure 和 Projectplace 的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   Projectplace-启用单一登录的订阅
  
后学完本教程，您已分配给 Projectplace Azure AD 用户将能够到 Projectplace 公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](https://msdn.microsoft.com/library/dn308586)应用程序的单一登录
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 Projectplace
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-projectplace-tutorial/IC790217.png "Scenario")
##启用应用程序集成为 Projectplace
  
本部分的目的是概述如何启用应用程序集成为 Projectplace。

###若要启用应用程序集成为 Projectplace，请执行以下步骤︰

1.  在 Azure 管理门户中，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录](./media/active-directory-saas-projectplace-tutorial/IC700993.png "Active Directory")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-projectplace-tutorial/IC700994.png "Applications")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-projectplace-tutorial/IC749321.png "Add application")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![从 gallerry 中添加应用程序](./media/active-directory-saas-projectplace-tutorial/IC749322.png "Add an application from gallerry")

6.  在**搜索框**中，键入**Projectplace**。

    ![应用程序库](./media/active-directory-saas-projectplace-tutorial/IC790218.png "Application Gallery")

7.  在结果窗格中，选择**Projectplace**，，然后单击**完成**添加应用程序。

    ![ProjectPlace](./media/active-directory-saas-projectplace-tutorial/IC790219.png "ProjectPlace")
##配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 Projectplace。

###若要配置单一登录，请执行以下步骤︰

1.  在 Azure 广告门户中， **Projectplace**应用程序集成在页上，单击**配置单一登录**来打开**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-projectplace-tutorial/IC790220.png "Configure Single SignOn")

2.  在**您想怎样来登录到 Projectplace 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-projectplace-tutorial/IC790221.png "Configure Single SignOn")

3.  在**配置应用程序 URL**页面上的**Projectplace 号上的 URL**文本框中，键入 ProjectPlace 租户 URL (例如:"*http://company.projectplace.com*")，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-projectplace-tutorial/IC790222.png "Configure App URL")

4.  在**配置单一登录 Projectplace 在**页上，单击**下载元数据**，然后将保存在您的计算机上的元数据文件。

    ![配置单一登录](./media/active-directory-saas-projectplace-tutorial/IC790223.png "Configure Single SignOn")

5.  将元数据文件发送给 Projectplace 的支持团队。

    >[AZURE.NOTE] 单一登录配置已通过 Projectplace 的支持团队来执行。 一旦配置完成后，您将得到通知。

6.  在 Azure 广告门户，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-projectplace-tutorial/IC790227.png "Configure Single SignOn")
##配置用户设置
  
为了使 Azure AD 用户能够登录到 Projectplace，他们必须到 Projectplace 配置。  
对于 Projectplace，资源调配是手动任务。

###若要设置用户帐户，请执行以下步骤︰

1.  以管理员身份登录到您的**Projectplace**公司网站。

2.  转**的人**，到，然后单击**成员**。

    ![人脉](./media/active-directory-saas-projectplace-tutorial/IC790228.png "People")

3.  单击**添加成员**。

    ![添加成员](./media/active-directory-saas-projectplace-tutorial/IC790232.png "Add Members")

4.  在**添加成员**部分中，执行以下步骤︰

    ![新成员](./media/active-directory-saas-projectplace-tutorial/IC790233.png "New Members")

    1.  在**新成员**文本框中，键入您要到相关文本框提供有效的 AAD 帐户的电子邮件地址。
    2.  单击**发送**

        >[AZURE.NOTE] 其中包括确认该帐户之前将变为活动状态的链接的电子邮件被发送到 Azure Active Directory 帐户持有者。
    
>[AZURE.NOTE]您可以使用任何其他 Projectplace 用户帐户创建工具或 Api 来设置 AAD 用户帐户由 Projectplace 提供。

##分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###要将用户分配到 Projectplace 中，执行以下步骤︰

1.  在 Azure 广告门户中，创建一个测试帐户。

2.  在**Projectplace**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-projectplace-tutorial/IC790234.png "Assign Users")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-projectplace-tutorial/IC767830.png "Yes")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](https://msdn.microsoft.com/library/dn308586)。
测试

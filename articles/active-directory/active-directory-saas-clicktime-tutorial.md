---
ms.openlocfilehash: e60d17c05e3d96a524ed1ebbe1170affb1dc9055
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties pageTitle="教程︰ Azure Active Directory 集成与 ClickTime |Microsoft Azure" description="了解如何使用 ClickTime Azure Active Directory 以启用单一登录、 自动化资源调配，和更多。" services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="08/01/2015" ms.author="markvi" />
#教程︰ 与 ClickTime 的 Azure Active Directory 集成
>[AZURE.TIP]反馈，请单击[此处](http://go.microsoft.com/fwlink/?LinkId=%20524782)。

本教程的目的是显示 Azure 和 ClickTime 的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   ClickTime 租户

后学完本教程，您已分配给 ClickTime Azure AD 用户将能够到 ClickTime 公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](https://msdn.microsoft.com/library/dn308586)应用程序的单一登录

在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 ClickTime
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-clicktime-tutorial/IC777274.png "Scenario")
##启用应用程序集成为 ClickTime

本部分的目的是概述如何启用应用程序集成为 ClickTime。

###若要启用应用程序集成为 ClickTime，请执行以下步骤︰

1.  在 Azure 管理门户中，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录](./media/active-directory-saas-clicktime-tutorial/IC700993.png "Active Directory")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-clicktime-tutorial/IC700994.png "Applications")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-clicktime-tutorial/IC749321.png "Add application")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![从 gallerry 中添加应用程序](./media/active-directory-saas-clicktime-tutorial/IC749322.png "Add an application from gallerry")

6.  在**搜索框**中，键入**ClickTime**。

    ![应用程序库](./media/active-directory-saas-clicktime-tutorial/IC777275.png "Application gallery")

7.  在结果窗格中，选择**ClickTime**，，然后单击**完成**添加应用程序。

    ![ClickTime](./media/active-directory-saas-clicktime-tutorial/IC777276.png "ClickTime")
##配置单一登录

本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 ClickTime。  
作为此过程的一部分，您都需要将 base 64 编码的证书上传到 ClickTime 租户。  
如果您不熟悉此过程，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)。

>[AZURE.IMPORTANT] 为了能够在 ClickTime 租户上配置单一登录，您需要首先联系 ClickTime 技术支持以获取启用此功能。

###若要配置单一登录，请执行以下步骤︰

1.  在 Azure 广告门户中， **ClickTime**应用程序集成在页上，单击**配置单一登录**来打开**配置单一登录**对话框。

    ![启用单一登录](./media/active-directory-saas-clicktime-tutorial/IC777277.png "Enable single sign-on")

2.  在**您想怎样来登录到 ClickTime 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-clicktime-tutorial/IC777278.png "Configure single sign-on")

3.  在**配置单一登录 ClickTime 在**页上，下载您的证书，单击**下载证书**，然后将保存在您的计算机上的证书文件。

    ![配置单一登录](./media/active-directory-saas-clicktime-tutorial/IC777279.png "Configure single sign-on")

4.  在不同的 web 浏览器窗口中，以管理员身份登录到 ClickTime 公司网站。

5.  在顶部的工具栏中，单击**首选项**，然后单击**安全设置**。

6.  在**单一登录首选项**配置部分中，执行以下步骤︰

    ![安全设置](./media/active-directory-saas-clicktime-tutorial/IC777280.png "Security Settings")

    1.  选择**允许**使用**OneLogin**的单一登录 (SSO) 登录。
    2.  在 Azure 的门户中，在**配置单一登录在 ClickTime**对话框页上，将**单一登录服务 URL**值，复制，然后将其粘贴到文本框中**的身份提供方终结点**。
    3.  创建您已下载的证书的**base 64 编码**的文件。  

        >[AZURE.TIP] 有关详细信息，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)

    4.  在**记事本**中打开 base 64 编码的证书，复制的内容，然后将其粘贴到文本框中**的 X.509 证书**。
    5.  单击**保存**。

7.  在 Azure 广告门户，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-clicktime-tutorial/IC777281.png "Configure single sign-on")
##配置用户设置

为了使 Azure AD 用户能够登录到 ClickTime，他们必须到 ClickTime 配置。  
对于 ClickTime，资源调配是手动任务。

###若要设置用户帐户，请执行以下步骤︰

1.  登录到**ClickTime**租户。

2.  在顶部的工具栏中，单击**公司**，，然后单击**人员**。

    ![人脉](./media/active-directory-saas-clicktime-tutorial/IC777282.png "People")

3.  单击**添加用户**。

    ![添加人员](./media/active-directory-saas-clicktime-tutorial/IC777283.png "Add Person")

4.  在新人部分中，执行以下步骤︰

    ![人脉](./media/active-directory-saas-clicktime-tutorial/IC777284.png "People")

    1.  在**电子邮件地址**文本框中，键入您的 Azure 的广告帐户的电子邮件地址。
    2.  在**全名**文本框中，键入 Azure 的广告帐户的名称。  

        >[AZURE.NOTE] 如果您愿意，您可以设置新的 person 对象的其他属性。

    3.  单击**保存**。

>[AZURE.NOTE] 您可以使用任何其他 ClickTime 用户帐户创建工具或 Api 来设置 AAD 用户帐户由 ClickTime 提供。

##分配用户

若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###要将用户分配到 ClickTime 中，执行以下步骤︰

1.  在 Azure 广告门户中，创建一个测试帐户。

2.  在**ClickTime**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-clicktime-tutorial/IC777285.png "Assign users")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-clicktime-tutorial/IC767830.png "Yes")

如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](https://msdn.microsoft.com/library/dn308586)。

测试

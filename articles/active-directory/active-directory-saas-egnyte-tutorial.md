---
ms.openlocfilehash: 58f6e010adb8c4026cea8f493dddbea35601c318
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties pageTitle="教程︰ Azure Active Directory 集成与 Egnyte |Microsoft Azure" description="了解如何使用 Azure Active Directory Egnyte 启用单一登录、 自动化资源调配，和更多。" services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="08/01/2015" ms.author="markvi" />
#教程︰ 与 Egnyte 的 Azure Active Directory 集成
>[AZURE.TIP]反馈，请单击[此处](http://go.microsoft.com/fwlink/?LinkId=528188)。
  
本教程的目的是显示 Azure 和 Egnyte 的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   Egnyte-启用单一登录的订阅
  
后学完本教程，您已分配给 Egnyte Azure AD 用户将能够到 Egnyte 公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](https://msdn.microsoft.com/library/dn308586)应用程序的单一登录
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 Egnyte
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-egnyte-tutorial/IC787812.png "Scenario")
##启用应用程序集成为 Egnyte
  
本部分的目的是概述如何启用 Egnyte 用于应用程序集成。

###若要启用 Egnyte 用于应用程序集成，请执行以下步骤︰

1.  在 Azure 管理门户中，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录](./media/active-directory-saas-egnyte-tutorial/IC700993.png "Active Directory")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-egnyte-tutorial/IC700994.png "Applications")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-egnyte-tutorial/IC749321.png "Add application")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![从 gallerry 中添加应用程序](./media/active-directory-saas-egnyte-tutorial/IC749322.png "Add an application from gallerry")

6.  在**搜索框**中，键入**egnyte**。

    ![应用程序库](./media/active-directory-saas-egnyte-tutorial/IC787813.png "Application Gallery")

7.  在结果窗格中，选择**Egnyte**，，然后单击**完成**添加应用程序。

    ![Egnyte](./media/active-directory-saas-egnyte-tutorial/IC787814.png "Egnyte")
##配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 Egnyte。  
作为此过程的一部分，您将需要创建一个 base 64 编码的证书文件。  
如果您不熟悉此过程，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)。

###若要配置单一登录，请执行以下步骤︰

1.  在 Azure 广告门户， **Egnyte**应用程序集成在页上，单击**配置单一登录**来打开**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-egnyte-tutorial/IC787815.png "Configure Single Sign-On")

2.  在**您想怎样来登录到 Egnyte 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-egnyte-tutorial/IC787816.png "Configure Single Sign-On")

3.  在**配置应用程序 URL**页上，在**Egnyte 号在 URL**文本框中，键入您的 URL 使用以下模式"*https://company.egnyte.com*"，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-egnyte-tutorial/IC787817.png "Configure App URL")

4.  在**配置单一登录 Egnyte 在**页上，单击**下载证书**，然后将保存在您的计算机上的证书文件。

    ![配置单一登录](./media/active-directory-saas-egnyte-tutorial/IC787818.png "Configure Single Sign-On")

5.  在不同的 web 浏览器窗口中，以管理员身份登录到 Egnyte 公司网站。

6.  单击**设置**。

    ![Settings](./media/active-directory-saas-egnyte-tutorial/IC787819.png "Settings")

7.  在菜单上，单击**设置**。

    ![Settings](./media/active-directory-saas-egnyte-tutorial/IC787820.png "Settings")

8.  单击**配置**选项卡，然后单击**安全**。

    ![安全性](./media/active-directory-saas-egnyte-tutorial/IC787821.png "Security")

9.  在**单一登录进行身份验证**部分中，执行以下步骤︰

    ![单一登录身份验证](./media/active-directory-saas-egnyte-tutorial/IC787822.png "Single Sign On Authentication")

    1.  与**单一登录进行身份验证**，请选择**SAML 2.0**。
    2.  为**身份标识提供程序**，请选择**AzureAD**。
    3.  在 Azure 的门户中，在**配置单一登录 Egnyte 在**对话框页面上，复制的**远程登录 URL**值，，然后将其粘贴到**标识提供程序的登录 URL**文本框。
    4.  在 Azure 的门户中，在**配置单一登录 Egnyte 在**对话框页面上，**实体 ID**值，复制，然后将其粘贴到**标识提供程序实体 ID**文本框。
    5.  创建您已下载的证书的**base 64 编码**的文件。  

        >[AZURE.TIP]有关详细信息，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)

    6.  在记事本中打开您的 base 64 编码的证书，将其内容复制到剪贴板，然后将其粘贴到**标识提供程序证书**文本框。
    7.  作为**缺省用户映射**，请选择**电子邮件地址**。
    8.  作为**使用特定于域的颁发者值**，选择**禁用**。
    9.  单击**保存**。

10. 在 Azure 广告门户，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-egnyte-tutorial/IC787823.png "Configure Single Sign-On")
##配置用户设置
  
为了使 Azure AD 用户能够登录到 Egnyte，他们必须到 Egnyte 提供。  
Egnyte，在资源调配是手动任务。

###若要设置用户帐户，请执行以下步骤︰

1.  以管理员身份登录到您**Egnyte** Egnyte 的公司网站。

2.  转到**设置\>的用户和组**。

3.  单击**添加新用户**，然后选择您想要添加的用户的类型。

    ![用户](./media/active-directory-saas-egnyte-tutorial/IC787824.png "Users")

4.  在**新的标准用户**部分中，执行以下步骤︰

    ![新的标准用户](./media/active-directory-saas-egnyte-tutorial/IC787825.png "New Standard User")

    1.  键入**的电子邮件**、**用户名**和其他您希望提供一个有效的 Azure Active Directory 帐户的详细信息。
    2.  单击**保存**。

    >[AZURE.NOTE] Azure Active Directory 帐户持有者会收到通知电子邮件。

>[AZURE.NOTE] 您可以使用任何其他 Egnyte 用户帐户创建工具或 Api 来设置 AAD 用户帐户由 Egnyte 提供。

##分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###若要将用户分配到 Egnyte，请执行以下步骤︰

1.  在 Azure 广告门户中，创建一个测试帐户。

2.  **Egnyte**应用程序集成在页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-egnyte-tutorial/IC787826.png "Assign Users")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-egnyte-tutorial/IC767830.png "Yes")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](https://msdn.microsoft.com/library/dn308586)。
测试

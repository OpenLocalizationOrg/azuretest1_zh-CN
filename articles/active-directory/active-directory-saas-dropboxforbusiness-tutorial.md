---
ms.openlocfilehash: 07ddf36884fcd14bf064f218ee0609901ab01254
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties pageTitle="教程︰ Azure Active Directory 集成业务的收存箱与 |Microsoft Azure" description="了解如何使用业务的 Azure Active Directory 收存箱来启用单一登录、 自动化资源调配，和更多。" services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="08/01/2015" ms.author="markvi" />
#教程︰ 与业务的收存箱的 Azure Active Directory 集成
>[AZURE.TIP]反馈，请单击[此处](http://go.microsoft.com/fwlink/?LinkId=522415)。
  
本教程的目的是显示 Azure 和收存箱用于业务集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   在业务的收存箱测试租户
  
在完成本教程之后, 已归入收存箱的业务将能够登录单个应用程序在您的业务的收存箱到 Azure AD 用户公司网站 （服务提供程序初始化登录），或使用[接入面板简介](active-directory-saas-access-panel-introduction.md)
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用用于收存箱为业务应用程序集成
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769508.png "Scenario")



##启用用于收存箱为业务应用程序集成
  
本部分的目的是概述如何启用的收存箱的业务应用程序集成。

###若要启用的收存箱的业务应用程序集成，请执行以下步骤︰

1.  在 Azure 管理门户中，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录](./media/active-directory-saas-dropboxforbusiness-tutorial/IC700993.png "Active Directory")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-dropboxforbusiness-tutorial/IC700994.png "Applications")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-dropboxforbusiness-tutorial/IC749321.png "Add application")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![从 gallerry 中添加应用程序](./media/active-directory-saas-dropboxforbusiness-tutorial/IC749322.png "Add an application from gallerry")

6.  在**搜索框**中，键入**为企业收存箱**。

    ![应用程序库](./media/active-directory-saas-dropboxforbusiness-tutorial/IC701010.png "Application gallery")

7.  在结果窗格中，选择**收存箱的业务**，，然后单击**完成**添加应用程序。

    ![企业的收存箱](./media/active-directory-saas-dropboxforbusiness-tutorial/IC701011.png "Dropbox for Business")
##配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到业务的收存箱。

作为此过程的一部分，您都需要将 base 64 编码的证书上传到您的商业租户的收存箱。 如果您不熟悉此过程，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)。

###若要配置单一登录，请执行以下步骤︰

1.  在 Azure 广告门户中，**收存箱为业务**应用程序集成在页上，单击**配置单一登录**来打开**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-dropboxforbusiness-tutorial/IC749323.png "Configure single sign-on")

2.  在**您想怎样来登录到业务的收存箱的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-dropboxforbusiness-tutorial/IC749327.png "Configure single sign-on")

3.  在**配置应用程序 URL**页上，请执行以下步骤︰

     3.1。 登录到您的收存箱的商业租户。 <br><br>  ![配置单一登录](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769509.png "Configure single sign-on")

     3.2。 在左侧导航窗格中，单击**管理控制台**。 <br><br>  ![配置单一登录](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769510.png "Configure single sign-on")

     3.3。 在**管理控制台**上，单击在左侧的导航窗格中的**身份验证**。 <br><br>  ![配置单一登录](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769511.png "Configure single sign-on")

     3.4。 在**单一登录**部分中，选择**启用单一登录**，然后单击**更多**来扩展该部分。  <br><br>  ![配置单一登录](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769512.png "Configure single sign-on")

     3.5 英寸。 复制的 URL，**用户可以通过输入其电子邮件地址登录，或者它们可以直接转到**旁边。 <br><br>  ![配置单一登录](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769513.png "Configure single sign-on")

     3.6。 在 Azure 的门户网站，在**为企业收存箱登录**URL 文本框中，粘贴 URL。 <br><br>  ![配置单一登录](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769514.png "Configure single sign-on")  



4. 在**配置单一登录为企业收存箱在**页上，单击**下载证书**，然后将保存在您的计算机上的证书文件。  <br><br>  ![配置单一登录](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769515.png "Configure single sign-on")


5. 在您收存的商业租户，在**身份验证**页的**单一登录**部分中，执行以下步骤︰ <br><br>  ![配置单一登录](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769516.png "Configure single sign-on")

     5.1。 单击**所需**。

     5.2。 在 Azure 的门户中，在**配置单一登录为企业收存箱在**对话框页面上，复制的**登录页的 URL**值，，然后将其粘贴到**登录 URL**文本框。


     5.3。 创建您已下载的证书的**Base 64 编码**的文件。 >[AZURE.TIP]更多详细信息，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)。


     5.4。 单击**选择证书**，然后浏览到您的**base 64 编码的证书文件**。


     5.5。 单击**保存更改**来完成您的商业租户的收存箱上的配置。


6. 在 Azure 广告门户，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。 <br><br>  ![配置单一登录](./media/active-directory-saas-dropboxforbusiness-tutorial/IC749329.png "Configure single sign-on")





##配置用户设置
  
本部分的目的是概述如何启用用户供应的 Active Directory 用户帐户添加到业务的收存箱。


### 若要配置用户设置，请执行以下步骤︰

1. 在 Azure 管理门户中，**收存箱为业务**应用程序集成在页上，单击**配置用户设置**以打开**配置用户设置**对话框。

2. 在启用用户供应到收存箱业务页，单击启用用户提供打开登录到收存箱链接使用 Azure AD 对话。  <br><br> ![用户资源调配](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769517.png "User provisioning")

3. 在**登录到收存箱与 Azure 的广告链接**对话框中，登录到您的商业租户的收存箱。 <br><br> ![用户资源调配](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769518.png "User provisioning")



4. 单击**允许**授予 Azure 广告权收存箱。 <br><br> ![用户资源调配](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769519.png "User provisioning")



5. 若要完成配置，请单击**完成**按钮。  <br><br> ![用户资源调配](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769520.png "User provisioning")




##分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###若要将用户分配到业务的收存箱，请执行以下步骤︰

1.  在 Azure 广告门户中，创建一个测试帐户。

2.  在**收存箱为业务**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769521.png "Assign users")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-dropboxforbusiness-tutorial/IC767830.png "Yes")
  


现在应该等待 10 分钟，并验证该帐户已被同步到收存箱的业务。

作为第一个验证步骤，可以通过单击**操控板**中**收存的业务**应用程序集成页在 Azure 管理门户网站上检查供应状态，

<br><br>  ![分配用户](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769522.png "Assign users")


已成功完成的用户供应周期的相关状态指示。

<br><br>  ![分配用户](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769523.png "Assign users")


如果您想要测试单个登录设置，打开后盖。
关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。




## 其他资源

* [如何将与 Azure Active Directory 集成在 SaaS 应用程序的教程列表](active-directory-saas-tutorial-list.md)
* [应用程序访问权限和单一登录使用 Azure 活动目录是什么？](active-directory-appssoaccess-whatis.md)测试

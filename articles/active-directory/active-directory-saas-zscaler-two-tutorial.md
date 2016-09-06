---
ms.openlocfilehash: 6e36542154e058d2d522035c5ee0857217ad10eb
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties pageTitle="教程︰ 与 Zscaler 两个 Azure Active Directory 集成 |Microsoft Azure" description="了解如何使用 Zscaler 两个与 Azure Active Directory 以启用单一登录、 自动化资源调配，和更多。" services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="08/01/2015" ms.author="markvi" />
#与 Zscaler 两个教程︰ Azure Active Directory 集成
>[AZURE.TIP]反馈，请单击[此处](http://go.microsoft.com/fwlink/?LinkId=614871)。
  
本教程的目的是显示 Azure 和两个 ZScaler 的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   ZScaler 两个单一登录启用了订阅
  
后学完本教程，您已分配给两个 ZScaler Azure AD 用户将能够到单一登录到应用程序在两个 ZScaler 公司网站 （服务提供程序初始化登录），或使用[接入面板简介](https://msdn.microsoft.com/library/dn308586)
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用 ZScaler 两个应用程序的集成
2.  配置单一登录
3.  配置代理服务器设置
4.  配置用户设置
5.  分配用户

![配置单一登录](./media/active-directory-saas-zscaler-two-tutorial/IC800199.png "Configure Single Sign-On")

##启用 ZScaler 两个应用程序的集成
  
本部分的目的是概述如何启用 ZScaler 两个应用程序的集成。

###若要启用应用程序集成的两个 ZScaler，请执行以下步骤︰

1.  在 Azure 管理门户中，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录](./media/active-directory-saas-zscaler-two-tutorial/IC700993.png "Active Directory")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-zscaler-two-tutorial/IC700994.png "Applications")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-zscaler-two-tutorial/IC749321.png "Add application")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![从 gallerry 中添加应用程序](./media/active-directory-saas-zscaler-two-tutorial/IC749322.png "Add an application from gallerry")

6.  在**搜索框**中，键入**两个 ZScaler**。

    ![应用程序库](./media/active-directory-saas-zscaler-two-tutorial/IC800200.png "Application Gallery")

7.  在结果窗格中，选择**两个 ZScaler**，，然后单击**完成**添加应用程序。

    ![ZScaler 两个](./media/active-directory-saas-zscaler-two-tutorial/IC800201.png "ZScaler Two")

##配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 ZScaler 两个。  
作为此过程的一部分，您都需要将 base 64 编码的证书上传到您的 ZScaler 两个租户。  
如果您不熟悉此过程，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)

###若要配置单一登录，请执行以下步骤︰

1.  在 Azure 广告门户中， **ZScaler 两个**应用程序集成在页上，单击**配置单一登录**来打开**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-zscaler-two-tutorial/IC800202.png "Configure Single Sign-On")

2.  在**您想怎样用户能够登录到 ZScaler 两个**页面上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-zscaler-two-tutorial/IC800203.png "Configure Single Sign-On")

3.  在**配置应用程序 URL**页上，在**ZScaler 两个符号在 URL**文本框中，键入您登录到 ZScaler 两个应用程序中，用户所使用的 URL，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-zscaler-two-tutorial/IC800204.png "Configure App URL")

    >[AZURE.NOTE] 如果需要可以获取的环境从两个 ZScaler 的支持团队的实际值。

4.  在**配置单一登录在 ZScaler 两个**页面上，下载您的证书，单击**下载证书**，然后将保存在您的计算机上的证书文件。

    ![配置单一登录](./media/active-directory-saas-zscaler-two-tutorial/IC800205.png "Configure Single Sign-On")

5.  在不同的 web 浏览器窗口中，以管理员身份登录到两个 ZScaler 公司网站。

6.  在菜单上，单击**管理**。

    ![管理](./media/active-directory-saas-zscaler-two-tutorial/IC800206.png "Administration")

7.  下**管理管理员和角色**，单击**管理用户和身份验证**。

    ![管理用户和身份验证](./media/active-directory-saas-zscaler-two-tutorial/IC800207.png "Manage Users & Authentication")

8.  在**选择您的组织的身份验证选项**部分中，执行以下步骤︰

    ![身份验证](./media/active-directory-saas-zscaler-two-tutorial/IC800208.png "Authentication")

    1.  选择**身份验证使用 SAML 单一登录**。
    2.  单击**配置 SAML 单一登录参数**。

9.  **配置 SAML 单一登录参数**对话框页上，执行下面的步骤，然后单击**完成**:

    ![单一登录](./media/active-directory-saas-zscaler-two-tutorial/IC800209.png "Single Sign-On")

    1.  在 Azure 的门户中，在**配置单一登录在 ZScaler 两个**对话框页上，将**身份验证请求 URL**值，复制，然后将其粘贴到**进行身份验证的用户发送到 SAML 门户网站的 URL**文本框。
    2.  在**属性包含登录名**文本框中，键入**NameID**。
    3.  若要上载您已下载的证书，请单击**Zscaler pem**。
    4.  选择**启用 SAML 自动资源调配**。

10. 在**配置用户身份验证**对话框页面上，请执行以下步骤︰

    ![管理](./media/active-directory-saas-zscaler-two-tutorial/IC800210.png "Administration")

    1.  单击**保存**。
    2.  单击**立即激活**。

11. 在 Azure 的门户中，在**配置单一登录在 ZScaler 两个**对话框页面上，选择单一登录配置进行确认，，然后单击**完成**。

    ![配置单一登录](./media/active-directory-saas-zscaler-two-tutorial/IC800211.png "Configure Single Sign-On")

##配置代理服务器设置

###要在 Internet Explorer 中配置代理服务器设置

1.  启动**Internet Explorer**。

2.  从**工具**菜单上打开**Internet 选项**对话框中选择**Internet 选项**。

    ![Internet 选项](./media/active-directory-saas-zscaler-two-tutorial/IC769492.png "Internet Options")

3.  单击**连接**选项卡。

    ![连接](./media/active-directory-saas-zscaler-two-tutorial/IC769493.png "Connections")

4.  单击**局域网设置**以打开**局域网设置**对话框。

5.  在代理服务器部分中，执行以下步骤︰

    ![代理服务器](./media/active-directory-saas-zscaler-two-tutorial/IC769494.png "Proxy server")

    1.  选择为 LAN 使用代理服务器。
    2.  在地址文本框中，键入**gateway.zscalerone.net**。
    3.  在端口文本框中，键入**80**。
    4.  选择**对于本地地址不使用代理服务器**。
    5.  单击**确定**以关闭**局域网 (LAN) 设置**对话框。

6.  单击**确定**以关闭**Internet 选项**对话框。

##配置用户设置
  
为了使 Azure AD 用户能够登录到 ZScaler 两种，它们必须与 ZScaler 两种设置。  
在 ZScaler 两种，资源调配是手动任务。

###若要配置用户设置，请执行以下步骤︰

1.  登录到**Zscaler**租户。

2.  单击**管理**。

    ![管理](./media/active-directory-saas-zscaler-two-tutorial/IC781035.png "Administration")

3.  单击**用户管理**。

    ![Add](./media/active-directory-saas-zscaler-two-tutorial/IC781037.png "Add")

4.  在**用户**选项卡上，单击**添加**。

    ![Add](./media/active-directory-saas-zscaler-two-tutorial/IC781037.png "Add")

5.  在添加用户部分中，执行以下步骤︰

    ![添加用户](./media/active-directory-saas-zscaler-two-tutorial/IC781038.png "Add User")

    1.  键入**用户 Id**、**显示用户名**、**密码**、**确认密码**，然后选择**组**和您希望提供一个有效的 AAD 帐户**部门**。
    2.  单击**保存**。

>[AZURE.NOTE] 您可以使用任何其他 ZScaler 两个用户帐户创建工具或 Api 提供的两个 ZScaler 来设置 AAD 用户帐户。

##分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###要将用户分配给两个 ZScaler，请执行以下步骤︰

1.  在 Azure 广告门户中，创建一个测试帐户。

2.  在**ZScaler 两个**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-zscaler-two-tutorial/IC800212.png "Assign Users")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-zscaler-two-tutorial/IC767830.png "Yes")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](https://msdn.microsoft.com/library/dn308586)。
测试

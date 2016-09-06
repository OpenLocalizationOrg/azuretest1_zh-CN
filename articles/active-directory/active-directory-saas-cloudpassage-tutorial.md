---
ms.openlocfilehash: b1bb97ca77b2d46339eee7b22f4422fa94f0d040
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="教程︰ Azure Active Directory 集成与 CloudPassage |Microsoft Azure"
    description="了解如何配置单一登录 Azure Active Directory 和 CloudPassage 之间。"
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="stevenpo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/26/2015"
    ms.author="markvi"/>


# 教程︰ 与 CloudPassage 的 Azure Active Directory 集成

本教程的目的是向您展示如何使用 Azure 活动目录 (AD Azure) 集成 CloudPassage。<br>与 Azure AD 集成 CloudPassage 提供了以下好处︰ 

- 您可以控制有权访问 CloudPassage Azure AD 中 
- 您可以使用户可以自动获取签名的 CloudPassage （单一登录） 到其 Azure 的广告帐户
- 您可以管理您的帐户在一个中心位置的 Azure 活动目录门户

如果您想要了解有关使用 Azure AD 的 SaaS 应用程序集成的更多详细信息，请参阅[什么是应用程序访问和使用 Azure Active Directory 的单一登录](active-directory-appssoaccess-whatis.md)。

## 先决条件 

要配置 CloudPassage Azure 广告集成，您需要以下各项︰

- Azure 广告订阅
- 在已启用的订阅了 CloudPassage 单点登录


> [AZURE.NOTE] 若要测试步骤在本教程中，我们不建议使用生产环境中。


若要测试步骤在本教程中，您应按照这些建议︰

- 您不应使用生产环境中，除非这是必要的。
- 如果您没有 Azure 广告测试环境，则可以获得免费一个月 Azure 试用[这里](https://azure.microsoft.com/pricing/free-trial/)。 

 
## 方案说明
本教程的目的是使您可以在测试环境中测试 Azure AD 单一登录。 <br>
在本教程中介绍的方案由两个主要的构造块组成︰

1. 从库中添加 CloudPassage 
2. 配置和测试 Azure AD 单一登录


## 从库中添加 CloudPassage
若要配置 CloudPassage 集成到 Azure 的广告，您需要将 CloudPassage 从库添加到托管的 SaaS 应用程序的列表。

### 要从库添加 CloudPassage，请执行以下步骤︰

1. 在**Azure 管理门户**，在左侧的导航窗格中，单击**活动目录**。 <br><br>
![活动目录][1]

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。<br><br>
![应用程序][2]
4. 单击页面底部的**添加**。<br><br>
![应用程序][3]
5. 在**您想要执行**对话框中，单击**添加应用程序从库**。<br><br>
![应用程序][4]
6. 在搜索框中，键入**CloudPassage**。<br><br>
![应用程序][5]
7. 在结果窗格中，选择**CloudPassage**，，然后单击**完成**添加应用程序。<br><br>
![应用程序][6]



##  配置和测试 Azure AD 单一登录
本部分的目的是向您展示如何配置和测试 Azure AD 单一登录 CloudPassage 基于一个名为"Britta Simon"的测试用户。

对于单一登录工作，Azure 广告需要知道 CloudPassage 在 Azure AD 用户对应的用户。 换句话说，Azure AD 用户和 CloudPassage 中的相关的用户之间的链接关系需要建立。<br>
此链接关系的建立在 Azure 广告作为**用户名**CloudPassage 中的值指定的**用户名**的值。
 
要配置和测试 Azure AD 单一登录使用 CloudPassage，您需要完成以下构造块︰

1. **[Azure AD 配置一个 Single Sign-on](#configuring-azure-ad-single-single-sign-on)** -若要使用户可以使用此功能。
2. **[创建 Azure 广告测试用户](#creating-an-azure-ad-test-user)**的 Azure AD 单一登录 Britta Simon 与测试。
4. **[创建 CloudPassage 测试用户](#creating-a-halogen-software-test-user)**的链接到她的 Azure 广告表示的 CloudPassage 中具有 Britta Simon 的副本。
5. **[分配的 Azure 广告测试用户](#assigning-the-azure-ad-test-user)**-启用 Britta Simon Azure AD 单一登录使用。
5. **[测试单一登录](#testing-single-sign-on)**的验证配置是否有效。

### Azure 的广告单登录配置

本节的目的是 Azure AD 单一登录 Azure 广告门户中启用并配置单一登录，CloudPassage 应用程序中。<br>
CloudPassage 应用程序需要以特定的格式，这就需要你对 SAML 令牌的属性配置中添加自定义属性映射 SAML 断言。 下面的屏幕快照显示此示例。
<br><br> ![配置单一登录][21]

**以 CloudPassage Azure AD 单一登录配置，请执行以下步骤︰**

1. 在 Azure 广告门户中， **CloudPassage**应用程序集成在页上，单击**配置单一登录**来打开**配置单一登录**对话框。<br><br>
![配置单一登录][7]

2. 在**您想怎样来登录到 CloudPassage 的用户**页上，选择**Azure AD Single Sign-on**，，然后单击**下一步**。<br><br>
![配置单一登录][8]

3. 在**配置应用程序设置**对话框页面上，请执行以下步骤︰ <br><br>![配置应用程序设置][9]
 
     3.1。 在**符号上 URL**文本框中，键入您为登录到您的 CloudPassage 应用程序的用户使用的 URL (例如︰ *https://portal.cloudpassage.com/saml/init/accountid*)。 

     3.2。 在回复 URL 文本框中，键入 AssertionConsumerService URL (例如︰ *https://portal.cloudpassage.com/saml/consume/accountid*)。 <br> 通过单击**SSO 安装文档**CloudPassage 门户的**单一登录设置**部分中，可以为此属性获取值。 <br><br>![配置单一登录][10]

     3.3。 单击**下一步**。



4. 在**配置单一登录 CloudPassage 在**页上，单击**下载证书**，然后保存在您的计算机的本地证书文件。 <br><br>![配置单一登录][11]

5. 在不同的浏览器窗口中，登录到您的 CloudPassage 公司网站以管理员的身份。

6. 在菜单上，单击**设置**，然后单击**网站管理**。 <br><br> ![配置单一登录][12]

7. 单击**身份验证设置**选项卡。 <br><br> ![配置单一登录][13]


8. 在**单一登录设置**部分中，执行以下步骤︰ <br><br> ![配置单一登录][14]


     8.1。 在 Azure 的门户中，在**配置单一登录在 CloudPassage**对话框页上，将**颁发者 URL**值，复制，然后将其粘贴到**SAML 颁发者 URL**文本框。

     8.2。 在 Azure 的门户中，在**配置单一登录在 CloudPassage**对话框页面上，复制的**服务提供商 (SP) 启动终结点**值，，然后将其粘贴到**SAML 端点 URL**文本框。

     8.3。 在 Azure 的门户中，在**配置单一登录在 CloudPassage**对话框页上，将**注销 URL**值，复制，然后将其粘贴到文本框中**注销登录页**。

     8.4。 从下载证书创建**base 64**编码的文件。 
          
      >[AZURE.TIP] 有关详细信息，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)。

     8.5。 在记事本中打开您的 base 64 编码的证书，将其内容复制到剪贴板，然后将其粘贴到**x509 证书**文本框。

     8.6。 单击**保存**。


9. 在 Azure 广告门户，选择单一登录配置进行确认，，然后单击**下一步**。 <br><br> ![配置单一登录][15]


10. 在**单一登录确认**页上，单击**完成**。 <br><br> ![配置单一登录][16]



11. 在顶端上 nu，单击**属性**以打开**SAML 令牌的属性**对话框。 <br><br> ![配置单一登录][17]

12. 若要添加下表中的每一行所需的用户属性，请执行以下步骤︰ <br>

| 属性名称 | 属性值 |
| --- | --- |
| 名字字段 | user.givenname |
| 姓氏 | user.surname |
| 电子邮件 | user.mail |  

     12.1. Click **add user attribute**. <br><br> ![Configure Single Sign-On][18]

     12.2. In the **Attribute Name** textbox, type the attribute name shown for that row and as **Attribure Value**, select the attribute value shown for that row . <br><br> ![Configure Single Sign-On][19]
     
     12.2.3 Click **Complete**.


13. 在 tollbar 底部，单击**应用更改**。 <br><br> ![配置单一登录][20]



### Azure 广告测试用户

本节的目的是在调用 Britta Simon Azure 门户创建的测试用户。<br><br>
在用户列表中，选择**Britta Simon**。<br>![Azure 广告测试用户](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_01.png)

**在 Azure AD 中创建一个测试用户，请执行以下步骤︰**

1. 在**Azure 管理门户**，在左侧的导航窗格中，单击**活动目录**。<br>
![Azure 广告测试用户](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_02.png) 

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要显示列表的用户，在顶部菜单中，单击**用户**。<br>![Azure 广告测试用户](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_03.png) 
 
4. 若要打开**添加用户**对话框中，在底部工具栏中，单击**添加用户**。 <br>![Azure 广告测试用户](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_04.png) 

5. 在**告诉我们有关此用户**对话框页面上，请执行以下步骤︰ <br>![Azure 广告测试用户](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_05.png) 
  1. 作为用户类型，选择您的组织中的新用户。
  2. 在用户名**文本框**中，键入**BrittaSimon**。
  3. 单击下一步。

6.  在**用户配置文件**对话框页面上，请执行以下步骤︰ <br>![Azure 广告测试用户](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_06.png) 
  1. 在**名字**文本框中，键入**Britta**。  
  2. 在**姓氏**文本框，类型， **Simon**。
  3. 在**显示名称**文本框中，键入**Britta Simon**。
  4. 在**角色**列表中，选择**用户**。
  5. 单击**下一步**。

7. 在**获得临时密码**对话框页面上，单击**创建**。<br>![Azure 广告测试用户](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_07.png) 
 
8. 在**获得临时密码**对话框页面上，请执行以下步骤︰<br>![Azure 广告测试用户](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_08.png) 
  1. 记下**新密码**的值。
  2. 单击**完成**。   


  
 
### CloudPassage 测试用户

本节的目的是创建一个名 Britta Simon CloudPassage 中的用户。

#### 若要创建一个名为 Britta Simon CloudPassage 在用户，请执行以下步骤︰

1.  登录到您的**CloudPassage**公司网站管理员身份。 

2.  在顶部的工具栏中，单击**设置**，然后单击**网站管理**。 <br>![CloudPassage 测试用户][22] 

3.  单击**用户**选项卡，然后单击**添加新用户**。 <br>![CloudPassage 测试用户][23]
    
4.  在**添加新用户**部分中，执行以下步骤︰ <br>![CloudPassage 测试用户][24]

     4.1。 在**名字**文本框中，键入 Britta。

     4.2。 在**姓氏**文本框中，键入 Simon。

     4.3。 在**用户名**文本框中，**电子邮件**文本框，**请重新键入电子邮件**文本框中，键入 Britta 的用户名在 Azure 的广告。

     4.4。 作为**访问类型**，选择**启用光环门户访问权限**。

     4.5。 单击**添加**。










### 将 Azure 广告测试用户分配

本部分的目的是使 Britta Simon CloudPassage 授予她访问使用 Azure 单一登录。
<br><br>![分配用户][30]

**若要分配 Britta Simon CloudPassage，请执行以下步骤︰**

1. 在 Azure 的门户网站，若要打开应用程序视图中，目录视图中，单击**应用程序**顶部的菜单中。<br>
<br><br>![分配用户][26]
2. 在应用程序列表中，选择**CloudPassage**。
<br><br>![分配用户][27]
1. 在菜单上，单击**用户**。<br>
<br><br>![分配用户][25]
1. 在用户列表中，选择**Britta Simon**。

2. 在底部工具栏中，单击**指派**。
<br><br>![分配用户][29]



### 单一登录测试

本部分的目的是测试 Azure AD 单一登录配置使用访问权限面板。<br>
单击访问权限面板中的 CloudPassage 平铺时，您应该获取自动签名对 CloudPassage 应用程序。


## 其他资源

* [如何将与 Azure Active Directory 集成在 SaaS 应用程序的教程列表](active-directory-saas-tutorial-list.md)
* [应用程序访问权限和单一登录使用 Azure 活动目录是什么？](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_01.png
[6]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_02.png
[7]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_05.png
[8]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_03.png
[9]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_04.png
[10]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_05.png
[11]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_06.png
[12]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_07.png
[13]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_08.png
[14]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_09.png
[15]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_10.png
[16]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_11.png
[17]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_10.png
[18]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_11.png
[19]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_12.png
[20]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_14.png
[21]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_14.png
[22]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_15.png
[23]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_16.png
[24]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_17.png
[25]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_15.png
[26]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_12.png
[27]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_13.png
[28]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_15.png
[29]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_16.png
[30]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_17.png






















测试

---
ms.openlocfilehash: 276104173b0a1e50de2ed4cece26b42c75605d14
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="教程︰ 与 SciQuest 花主管的 Azure Active Directory 集成"
    description="了解如何配置单一登录 Azure Active Directory 和 SciQuest 花主管之间。"
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
    ms.date="07/30/2015"
    ms.author="markusvi"/>


# 教程︰ 与 SciQuest 花主管的 Azure Active Directory 集成

本教程的目的是向您展示如何使用 Azure 活动目录 (AD Azure) 集成 SciQuest 花主管。<br>与 Azure AD 集成 SciQuest 花主任提供以下好处︰ 

- 您可以控制有权访问 SciQuest 花主管 Azure AD 中 
- 您可以启用用户自动获得签名上到 SciQuest （单一登录） 的花费主管其 Azure 的广告帐户
- 您可以管理您的帐户在一个中心位置的 Azure 活动目录门户

如果您想要了解有关使用 Azure AD 的 SaaS 应用程序集成的更多详细信息，请参阅[什么是应用程序访问和使用 Azure Active Directory 的单一登录](active-directory-appssoaccess-whatis.md)。

## 先决条件 

要配置 SciQuest 花主管 Azure 广告集成，您需要以下各项︰

- Azure 广告订阅
- SciQuest 花主任单点登录启用订阅


> [AZURE.NOTE] 若要测试步骤在本教程中，我们不建议使用生产环境中。


若要测试步骤在本教程中，您应按照这些建议︰

- 您不应使用生产环境中，除非这是必要的。
- 如果没有 Azure AD 试用环境，您可以获得一个月试用版在[这里](https://azure.microsoft.com/pricing/free-trial/)。 

 
## 方案说明
本教程的目的是使您可以在测试环境中测试 Azure AD 单一登录。 <br>
在本教程中介绍的方案由两个主要的构造块组成︰

1. 从库中添加 SciQuest 花主管 
2. 配置和测试 Azure AD 单一登录


## 从库中添加 SciQuest 花主管
若要配置 SciQuest 花控制器集成到 Azure 的广告，您需要将 SciQuest 花主管从库添加到托管的 SaaS 应用程序的列表。

**要从库添加 SciQuest 花控制器，请执行以下步骤︰**

1. 在**Azure 管理门户**，在左侧的导航窗格中，单击**活动目录**。 <br><br>
![活动目录][1]

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。<br><br>
![应用程序][2]
4. 单击页面底部的**添加**。<br><br>
![应用程序][3]
5. 在**您想要执行**对话框中，单击**添加应用程序从库**。<br><br>
![应用程序][4]
6. 在搜索框中，键入**sciQuest 花主管**。<br>
![应用程序][5]
7. 在结果窗格中，选择**SciQuest 花导演**，然后单击**完成**添加应用程序。<br>
![应用程序][6]


##  配置和测试 Azure AD 单一登录
本部分的目的是向您展示如何配置和测试 Azure AD 单一登录 SciQuest 花控制器基于一个名为"Britta Simon"的测试用户。

对于单一登录工作，Azure 广告需要知道在 Azure AD 用户 SciQuest 花控制器中对应的用户。 换句话说，Azure AD 用户和 SciQuest 花控制器中相关的用户之间的链接关系需要建立。<br>
此链接关系的建立在 Azure AD SciQuest 花控制器中的**用户名**的值中指定的**用户名**的值。
 
要配置和 Azure AD 单一登录 SciQuest 花控制器与测试，您需要完成以下构造块︰

1. **[Azure AD 配置一个 Single Sign-on](#configuring-azure-ad-single-single-sign-on)** -若要使用户可以使用此功能。
2. **[创建 Azure 广告测试用户](#creating-an-azure-ad-test-user)**的 Azure AD 单一登录 Britta Simon 与测试。
4. **[创建 SciQuest 花导演测试用户](#creating-a-halogen-software-test-user)**-SciQuest 花控制器链接到她的 Azure 广告表示中具有 Britta Simon 的副本。
5. **[分配的 Azure 广告测试用户](#assigning-the-azure-ad-test-user)**-启用 Britta Simon Azure AD 单一登录使用。
5. **[测试单一登录](#testing-single-sign-on)**的验证配置是否有效。

### Azure 的广告单单一登录配置

本节的目的是 Azure AD 单一登录 Azure 广告门户中启用并配置单一登录 SciQuest 花控制器应用程序中。<br>

**要配置 Azure AD 单一登录 SciQuest 花控制器，请执行以下步骤︰**

1. 在 Azure 广告门户中， **SciQuest 花控制器**应用程序集成在页上，单击**配置单一登录**来打开**配置单一登录**对话框。<br><br>
![配置单一登录][8]

2. 在**您想怎样用户能够登录到 SciQuest 花主管**页上，选择**Azure AD Single Sign-on**，，然后单击**下一步**。<br><br>
![Azure 的广告单登录][9]

3. 在**配置应用程序设置**对话框页面上，请执行以下步骤︰ <br><br>![配置应用程序设置][10]
 
     3.1。 在**符号上 URL**文本框中，键入用户用于登录到使用以下模式 SciQuest 花控制器应用程序 URL: *https://。*sciquest.com/.**

     3.2。 在**回复 URL**文本框中，键入**符号在 URL**文本框中键入相同的值。 

     3.3。 单击**下一步**。
 
4. 在**配置单一登录 SciQuest 花主管在**页上，单击**下载元数据**，并保存在您的计算机上的本地元数据文件。<br><br>![什么是 Azure AD 连接][11]

5. 联系 SciQuest 支持启用该身份验证方法使用上面下载元数据。

6. 在 Azure 广告门户，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。 <br><br>![什么是 Azure AD 连接][15]
10. 在**单一登录确认**页上，单击**完成**。  <br><br>![什么是 Azure AD 连接][] 16




### Azure 广告测试用户
本节的目的是在调用 Britta Simon Azure 门户创建的测试用户。

**在 Azure AD 中创建一个测试用户，请执行以下步骤︰**

1. 在**Azure 管理门户**，在左侧的导航窗格中，单击**活动目录**。
<br><br>![什么是 Azure AD 连接][100] 
2. 从**目录**列表中，选择要为其启用目录集成的目录。
3. 若要显示列表的用户，在顶部菜单中，单击**用户**。
<br><br>![什么是 Azure AD 连接][101] 
4. 若要打开**添加用户**对话框中，在底部工具栏中，单击**添加用户**。 
<br><br>![什么是 Azure AD 连接][102] 
5. 在**告诉我们有关此用户**对话框页面上，请执行以下步骤︰
<br><br>![什么是 Azure AD 连接][103] 
  1. 作为**用户的类型**，选择**您的组织中的新用户**。
  2. 在用户名**文本框**中，键入**BrittaSimon**。
  3. 单击下一步。
6.  在**用户配置文件**对话框页面上，请执行以下步骤︰
<br><br>![什么是 Azure AD 连接][104] 
  1. 在**名字**文本框中，键入**Britta**。  
  2. 在**最后一名**txtbox，类型， **Simon**。
  3. 在**显示名称**文本框中，键入**Britta Simon**。
  4. 在**角色**列表中，选择**用户**。
  5. 单击**下一步**。
7. 在**获得临时密码**对话框页面上，单击**创建**。
<br><br>![什么是 Azure AD 连接][105]  
8. 在**获得临时密码**对话框页面上，请执行以下步骤︰
<br><br>![什么是 Azure AD 连接][106]   
  1. 记下**新密码**的值。
  2. 单击**完成**。   
  
 
### 创建一个 SciQuest 花控制器测试用户

本节的目的是创建一个名 Britta Simon SciQuest 花控制器中的用户。

您需要联系您 SciQuest 花主管的支持团队，并为他们提供有关您的测试帐户，即可使用它创建的详细信息。

或者，您还可以利用在实时资源调配，单一登录功能支持的 SciQuest 花主管。 <br>
如果启用了刚好及时的资源调配，则用户期间自动创建通过 SciQuest 花导演一次登录尝试如果它们不存在。 此功能消除了手动创建单一登录对应用户的需要。

若要获取在及时的资源调配启用，您需要联系你 SciQuest 花主管的支持团队。
  

### 将 Azure 广告测试用户分配

本节的目的是使 Britta Simon SciQuest 花主管授予她访问使用 Azure 单一登录。
<br><br>![什么是 Azure AD 连接][200]

**若要分配 Britta Simon SciQuest 花控制器，请执行以下步骤︰**

1. 在 Azure 的门户网站，若要打开应用程序视图中，目录视图中，单击**应用程序**顶部的菜单中。<br>
<br><br>![什么是 Azure AD 连接][201]
2. 在应用程序列表中，选择**SciQuest 花主管**。
<br><br>![什么是 Azure AD 连接][202]
1. 在菜单上，单击**用户**。<br>
<br><br>![什么是 Azure AD 连接][203]
1. 在用户列表中，选择**Britta Simon**。
<br><br>![什么是 Azure AD 连接][204]
2. 在底部工具栏中，单击**指派**。
<br><br>![什么是 Azure AD 连接][205]



### 单一登录测试

本部分的目的是测试 Azure AD 单一登录配置使用访问权限面板。<br>
当您单击访问权限面板中的 SciQuest 花主任平铺时，则应获得自动签名对 SciQuest 花控制器应用程序。


## 其他资源

* [如何将与 Azure Active Directory 集成在 SaaS 应用程序的教程列表](active-directory-saas-tutorial-list.md)
* [应用程序访问权限和单一登录使用 Azure 活动目录是什么？](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_01.png
[2]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_02.png
[3]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_03.png
[4]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_04.png
[5]: ./media/active-directory-saas-sciquest-spend-director/tutorial_sciquest_spend_director_01.png
[6]: ./media/active-directory-saas-sciquest-spend-director/tutorial_sciquest_spend_director_05.png
[8]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_06.png
[9]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_07.png
[10]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_08.png
[11]: ./media/active-directory-saas-sciquest-spend-director/tutorial_sciquest_spend_director_03.png
[15]: ./media/active-directory-saas-sciquest-spend-director/tutorial_sciquest_spend_director_04.png

[100]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_09.png 
[101]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_10.png 
[102]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_11.png 
[103]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_12.png 
[104]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_13.png 
[105]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_14.png 
[106]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_15.png 
[200]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_16.png 
[201]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_17.png 
[202]: ./media/active-directory-saas-sciquest-spend-director/tutorial_sciquest_spend_director_06.png
[203]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_18.png
[204]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_19.png
[205]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_20.png


测试

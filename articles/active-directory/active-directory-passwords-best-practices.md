---
ms.openlocfilehash: bebd56f7448a085f9e76005f36b7adb6132af007
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="最佳实践︰ Azure AD 密码管理 |Microsoft Azure" 
    description="部署和使用最佳方法、 样本最终用户文档和 Azure Active Directory 中的密码管理的培训指南。" 
    services="active-directory" 
    documentationCenter="" 
    authors="asteen" 
    manager="kbrint" 
    editor="billmath"/>

<tags 
    ms.service="active-directory" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/08/2015" 
    ms.author="asteen"/>

# 部署了密码管理和培训用户使用它
启用密码重置后，您需要采取的下一步是使您的组织中使用该服务的用户。 若要执行此操作，您将需要确保您的用户配置为使用该服务正确以及您的用户具有的培训他们需要成功地管理自己的密码。 这篇文章将向您介绍以下概念︰

* [**如何使用户密码管理配置**](#how-to-get-users-configured-for-password-reset)
  * [是什么使一个帐户配置密码重置为](#what-makes-an-account-configured)
  * [可以用来填充自己的身份验证数据的方法](#ways-to-populate-authentication-data)
* [**更好地推广密码重置为您的组织**](#what-is-the-best-way-to-roll-out-password-reset-for-users)
  * [基于电子邮件的首展 + 示例电子邮件通信](#email-based-rollout)
  * [如何使用强制的注册来强制用户在登录注册](#using-enforced-registration)
  * [如何将用户帐户的身份验证数据上载](#uploading-data-yourself)
* [**示例用户和支持培训资料 （即将提供） ！**](#sample-training-materials)

## 如何获取用户的密码重置配置
本部分介绍给您可以确保您的组织中的每个用户可以使用自助服务的密码，在用户忘记其密码的情况下有效地重置其中的各种方法。

### 是什么使得配置帐户
用户可以使用密码重置之前，**所有**以下条件必须满足︰

1.  在目录中，则必须启用密码重置。  了解如何启用通过阅读[使用户重置其 Azure 的 AD 密码](active-directory-passwords-getting-started.md#enable-users-to-reset-their-azure-ad-passwords)或[使用户能够重置或更改其 AD 密码](active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords)重置密码
2.  用户必须被授予使用许可。
 - 对于云的用户，用户必须有**任何付费的 Office 365 提供许可证**， **AAD 基本**或**AAD 特优许可**分配。
 - 对于上 prem 用户 （联合或哈希同步），**必须拥有 AAD 特优许可证分配**的用户。
3.  用户必须具有符合当前的密码重置策略**定义的身份验证数据的最小集合**。
 - 如果在目录中的相应字段包含格式良好的数据定义认为身份验证数据。
 - 如果配置了两个门策略，在**至少有一个**启用的身份验证选项，如果配置一个关口策略，或在**至少两**个启用的身份验证选项定义身份验证数据的最小集合。
4.  如果用户正在使用本地帐户，然后[写回密码](active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords)必须启用并打开

### 填充的身份验证数据的方法
有几个选项如何在您的组织中指定用户的数据，用于进行密码重置。

- 编辑在[Azure 管理门户](https://manage.windowsazure.com)或[Office 365 管理门户](https://portal.microsoftonline.com)的用户
- AADSync 用于同步到 Azure AD 内部部署 Active Directory 域中的用户属性
- 使用 Windows PowerShell 编辑用户属性
- 允许用户通过在[http://aka.ms/ssprsetup](http://aka.ms/ssprsetup)注册门户指导注册他们自己的数据
- 要求用户注册他们登录在[http://myapps.microsoft.com](http://myapps.microsoft.com)的访问面板通过将**要求用户注册 SSPR**配置选项设置为**是**时，重置密码。

用户不需注册密码重置系统发挥作用的。  例如，如果您有现有的电话号码在您的本地目录中，可以在 Azure AD 同步，我们将自动重置密码的使用它们。

## 最佳的方法，推出重置用户的密码是什么？
密码重置的常规部署步骤如下︰

1.  启用密码重设您的目录中，通过转到[Azure 管理门户](https://manage.windowsazure.com)中的**配置**选项卡并为**用户启用密码重置**选项选择**是**。
2.  为您希望向其提供密码重置在每个用户分配相应的许可证转到[Azure 管理门户](https://manage.windowsazure.com)中的**许可证**选项卡。
3.  还可以限制密码重置为一组用户推出了功能慢慢随着时间的推移通过设置**限制访问密码重新设置**切换为**是**，然后选择安全组启用密码重置 （注意这些用户必须具有分配给他们的许可证）。
4.  此时应指导用户使用密码重置或者向他们发送一封电子邮件，指示它们要注册，请启用强制注册上盖板，或自己通过目录同步，PowerShell 或[Azure 管理门户](https://manage.windowsazure.com)上载这些用户的适当的身份验证数据。  对此的更多详细信息如下。
5.  随着时间的推移查看用户注册通过导航到报告选项卡并查看[**密码重置注册的活动**](active-directory-passwords-get-insights.md#view-password-reset-registration-activity)报告。
6.  一旦注册用户一个好数字，观察他们使用密码重设通过导航到报告选项卡并查看[**密码重置事件**](active-directory-passwords-get-insights.md#view-password-reset-activity)报告。

有几种方法来通知用户，他们可以注册并使用密码重设您的组织中。  他们详细列出。

### 基于电子邮件的首展
也许最简单的方法，以通知用户有关注册或使用密码重置是向他们发送一封电子邮件，指示它们这样做。  下面是您可以使用此的模板。  您可以随意替换颜色 / 徽标与您自己的自定义以满足您的需要选择。

  ![][001]

您可以下载的电子邮件模板[在此处](http://1drv.ms/1xWFtQM)。

### 使用强制的注册
如果您希望您的用户密码重置自身注册，您也可以强制它们注册时登录到[http://myapps.microsoft.com](http://myapps.microsoft.com)在访问权限面板。  可以通过启用**需要用户注册登录接入面板**选项启用此选项从您的目录**配置**选项卡。  

您还可以定义将要求他们重新注册一个可配置的时间段之后通过修改**之前用户必须确认其联系人数据的天数**可以是一个非零值。 有关更多信息，请参见[自定义用户密码管理行为](active-directory-passwords-customize.md#password-management-behavior)。

  ![][002]

启用此选项，当用户登录到访问权限面板后，他们将看到弹出窗口，通知他们其管理员有需要它们来验证他们的联系信息。 他们可以使用它来重置其密码，如果以往任何时候都无法访问到他们的帐户。

  ![][003]

单击**立即验证**将它们带到**密码重置注册门户** [http://aka.ms/ssprsetup](http://aka.ms/ssprsetup)处，并要求他们注册。  通过此方法登记可通过单击**取消**按钮或关闭窗口，将其忽略，但每次如果没有注册在他们登录时提醒用户。

  ![][004]

### 上传用户数据
如果您想要将上载用户身份验证数据，然后用户需要注册后才能重置其密码重新设置密码。  只要用户具有在他们符合密码的帐户定义的身份验证数据重置的策略定义，然后这些用户将能重置其密码。

若要了解哪些属性可以设置通过 AAD 连接或 Windows PowerShell，查看[哪些数据使用的密码重置](active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset)。

通过执行以下步骤，可上载通过[Azure 管理门户网站](https://manage.windowsazure.com)的身份验证数据︰

1.  导航到您在[Azure 管理门户](https://manage.windowsazure.com)中的**Active Directory 扩展**中的目录。
2.  在**用户**选项卡上单击。
3.  选择您所感兴趣的列表中的用户。
4.  在第一个选项卡上，您将找到**备用电子邮件**，它可以用作属性以启用密码重置。 

    ![][005]

5.  在**工作信息**选项卡上单击。
6.  在此页上，您会发现**办公室电话**、**手机**、**身份验证电话**和**电子邮件身份验证**。  这些属性也可以设置为允许用户重置其密码。 

    ![][006]

了解[哪些数据使用的密码重置](active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset)如何使用上述每个属性，请参阅。

## 示例培训资料
我们正在从事的示例培训资料可以用来使您的 IT 部门和您的用户快速快速如何部署和使用密码重置。  请继续关注 ！


<br/>
<br/>
<br/>

**其他资源**


* [什么是密码管理](active-directory-passwords.md)
* [密码管理的工作原理](active-directory-passwords-how-it-works.md)
* [密码管理入门](active-directory-passwords-getting-started.md)
* [自定义密码管理](active-directory-passwords-customize.md)
* [如何获取运营洞察力与密码管理报告](active-directory-passwords-get-insights.md)
* [密码管理常见问题解答](active-directory-passwords-faq.md)
* [解决密码管理](active-directory-passwords-troubleshoot.md)
* [了解更多信息](active-directory-passwords-learn-more.md)
* [在 MSDN 上的密码管理](https://msdn.microsoft.com/library/azure/dn510386.aspx)



[001]: ./media/active-directory-passwords-best-practices/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-best-practices/002.jpg "Image_002.jpg"
[003]: ./media/active-directory-passwords-best-practices/003.jpg "Image_003.jpg"
[004]: ./media/active-directory-passwords-best-practices/004.jpg "Image_004.jpg"
[005]: ./media/active-directory-passwords-best-practices/005.jpg "Image_005.jpg"
[006]: ./media/active-directory-passwords-best-practices/006.jpg "Image_006.jpg"
 
测试

---
ms.openlocfilehash: 4b819b26815e405706826847b0934f4458c86879
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="要开始使用 Azure 云环境中的多因素身份验证" 
    description="这是介绍如何开始使用 Azure MFA 云中的 Azure 的多因素身份验证页。" 
    services="multi-factor-authentication" 
    documentationCenter="" 
    authors="billmath" 
    manager="stevenpo" 
    editor="curtand"/>

<tags 
    ms.service="multi-factor-authentication" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/24/2015" 
    ms.author="billmath"/>

# 要开始使用 Azure 云环境中的多因素身份验证



<center>![云](./media/multi-factor-authentication-get-started-cloud/cloud2.png)</center>

现在，我们确定了我们在云环境中使用多因素身份验证，让我们开始吧 ！  请注意，是否您使用多因素身份验证 Office 365 或多因素身份验证 Azure 管理员您可以跳到步骤 3。  此外，本文档处理 


1. **注册 Azure 的订阅**
    - 如果您已经没有 Azure 的订阅，您需要注册一个。 如果您是刚开始并探索使用 Azure MFA 可以使用试用版订购
2. **创建一个多因素身份验证提供程序，或将 Azure AD 津贴或企业移动套件许可证分配给用户**
    - 您将需要创建 Azure 多因素身份验证提供程序并将其分配给您的目录或指定给 Azure AD 津贴或 EMS 用户许可证。 Azure 的多因素身份验证包含在 Azure 活动目录特优，因此也是随企业移动套件。 如果您有 Azure AD 津贴或 EMS 不需要创建多因素身份验证提供程序，而是为了为 Azure AD 津贴或 EMS 的用户启用 MFA Azure AD 津贴或 EMS 的许可证需要分配给用户和管理员可以通过管理门户的用户分配 MFA。 有关如何将许可证分配给用户，请参阅下面的部分。
3. **为您的用户打开多因素身份验证** 
    - 在您的用户通过 Office 365 或 Azure 的门户网站上启用 Azure MFA。 有关如何执行此操作，请参阅下面信息的部分。
4. **将电子邮件发送给最终用户，通知他们有关 MFA**
    - 一旦用户已开启自己帐户的多因素身份验证，则建议您将为其发送一封电子邮件，通知他们此。 将提示用户以完成此过程在下一次登录，这样，它们没有什么使他们期望。 请参阅下面的电子邮件模板示例一节。



## 创建 Azure 的多因素身份验证提供程序
多因素身份验证是默认情况下，为全局管理员必须 Azure Active Directory 租户。 但是，如果您想要扩展到所有用户的多因素身份验证和/或为全局管理员希望能够获取优势功能，如管理门户、 自定义问候语和报表，然后您必须购买和配置的多因素身份验证提供程序。



### 若要创建一个多因素身份验证提供程序
--------------------------------------------------------------------------------

1. Azure 门户以管理员身份登录。
2. 在左边，选择活动目录。
3. 在活动的目录页上，在顶部，选择多因素身份验证提供程序。 然后在底部，单击**新建**。
4. 在应用程序的服务，选择活动的身份验证提供程序并选择快速创建。
5. 填写以下字段，然后选择创建。
    1. 名称 – 当前身份验证提供程序的名称。
    2. 使用模型--使用模型的多因素身份验证提供程序。
        - 每次身份验证 – 采购费用每次身份验证的模型。 通常用于在应用程序中使用 Azure 多因素身份验证方案。
        - 每个已启用用户 – 每个费用的购买方式启用用户。 通常用于 Office 365 的方案。
    2. 目录 – Azure Active Directory 租户多因素身份验证提供程序相关联。 请注意以下事项︰
        - 您不需要一个 Azure 的广告目录创建多因素身份验证提供程序。  这可以保留为空，如果计划只使用 Azure 多因素身份验证服务器或 SDK。
        - 您将需要将多因素身份验证提供程序与 Azure 的广告目录关联，如果您想要扩展到所有用户的多因素身份验证和/或希望全局管理员能够充分利用的功能，如管理门户、 自定义的问候，并报告。
        - 目录同步或 AAD 同步是仅要求如果与 Azure 的广告目录同步内部活动目录环境。  如果您只使用与活动目录的内部部署的实例不同步 Azure 的广告目录，您不需要目录同步或 AAD 同步。
        



5. 一旦您单击创建多因素身份验证提供程序将创建，您应该会看到一条消息指出︰ 成功创建多因素身份验证提供程序。 单击确定。

<center>![云](./media/multi-factor-authentication-get-started-cloud/provider.png)</center>
     
## 向用户分配许可证 Azure AD 津贴或企业移动性

如果您有 Azure AD 津贴或企业移动套件，您不需要创建一个多因素身份验证提供程序。  您需要许可证直接指派给用户，然后开始打开用户的 mfa。

### 要分配的 Azure AD 津贴或企业 Mobilitiy 套件许可证
--------------------------------------------------------------------------------
<ol>

<li>登录到 Azure 门户管理员身份。</li>
<li>在左边，选择**活动目录**。</li>
<li>在活动的目录页上，双击包含您要启用的用户目录。</li>
<li>在目录页的顶部，选择**许可证**。</li>
<li>在许可证页上，选择活动目录津贴或企业移动套件，然后单击**分配**。</li>

<center>![云](./media/multi-factor-authentication-get-started-cloud/license.png)</center>

<li>在对话框中，选择您要分配许可证的用户，然后单击复选标记图标以保存更改。</li>



## 开启多因素身份验证的用户

在 Azure 多因素身份验证的用户帐户有以下三种不同的状态︰

省/市/自治区 | 说明 |非浏览器 apss 受影响| 注释 
:-------------: | :-------------: |:-------------: |:-------------: |
禁用 | 多因素身份验证未注册的新用户的默认状态。|否|用户当前未使用的多因素身份验证。
已启用 |在多因素身份验证中，已注册用户。|No.  他们将继续工作直到完成注册过程。|用户已启用，但尚未完成注册过程。 将会提示他们完成在下次登录过程。
强制执行|用户已注册并使用多因素身份验证的注册过程完成。|是。  创建和使用应用程序密码之前，他们将无法工作。 | 用户可能或不可能完成登记。 如果他们完成了注册过程，他们都使用多因素身份验证。 否则，用户将会提示您为补足者在下次登录过程
现在，我们有哪种身份验证提供程序，或者已分配我们的用户许可证下, 一步是打开 mfa 目录中的用户。  使用以下过程为您的用户启用 mfa。

### 若要启用多因素身份验证
--------------------------------------------------------------------------------
1.  登录到 Azure 管理门户管理员身份。
2.  在左边，单击撤销。
3.  在下，目录单击您要启用的用户目录。
4.  在顶部，单击用户。
5.  在页面的底部，单击管理多元验证
6.  查找您想要启用的多因素身份验证的用户。 您可能需要更改在顶部的视图。 确保已禁用该用户的状态，其名称旁边的框中打勾。
7.  这会将最右侧的两个选项，启用和管理用户设置。 单击启用。 这将弹出弹出窗口将指定需要与您的用户采取下一步行动。 单击启用多因素验证
8.  一旦您已经启用了您的用户，建议您将发送一封电子邮件，通知他们，他们如何使用其非浏览器应用程序并不会锁定的用户。

<center>![云](./media/multi-factor-authentication-get-started-cloud/user.png)</center>

若要更改使用 Windows PowerShell 的用户的状态，您可以使用。  您可以更改`$st.State`等于上面提到的状态之一。

        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName bsimon@contoso.com -StrongAuthenticationRequirements $sta


## 向最终用户发送电子邮件

一旦您已经启用了您的用户，建议您将发送您的用户的电子邮件，通知他们，他们将需要提供那里联系信息。 下面是可用于包括用户可以观看视频的链接的电子邮件模板。

        Subject: ACTION REQUIRED: Your password for Outlook and other apps needs updated

        Body:

        For added security, we have enabled multi-factor authentication for your account. 

        Action Required: You will need to complete the enrollment steps below to make your account secure with multi-factor authentication.  

        What to expect once MFA is enabled:

        Multi-factor authentication requires a password that you know and a phone that you have in order to sign into browser applications and to access Office 365, Azure portals.

        For Office 365 non-browser applications such as outlook, lync, a mail client on your mobile device etc, a special password called an app password is required instead of your account password to sign in. App passwords are different than your account password, and are generated during the multi-factor authentication set up process. 

        Please follow these enrollment steps to avoid interruption of your Office 365 service:

            1.  Sign in to the Office 365 Portal at http://portal.microsoftonline.com.
            2.  Follow the instructions to set up your preferred multi-factor authentication method when signing into Office 365 using a web browser. 
            3.  Create one app password for each device.
            4.  Enter the same app password in all applicable apps on that device e.g. Outlook, Mail client, Lync, Word, Powerpoint, Excel, CRM etc. 
            5.  Update your Office client applications or other mobile applications to use an app password.

        You can visit http://aka.ms/mfasetup to create app passwords or change your MFA Setting.  Please bookmark this.

        NOTE: Before entering an app password, you will need to clear the sign-in information (delete sign-in info), restart the application,   and sign-in with the username and app password. Follow the steps documented : http://technet.microsoft.com/library/dn270518.aspx#apppassword.


        Watch a video showing these steps at http://g.microsoftonline.com/1AX00en/175.

        Best Regards,
        Your Administrator

## 下一步行动
现在，您在云中有安装多因素身份验证，您可以移动到什么是下一步转到[配置 Azure 多因素身份验证。](multi-factor-authentication-whats-next.md)  那里您将学习如何报告欺诈警报自定义语音邮件和所有 Azure 多因素身份验证所提供的功能。  

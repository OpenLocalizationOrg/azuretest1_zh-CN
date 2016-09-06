---
ms.openlocfilehash: a66408885dca34ad23f6d7a914e09431a713ab8f
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="保护使用 Azure 多因素身份验证和 AD FS 的云资源" 
    description="这是介绍如何开始使用 Azure MFA 并在云中的 AD FS Azure 的多因素身份验证页。" 
    services="multi-factor-authentication" 
    documentationCenter="" 
    authors="billmath" 
    manager="stevenpo" 
    editor="curtland"/>

<tags 
    ms.service="multi-factor-authentication" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/24/2015" 
    ms.author="billmath"/>

# 保护使用 Azure 多因素身份验证和 AD FS 的云资源

如果您的组织联盟与 Azure Active Directory 并且有 Azure AD 的访问资源，可以使用 Azure 多因素身份验证或 Active Directory 联合身份验证服务来保护这些资源。 使用下面的步骤来保护 Azure Active Directory 资源与 Azure 多因素身份验证或 Active Directory 联合身份验证服务。

## 力保 Azure 广告使用 AD FS 资源执行以下操作︰ 



1. 使用中[开启多因素身份验证](active-directory/multi-factor-authentication-get-started-cloud.md#turn-on-multi-factor-authentication-for-users)的用户列出的步骤来启用帐户。
2. 请按下列步骤设置声明规则︰

![云](./media/multi-factor-authentication-get-started-adfs-cloud/adfs1.png)

-   启动 AD FS 管理控制台。
-   导航到信赖方信任和信赖方信任上右击。 选择编辑索赔规则...
-   单击添加规则。.
-   从下拉向下，选择发送索赔使用的自定义规则并单击下一步。
-   请输入报销申请规则的名称。
-   在自定义规则︰ 添加以下︰


        => issue(Type = "http://schemas.microsoft.com/claims/authnmethodsreferences", Value = "http://schemas.microsoft.com/claims/multipleauthn");

    相应的索赔︰

        <saml:Attribute AttributeName="authnmethodsreferences" AttributeNamespace="http://schemas.microsoft.com/claims">
        <saml:AttributeValue>http://schemas.microsoft.com/claims/multipleauthn</saml:AttributeValue>
        </saml:Attribute>
- 单击确定。 单击完成。 关闭 AD FS 管理控制台。

然后用户可以完成登录使用内部方法 （如智能卡）。

## 联盟用户的信任的 Ip
信任的 Ip 允许管理员可以绕过多因素身份验证，对于特定的 IP 地址或联盟有他们自己的 intranet 内来自请求的用户。 以下各节将介绍如何配置与联盟的用户和绕过多因素身份验证，Azure 多因素身份验证信任的 Ip，当请求源自中的联合的用户内部网。  这被通过配置 AD FS 可以使用一种穿过或筛选内部企业网络声明类型的传入声明模板。  本示例使用 Office 365 我们信赖方信任。

### 配置 AD FS 声明规则

我们需要做的第一件事是配置 AD FS 声明。 我们将创建两份索赔规则、 一个用于内部公司网络声明类型和另一来保持我们的用户登录。

1. 打开 AD FS 管理。
2. 在左边，选择信赖方信任。
3. 在中间，Microsoft Office 365 标识平台右击，然后选择**编辑声明规则...**
![云](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip1.png)
4. 在颁发转换规则单击**添加规则。**
![云](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip2.png)
5. 添加转换声明规则向导中，在选择穿透或关闭筛选传入声明从下拉并单击下一步。
![云](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip3.png)
6. 声明规则名称旁边的框中，为该规则指定名称。 例如︰ InsideCorpNet。
7. 从下拉列表旁边传入声明类型，请选择内部公司网络。
![云](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip4.png)
8. 单击完成。
9. 在颁发转换规则单击**添加规则**。
10. 在添加转换声明规则向导中，选择发送索赔使用从中删除一个自定义规则并单击下一步。
11. 在中下声明规则名称︰ 输入保留用户签名中。
12. 在自定义规则框中输入︰
        
        c:[Type == "http://schemas.microsoft.com/2014/03/psso"]
            => issue(claim = c);
![云](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip5.png)
13. 单击**完成**。
14. 单击**应用**。
15. 单击**确定**。
16. 关闭 AD FS 管理。



### 配置 Azure 的多因素身份验证信任与联盟用户的 Ip
既然索赔均已到位，我们杖配置受信任的 ip。

1. 登录到 Azure 的管理门户。
2. 在左边，单击撤销。
3. 在下，目录单击您想要安装程序上的信任的 Ip 的目录。
4. 在您选择的目录，单击配置。
5. 在多因素身份验证部分中，单击管理服务设置。
6. 在服务设置页上，在信任的 Ip 下选择**来自源自于我的 intranet 的联盟用户的请求。**
![云](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip6.png)
7. 单击保存。
8. 一旦应用了更新程序，单击关闭。


就这么简单 ！ 在这种情况下，联合的 Office 365 提供用户应只需时要使用 MFA 索赔源自公司内部网之外。







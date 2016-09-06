---
ms.openlocfilehash: 00753eda75a2a7c8ecdd99b9f382b39248d31763
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="Azure 的多因素身份验证-它是什么？" 
    description="Azure 的多因素身份验证是验证您的身份的一种方法，需要使用多个用户名和密码。 它提供了额外的用户登录和交易的安全。" 
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

# 什么是 Azure 多因素身份验证？
多因素身份验证 (MFA) 是一种身份验证需要使用多个验证方法和安全关键第二层向用户登录和交易记录的方法。 它的工作原理是需要任何两个或多个下面的验证方法︰

- 您知道 （通常为密码） 的东西
- 您可以 （不会轻松地重复，像电话一样可信的设备）
- 您的某 （生物）

<center>![用户名和密码](./media/multi-factor-authentication/pword.png)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![证书](./media/multi-factor-authentication/phone.png)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![智能电话](./media/multi-factor-authentication/hware.png)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![智能卡](./media/multi-factor-authentication/smart.png)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![虚拟智能卡](./media/multi-factor-authentication/vsmart.png)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![的用户名和密码](./media/multi-factor-authentication/cert.png)</center>



Azure 的多因素身份验证是验证您的身份的一种方法，需要使用多个用户名和密码。 提供第二层的用户登录和交易的安全。   

Azure 多因素身份验证可帮助保护访问数据和应用程序同时还可以满足一个简单的登录过程的用户要求。 它提供了强身份验证通过一系列简单的验证选项 — — 电话、 短信或移动应用程序通知或验证代码和第三方誓言令牌。

Azure 多因素身份验证的工作原理的概述，请参阅下面的视频。


<center>[AZURE.VIDEO multi-factor-authentication-overview]</center>

##为什么使用 Azure 多因素身份验证？

今天，现在比以往更，人们越来越多地连接。  智能手机、 平板电脑、 笔记本电脑和个人电脑，与人有多个不同选项上他们将连接并随时保持联系。  人们可以从任何地方，这意味着他们可以得到更多的工作和为他们的客户服务更好地访问他们的帐户和应用程序。

Azure 的多因素身份验证是简单易用、 可扩展且可靠的解决方案，提供身份验证的第二个方法，以便您的用户始终受到保护。 

![易于使用](./media/multi-factor-authentication/simple.png)| ![可扩展性](./media/multi-factor-authentication/scalable.png)| ![始终受到保护](./media/multi-factor-authentication/protected.png)|![可靠](./media/multi-factor-authentication/reliable.png)
:-------------: | :-------------: | :-------------: | :-------------: |
**易于使用**|**可扩展性**|**始终受到保护**|**可靠**

- **易于使用**-Azure 多元 Authenticaton 是易于安装和使用。  附带了 Azure 多因素身份验证的其他保护，用户可以使用和管理他们自己的设备，并在许多情况下，它可以安装与几个简单的点击。
- **可扩展**的 Azure 多元 Authenticaton 利用云的电源和与您的部署集成 AD 和自定义应用程序。  这种保护甚至扩展到大量使命关键方案。
- **始终受到保护**的 Azure 多因素身份验证提供了强身份验证使用最高的行业标准。
- **可靠**— 我们保证 99.9%的可用性的 Azure 多因素身份验证。 当它不能接收或处理的多因素身份验证的身份验证请求时，该服务被视为不可用。  

为何其他信息使用 Azure 多因素身份验证请参见下面的视频。

<center>[AZURE.VIDEO windows-azure-multi-factor-authentication]</center>


## Azure 多因素身份验证的工作原理

多因素身份验证的安全性在于其分层方法。 影响多个身份验证因素为攻击者提供一项重大挑战。 即使攻击者设法了解用户的密码，它毫无用处还不用受信任设备的所有权。 应用户丢失的设备，找到它的人将无法使用它，除非他或她也知道该用户的密码。

![Proofup](./media/multi-factor-authentication-how-it-works/howitworks.png)



Azure 多因素身份验证可帮助保护访问数据和应用程序同时还可以满足一个简单的登录过程的用户要求。  它通过另一个窗体的身份验证要求提供额外的安全性，并提供了强身份验证通过一系列简单的验证选项︰

- 电话呼叫 
- 文本消息
- 移动应用程序通知 — — 使用户可以选择的方法他们喜欢
- 移动应用程序验证代码
- 第三方誓言标记

其他信息哦它的工作原理，请参阅下面的视频。

[AZURE.VIDEO multi-factor-authentication-deep-dive-securing-access-on-premises]

## 可用于多因素身份验证方法
当用户登录时，向用户发送额外的验证。  以下是可用于这第二个验证方法的列表。 

验证方法  | 说明 
------------- | ------------- |
电话呼叫 | 呼叫到用户的智能手机，让他们验证它们登录按 # 号。  这将完成验证过程。  此选项是可配置的并可以更改为您指定的代码。
文本消息 | 文本消息将发送到用户的智能手机，6 位代码。  输入此代码以完成验证过程。
移动应用程序通知 | 验证请求将发送到用户的智能手机从移动应用程序中选择验证要求他们完整的验证。 如果所选应用程序通知作为主要的验证方法，这会发生。  如果他们收到它们不能在登录的时候，他们可以选择报告它为欺诈行为。
使用移动应用程序的验证代码 | 验证码将发送到用户的智能手机运行的移动应用程序。  如果作为主要验证方法中选择一个验证码，这会发生。


## Azure 多因素身份验证的可用版本
Azure 的多因素身份验证有三个不同的版本。  下表描述每个详细。

版本  | 说明 
------------- | ------------- |
Office 365 的多因素身份验证 | 此版本 Office 365 提供应用程序独占使用，并从 Office 365 门户管理。 因此现在可以帮助管理员使用多因素身份验证保护其 Office 365 提供资源。  此版本提供了 Office 365 的订阅。
多因素身份验证的 Azure 管理员 | Office 365 的多因素身份验证功能的同一个子集将 Azure 的所有管理员无偿提供。 通过启用此核心的多因素身份验证功能，Azure 订阅每个管理帐户现在可以获得额外的保护。 想要访问 Azure 门户创建一个 VM，网站，管理员管理的存储，因此移动服务或任何其他 Azure 服务可以向其管理员帐户添加多因素身份验证。
Azure 的多因素身份验证 | Azure 的多因素身份验证提供一组丰富的功能。  它提供额外的配置选项，通过 Azure 管理门户、 高级报告和支持区域的内部和云应用程序。  Azure 的多因素身份验证是 Azure 活动目录津贴。

## 版本特征比较
下表下面列出的 Azure 多因素身份验证的各种版本中可用的功能。


功能  | （包含在 Office 365 Sku） Office 365 的多因素身份验证|多因素身份验证 （包括 Azure 的订阅） 的 Azure 管理员 | Azure 的多因素身份验证 （包括在 Azure AD 特优和企业移动套件） 
------------- | :-------------: |:-------------: |:-------------: |
管理员可以保护 MFA 的帐户| * | * （仅适用于 Azure 管理员帐户）|*
第二个因素为移动应用程序|* | * | *
第二个因素作为电话联络|* | * | *
第二个因素作为 SMS|* | * | *
对于不支持 MFA 的客户端应用程序密码|* | * | *
管理控制身份验证方法| | | *
PIN 模式| | | *
欺诈警报| | | *
MFA 报告| | | *
一次性的旁路| | | *
自定义问候语的电话联络| | | *
电话呼叫的呼叫方 ID 的自定义| | | *
事件确认| | | *
信任的 Ip| | | *
挂起 MFA 记忆设备 （公共预览）| | | *
MFA SDK| | | *
MFA 对于使用 MFA 服务器上部署应用程序| | | *


## 如何获取 Azure 多因素身份验证

Azure 的多因素身份验证是 Azure 活动目录特优和企业移动套件的一部分。  如果您已经有了这些则可以使用 Azure 多因素身份验证。

如果您是 Office 365 的用户或通过 Azure 多因素身份验证提供了 Azure 的订阅者和想要利用额外的功能，请继续阅读。

如果您没有上述任一操作，然后开始使用 Azure 多因素身份验证，首先需要 Azure 订阅或[Azure 的试用订阅](http://azure.microsoft.com/pricing/free-trial/)。 

当使用 Azure 多因素身份验证有两个付费选项可用︰

- **每个用户**。 一般的企业，想要启用固定数量的员工经常需要身份验证的多因素身份验证。
- **每次身份验证**。 一般的企业，想要启用一支庞大的外部用户很少需要身份验证的多因素身份验证。

有关定价的详细信息，请参阅[Azure MFA 定价。](http://azure.microsoft.com/pricing/details/multi-factor-authentication/)

选择最适合您的组织模型。  然后才能启动请参阅[入门 》](multi-factor-authentication-get-started.md)

## 为您选择的多因素安全解决方案

因为有几个不同特色的 Azure 多因素身份验证，我们必须确定几项事情为了弄清哪一个版本是使用正确的密码。  这些事项包括︰

-   [我要保护什么](#what-am-i-trying-to-secure)
-   [用户位于何处](#where-are-the-users-located)

以下各节将确定每个提供指导。

### 我要保护什么？

为了确定正确的多因素身份验证解决方案，首先我们必须先回答问题的是什么您想要使用的身份验证的第二个方法来保护。  它是在 Azure 应用程序？  或者是它的远程访问系统为例。  通过确定我们努力确保安全，我们将看到的多因素身份验证需要启用回答。  



您试图保护什么| 在云环境中的多因素身份验证|多因素身份验证服务器 
------------- | :-------------: | :-------------: |
第一方 Microsoft 应用程序|* |* |
Saas 应用程序的应用程序库中|* |* |
通过 Azure 应用程序代理广告发布的 IIS 应用程序|* |* |
IIS 应用程序不能通过 Azure 应用程序代理广告发布 | |* |
如 RDG VPN 的远程访问| |* |



### 用户位于何处

接下来，取决于用户的位置所在，我们可以确定适合的解决方案，无论它是在云环境中的多因素身份验证或者内部部署使用 MFA 服务器使用。



用户位置| 解决方案
------------- | :------------- | 
Azure 的活动目录| 在云环境中的多因素身份验证|
Azure 的 AD 和本地广告使用 AD FS 联合身份验证| 云中的 MFA 和 MFA 服务器都是可用的选项 
Azure 的 AD 和本地广告使用目录同步，Azure AD 同步，Azure AD 连接的任何密码同步|云中的 MFA 和 MFA 服务器都是可用的选项 
Azure 的 AD 和本地广告目录同步，Azure AD 同步，Azure AD 连接的使用密码同步|在云环境中的多因素身份验证
内部部署 Active Directory|多因素身份验证服务器

下表是一个比较的功能，是与在云环境中的多因素身份验证和多因素身份验证服务器。

 | 在云环境中的多因素身份验证 | 多因素身份验证服务器
------------- | :-------------: | :-------------: |
第二个因素为移动应用程序通知 | ● | ● |
第二个因素为移动应用程序验证代码 | ● | ●
第二个因素作为电话联络 | ● | ● 
第二个因素作为单向 SMS | ● | ●
第二个因素作为双向 SMS |  | ● 
第二个因素作为硬件令牌 |  | ● 
对于不支持 MFA 的客户端应用程序密码 | ● |  
管理控制身份验证方法 |  | ● 
PIN 模式 |  | ●
欺诈警报 | ● | ●
MFA 报告 | ● | ● 
一次性的旁路 | ● | ● 
自定义问候语的电话联络 | ● | ● 
可自定义来电显示的电话联络 | ● | ● 
信任的 Ip | ● | ● 
挂起 MFA 记忆设备 （公共预览） | ● |  
条件接收 | ● | ● 
高速缓存 | ● | ● 

现在，我们就能确定是否使用云多因素身份验证或 MFA 服务器内部，我们可以开始设置和使用 Azure 多因素身份验证。   **选择代表您的方案的图标 ！**

<center>




[![云](./media/multi-factor-authentication-get-started/cloud2.png)](multi-factor-authentication-get-started-cloud.md)  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[![Proofup](./media/multi-factor-authentication-get-started/server2.png)](multi-factor-authentication-get-started-server.md) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</center>

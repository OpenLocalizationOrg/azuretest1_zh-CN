---
ms.openlocfilehash: 8bb98ee08afbe7cf472acafb8fc7d657001ddc12
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="Azure 的多因素身份验证常见问题解答" 
    description="Azure 的多因素身份验证是验证您的身份的一种方法，需要使用多个用户名和密码。 它提供了额外的用户登录和交易的安全。" 
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
    ms.topic="article" 
    ms.date="08/24/2015" 
    ms.author="billmath"/>

# Azure 的多因素身份验证常见问题解答


本常见问题解答回答有关 Azure 多因素身份验证问题。 此常见问题解答解释有关使用该服务，包括计费模型和可用性问题。

## 常规

**问︰ 如何获取帮助 Azure 多因素身份验证？**

[搜索 Microsoft 知识库 (KB)](http://search.microsoft.com/supportresults.aspx?form=mssupport&q=phonefactor)

- 搜索 Microsoft 知识库 (KB) 的技术解决方案常见的中断修复问题，Microsoft Azure 多因素身份验证服务器 （电话因子） 的支持。

[Microsoft Azure Active Directory 论坛](https://social.msdn.microsoft.com/Forums/azure/home?forum=WindowsAzureAD)

- 您可以搜索和浏览的技术问题并回答来自社区或通过单击提出自己的问题[这里](https://social.msdn.microsoft.com/Forums/azure/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required)。

[密码重置](mailto:phonefactorsupport@microsoft.com)

- 旧式 Phonefactor 客户有任何的疑问围绕重置您的密码或帮助以获取您重新设置密码，请使用以下链接在线订购开启支持案例。

[Microsoft Azure 多因素身份验证服务器 （电话因子） 客户支持部门](https://support.microsoft.com/oas/default.aspx?prid=14947)

- 使用此链接可与 Microsoft 支持专业人员联系。 我们将要求您几个问题，以帮助我们确定您的支持选项。 支持选项可能包括电子邮件、 在线提交或电话支持。 

[有关 Microsoft Azure 多因素身份验证服务器 （电话因子） 的常规查询](http://azure.microsoft.com/services/multi-factor-authentication)

- 若要了解有关 Microsoft Azure 多因素身份验证服务器 （电话因子），或者如果有说法如何购买的产品和不同的支持选项的详细信息，请访问，或通过电子邮件发送[pfsales@microsoft.com](mailto:pfsales@microsoft.com)。 



**问︰ 如何 Azure 的多因素身份验证服务器无法处理用户数据？**

在使用内部部署多因素身份验证 (MFA) 服务器时，用户的数据存储在内部部署服务器。 没有永久的用户数据存储在云中。 当用户执行双因素身份验证时，MFA 服务器将数据发送到 Azure MFA 云服务来执行身份验证。 这些身份验证请求发送到云服务时，以下字段发送请求和日志中，以便可以在客户的身份验证中的使用报告。 这样可以启用或禁用在多因素身份验证服务器中，某些字段是可选的。 MFA 服务器从通信到 MFA 云服务使用通过端口 443 出站 SSL/TLS。 这些字段是︰

- 唯一 ID-用户名或内部 MFA 服务器 ID
- 第一个和最后一个名称-可选
- 电子邮件地址-可选
- 电话号码-进行语音呼叫或 SMS 身份验证时
- 令牌设备进行移动应用程序身份验证时
- 身份验证模式 
- 身份验证结果 
- MFA 服务器名称 
- MFA 服务器 IP 
- 客户端 IP--如果有的话



除了上面的域身份验证结果 （成功/拒绝） 和任何被拒的原因也是使用的身份验证数据存储并可通过身份验证中的使用报告。




## 帐单

**问︰ 我的公司会收取电话联络或文本的将我的用户进行身份验证的消息吗？**

所有成本都计入每用户或每个身份验证服务的成本。 公司不收取个别电话放或文本消息发送给您的最终用户使用 Azure 多因素身份验证。 电话所有者可能会从他们的电话运营商接收电话或短信的漫游相关或其他成本。

**"问︰ 如何组织记帐时间为 Azure 多因素身份验证？"**

在 Azure 管理门户中创建一个多因素身份验证提供程序时，每用户或每个身份验证选择帐单/使用模型。 它是基于消耗的资源记帐针对组织的 Azure 订阅，就像网站的虚拟机，根据预订计费等。

**问︰ 执行每用户计费模型费用根据用户启用了多因素身份验证或执行验证的用户数数吗？**

多因素身份验证为启用的用户数为基础计费。

## 可用性

**问︰ 如果我没有我的电话上收到响应，或者如果我忘记了我的手机做什么？**

如果您以前配置备份的电话，再试一次通过电话从登录页面提示时。  如果您没有配置的另一种方法，与您的管理员联系，要求他们更新编号分配到主电话 – 移动或办公室。

**问︰ 我从管理员角色中删除用户，但忘了禁用多因素身份验证和现在未显示在列表中如何删除这项功能？**

- 在左窗格中，使用哪个门户根据单击用户或用户和组。
- 选择您想要编辑的用户旁边的复选框，然后单击编辑或编辑图标。
- 在分派角色下，单击设置，选择是，将该用户添加回前面的管理角色。
- 请转到多因素身份验证页。 该帐户现在应页上的列表中显示。 
- 请按照上述步骤以禁用帐户的多因素身份验证。 
- 此时，您现在可以从管理角色删除帐户。


**问︰ 如果用户联系我，已被锁定的帐户具有管理员，怎么办？**

可以将用户重置强制用户重新经历注册过程。 为此请参阅[管理用户和使用 Azure 云环境中的多因素身份验证的设备设置](multi-factor-authentication-manage-users-and-devices.md)

**问︰ 如果用户已挂失有一个设备正在使用的应用程序密码怎么办？**

您可以删除所有用户应用程序密码，以防止任何未经授权的访问。 一旦更换设备，用户可以重新创建它们。 为此请参阅[管理用户和使用 Azure 云环境中的多因素身份验证的设备设置](multi-factor-authentication-manage-users-and-devices.md)

**问︰ 如果用户不能登录到非浏览器应用程序？**

- 多因素身份验证为启用的用户需要登录到某些非浏览器应用程序的应用程序密码。
- 用户将需要清除登录信息 （删除登录信息） 并重新启动应用程序中使用的符号的应用程序的用户名和密码。 

有关创建应用程序的密码信息，请参阅[使用帮助应用程序密码](multi-factor-authentication-end-user-app-passwords.md)


>[AZURE.NOTE] 现代办公室 2013年客户端的身份验证
>
> Office 2013 客户端 (包括 Outlook) 现在支持新的身份验证协议，并且可支持多因素身份验证。  这意味着，一旦启用，应用程序密码不是必需的 Office 2013 客户端使用。  有关更多信息，请参见[Office 2013 现代身份验证公共预览宣布](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/)。

**问︰ 如果我不会收到短信，或者回复双向文本消息但验证超时时怎么办？**

Azure 多因素身份验证服务将发送通过 SMS 聚合器的文本消息。 许多因素可能会影响文本消息传递和包括聚合函数使用的收据，目的地国家或地区，手机运营商和信号强度的可靠性。 因此，文本消息传递和接收 SMS 进行答复时执行双向 SMS 不能保证。 使用单向 SMS 是建议优先于双向 SMS 时可能因为它是更可靠，并可防止用户招致全球 SMS 费用由来自另一个国家/地区的短信回复。 

文本消息验证在美国和加拿大等一些国家也更可靠。 请改为选择的移动应用程序或电话联络的方法鼓励遇到困难时使用 Azure 多因素身份验证可靠接收文本消息的用户。 移动应用程序的优点，在于可以通过移动电话和 Wi-fi 连接，接收移动应用程序通知，即使设备根本没有信号显示移动应用程序密码。  Azure 身份验证器的应用程序是可用于[Windows Phone](http://www.windowsphone.com/store/app/azure-authenticator/03a5b2bf-6066-418f-b569-e8aecbc06e50)、 [Android](https://play.google.com/store/apps/details?id=com.azure.authenticator)，和[IOS](https://itunes.apple.com/us/app/azure-authenticator/id983156458)。


## Errors

**问︰ 当我看到"出现身份验证请求激活帐户不是"出错时我通过移动应用程序通知进行身份验证时怎么办？**

- 转到[https://account.activedirectory.windowsazure.com/profile/](https://account.activedirectory.windowsazure.com/profile/) ，并使用您组织的帐户登录。
- 如果需要请单击其他的验证选项，选择不同的选项来完成帐户验证。
- 单击附加的安全性验证。
- 从移动应用程序中删除现有帐户。
- 单击配置，然后按照说明进行操作以重新配置移动应用程序。




**问︰ 当我在试着使用非浏览器应用程序登录时看到一个 0x800434D4L 错误时怎么办？**

目前，其他安全验证仅可与应用程序/服务，您可以通过浏览器访问。 非浏览器的应用程序 （也称为胖客户端应用程序） 如 Windows Powershell 无法在所需的更高的安全性验证的帐户使用本地计算机安装。 在这种情况下，您可能看到应用程序生成错误 0x800434D4L。

对于这一种解决方法是让非管理员操作与管理相关的操作的单独的用户帐户。 以后可以链接之间您的管理帐户和非管理帐户的邮箱，因此您可以登录到 outlook 使用非管理员帐户。 关于这的更多详细信息，请参阅[使管理员能够打开和查看用户的邮箱的内容](http://help.outlook.com/141/gg709759(d=loband).aspx?sl=1)。










---
ms.openlocfilehash: aab9631b5dee5cb4cb191e138aa4d01167907701
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="获取观察︰ Azure AD 密码管理报告 |Microsoft Azure" 
    description="本文介绍如何使用报表来获取组织中深入了解密码管理操作。" 
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

# 如何获取密码管理与运营经验报告
本部分介绍如何使用 Azure 活动目录的密码管理报表以查看用户如何使用密码重置和更改您的组织中。

- [**密码管理报告概述**](#overview-of-password-management-reports)
- [**如何查看密码管理报告**](#how-to-view-password-management-reports)
- [**查看密码重置注册您的组织中的活动**](#view-password-reset-registration-activity)
- [**查看密码重新设置您的组织中的活动**](#view-password-reset-activity)

## 密码管理报告概述
一旦部署密码重置时，最常见的下一个步骤之一是如何在您的组织正在使用，请参阅。  例如，您可能需要获得真知灼见到用户密码重置，或多少密码的注册方式重置已完成在过去的几天内。  以下是一些常见的问题，您将能够回答当今[Azure 管理门户](https://manage.windowsazure.com)中的密码管理报表︰

- 究竟有多少人已注册的密码重置？
- 谁已被注册密码重置？
- 人要注册的数据？
- 究竟有多少人在过去的 7 天内重置密码？
- 什么是最常见的方法用户或管理员使用重置其密码？
- 什么是公共问题，用户或管理员脸时尝试使用密码重置？
- 哪些管理员正在频繁地重设自己的密码？
- 是否有任何可疑的活动，重新设置密码的发展？


## 如何查看密码管理报告
若要查找密码管理报表，请按照以下步骤︰

1.  在**Active Directory**扩展[Azure 管理门户](https://manage.windowsazure.com)中单击。
2.  从门户中显示的列表中选择您的目录。
3.  在**报表**选项卡上单击。
4.  查看在**活动日志**部分。
5.  选择**密码重置活动**报告或**重置密码注册的活动**报告。

    ![][001]

## 查看密码重置注册的活动

密码重置注册活动报告显示所有的密码重设登记您的组织中发生的。  密码重置注册将显示在该报表中的任何用户都已成功注册在密码重置注册门户网站 ([http://aka.ms/ssprsetup](http://aka.ms/ssprsetup)) 的身份验证信息。

- **最大时间范围**︰ 1 个月
- **最大行数**︰ 不限
- **可下载字体**︰ 是的通过 CSV 文件

    ![][002]

### 报表列的说明
下表介绍每个报表中的列详细信息︰

- **用户**-用户的尝试进行了密码重置注册操作。
- **角色**--在目录中的用户角色。
- **日期和时间**的日期和时间的尝试。
- **注册数据**– 用户提供密码的身份验证数据重置注册。

### 报告的值的说明
下表描述了不同允许为每个列的值︰

列|允许的值以及它们的含义
---|---
注册数据| **备选电子邮件**– 用户使用备用电子邮件或进行身份验证的身份验证电子邮件<p><p>**办公室电话**– 进行身份验证的用户使用的办公室电话<p>**移动电话**-用户使用移动电话或进行身份验证的身份验证电话<p>**安全问题**– 用户使用安全问题进行身份验证<p>**以上的任意组合 （如备用电子邮件和移动电话）** — 2 门策略指定并显示的两种方法使用的用户时发生身份验证密码重置请求。

## 查看密码重置活动

此报表显示所有密码重都置尝试在您的组织中发生的。

- **最大时间范围**︰ 1 个月
- **最大行数**︰ 不限
- **可下载字体**︰ 是的通过 CSV 文件

    ![][003]

### 报表列的说明
下表介绍每个报表中的列详细信息︰

1. **用户**-用户的尝试进行了密码重置操作 （根据提供当用户重置密码的用户 ID 字段）。
2. **角色**--在目录中的用户角色。
3. **日期和时间**的日期和时间的尝试。
4. **方法使用**– 此用户使用哪种身份验证方法重置操作。
5. **结果**– 最终结果的密码重置操作。
6. **详细信息**– 为什么重设密码的详细信息会导致以前的值。  此外包括任何缓解可能采取的步骤来解决错误。

### 报告的值的说明
下表描述了不同允许为每个列的值︰


列|允许的值以及它们的含义
---|---
使用的方法|**备选电子邮件**– 用户使用备用电子邮件或进行身份验证的身份验证电子邮件<p>**办公室电话**– 进行身份验证的用户使用的办公室电话<p>**移动电话**-用户使用移动电话或进行身份验证的身份验证电话<p>**安全问题**– 用户使用安全问题进行身份验证<p>**以上的任意组合 （如备用电子邮件和移动电话）** — 2 门策略指定并显示的两种方法使用的用户时发生身份验证密码重置请求。
说明|**已放弃**– 用户启动密码重置，但又停止一半通过而无需填写<p>**已阻止**– 用户帐户无法使用密码重设因试图使用密码重置页面或单个重设密码门在 24 小时内多次<p>**已取消**-用户启动密码重置，但是然后又单击了取消按钮取消部分能通过会话 <p>**联系管理**– 用户他无法解析其会话期间存在的问题，以便在用户单击"与您的管理员联系"链接而不是精加工的密码重置流<p>**失败**-用户无法重置密码，可能是因为用户没有配置要使用的功能 （例如没有许可证，则会为缺少身份验证信息，密码管理 prem 但写回已关闭）。<p>**成功**-重置密码成功。
详细信息|请参见下表

### 允许明细数据列的值
下面是您可能会希望使用密码重置活动报告的结果类型的列表︰

详细信息 | 结果类型 
----|----
用户完成电子邮件验证选项后放弃  | 放弃
用户在完成移动 SMS 验证选项后放弃|放弃 
用户在完成移动语音通话验证选项后放弃 | 放弃 
用户完成 office 语音呼叫验证选项后放弃 | 放弃
用户放弃后完成安全问题的选项|放弃 
用户输入他们的用户 ID 后放弃| 放弃 
用户启动的电子邮件验证选项后放弃|放弃
用户启动的移动 SMS 验证选项后放弃|放弃
用户启动的移动语音通话验证选项后放弃|放弃
用户启动的 office 语音呼叫验证选项后放弃|放弃
用户启动的安全问题选项后宣布放弃| 放弃
用户在选择新密码之前放弃| 放弃
用户选择一个新密码时放弃| 放弃
用户输入太多无效的 SMS 验证代码，24 小时内被阻止|已阻止
用户已移动电话语音验证尝试次数太多，24 小时被阻止|已阻止
用户尝试次数过多的办公室电话语音验证和 24 小时内被阻止 |已阻止
用户试图回答次数过多的安全问题，24 小时内被阻止| 已阻止
用户试图验证电话号码次数过多，24 小时被阻止|已阻止
用户取消了之前传递所需的身份验证方法|Cancelled
用户已取消提交新密码之前|Cancelled
用户尝试安装电子邮件验证选项后联系管理员 |联系的管理员
用户尝试移动 SMS 验证选项后联系管理员|联系的管理员
用户尝试移动语音通话验证选项后联系管理员|联系的管理员
用户尝试安装 office 语音呼叫验证选项后联系管理员 |联系的管理员
用户尝试安全问题验证选项后联系管理员|联系的管理员
没有为该用户启用密码重置。 启用密码重设在配置选项卡来解决此问题|  失败
用户没有许可证。 可以将许可证添加到用户若要解决此问题|失败
已尝试重置设备没有启用 cookie 的用户| 失败
用户帐户具有足够的身份验证方法定义。 添加身份验证信息，用于解决此问题|失败
用户的密码是在内部托管。 您可以启用密码写回，若要解决此问题|失败
我们无法到达您的本地密码重置服务。 检查同步计算机的事件日志|失败
在重置用户的本地密码时，我们遇到了问题。 检查同步计算机的事件日志 | 失败
此用户不是密码重置用户组的成员。 将此用户添加到该组，若要解决此问题。|失败
完全为此租户已禁用密码重置。 看到[这里](http://aka.ms/ssprtroubleshoot)来解决这一问题。 | 失败 
用户成功地重置密码|成功

**其他资源**


* [什么是密码管理](active-directory-passwords.md)
* [密码管理的工作原理](active-directory-passwords-how-it-works.md)
* [密码管理入门](active-directory-passwords-getting-started.md)
* [自定义密码管理](active-directory-passwords-customize.md)
* [密码管理最佳做法](active-directory-passwords-best-practices.md)
* [密码管理常见问题解答](active-directory-passwords-faq.md)
* [解决密码管理](active-directory-passwords-troubleshoot.md)
* [了解更多信息](active-directory-passwords-learn-more.md)
* [在 MSDN 上的密码管理](https://msdn.microsoft.com/library/azure/dn510386.aspx)



[001]: ./media/active-directory-passwords-get-insights/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-get-insights/002.jpg "Image_002.jpg"
[003]: ./media/active-directory-passwords-get-insights/003.jpg "Image_003.jpg"
 

测试

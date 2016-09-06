---
ms.openlocfilehash: 04ec2fe3ee255f0a98d32db64e6f72a83d50934e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="故障排除指南-Api 的 azure 移动服务" 
   description="故障排除指南 Azure 移动服务的 Api" 
   services="mobile-engagement" 
   documentationCenter="" 
   authors="piyushjo" 
   manager="dwrede" 
   editor=""/>

<tags
   ms.service="mobile-engagement"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="mobile-multiple"
   ms.workload="mobile" 
   ms.date="06/18/2015"
   ms.author="piyushjo"/>

# 对于 API 的问题的故障排除指南

下面是可能出现的问题，您可能会遇到与管理员与 Azure 移动服务通过 Api 的交互方式。

## 语法问题

### 问题
- 使用 API （或意外的行为） 的语法错误。

### 原因

- 语法问题︰
    - 一定要检查的特定 API 用来确认选项可用语法。
    - 与 API 使用一个常见的问题是混淆到达 API 和推 API （应与到达 API 而不是推 API 执行大多数任务）。 
    - 与 SDK 集成和 API 使用另一个常见的问题是混淆的 SDK 键和 API 密钥。
    - 对 Api 连接的脚本需要至少每隔 10 分钟发送数据或连接可能会超时 （特别是通用数据侦听监视器 API 脚本中）。 为了防止超时，有脚本发送 XMPP ping 每隔 10 分钟来保持会话处于活动状态的服务器。

### 请参见
 
- [API 文档][链接 4]
- [XMPP 协议信息]( http://xmpp.org/extensions/xep-0199.html)
 
## 不能使用 API 来执行相同的操作在 Azure 的移动服务用户界面中可用

### 问题
- 从相关的 Azure 移动服务 API，从 Azure 移动服务用户界面的工作方式的操作不起作用。

### 原因

- 确认您可以从 Azure 移动服务用户界面执行相同的操作显示，具有正确使用 SDK 集成 Azure 移动签约的这一功能。

### 请参见
 
- [用户界面文档][链接 1]
 
## 错误消息

### 问题
- 使用 API 在运行时或在日志中显示错误代码。

### 原因

- 下面是公共 API 状态代码编号，以便参考和初步诊断复合列表︰

        200        Success.
        200        Account updated: device registered, associated, updated, or removed from the current account.
        200        Returns a list of projects as a JSON object or an authentication token generated and returned in the response’s body.
        201        Account created.
        400        Invalid parameter or validation exception (check payload for details). The parameters provided to the API or service are invalid. In this case, the HTTP response will embed more details. Make sure to test for the MIME type of the response as the payload can either be plain text or a JSON object.
        401        Authentication error. No user is currently authenticated or connected (check the AppID and SDK key).
        402        Billing lock. The application is either off its quotas or is currently in a bad billing state.
        403        The application is not enabled or the specific API is disabled for this application.
        403        Unauthorized access to the project or application, invalid access key (the key must match the one provided when created).
        403        Campaign specific errors: campaign must be finished (or has already been activated), the suspend action can only be performed on an scheduled campaign, cannot finish a campaign that is not currently “in progress”, campaign must be “in progress” and the campaign’s property named, manual Push must be set to true.
        403        The email address is already associated to another account (a super user for instance). No authentication token will be generated.
        404        Application, device, campaign, or project identifier not found.
        404        Query parameter is invalid JSON or has a field with an unexpected value.
        404        The email address is not associated with an account. Please create or update the account first.
        405        Invalid HTTP method (GET, POST, etc.) or trying to edit a read only segment (i.e. add or update or delete a criterion). A segment becomes read only after it has been computed for the first time.
        409        Name already associated to a different device ID or campaign.
        413        Too many device identifiers (current limit is 1,000), POST URL encoded entity is over 2MB, or the period is too large to be displayed (the server didn’t retrieve the analytics because the user request is for a period that is too large).
        503        Analytics not available yet (the requested information is not computed yet for an application).
        504        The server was not able to handle your request in a reasonable time (if you make multiple calls to an API very quickly, try to make one call at a time and spread the calls out over time).

### 请参见

- [API 文档-在每个特定的 API 的详细错误][链接 4]
 
## 静默失败

### 问题
- API 操作将失败，并显示在运行时或在日志中没有错误消息。

### 原因

- 多个项如果它们不正常，集成将 Azure 移动服务用户界面中禁用但将悄悄从 API，所以请记住，测试用户界面以查看其是否正常工作中相同的功能。
- Azure 的移动服务，并且您尝试使用 Azure 移动服务的许多高级的功能需要单独集成到 sdk 应用程序作为独立的步骤之前，您可以使用它们。

### 请参见

- [故障排除指南-SDK][链接 25]
 
<!--Link references-->
[链接 1]: mobile-engagement-user-interface.md
[链接 2]: mobile-engagement-troubleshooting-guide.md
[链接 3]: mobile-engagement-how-tos.md
[链接 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[链接 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[6 链接]: http://go.microsoft.com/fwlink/?LinkId=525555
[链接 7]: https://account.windowsazure.com/PreviewFeatures
[链接 8]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=azuremobileengagement
[链接 9]: http://azure.microsoft.com/en-us/services/mobile-engagement/
[10 链接]: http://azure.microsoft.com/en-us/documentation/services/mobile-engagement/
[链接 11]: http://azure.microsoft.com/en-us/pricing/details/mobile-engagement/
[链接 12]: mobile-engagement-user-interface-navigation.md
[链接 13]: mobile-engagement-user-interface-home.md
[链接 14]: mobile-engagement-user-interface-my-account.md
[15 链接]: mobile-engagement-user-interface-analytics.md
[链接 16]: mobile-engagement-user-interface-monitor.md
[链接 17]: mobile-engagement-user-interface-reach.md
[链接 18]: mobile-engagement-user-interface-segments.md
[链接 19]: mobile-engagement-user-interface-dashboard.md
[链接 20]: mobile-engagement-user-interface-settings.md
[链接 21]: mobile-engagement-troubleshooting-guide-analytics.md
[链接 22]: mobile-engagement-troubleshooting-guide-apis.md
[链接 23]: mobile-engagement-troubleshooting-guide-push-reach.md
[链接 24]: mobile-engagement-troubleshooting-guide-service.md
[链接 25]: mobile-engagement-troubleshooting-guide-sdk.md
[链接 26]: mobile-engagement-troubleshooting-guide-sr-info.md
[链接 27]: mobile-engagement-user-interface-reach-campaign.md
[链接 28]: mobile-engagement-user-interface-reach-criterion.md
[链接 29]: mobile-engagement-user-interface-reach-content.md
 
测试

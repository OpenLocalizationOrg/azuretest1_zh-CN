---
ms.openlocfilehash: 3cc64f25ab61455e4cd72d572c24075c3ca2841d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="第一次使用 Azure 多因素身份验证登录" 
    description="本页面描述了什么是用户体验第一次他们登录。" 
    services="multi-factor-authentication" 
    documentationCenter="" 
    authors="billmath" 
    manager="stevenp" 
    editor="curtland"/>

<tags 
    ms.service="multi-factor-authentication" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/24/2015" 
    ms.author="billmath"/>
# Azure 多因素身份验证的安装体验

 管理员已配置您的帐户需要密码和手机的响应必须用于验证您的身份时，将使用其他安全验证设置。 如果管理员已经配置了您的帐户需要更高的安全性验证，**您将不能登录直到完成自动注册进程**。 

## 确定如何将使用多因素身份验证

 第一次登录后您的帐户已配置，则将提示开始自动注册过程。  您可以通过单击开始此过程**现在设置。** 

![安装](./media/multi-factor-authentication-end-user-first-time/first.png)

使用注册过程可以指定您的验证的首选的方法。  这可以是下表中下列任意项。  有关其他信息，包括预排只需单击一种方法。

方法|说明
:------------- | :------------- | 
[移动电话](multi-factor-authentication-end-user-first-time-mobile-phone.md)|  将放置到身份验证电话自动的语音电话。 用户应答呼叫，然后按 # 在电话键盘进行身份验证。 该号码将不会同步到内部部署 Active Directory 中。
[手机短信](multi-factor-authentication-end-user-first-time-mobile-phone.md)|发送包含一个验证码给用户的文本消息。 回复短信验证码或登录界面中输入验证码到提示用户。
[办公电话](multi-factor-authentication-end-user-first-time-office-phone.md)|将向用户自动的语音电话。 用户应答呼叫，然后按 # 在电话键盘进行身份验证。
[移动应用程序](multi-factor-authentication-end-user-first-time-mobile-app.md)|将通知用户的智能手机或 tablet 推入到多元的移动应用程序。 在应用程序中进行身份验证，用户点击"确认"。 或者，应用程序也可作为 OTP 令牌用于离线的身份验证。 用户输入到登录屏幕中进行身份验证的令牌。<br><p>  多因素身份验证的应用程序可以运行在 2 不同的模式，提供多因素身份验证服务，可以提供额外的安全性。 这些优点如下︰<li>**通知**-在此模式下，多因素身份验证的应用程序可避免未经授权的访问帐户和停止欺诈交易。 这是使用推式通知给您的电话或已注册的设备。 只需查看通知，如果是合理的选择进行身份验证。 否则为可能会选择拒绝或选择拒绝并报告欺诈的通知。 报告虚假通知有关如何使用拒绝和报告欺诈功能多因素身份验证。</li><p><li>**一次性密码**-在此模式下，多因素身份验证的应用程序可作为一个软件令牌生成的誓言验证代码。 然后可以将此验证码输入以及用户名和密码以提供第二个窗体的身份验证。</li><br><p> Azure 身份验证器的应用程序是可用于[Windows Phone](http://www.windowsphone.com/en-us/store/app/azure-authenticator/03a5b2bf-6066-418f-b569-e8aecbc06e50)、 [Android](https://play.google.com/store/apps/details?id=com.azure.authenticator)，和[IOS](https://itunes.apple.com/us/app/azure-authenticator/id983156458)。

 

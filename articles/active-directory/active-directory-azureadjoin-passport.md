---
ms.openlocfilehash: 164a3785ea2d0f68b2d065f0456a86415ed6a67b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="设置 Azure 广告通过首次运行体验的新设备 |Microsoft Azure" 
    description="解释如何用户设置 Azure 广告加入在其首次运行体验过程的主题。" 
    services="active-directory" 
    documentationCenter="" 
    authors="femila" 
    manager="swadhwa" 
    editor=""/>

<tags 
    ms.service="active-directory" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/02/2015" 
    ms.author="femila"/>

# 没有通过 Microsoft Passport 密码身份验证身份

使用单独的密码进行身份验证的当前方法都不能保证用户的安全。 用户重用，并且忘记了密码。 密码是 breachable，phishable，容易产生裂纹，并猜出。 他们还将获取难以记住并且易于遭受攻击，如"通过哈希"。

## 什么是 Microsoft 护照
Passport 是一个新的专用/公共密钥或超出密码的基于证书的身份验证方法为企业和消费者。 这种形式的身份验证依赖于这些密钥对凭据，可以替换密码并能够抵抗破坏，失窃案和网络钓鱼 Microsoft Passport 让 Microsoft 帐户、 Active Directory 帐户、 Microsoft Azure 活动目录 (AD) 帐户或非 Microsoft 服务支持快速 ID 联机 (FIDO) 身份验证进行身份验证的用户。 最初的两步验证之后 Microsoft Passport 注册时，Microsoft Passport 用户的设备上设置和用户设置笔势，它可以是 Windows 你好或 PIN。 用户提供的笔势来验证身份。Windows 使用 Microsoft Passport 来验证用户的身份，并帮助他们访问受保护的资源和服务。

由只能通过"用户笔势"如针、 生物、 远程设备，如智能卡用户用于登录到设备，此信息链接到证书或非对称密钥对的私钥。 此私钥是 attested，如果设备具有受信任的平台模块 (TPM) 芯片的硬件。 私钥从不离开设备。

公共密钥注册 Azure Active Directory 和 Windows 服务器的 Active Directory （用于内部部署）。 身份提供程序 (IDPs) 通过映射的用户的公钥的私钥来验证用户，并提供登录信息通过一次性密码 (OTP)、 Phonefactor 或不同的通知机制。
## 企业为什么采用 Microsoft Passport
通过启用 Microsoft Passport，企业可以进行他们的资源通过更加安全︰

* 这意味着，将生成密钥在 TPM 1.2 或 TPM 2.0 时可用，通过软件 TPM 不可用时使用硬件首选选项设置 Microsoft Passport，请。 

* 定义的复杂性和长度的 PIN，以及是否在您的组织中启用呼叫使用

* 配置 Microsoft Passport 来支持智能卡式方案使用基于证书的信任。

## 它是如何工作的
1. 在硬件上生成密钥。 大量的计算机具有通过集成到设备加密密钥保护硬件的内置可信的平台模块 (TPM) 芯片。 TPM 1.2 或 TPM 2.0 用于生成密钥或将从生成的密钥相关的证书。

2. 这些硬件绑定项 attested 由 TPM 和企业可以 

3. 解除锁定单势将解锁设备，将允许此笔势来加入域或 Azure 广告加入设备是否获得访问多个资源。 

## Passport 生命周期
Microsoft Passport 身份验证生命周期![](./media/active-directory-azureadjoin/active-directory-azureadjoin-microsoft-passport.png)上面的关系图说明了专用公共密钥对和标识提供程序的验证。 下面详细说明每个步骤︰

1. 用户通过手势、 物理智能卡 （多因素身份验证） 的多个内置校对方法证明其身份，并将此信息发送到身份提供程序 (IDP) Azure Active Directory 或 Active Directory 等。

2.  设备然后创建键、 证明该项、 采用此密钥的公共部分、 附加它站语句、 登陆并发送到 IDP 注册此注册表项。 

3. 只要在 IDP 中注册的密钥的公共部分，它面临的挑战要使用密钥的隐私部分签名的设备。 IDP 然后验证并发出允许用户访问受保护的资源的身份验证令牌。

4. 只要在 IDP 中注册的密钥的公共部分，它挑战的设备的挑战，要用专用部分密钥进行签名。 

5. IDP 然后验证并发出身份验证令牌，您的用户和设备访问受保护的资源。 IDPs 可以编写跨平台的应用程序，或使用浏览器支持通过 JS/Webcrypto Api） 来创建，并为他们的用户使用 Microsoft Passport 凭据。

## 部署的要求。
在企业级
---------------------------
* Azure 的订阅

在用户级别
-------------------------------------------------------------
* 计算机必须运行 Windows 10 专业版或者企业 SKU

## 其他信息

* [扩展到 Windows 10 设备通过 Azure 活动目录加入云功能](active-directory-azureadjoin-user-upgrade.md)
* [设置 Azure 广告加入](active-directory-azureadjoin-setup.md)


测试

---
ms.openlocfilehash: 546c960c3e5446f16b39673d1d609e706ab98613
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="移动应用程序用作 Azure MFA 与您联系的方法" 
    description="此页将显示用户如何使用 Azure MFA 作为主要的联系方法移动应用程序。" 
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

# 作为联系与 Azure 多因素身份验证方法使用移动应用程序

如果您想要用作主要联系方式的移动应用程序可以使用这篇文章。  它将引导您完成设置多因素身份验证，您的移动应用程序用作主要联系方式。

Azure 身份验证器的应用程序是可用于[Windows Phone](http://www.windowsphone.com/en-us/store/app/azure-authenticator/03a5b2bf-6066-418f-b569-e8aecbc06e50)、 [Android](https://play.google.com/store/apps/details?id=com.azure.authenticator)，和[IOS](https://itunes.apple.com/us/app/azure-authenticator/id983156458)。

## 移动应用程序作为您的联系方式


- 从下拉列表中选择移动应用程序。


![安装](./media/multi-factor-authentication-end-user-first-time-mobile-app/mobileapp.png)

- 选择通知或一次性密码，单击设置。
- Azure 身份验证应用程序安装了电话，在启动应用程序，然后单击扫描条形码。  要添加已有 Azure MFA 或第三方帐户的帐户，请参阅[手动添加帐户](#adding-an-account-manually)。

![安装](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan.png)

- 扫描条码图片所附带的配置移动应用程序屏幕。  单击完成以关闭条形码屏幕。  如果无法获取条码进行扫描，您可以手动输入信息。

![安装](./media/multi-factor-authentication-end-user-first-time-mobile-app/barcode.png)

- 在电话上，就会开始激活，完成后请单击联系我。  这将发送一个通知或验证代码到您的电话。  单击验证。

![安装](./media/multi-factor-authentication-end-user-first-time-mobile-app/verify.png)

- 单击关闭。  在这种情况下，验证应该会成功。
- 现在，输入您的移动电话号码，以防您无法访问您的移动应用程序的建议。
- 指定下拉列表从国家和国家旁边的框中输入您的移动电话号码。  单击下一步。
- 在这种情况下，必须设置您的联系方式，现在是时候为安装应用程序密码非浏览器应用程序如 Outlook 2010 或更旧的。 如果您不使用这些应用程序单击**完成**。  否则继续执行下一步。

![安装](./media/multi-factor-authentication-end-user-first-time-mobile-app/step4.png)

- 如果正在使用这些应用程序然后提供的应用程序密码复制并粘贴到非浏览器应用程序的密码。 有关 Outlook 和 Lync 单个应用程序的步骤请参阅如何在您的电子邮件将密码更改为应用程序密码以及如何更改在您的应用程序到应用程序密码的密码。
- 单击完成。


## 手动添加帐户
如果您想手动添加帐户，选择输入帐户手动按钮。  

![安装](./media/multi-factor-authentication-end-user-first-time-mobile-app/addaccount.png)

现在如果您有已经 Azure MFA 帐户，输入的代码和条码将显示同一页提供的 url。  这将在代码和 url 框在移动应用程序上。  这将开始激活。

![安装](./media/multi-factor-authentication-end-user-first-time-mobile-app/barcode2.png)

完成后单击联系我。 这将发送一个通知或验证代码到您的电话。 单击验证。  完成后，按照上述数字 6 处开始。

如果您正在使用第三方移动应用程序的帐户提供的框中输入帐户名称和安全密钥，然后激活该帐户。  一旦您完成这些操作并验证帐户，按照上述数字 6 处开始。


![安装](./media/multi-factor-authentication-end-user-first-time-mobile-app/add3rdparty.png)

>[AZURE.NOTE]如果您看到"添加工作帐户"，这是联接的工作场所而不是针对多因素身份验证。  您可以忽略此。
 

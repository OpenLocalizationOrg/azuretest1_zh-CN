---
ms.openlocfilehash: 5d099b845714554b02b80f0379d4896553750340
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何拨打电话 Twilio (PHP) |Microsoft Azure" 
    description="了解如何拨打电话和发送短消息与 Twilio API 服务在 Azure 上。 PHP 应用程序的示例。" 
    documentationCenter="php" 
    services="" 
    authors="devinrader" 
    manager="twilio" 
    editor="mollybos"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="PHP" 
    ms.topic="article" 
    ms.date="11/25/2014" 
    ms.author="microsofthelp@twilio.com"/>

# 如何使在 Azure 上 PHP 应用程序中使用 Twilio 电话呼叫 

下面的示例演示如何使用 Twilio 从 PHP 网页在 Azure 托管调用。 生成的应用程序将提示用户输入电话值，如下面的屏幕快照中所示。

![使用 Twilio 和 PHP 的 azure 调用窗体][twilio_php]

您将需要执行下列操作以使用本主题中的代码︰

1. 获得 Twilio 帐户和身份验证令牌。 若要开始使用 Twilio，评估定价在[http://www.twilio.com/pricing][twilio_pricing]。 您可以在[https://www.twilio.com/try-twilio][try_twilio]的试用帐户注册。 有关 API 由 Twilio 提供的信息，请参阅[http://www.twilio.com/api][twilio_api]。
2. 获得 PHP Twilio 库。 可以从 GitHub ([https://github.com/twilio/twilio-php][twilio_php_github]) 下载或将其作为梨树包安装。 有关详细信息，请参阅[https://github.com/twilio/twilio-php/blob/master/README.md][twilio_github_readme]。
3. 为 PHP 安装 Azure 的 SDK。 有关 SDK 和安装说明的概述，请参阅[设置 php Azure SDK][setup_php_sdk]。

## 创建 web 窗体发出呼叫

以下 HTML 代码演示如何生成检索呼叫用户数据的 web 页 (**callform.html**):

    <html>
    <head>
        <title>Automated call form</title>
    </head>
    <body>
    <h1>Automated Call Form</h1>
    <p>Fill in all fields and click <b>Make this call</b>.</p>
    <form action="makecall.php" method="post">
    <table>
        <tr>
            <td>To:</td>
            <td><input type="text" size=50 name="callTo" value=""></td>
        </tr>
        <tr>
            <td>From:</td>
            <td><input type="text" size=50 name="callFrom" value=""></td>
        </tr>
        <tr>
            <td>Call message:</td>
            <td><input type="text" size=100 name="callText" value="Hello. This is the call text. Good bye." /></td>
        </tr>
        <tr>
            <td colspan=2><input type="submit" value="Make this call"></td>
        </tr>
    </table>
    </form>
    <br/>
    </body>
    </html>

## 创建的代码进行调用
下面的代码演示如何生成 web 页 (**makecall.php**)，当用户提交窗体显示的**callform.html**调用。 以下代码创建调用消息并生成调用。 （使用您的 Twilio 帐户和身份验证令牌，而不是分配给了**$sid**和**$token**下面的代码中的占位符值）。

    <html>
    <head><title>Making call...</title></head>
    <body>
    <p>Your call is being made.</p>

    <?php
    require_once 'Services/Twilio.php';

    $sid = "your_account_sid";
    $token = "your_authentication_token";

    $from_number = $_POST['callFrom']; // Calls must be made from a registered Twilio number.
    $to_number = $_POST['callTo'];
    $message = $_POST['callText'];

    $client = new Services_Twilio($sid, $token, "2010-04-01");

    $call = $client->account->calls->create(
        $from_number, 
        $to_number,
        'http://twimlets.com/message?Message='.urlencode($message)
    );

    echo "Call status: ".$call->status."<br />";
    echo "URI resource: ".$call->uri."<br />";
    ?>
    </body>
    </html>

除了进行调用， **makecall.php**显示某些调用的元数据 （如下面的屏幕截图所示）。 有关调用元数据的详细信息，请参阅[https://www.twilio.com/docs/api/rest/call#instance-properties][twilio_call_properties]。

![使用 Twilio 和 PHP 的 azure 调用响应][twilio_php_response]

## 运行应用程序
下一步是部署到 Azure 网站应用程序。 下列文章包含的信息用于创建网站和部署使用 Git、 FTP 或 WebMatrix 代码 （虽然不是所有信息的每篇文章都是相关的）︰

* [创建 PHP MySQL Azure 网站和部署使用 Git][网站 git]
* [创建 PHP MySQL Azure 网站并使用 FTP 部署][网站的 ftp]
* [创建和部署使用 WebMatrix PHP MySQL Azure 网站][网站 webmatrix]

## 下一步行动
提供此代码来显示您在 PHP 在 Azure 上使用 Twilio 的基本功能。 在生产环境中部署到 Azure 之前, 可能需要添加更多错误处理或其他功能。 例如︰

* 而不是使用 web 窗体，可以使用 Azure 存储 blob 或 SQL 数据库来存储电话号码并调用文本。 在 PHP 中使用 Azure 存储 blob 信息，请参阅[使用 PHP 应用程序中使用 Azure 存储][howto_blob_storage_php]。 在 PHP 中使用 SQL 数据库的信息，请参阅[使用 PHP 应用程序中使用 SQL 数据库][howto_sql_azure_php]。
* **Makecall.php**代码使用 Twilio 提供的 URL ([http://twimlets.com/message][twimlet_message_url]) 提供的 Twilio 标记语言 (TwiML) 响应，如何继续执行该调用通知 Twilio。 例如，可以包含返回 TwiML`<Say>`调用收件人正在说话的文字会导致的谓词。 而不是使用 Twilio 提供的 URL，您可以构建自己的服务响应 Twilio 的请求;有关详细信息，请参阅[如何使用语音和 PHP 中的 SMS 功能的 Twilio][howto_twilio_voice_sms_php]。 有关 TwiML 的详细信息可从[http://www.twilio.com/docs/api/twiml][twiml]，并详细了解`<Say>`和[http://www.twilio.com/docs/api/twiml/say][twilio_say]可以找到其他 Twilio 动词。
* 阅读在[https://www.twilio.com/docs/security][twilio_docs_security]的 Twilio 安全准则。

Twilio 有关的其他信息，请参阅[https://www.twilio.com/docs][twilio_docs]。

## 请参见
* [如何使用 Twilio 进行语音和 PHP 中的 SMS 功能](partner-twilio-php-how-to-use-voice-sms.md)

[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/api
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[twilio_php]: https://github.com/twilio/twilio-php
[twilio_github_readme]: https://github.com/twilio/twilio-php/blob/master/README.md
[setup_php_sdk]: http://azurephp.interoperabilitybridges.com/articles/setup-the-windows-azure-sdk-for-php
[twimlet_message_url]: http://twimlets.com/message
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api_service]: http://api.twilio.com
[build_php_azure_app]: http://azurephp.interoperabilitybridges.com/articles/build-and-deploy-a-windows-azure-php-application
[howto_twilio_voice_sms_php]: partner-twilio-php-how-to-use-voice-sms.md
[howto_blob_storage_php]: http://azure.microsoft.com/documentation/articles/storage-php-how-to-use-blobs/
[howto_sql_azure_php]: http://azure.microsoft.com/documentation/articles/sql-database-php-how-to-use/
[twilio_call_properties]: https://www.twilio.com/docs/api/rest/call#instance-properties
[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say
[ssl_validation]: http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html
[twilio_php]: ./media/partner-twilio-php-make-phone-call/WA_TwilioPHPCallForm.jpg
[twilio_php_response]: ./media/partner-twilio-php-make-phone-call/WA_TwilioPHPMakeCall.jpg
[网站 git]: https://www.windowsazure.com/develop/php/tutorials/website-w-mysql-and-git/
[网站的 ftp]: https://www.windowsazure.com/develop/php/tutorials/website-w-mysql-and-ftp/
[网站 webmatrix]: https://www.windowsazure.com/develop/php/tutorials/website-w-mysql-and-webmatrix/
[twilio_php_github]: https://github.com/twilio/twilio-php

测试

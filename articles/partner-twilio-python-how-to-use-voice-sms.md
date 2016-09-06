---
ms.openlocfilehash: 761cdfd9748d742b43ef049e28d4395f63ed5d9e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="如何使用语音和 SMS (PHP) 的 Twilio |Microsoft Azure"
    description="了解如何拨打电话和发送短消息与 Twilio API 服务在 Azure 上。 用 PHP 编写的代码样本。"
    services=""
    documentationCenter="python"
    authors="devinrader"
    manager="twilio"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="python" 
    ms.topic="article"
    ms.date="02/19/2015"
    ms.author="MicrosoftHelp@twilio.com"/>





# 如何使用 Twilio 进行语音和 PHP 中的 SMS 功能
本指南说明如何执行常见的编程任务，与 Twilio API 服务在 Azure 上。 所包含的方案包括电话拨打电话和发送短消息服务 (SMS) 消息。 Twilio 和在您的应用程序中使用语音和 SMS 的详细信息，请参阅[后续步骤](#NextSteps)部分。

## <a id="WhatIs"></a>Twilio 是什么？
Twilio 正在使开发人员可以将嵌入的声音、 VoIP 和消息传递到应用程序的业务通信的未来。 它们虚拟化在基于云的全球环境中，通过 Twilio 通信 API 平台公开它所需的所有基础结构。 应用程序构建简单和可扩展性。 享受与付薪的为-您的灵活性转定价，并受益于云的可靠性。

**Twilio 语音**，您的应用程序，可以拨打和接听电话。 **Twilio SMS**使应用程序可以发送和接收文本消息。 **Twilio 客户端**允许您从任何电话、 平板电脑或浏览器的 VoIP 呼叫并支持 WebRTC。

## <a id="Pricing"></a>Twilio 的定价和特殊优惠

当您升级您的 Twilio 帐户时，azure 客户收到 [特价商品] Twilio 贷方的 10 美元。 该 Twilio 贷方可应用于任何 Twilio 用法 （10 美元信用等效于将达 1000 短消息发送或接收设置 1000 个入站的语音分钟，具体取决于您的电话号码和消息或调用目标的位置）。 兑换此 Twilio 贷方，并由此开始: [ahoy.twilio.com/azure]。

Twilio 是一种使用付费服务。 有没有设置费用，而且可以随时关闭您的帐户。 在[定价 Twilio] [twilio_pricing]，您可以找到更多详细信息。

## <a id="Concepts"></a>概念
Twilio API 是 rest 风格 API 为应用程序提供语音和 SMS 的功能。 客户端库都可用多种语言;有关列表，请参阅[Twilio API 库] [twilio_libraries]。

Twilio API 的主要方面是 Twilio 动词和 Twilio 标记语言 (TwiML)。

### <a id="Verbs"></a>Twilio 动作
该 API 利用 Twilio 动词;例如，**&lt;说&gt;**动词指示 Twilio 文本在调用中传递邮件。

以下是 Twilio 谓词的列表。 学习动词和 [Twilio 标记语言文档] 通过能力 [http://www.twilio.com/docs/api/twiml]。

* **&lt;拨&gt;**︰ 将调用方连接到另一个电话。
* **&lt;收集&gt;**︰ 收集在电话键盘上输入的数字。
* **&lt;挂断&gt;**︰ 结束呼叫。
* **&lt;播放&gt;**︰ 播放音频文件。
* **&lt;暂停&gt;**︰ 悄悄等待指定的秒数。
* **&lt;记录&gt;**︰ 记录呼叫者的声音，并返回包含录制的文件的 URL。
* **&lt;重定向&gt;**︰ 将呼叫或 SMS 的控制权移交给 TwiML 在不同的 URL。
* **&lt;拒绝&gt;**︰ 无需付费您拒绝为 Twilio 号的传入呼叫
* **&lt;说&gt;**︰ 转换为文本到语音呼叫进行。
* ** &lt;Sms&gt;**︰ 发送 SMS 消息。

### <a id="TwiML"></a>TwiML
TwiML 是如何的一套基于 Twilio 处理呼叫或 SMS 通知 Twilio 谓词的基于 XML 的说明。

例如，下面的 TwiML 将转换成语音的文本**Hello World** 。

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

当应用程序调用 Twilio API 时，这些 API 的参数之一是 TwiML 响应返回的 URL。 出于开发目的，可以使用 Twilio 提供的 Url 来提供应用程序所使用的 TwiML 响应。 您还可以存放自己的 Url 生成 TwiML 的响应，和另一种方法是使用**TwiMLResponse**对象。

有关 Twilio 谓词及其特性，以及 TwiML 的详细信息，请参阅[TwiML] [twiml]。 关于 Twilio API 的其他信息，请参阅[Twilio API] [twilio_api]。

## <a id="CreateAccount"></a>创建一个 Twilio 帐户
准备就绪以获取 Twilio 帐户后，请在[尝试 Twilio] [try_twilio]。 可以使用免费的帐户，开始和以后再升级您的帐户。

当您注册为 Twilio 帐户时，您收到的帐户 ID 和身份验证令牌。 两者都需要进行 Twilio API 调用。 为了防止未经授权的访问到您的帐户，保护身份验证令牌的安全。 您的帐户 ID 和身份验证令牌是在[Twilio 帐户页] [twilio_account]，分别标记**帐户 SID**和**身份验证令牌**，字段中可查看。

## <a id="create_app"></a>创建一个 PHP 应用程序
PHP 应用程序，它使用 Twilio 服务并在 Azure 中运行会比其他任何 PHP 应用程序使用 Twilio 服务没有什么不同。 Twilio 服务是基于 REST 和以下几种方式可以从 PHP 中调用，而这篇文章将重点介绍如何使用[Twilio 库从 GitHub 的 php][twilio_php]Twilio 服务。 有关使用的 PHP 的 Twilio 库的详细信息，请参阅[http://readthedocs.org/docs/twilio-php/en/latest/index.html][twilio_lib_docs]。

构建和部署到 Azure 的 Twilio/PHP 应用程序的详细的说明都在[如何使在 PHP 应用程序中在 Azure 上电话使用 Twilio][howto_phonecall_php]。

## <a id="configure_app"></a>配置应用程序使用 Twilio 库
您可以配置您的应用程序，PHP Twilio 库使用以下两种方式︰

1. 下载 php 从 GitHub ([https://github.com/twilio/twilio-php][twilio_php]) 的 Twilio 库，并将**服务**目录添加到您的应用程序。

    -或-

2. 作为梨树包安装 PHP Twilio 库。 可以使用以下命令安装︰

        $ pear channel-discover twilio.github.com/pear
        $ pear install twilio/Services_Twilio

一旦安装了 PHP Twilio 库，然后可以在您的 PHP 文件引用的库的顶部添加**require_once**语句︰

        require_once 'Services/Twilio.php';

有关详细信息，请参阅[https://github.com/twilio/twilio-php/blob/master/README.md][twilio_github_readme]。

## <a id="howto_make_call"></a>如何︰ 发出传出调用
下面演示如何使用**Services_Twilio**类的传出呼叫。 此代码还使用提供 Twilio 的网站返回的 Twilio 标记语言 (TwiML) 响应。 替换为您的值的**源**和**目标**电话号码，并确保您为您的 Twilio 帐户在运行代码前验证**从**电话号码。

    // Include the Twilio PHP library.
    require_once 'Services/Twilio.php';

    // Library version.
    $version = "2010-04-01";

    // Set your account ID and authentication token.
    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";

    // The number of the phone initiating the the call.
    // (Must be previously validated with Twilio.)
    $from_number = "NNNNNNNNNNN";

    // The number of the phone receiving call.
    $to_number = "NNNNNNNNNNN";

    // Use the Twilio-provided site for the TwiML response.
    $url = "http://twimlets.com/message";

    // The phone message text.
    $message = "Hello world.";

    // Create the call client.
    $client = new Services_Twilio($sid, $token, $version);

    //Make the call.
    try
    {
        $call = $client->account->calls->create(
            $from_number,
            $to_number,
            $url.'?Message='.urlencode($message)
        );
    }
    catch (Exception $e)
    {
        echo 'Error: ' . $e->getMessage();
    }

如上所述，此代码使用 Twilio 提供的站点返回 TwiML 响应。 而是可以使用您自己的网站提供 TwiML 的响应;有关详细信息，请参阅[如何提供 TwiML 响应从您自己的 Web 站点](#howto_provide_twiml_responses)。


- **注意**︰ SSL 证书验证错误进行疑难解答，请参阅[http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html][ssl_validation]


## <a id="howto_send_sms"></a>如何︰ 发送短信
下面演示如何发送 SMS 消息使用**Services_Twilio**类。 由试用帐户来发送 SMS 消息的 Twilio 提供**从**数。 **到**数必须验证为您的 Twilio 帐户之前运行的代码。

    // Include the Twilio PHP library.
    require_once 'Services/Twilio.php';

    // Library version.
    $version = "2010-04-01";

    // Set your account ID and authentication token.
    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";


    $from_number = "NNNNNNNNNNN"; // With trial account, texts can only be sent from your Twilio number.
    $to_number = "NNNNNNNNNNN";
    $message = "Hello world.";

    // Create the call client.
    $client = new Services_Twilio($sid, $token, $version);

    // Send the SMS message.
    try
    {
        $client->account->sms_messages->create($from_number, $to_number, $message);
    }
    catch (Exception $e)
    {
        echo 'Error: ' . $e->getMessage();
    }

## <a id="howto_provide_twiml_responses"></a>如何︰ 提供从您自己的网站 TwiML 响应
当应用程序启动到 Twilio API 的调用时，Twilio 将请求发送到需要将 TwiML 响应返回的 URL。 上面的示例使用 Twilio 提供的 URL [http://twimlets.com/message][twimlet_message_url]。 （尽管 TwiML 供使用，通过 Twilio，您可以查看它在浏览器中。 例如，单击[http://twimlets.com/message][twimlet_message_url]为空，请参阅`<Response>`元素;举一个例子，请单击[http://twimlets.com/message?Message%5B0%5D=Hello%20World][twimlet_message_url_hello_world]来查看`<Response>`元素，其中包含`<Say>`元素。)

而不是依赖的 Twilio 提供的 URL，您可以创建您自己的站点返回 HTTP 响应。 您可以在任何语言中返回 XML 响应; 创建网站本主题假定您将使用 PHP 创建 TwiML。

下面的 PHP 页面会导致调用时说**Hello World** TwiML 响应。

    <?php
        header("content-type: text/xml");
        echo "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n";
    ?>
    <Response>
        <Say>Hello world.</Say>
    </Response>

从上面的例子中可以看出，TwiML 响应是简单的 XML 文档。 PHP 的 Twilio 库包含将为您生成 TwiML 的类。 下面的示例会产生同等的响应，如上所示，但使用**服务\_Twilio\_Twiml** Twilio 的 PHP 库中的类︰

    require_once('Services/Twilio.php');

    $response = new Services_Twilio_Twiml();
    $response->say("Hello world.");
    print $response;

有关 TwiML 的详细信息，请参阅[https://www.twilio.com/docs/api/twiml][twiml_reference]。

一旦您的 PHP 页设置为提供 TwiML 的响应，使用 PHP 页面的 URL，如 URL 传递到`Services_Twilio->account->calls->create`方法。 如果您具有名为**MyTwiML**的部署到一个 Web 应用程序例如，Azure 托管服务，和 PHP 页面的名称是**mytwiml.php**，可以将 URL 传递给**Services_Twilio-> 帐户-> 调用-> 创建**在下面的示例所示︰

    require_once 'Services/Twilio.php';

    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";
    $from_number = "NNNNNNNNNNN";
    $to_number = "NNNNNNNNNNN";
    $url = "http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.php";

    // The phone message text.
    $message = "Hello world.";

    $client = new Services_Twilio($sid, $token, "2010-04-01");

    try
    {
        $call = $client->account->calls->create(
            $from_number,
            $to_number,
            $url.'?Message='.urlencode($message)
        );
    }
    catch (Exception $e)
    {
        echo 'Error: ' . $e->getMessage();
    }

有关在使用 PHP 的 Azure 中使用 Twilio 的其他信息，请参阅[如何在 PHP 应用程序中在 Azure 上电话使用 Twilio][howto_phonecall_php]。

## <a id="AdditionalServices"></a>如何︰ 使用附加 Twilio 服务
除了此处所示的示例中，Twilio 提供了基于 web 的 Api，可用于利用从 Azure 应用程序的附加 Twilio 功能。 有关完整的详细信息，请参阅[Twilio API 文档] [twilio_api_documentation]。

## <a id="NextSteps"></a>下一步行动
现在，您已经学习了 Twilio 服务的基本知识，按照这些链接以了解详细信息︰

* [Twilio 安全准则] [twilio_security_guidelines]
* [Twilio 如何指南和示例代码] [twilio_howtos]
* [Twilio 快速入门教程][twilio_quickstarts]
* [在 GitHub 上的 Twilio] [twilio_on_github]
* [谈到 Twilio 支持] [twilio_support]

[twilio_php]: https://github.com/twilio/twilio-php
[twilio_lib_docs]: http://readthedocs.org/docs/twilio-php/en/latest/index.html
[twilio_github_readme]: https://github.com/twilio/twilio-php/blob/master/README.md
[ssl_validation]: http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html

[howto_phonecall_php]: http://windowsazure.com/documentation/articles/partner-twilio-php-make-phone-call
[twimlet_message_url]: http://twimlets.com/message
[twimlet_message_url_hello_world]: http://twimlets.com/message?Message%5B0%5D=Hello%20World
[twiml_reference]: https://www.twilio.com/docs/api/twiml
[twilio_pricing]: http://www.twilio.com/pricing

[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api]: http://www.twilio.com/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_account]:  https://www.twilio.com/user/account
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[twilio_api_documentation]: http://www.twilio.com/api
[twilio_security_guidelines]: http://www.twilio.com/docs/security
[twilio_howtos]: http://www.twilio.com/docs/howto
[twilio_on_github]: https://github.com/twilio
[twilio_support]: http://www.twilio.com/help/contact
[twilio_quickstarts]: http://www.twilio.com/docs/quickstart

测试

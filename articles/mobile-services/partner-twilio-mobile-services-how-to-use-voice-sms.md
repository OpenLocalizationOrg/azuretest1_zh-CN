---
ms.openlocfilehash: b4bfa368db35ecf24221aee7976fa1f343ff95ac
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="Twilio 用于语音和 SMS 功能 |Microsoft Azure"
    description="了解如何使用 Azure 移动服务使用 Twilio API 执行常见任务。"
    services="mobile-services"
    documentationCenter=""
    authors="devinrader"
    manager="twilio"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="06/16/2015"
    ms.author="devinrader"/>


# 如何使用 Twilio 进行语音和移动服务的 SMS 功能

本主题演示如何执行 Twilio API 使用 Azure 移动服务的常见任务。 在本教程中，您将学习如何创建自定义的 API 的脚本，使用 Twilio API 来启动电话呼叫和发送短消息服务 (SMS)。 

## <a id="WhatIs"></a>Twilio 是什么？
Twilio 正在使开发人员可以将嵌入的声音、 VoIP 和消息传递到应用程序的业务通信的未来。 它们虚拟化在基于云的全球环境中，通过 Twilio 通信 API 平台公开它所需的所有基础结构。 应用程序构建简单和可扩展性。 享受与付薪的为-您的灵活性转定价，并受益于云的可靠性。

**Twilio 语音**，您的应用程序，可以拨打和接听电话。 **Twilio SMS**使您的应用程序发送和接收短信。 **Twilio 客户端**允许您从任何电话、 平板电脑或浏览器的 VoIP 呼叫并支持 WebRTC。

## <a id="Pricing"></a>Twilio 的定价和特殊优惠
Azure 的客户可以获得[特殊优惠][special_offer]︰ 免费赠送 10 美元的 Twilio 信用升级您的 Twilio 帐户时。 该 Twilio 贷方可应用于任何 Twilio 用法 （10 美元信用等效于将达 1000 短消息发送或接收设置 1000 个入站的语音分钟，具体取决于您的电话号码和消息或调用目标的位置）。 兑换此 Twilio 贷方和[ahoy.twilio.com/azure][special_offer]由此开始。

Twilio 是一种使用付费服务。 有没有设置费用，而且可以随时关闭您的帐户。 在[定价 Twilio][twilio_pricing]，您可以找到更多详细信息。  

## <a id="Concepts"></a>概念
Twilio API 是 rest 风格 API 为应用程序提供语音和 SMS 的功能。 客户端库都可用多种语言;有关列表，请参阅[Twilio API 库] [twilio_libraries]。  为使用[.NET][azure_twilio_howto_dotnet]、 [node.js][azure_twilio_howto_node]、 [Java][azure_twilio_howto_java]、 [PHP][azure_twilio_howto_php]、 [Python][azure_twilio_howto_python]或[azure_twilio_howto_ruby]的[Ruby]编写的任何 Azure 应用程序的 Twilio 提供了其他教程。

Twilio API 的主要方面是 Twilio 动词和 Twilio 标记语言 (TwiML)。

### <a id="Verbs"></a>Twilio 动作
该 API 利用 Twilio 动词;例如，**&lt;说&gt;**动词指示 Twilio 文本在调用中传递邮件。

以下是 Twilio 谓词的列表。  学习动词和通过[Twilio 标记语言文档](http://www.twilio.com/docs/api/twiml)的功能。

* **&lt;拨&gt;**︰ 将调用方连接到另一个电话。
* **&lt;收集&gt;**︰ 收集在电话键盘上输入的数字。
* **&lt;挂断&gt;**︰ 结束呼叫。
* **&lt;播放&gt;**︰ 播放音频文件。
* **&lt;暂停&gt;**︰ 悄悄等待指定的秒数。
* **&lt;记录&gt;**︰ 记录呼叫者的声音，并返回包含该录制的文件的 URL。
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

有关 Twilio 谓词及其特性，以及 TwiML 的详细信息，请参阅[TwiML][twiml]。 关于 Twilio API 的其他信息，请参阅[Twilio API][twilio_api]。

## <a id="CreateAccount"></a>创建一个 Twilio 帐户
您准备就绪以获取 Twilio 帐户，注册在[尝试 Twilio][try_twilio]。 可以使用免费的帐户，开始和以后再升级您的帐户。

当您注册为 Twilio 帐户时，您将收到的帐户 ID 和身份验证令牌。 两者都需要进行 Twilio API 调用。 为了防止未经授权的访问到您的帐户，保护身份验证令牌的安全。 您的帐户 ID 和身份验证令牌是在[Twilio 帐户页][twilio_account]，分别标记**帐户 SID**和**身份验证令牌**，字段中可查看。

## <a id="create_app"></a>创建一个移动服务
Twilio 的主机启用应用程序移动服务是与任何其他移动服务没有什么不同。 您只需添加 Twilio node.js 库，以便从您的移动服务自定义 API 脚本中引用它。 有关创建初始的移动服务的信息，请参阅[开始使用移动服务](mobile-services-ios-get-started.md)。

## <a id="ConfigureMobileService"></a>配置您要使用 Twilio Node.js 库的移动服务
Twilio 提供了 Node.js 库包装 Twilio 提供生成 TwiML 响应的 REST API，Twilio 和 Twilio 客户端交互的简便方法的各个方面。

若要使用您的移动服务中的 Twilio node.js 库，您需要利用移动服务 npm 模块支持，可以通过将脚本存储在源代码管理中执行此操作。

1. 完成教程[将脚本存储在源代码管理中](mobile-services-store-scripts-source-control.md)。 这引导您完成设置源控件为您的移动服务和 Git 存储库中存储服务器脚本。

2. 为您的移动服务设置了源控件后，打开本地计算机上的存储库、 浏览到`\services`子文件夹中，在文本编辑器中打开 package.json 文件，并将以下字段添加到该**依赖项**对象︰

        "twilio": "~1.7.0"

3. 添加**依赖项**对象的 Twilio 包引用后，package.json 文件应如下所示︰

        {
          "name": "todolist",
          "version": "1.0.0",
          "description": "todolist - hosted on Azure Mobile Services",
          "main": "server.js",
          "engines": {
            "node": ">= 0.8.19"
          },
          "dependencies": {
            "twilio": "~1.7.0"
          },
          "devDependencies": {},
          "scripts": {},
          "author": "unknown",
          "licenses": [],
          "keywords":[]
        }

    >[AZURE.NOTE]应作为添加依赖项的 Twilio `"twilio": "~1.7.0"`，与 （~）。 不支持使用插入符号 (^) 的引用。

4. 提交此文件更新以及将更新推送回移动服务。

    Package.json 文件到此更新将重新启动您的移动服务。

现在，移动服务安装并加载 Twilio 包，以便您可以引用和使用您的自定义的 API 和表脚本中的 Twilio 库。

## <a id="howto_make_call"></a>如何︰ 发出传出调用
下面的脚本显示如何启动从您的移动服务外发调用使用**makeCall**功能。 此代码还使用提供 Twilio 的网站返回的 Twilio 标记语言 (TwiML) 响应。 替换为您的值的**源**和**目标**电话号码，并确保您为您的 Twilio 帐户在运行代码前验证**从**电话号码。

    var twilio = require('twilio');

    exports.post = function(request, response) {

        var client = new twilio.RestClient('[ACCOUNT_SID]', 'AUTH_TOKEN');

        client.makeCall({
            to:'+16515556677',
            from: '+14506667788',
            url: 'http://www.example.com/twiml.php'

        }, function(err, responseData) {
            console.log(responseData.from);
            response.send(200, '');
        });
    };

有关传递到**client.makeCall**函数中的参数的详细信息，请参阅[http://www.twilio.com/docs/api/rest/making-calls][twilio_rest_making_calls]。

如上所述，此代码使用 Twilio 提供的站点返回 TwiML 响应。 而是可以使用您自己的网站将提供 TwiML 响应。 有关详细信息，请参阅[如何︰ 响应您自己的 web 站点，提供 TwiML](#howto_provide_twiml_responses)。

## <a id="howto_send_sms"></a>如何︰ 发送短信
下面的代码演示如何发送 SMS 消息使用**sendSms**功能。 由试用帐户来发送 SMS 消息的 Twilio 提供**从**数。 必须为您的 Twilio 帐户验证**到**数之前运行的代码。

    var twilio = require('twilio');

    exports.post = function(request, response) {

        var client = new twilio.RestClient('[ACCOUNT_SID]', 'AUTH_TOKEN');

        client.sendSms({
            to:'[]',
            from:'[]',
            body:'ahoy hoy! Testing Twilio and node.js'
        }, function(error, message) {

            // The "error" variable will contain error information, if any.
            // If the request was successful, this value will be "false"
            if (!error) {
                console.log('Success! The SID for this SMS message is: ' + message.sid);
                console.log('Message sent on: ' + message.dateCreated);
            }
            else {
                console.log('Oops! There was an error.');
            }
            response.send(200, { error : error } );
        });
    };


## <a id="howto_provide_twiml_responses"></a>如何︰ 提供从您自己的网站 TwiML 响应

当应用程序启动到 Twilio API-例如，通过客户端的调用。InitiateOutboundCall 方法-Twilio 向即将返回 TwiML 响应 URL 发送您的请求。 如何在示例︰ 使传出调用使用 Twilio 提供的 URL http://twimlets.com/message 返回响应。

> [AZURE.NOTE] 而 TwiML 用于设计 web 服务，可以在浏览器中查看 TwiML。 例如，单击[twimlet_message_url](http://twimlets.com/message)为空，请参阅&lt;响应&gt;元素;举一个例子，请单击查看[twimlet_message_url_hello_world](http://twimlets.com/message?Message%5B0%5D=Hello%20World) &lt;响应&gt;元素，其中包含&lt;说&gt;元素。

而不是依赖的 Twilio 提供的 URL，您可以创建您自己的 URL 站点返回 HTTP 响应。 您可以返回 HTTP 响应任何语言创建网站。 本主题假定您将主持从 ASP.NET 泛型处理程序的 URL。

下面的脚本会导致调用时说 Hello World TwiML 响应。

    var twilio = require('twilio');

    exports.post = function(request, response) {
        var resp = new twilio.TwimlResponse();
        resp.say({voice:'woman'}, 'ahoy hoy! Testing Twilio and node.js');
        response.set('Content-Type', 'text/xml');
        response.send(200, resp.toString());
    };

有关 TwiML 的详细信息，请参阅[https://www.twilio.com/docs/api/twiml](https://www.twilio.com/docs/api/twiml)。

一旦您设置了提供 TwiML 响应的方式，可以将该 URL 传递到**client.makeCall**方法，如下面的代码示例所示︰

    var twilio = require('twilio');

    exports.post = function(request, response) {

        var client = new twilio.RestClient('[ACCOUNT_SID]', 'AUTH_TOKEN');

        client.makeCall({
            to:'+16515556677',
            from: '+14506667788',
            url: 'http://<your_mobile_service>.azure-mobile.net/api/makeCall'

        }, function(err, responseData) {

            console.log(responseData.from);
            response.send(200, '');
        });
    };

[AZURE.INCLUDE [twilio_additional_services_and_next_steps](../../includes/twilio_additional_services_and_next_steps.md)]


[twilio_rest_making_calls]: http://www.twilio.com/docs/api/rest/making-calls

[twilio_pricing]: http://www.twilio.com/pricing
[special_offer]: http://ahoy.twilio.com/azure
[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api]: http://www.twilio.com/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_account]:  https://www.twilio.com/user/account
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#


[azure_twilio_howto_dotnet]: /develop/net/how-to-guides/twilio-voice-and-sms-service/
[azure_twilio_howto_java]: /develop/java/how-to-guides/twilio-voice-and-sms-service/
[azure_twilio_howto_node]: /develop/nodejs/how-to-guides/twilio-voice-and-sms-service/
[azure_twilio_howto_ruby]: /develop/ruby/how-to-guides/twilio-voice-and-sms-service/
[azure_twilio_howto_python]: /develop/python/how-to-guides/twilio-voice-and-sms-service/
[azure_twilio_howto_php]: /develop/php/how-to-guides/twilio-voice-and-sms-service/

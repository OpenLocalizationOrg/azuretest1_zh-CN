---
ms.openlocfilehash: 0f7f1499c59dc628aeaab6fc341359aca3a1ccdb
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何使用语音和 SMS （拼音） 的 Twilio |Microsoft Azure" 
    description="了解如何拨打电话和发送短消息与 Twilio API 服务在 Azure 上。 用 Ruby 编写的代码样本。" 
    services="" 
    documentationCenter="ruby" 
    authors="devinrader" 
    manager="twilio" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ruby" 
    ms.topic="article" 
    ms.date="11/25/2014" 
    ms.author="MicrosoftHelp@twilio.com"/>





# 如何使用 Twilio 进行语音和 Ruby 中的 SMS 功能
本指南说明如何执行常见的编程任务，与 Twilio API 服务在 Azure 上。 所包含的方案包括电话拨打电话和发送短消息服务 (SMS) 消息。 Twilio 和在您的应用程序中使用语音和 SMS 的详细信息，请参阅[后续步骤](#NextSteps)部分。

## <a id="WhatIs"></a>Twilio 是什么？
Twilio 是一个电话服务 web 服务 API，使您可以使用您现有的 web 语言和技能来构建语音和 SMS 的应用程序。 Twilio 是一个第三方服务 （不 Azure 的功能并不是 Microsoft 产品）。

**Twilio 语音**，您的应用程序，可以拨打和接听电话。 **Twilio SMS**允许您的应用程序和接收短信。 **Twilio 客户端**允许您的应用程序能够使用现有的互联网连接，包括移动语音通信。

## <a id="Pricing"></a>Twilio 的定价和特殊优惠
提供[定价 Twilio] [twilio_pricing]Twilio 定价有关的信息了。 Azure 的客户可以获得[特殊优惠][special_offer]︰ 免费借 1000年文本或 1000 站分钟。 要注册此服务或获取详细信息，请访问[http://ahoy.twilio.com/azure][special_offer]。  

## <a id="Concepts"></a>概念
Twilio API 是 rest 风格 API 为应用程序提供语音和 SMS 的功能。 客户端库都可用多种语言;有关列表，请参阅[Twilio API 库] [twilio_libraries]。

### <a id="TwiML"></a>TwiML
TwiML 是一套基于 XML 的指示如何处理调用的 Twilio 或 SMS 通知。

例如，下面的 TwiML 将转换成语音的文本**Hello World** 。

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

所有的 TwiML 文档`<Response>`作为它们的根元素。 从那里，您可以使用 Twilio 谓词来定义您的应用程序的行为。

### <a id="Verbs"></a>TwiML 动作
Twilio 谓词是告诉 Twilio**执行**到什么的 XML 标记。 例如，**&lt;说&gt;**动词指示 Twilio 文本在调用中传递邮件。 

以下是 Twilio 谓词的列表。

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

有关 Twilio 谓词及其特性，以及 TwiML 的详细信息，请参阅[TwiML] [twiml]。 关于 Twilio API 的其他信息，请参阅[Twilio API] [twilio_api]。

## <a id="CreateAccount"></a>创建一个 Twilio 帐户
您准备就绪以获取 Twilio 帐户，注册在[尝试 Twilio] [try_twilio]。 可以使用免费的帐户，开始和以后再升级您的帐户。

当您注册为 Twilio 帐户时，您将为应用程序获得免费电话号码。 您还可以获得 SID 的帐户和身份验证令牌。 两者都需要进行 Twilio API 调用。 为了防止未经授权的访问到您的帐户，保护身份验证令牌的安全。 您的帐户的 SID 和身份验证令牌是在[Twilio 帐户页][twilio_account]，分别标记**帐户 SID**和**身份验证令牌**，字段中可查看的。

### <a id="VerifyPhoneNumbers"></a>验证电话号码
除了数由 Twilio，您还可以验证在应用程序中使用的数字，您可以控制 （如您的移动电话或家庭电话号码）。 

有关如何验证电话号码的信息，请参阅[管理编号] [verify_phone]。

## <a id="create_app"></a>创建 Ruby 应用程序
Ruby 应用程序使用 Twilio 服务，并运行在 Azure 是比其他任何 Ruby 应用程序使用 Twilio 服务没有什么不同。 Twilio 服务是 RESTful 和以下几种方式可以从 Ruby 中调用，而这篇文章将重点介绍如何使用[Ruby 的 Twilio 帮助器库][twilio_ruby]Twilio 服务。

首先，[设置一个新的 Azure Linux 虚拟机][azure_vm_setup]作为新的 Ruby 的 web 应用程序的宿主。 忽略涉及创建 Rails 应用程序，只需设置虚拟机的步骤。 确保与外部端口 80 和 5000 内部端口创建终结点。

在下面的示例中，我们将使用[Sinatra][sinatra]，Ruby 的非常简单的 web 框架。 但可以肯定使用 Twilio 帮助器库为 Ruby 与任何其他 web 框架，包括 Ruby on Rails。

SSH 到新的虚拟机并为您的新应用程序创建一个目录。 在该目录中，创建名为 Gemfile 的文件并将下面的代码复制到其中︰

    source 'https://rubygems.org'
    gem 'sinatra'
    gem 'thin'

在命令行运行`bundle install`。 这将安装上面的依赖关系。 接下来，创建一个名为文件`web.rb`。 这将是您的 web 应用程序的代码所在的位置。 将以下代码粘贴到它︰

    require 'sinatra'

    get '/' do
        "Hello Monkey!"
    end

现在您应能够运行的命令`ruby web.rb -p 5000`。 这将加速小型 web 服务器在端口 5000 上。 您应该能够浏览到此应用程序在浏览器中访问该 URL 您设置 Azure VM。 一旦您可以访问您在浏览器中的 web 应用程序，您就可以开始建立一个 Twilio 应用程序。

## <a id="configure_app"></a>配置应用程序使用 Twilio
您可以配置您的 web 应用程序要使用 Twilio 库的更新您`Gemfile`包含以下行︰

    gem 'twilio-ruby'

在命令行上运行`bundle install`。 现在打开`web.rb`和包括顶部这行︰

    require 'twilio-ruby'

您现在都准备在您的 web 应用程序中使用 Ruby Twilio 帮助器库。

## <a id="howto_make_call"></a>如何︰ 发出传出调用
下面演示如何发出传出调用。 主要概念包括使用 Ruby 的 Twilio 帮助器库进行 REST API 调用并呈现 TwiML。 替换为您的值的**源**和**目标**电话号码，并确保您为您的 Twilio 帐户在运行代码前验证**从**电话号码。

添加到此函数`web.md`:

    # Set your account ID and authentication token.
    sid = "your_twilio_account_sid";
    token = "your_twilio_authentication_token";

    # The number of the phone initiating the the call.
    # This should either be a Twilio number or a number that you've verified
    from = "NNNNNNNNNNN";

    # The number of the phone receiving call.
    to = "NNNNNNNNNNN";

    # Use the Twilio-provided site for the TwiML response.
    url = "http://yourdomain.cloudapp.net/voice_url";
      
    get '/make_call' do
      # Create the call client.
      client = Twilio::REST::Client.new(sid, token);
      
      # Make the call
      client.account.calls.create(to: to, from: from, url: url)
    end

    post '/voice_url' do
      "<Response>
         <Say>Hello Monkey!</Say>
       </Response>"
    end
    
如果您打开向上`http://yourdomain.cloudapp.net/make_call`在浏览器中，会触发 Twilio API 调用以将电话联络。 在前两个参数`client.account.calls.create`都一目了然︰ 号码的呼叫是`from`的号码呼叫是`to`。 

第三个参数 (`url`) 是 Twilio 请求以获取有关如何处理该呼叫连接后说明的 URL。 在这种情况下我们设置一个 URL (`http://yourdomain.cloudapp.net`)，返回一个简单的 TwiML 文档，并使用`<Say>`谓词来做一些文字到语音功能和给打来的人说"你好猴子"。

## <a id="howto_recieve_sms"></a>如何︰ 接收到的短消息
在前面的示例中我们发起一个**传出**的电话。 这次，我们将使用 Twilio 在注册过程中给予我们的电话号码来处理**收到**的短信。

第一，登录到您[的仪表板 Twilio][twilio_account]。 单击顶部导航中的"数"，然后单击所提供的 Twilio 号上。 您将看到两个可配置的 Url。 语音请求 URL 和短消息请求的 URL。 这些都是每当进行电话呼叫或 SMS 发送到您的号时，将调用 Twilio 的 Url。 Url 还被称为"web 挂钩"。

我们想要处理传入短信，让我们更新的 URL `http://yourdomain.cloudapp.net/sms_url`。 继续操作并单击保存更改在页面的底部。 现在，回`web.rb`让我们应用程序中处理此项工作︰

    post '/sms_url' do
      "<Response>
         <Message>Hey, thanks for the ping! Twilio and Azure rock!</Message>
       </Response>"
    end

之后进行更改，请确保重新启动您的 web 应用程序。 现在，请拿出您的电话，并将 SMS 发送给您的 Twilio 号。 应该及时获取 SMS 回应说"您好，感谢您 ping 的 ！ Twilio 和 Azure 岩石 ！"。

## <a id="additional_services"></a>如何︰ 使用附加 Twilio 服务
除了此处所示的示例中，Twilio 提供了基于 web 的 Api，可用于利用从 Azure 应用程序的附加 Twilio 功能。 有关完整的详细信息，请参阅[Twilio API 文档] [twilio_api_documentation]。

### <a id="NextSteps"></a>下一步行动
现在，您已经学习了 Twilio 服务的基本知识，按照这些链接以了解详细信息︰

* [Twilio 安全准则] [twilio_security_guidelines]
* [Twilio Howto 和代码示例] [twilio_howtos]
* [Twilio 快速入门教程][twilio_quickstarts] 
* [在 GitHub 上的 Twilio] [twilio_on_github]
* [谈到 Twilio 支持] [twilio_support]

[twilio_ruby]: https://www.twilio.com/docs/ruby/install





[twilio_pricing]: http://www.twilio.com/pricing
[special_offer]: http://ahoy.twilio.com/azure
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
[sinatra]: http://www.sinatrarb.com/
[azure_vm_setup]: http://www.windowsazure.com/develop/ruby/tutorials/web-app-with-linux-vm/

测试

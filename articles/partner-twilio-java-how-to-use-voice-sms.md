---
ms.openlocfilehash: 7802627b2be9f9e7b8cea3de702959777a7fbc90
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何使用语音和 SMS (Java) 的 Twilio |Microsoft Azure" 
    description="了解如何拨打电话和发送短消息与 Twilio API 服务在 Azure 上。 用 Java 编写的代码样本。" 
    services="" 
    documentationCenter="java" 
    authors="devinrader" 
    manager="twilio" 
    editor="mollybos"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="11/25/2014" 
    ms.author="microsofthelp@twilio.com"/>

# 如何使用 Twilio 进行语音和 Java 中的 SMS 功能

本指南说明如何执行常见的编程任务，与 Twilio API 服务在 Azure 上。 所包含的方案包括电话拨打电话和发送短消息服务 (SMS) 消息。 Twilio 和在您的应用程序中使用语音和 SMS 的详细信息，请参阅[后续步骤](#NextSteps)部分。

## <a id="WhatIs"></a>Twilio 是什么？
Twilio 是一个电话服务 web 服务 API，使您可以使用您现有的 web 语言和技能来构建语音和 SMS 的应用程序。 Twilio 是一个第三方服务 （不 Azure 的功能并不是 Microsoft 产品）。

**Twilio 语音**，您的应用程序，可以拨打和接听电话。 **Twilio SMS**允许您的应用程序和接收短信。 **Twilio 客户端**允许您的应用程序能够使用现有的互联网连接，包括移动语音通信。

## <a id="Pricing"></a>Twilio 的定价和特殊优惠
提供[定价 Twilio] [twilio_pricing]Twilio 定价有关的信息了。 Azure 的客户可以获得[特殊优惠][special_offer]︰ 免费借 1000年文本或 1000 站分钟。 要注册此服务或获取详细信息，请访问[http://ahoy.twilio.com/azure][special_offer]。  

## <a id="Concepts"></a>概念
Twilio API 是 rest 风格 API 为应用程序提供语音和 SMS 的功能。 客户端库都可用多种语言;有关列表，请参阅[Twilio API 库] [twilio_libraries]。

Twilio API 的主要方面是 Twilio 动词和 Twilio 标记语言 (TwiML)。

### <a id="Verbs"></a>Twilio 动作
该 API 利用 Twilio 动词;例如，**&lt;说&gt;**动词指示 Twilio 文本在调用中传递邮件。 

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
您准备就绪以获取 Twilio 帐户，注册在[尝试 Twilio] [try_twilio]。 可以使用免费的帐户，开始和以后再升级您的帐户。

当您注册为 Twilio 帐户时，您将收到的帐户 ID 和身份验证令牌。 两者都需要进行 Twilio API 调用。 为了防止未经授权的访问到您的帐户，保护身份验证令牌的安全。 您的帐户 ID 和身份验证令牌是在[Twilio 帐户页] [twilio_account]，分别标记**帐户 SID**和**身份验证令牌**，字段中可查看。

## <a id="create_app"></a>创建一个 Java 应用程序
1. 获取 Twilio jar/文件夹并将其添加到您的 Java 生成路径和战争部署程序集。 在[https://github.com/twilio/twilio-java][twilio_java]，可以下载 GitHub 源，并创建您自己的 JAR，或下载预构建的 JAR （有或没有依赖项）。
2. 确保您的 JDK **cacerts**密钥库包含 Equifax 安全证书颁发机构证书的 MD5 指纹 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (序列号为 35:DE:F4:CF，SHA1 指纹为 D2:32:09:AD:23:D3:14:23:21:74:E4:0 D: 7F:9 D: 62:13:97:86:63:3A)。 这是[https://api.twilio.com][twilio_api_service]服务，当您使用 Twilio Api 调用的证书颁发机构 (CA) 证书。 有关确保 JDK 的**cacerts**密钥库包含正确的 CA 证书，请参阅[添加到 Java CA 证书存储区的证书][add_ca_cert]。

详细的说明对 Java 中使用 Twilio 客户端库都在[如何使在 Java 应用程序在 Azure 上电话使用 Twilio][howto_phonecall_java]。

## <a id="configure_app"></a>配置应用程序使用 Twilio 库
在代码中，可以为要在您的应用程序中使用的类的 Twilio 软件包源文件的顶部添加**导**入语句。 

对于 Java 源代码文件︰

    import com.twilio.*;
    import com.twilio.sdk.*;
    import com.twilio.sdk.resource.factory.*;
    import com.twilio.sdk.resource.instance.*;

对于 Java 服务器页 (JSP) 源代码文件︰

    import="com.twilio.*"
    import="com.twilio.sdk.*"
    import="com.twilio.sdk.resource.factory.*"
    import="com.twilio.sdk.resource.instance.*"
这取决于您想要使用的 Twilio 包或类，您**导入**的语句可能会有所不同。

## <a id="howto_make_call"></a>如何︰ 发出传出调用
下面演示如何使用**CallFactory**类的传出呼叫。 此代码还使用提供 Twilio 的网站返回的 Twilio 标记语言 (TwiML) 响应。 替换为您的值的**源**和**目标**电话号码，并确保您为您的 Twilio 帐户在运行代码前验证**从**电话号码。

    // Use your account SID and authentication token instead
    // of the placeholders shown here.
    String accountSID = "your_twilio_account";
    String authToken = "your_twilio_authentication_token";

    // Create an instance of the Twilio client.
    TwilioRestClient client;
    client = new TwilioRestClient(accountSID, authToken);

    // Retrieve the account, used later to create an instance of the CallFactory.
    Account account = client.getAccount();

    // Use the Twilio-provided site for the TwiML response.
    String Url="http://twimlets.com/message";
    Url = Url + "?Message%5B0%5D=Hello%20World";

    // Place the call From, To and URL values into a hash map. 
    HashMap<String, String> params = new HashMap<String, String>();
    params.put("From", "NNNNNNNNNN"); // Use your own value for the second parameter.
    params.put("To", "NNNNNNNNNN");   // Use your own value for the second parameter.
    params.put("Url", Url);

    // Create an instance of the CallFactory class.
    CallFactory callFactory = account.getCallFactory();

    // Make the call.
    Call call = callFactory.create(params);

有关传递给**CallFactory.create**方法的参数的详细信息，请参阅[http://www.twilio.com/docs/api/rest/making-calls][twilio_rest_making_calls]。

如上所述，此代码使用 Twilio 提供的站点返回 TwiML 响应。 而是可以使用您自己的网站提供 TwiML 的响应;有关详细信息，请参阅[如何在 Java 应用程序在 Azure 上提供 TwiML 响应](#howto_provide_twiml_responses)。

## <a id="howto_send_sms"></a>如何︰ 发送短信
下面演示如何发送 SMS 消息使用**SmsFactory**类。 **从**号， **4155992671**，由试用帐户的 Twilio 来发送 SMS 消息。 **到**数必须验证为您的 Twilio 帐户之前运行的代码。

    // Use your account SID and authentication token instead
    // of the placeholders shown here.
    String accountSID = "your_twilio_account";
    String authToken = "your_twilio_authentication_token";

    // Create an instance of the Twilio client.
    TwilioRestClient client;
    client = new TwilioRestClient(accountSID, authToken);

    // Retrieve the account, used later to create an instance of the SmsFactory.
    Account account = client.getAccount();

    // Send an SMS message.
    MessageFactory messageFactory = account.getMessageFactory();
    
    List<NameValuePair> params = new ArrayList<NameValuePair>();
    params.add(new BasicNameValuePair("To", "+14159352345")); // Replace with a valid phone number for your account.
    params.add(new BasicNameValuePair("From", "+14158141829")); // Replace with a valid phone number for your account.
    params.add(new BasicNameValuePair("Body", "Where's Wallace?"));
    
    Message sms = messageFactory.create(params);
        
有关传递给**SmsFactory.create**方法的参数的详细信息，请参阅[http://www.twilio.com/docs/api/rest/sending-sms][twilio_rest_sending_sms]。

## <a id="howto_provide_twiml_responses"></a>如何︰ 提供从您自己的网站 TwiML 响应
当应用程序启动时 Twilio API 调用时，例如通过**CallFactory.create**方法中，Twilio 将请求发送到需要将 TwiML 响应返回的 URL。 上面的示例使用 Twilio 提供的 URL [http://twimlets.com/message][twimlet_message_url]。 （在 TwiML 设计用于通过 Web 服务，您可以查看 TwiML 在浏览器中。 例如，单击[http://twimlets.com/message][twimlet_message_url]为空，请参阅**&lt;响应&gt;**元素;作为另一个示例，请单击[http://twimlets.com/message?Message%5B0%5D=Hello%20World]来查看[twimlet_message_url_hello_world] **&lt;响应&gt;**元素，其中包含**&lt;说&gt;**元素。)

而不是依赖的 Twilio 提供的 URL，您可以创建您自己的 URL 站点返回 HTTP 响应。 您可以返回 HTTP 响应; 任何语言创建网站本主题假定您主持中 JSP 页的 URL。

下面的 JSP 页会导致调用时说**Hello World** TwiML 响应。

    <%@ page contentType="text/xml" %>
    <Response> 
        <Say>Hello World</Say>
    </Response>

下面的 JSP 页会导致 TwiML 响应说一些文本，有几个暂停，并说有关的 Twilio API 版本和 Azure 角色名称的信息。


    <%@ page contentType="text/xml" %>
    <Response> 
        <Say>Hello from Azure</Say>
        <Pause></Pause>
        <Say>The Twilio API version is <%= request.getParameter("ApiVersion") %>.</Say>
        <Say>The Azure role name is <%= System.getenv("RoleName") %>.</Say>
        <Pause></Pause>
        <Say>Good bye.</Say>
    </Response>

**ApiVersion**参数可用于 Twilio 语音请求 （而不是 SMS 请求）。 对于 Twilio 语音和 SMS 的请求可用请求参数，请参见<https://www.twilio.com/docs/api/twiml/twilio_request>和<https://www.twilio.com/docs/api/twiml/sms/twilio_request>，分别。 **角色名**环境变量均可用作为 Azure 部署的一部分。 （如果您想要添加自定义的环境变量，以便他们可以领取从**System.getenv**，看到[其他角色配置设置][misc_role_config_settings]环境变量部分。

一旦您的 JSP 页设置为提供 TwiML 的响应，使用 JSP 页面的 URL 作为传递给**CallFactory.create**方法的 URL。 例如，如果您有一个名为的 Web 应用程序 MyTwiML 部署到 Azure 托管服务，JSP 页的名称是 mytwiml.jsp，URL 可以传递给**CallFactory.create**在下面的示例所示︰

    // Place the call From, To and URL values into a hash map. 
    HashMap<String, String> params = new HashMap<String, String>();
    params.put("From", "NNNNNNNNNN");
    params.put("To", "NNNNNNNNNN");
    params.put("Url", "http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.jsp");

    CallFactory callFactory = account.getCallFactory();
    Call call = callFactory.create(params);

为响应与 TwiML 的另一个选项是通过**TwiMLResponse**类，即在**com.twilio.sdk.verbs**包中可用。

关于在 Azure 与 Java 中使用 Twilio 的其他信息，请参阅[如何在 Java 应用程序在 Azure 上电话使用 Twilio][howto_phonecall_java]。

## <a id="AdditionalServices"></a>如何︰ 使用附加 Twilio 服务
除了此处所示的示例中，Twilio 提供了基于 web 的 Api，可用于利用从 Azure 应用程序的附加 Twilio 功能。 有关完整的详细信息，请参阅[Twilio API 文档] [twilio_api_documentation]。

## <a id="NextSteps"></a>下一步行动
现在，您已经学习了 Twilio 服务的基本知识，按照这些链接以了解详细信息︰

* [Twilio 安全准则] [twilio_security_guidelines]
* [Twilio 如何和代码示例] [twilio_howtos]
* [Twilio 快速入门教程][twilio_quickstarts] 
* [在 GitHub 上的 Twilio] [twilio_on_github]
* [谈到 Twilio 支持] [twilio_support]

[twilio_java]: https://github.com/twilio/twilio-java
[twilio_api_service]: https://api.twilio.com
[add_ca_cert]: java-add-certificate-ca-store.md
[howto_phonecall_java]: partner-twilio-java-phone-call-example.md
[misc_role_config_settings]: http://msdn.microsoft.com/library/windowsazure/hh690945.aspx
[twimlet_message_url]: http://twimlets.com/message
[twimlet_message_url_hello_world]: http://twimlets.com/message?Message%5B0%5D=Hello%20World
[twilio_rest_making_calls]: http://www.twilio.com/docs/api/rest/making-calls
[twilio_rest_sending_sms]: http://www.twilio.com/docs/api/rest/sending-sms
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

测试

---
ms.openlocfilehash: 1c094e128e099865f2bb134ea68f76f3e12c286a
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何拨打电话 Twilio (Java) 从 |Microsoft Azure" 
    description="了解如何从 web 页在 Azure 中的 Java 应用程序中使用 Twilio 打电话。" 
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

# 如何使在 Azure 中的 Java 应用程序中使用 Twilio 电话呼叫 

下面的示例演示如何使用 Twilio 来打电话从 Azure 中承载的网页。 生成的应用程序将提示用户输入电话值，如下面的屏幕快照中所示。

![使用 Twilio 和 Java 的 azure 调用窗体][twilio_java]

您将需要执行下列操作以使用本主题中的代码︰

1. 获得 Twilio 帐户和身份验证令牌。 若要开始使用 Twilio，评估定价在[http://www.twilio.com/pricing][twilio_pricing]。 可以注册在[https://www.twilio.com/try-twilio][try_twilio]。 有关 API 由 Twilio 提供的信息，请参阅[http://www.twilio.com/api][twilio_api]。
2. 获得 Twilio JAR。 在[https://github.com/twilio/twilio-java][twilio_java_github]，可以下载 GitHub 源，并创建您自己的 JAR，或下载预构建的 JAR （有或没有依赖项）。
本主题中的代码编写使用预构建的 TwilioJava 3.3.8 与依赖性 JAR。
3. 将 JAR 添加到 Java 生成路径。
4. 如果使用 Eclipse 创建此 Java 应用程序，包括 Twilio JAR 中使用 Eclipse 的部署程序集功能将应用程序部署文件 （战争）。 如果您没有使用 Eclipse 创建此 Java 应用程序，请确保 Twilio JAR 作为 Java 应用程序中，相同的 Azure 角色中包含并添加到您的应用程序的类路径。
5. 确保您的 cacerts 密钥库包含 Equifax 安全证书颁发机构证书的 MD5 指纹 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (序列号为 35:DE:F4:CF，SHA1 指纹为 D2:32:09:AD:23:D3:14:23:21:74:E4:0 D: 7F:9 D: 62:13:97:86:63:3A)。 这是[https://api.twilio.com][twilio_api_service]服务，当您使用 Twilio Api 调用的证书颁发机构 (CA) 证书。 有关将此 CA 证书添加到您的 JDK cacert 的信息存储，请参阅[添加到 Java CA 证书存储区的证书][add_ca_cert]。

此外，强烈建议您所熟悉的信息[创建一个你好世界上应用程序使用 Azure 的日蚀式 （通过开放的 Microsoft 技术） 的 Java 插件][azure_java_eclipse_hello_world]，或用于承载 Azure 中的 Java 应用程序，如果您不使用 Eclipse，其他技术。

## 创建 web 窗体发出呼叫

下面的代码演示如何创建 web 窗体检索呼叫用户数据。 对于本示例而言，创建新的动态 web 项目，名为**TwilioCloud**，，并作为 JSP 文件添加的**callform.jsp** 。

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Automated call form</title>
    </head>
    <body>
     <p>Fill in all fields and click <b>Make this call</b>.</p>
     <br/>
      <form action="makecall.jsp" method="post">
       <table>
         <tr>
           <td>To:</td>
           <td><input type="text" size=50 name="callTo" value="" />
           </td>
         </tr>
         <tr>
           <td>From:</td>
           <td><input type="text" size=50 name="callFrom" value="" />
           </td>
         </tr>
         <tr>
           <td>Call message:</td>
           <td><input type="text" size=400 name="callText" value="Hello. This is the call text. Good bye." />
           </td>
         </tr>
         <tr>
           <td colspan=2><input type="submit" value="Make this call" />
           </td>
         </tr>
       </table>
     </form>
     <br/>
    </body>
    </html>

## 创建的代码进行调用
下面的代码，当用户完成窗体显示 callform.jsp 的调用时，创建调用消息并生成调用。 对于本示例而言，JSP 文件命名为**makecall.jsp**并已添加到**TwilioCloud**项目。 （使用您的 Twilio 帐户和身份验证令牌，而不是分配给**帐户**和**authToken**下面代码中的占位符值）。

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    import="java.util.*"
    import="com.twilio.*"
    import="com.twilio.sdk.*"
    import="com.twilio.sdk.resource.factory.*"
    import="com.twilio.sdk.resource.instance.*"
    pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Call processing happens here</title>
    </head>
    <body>
        <b>This is my make call page.</b><p/>
     <%
    try 
    {
         // Use your account SID and authentication token instead
         // of the placeholders shown here.
         String accountSID = "your_twilio_account";
         String authToken = "your_twilio_authentication_token";
     
         // Instantiate an instance of the Twilio client.     
         TwilioRestClient client;
         client = new TwilioRestClient(accountSID, authToken);

         // Retrieve the account, used later to retrieve the CallFactory.
         Account account = client.getAccount();

         // Display the client endpoint. 
         out.println("<p>Using Twilio endpoint " + client.getEndpoint() + ".</p>");
     
         // Display the API version.
         String APIVERSION = TwilioRestClient.DEFAULT_VERSION;
         out.println("<p>Twilio client API version is " + APIVERSION + ".</p>");
    
         // Retrieve the values entered by the user.
         String callTo = request.getParameter("callTo");  
         // The Outgoing Caller ID, used for the From parameter,
         // must have previously been verified with Twilio.
         String callFrom = request.getParameter("callFrom");
         String userText = request.getParameter("callText");
     
         // Replace spaces in the user's text with '%20', 
         // to make the text suitable for a URL.
         userText = userText.replace(" ", "%20");
     
         // Create a URL using the Twilio message and the user-entered text.
         String Url="http://twimlets.com/message";
         Url = Url + "?Message%5B0%5D=" + userText;
     
         // Display the message URL.
         out.println("<p>");
         out.println("The URL is " + Url);
         out.println("</p>");
    
         // Place the call From, To and URL values into a hash map. 
         HashMap<String, String> params = new HashMap<String, String>();
         params.put("From", callFrom);
         params.put("To", callTo);
         params.put("Url", Url);
     
         CallFactory callFactory = account.getCallFactory();
         Call call = callFactory.create(params);
         out.println("<p>Call status: " + call.getStatus()  + "</p>"); 
    } 
    catch (TwilioRestException e) 
    {
        out.println("<p>TwilioRestException encountered: " + e.getMessage() + "</p>");
        out.println("<p>StackTrace: " + e.getStackTrace().toString() + "</p>");
    }
    catch (Exception e) 
    {
        out.println("<p>Exception encountered: " + e.getMessage() + "");
        out.println("<p>StackTrace: " + e.getStackTrace().toString() + "</p>");
    }
    %>
    </body>
    </html>

除了进行调用，makecall.jsp 显示 Twilio 端点、 API 版本和通话状态。 例如，下面的屏幕快照︰

![使用 Twilio 和 Java 的 azure 调用响应][twilio_java_response]

## 运行应用程序
以下是高级步骤运行您的应用程序;可以在[创建你好世界上应用程序使用 Azure 的日蚀式 （通过开放的 Microsoft 技术） 的 Java 插件][azure_java_eclipse_hello_world]找到有关这些步骤的详细信息。

1. 导出到 Azure **approot**文件夹 TwilioCloud 战争。 
2. 修改**startup.cmd**解压缩 TwilioCloud 战争。
3. 针对计算仿真程序对应用程序进行编译。
4. 计算仿真程序中启动您的部署。
5. 打开一个浏览器，并运行**http://localhost:8080/TwilioCloud/callform.jsp**。
6. 在窗体中输入的值**进行此调用**，请单击，然后请参阅 makecall.jsp 中的结果。

当准备好部署到 Azure，重新编译，部署到云，将部署到 Azure，并在浏览器中运行 http://*your_hosted_name*.cloudapp.net/TwilioCloud/callform.jsp （替代您的*your_hosted_name*的值）。

## 下一步行动
提供此代码来显示您在 Azure 上使用 Twilio 在 Java 中的基本功能。 在生产环境中部署到 Azure 之前, 可能需要添加更多错误处理或其他功能。 例如︰

* 而不是使用 web 窗体，可以使用 Azure 存储 blob 或 SQL 数据库来存储电话号码并调用文本。 有关使用 Azure 存储 blob，在 Java 中的信息，请参阅[如何使用 Java 中的 Blob 存储服务][howto_blob_storage_java]。 有关使用 Java 中的 SQL 数据库的信息，请参阅[在 Java 中使用 SQL 数据库][howto_sql_azure_java]。
* 从您的部署配置设置，而不是硬编码的值在 makecall.jsp 中，您可以检索 Twilio 帐户 ID 和身份验证令牌使用**RoleEnvironment.getConfigurationSettings** 。 关于**RoleEnvironment**类的信息，请参阅[使用 Azure 服务运行库在 JSP 中]的[azure_runtime_jsp]和[http://dl.windowsazure.com/javadoc][azure_javadoc]的 Azure 服务运行时包文档。
* Makecall.jsp 代码将提供 Twilio 的 URL， [http://twimlets.com/message][twimlet_message_url]，分配给该**Url**变量。 此 URL 提供通知如何继续执行该调用 Twilio 的 Twilio 标记语言 (TwiML) 响应。 例如，可以包含返回 TwiML**&lt;说&gt;**调用收件人正在说话的文字会导致的谓词。 而不是使用 Twilio 提供的 URL，您可以构建自己的服务响应 Twilio 的请求;有关详细信息，请参阅[如何使用语音和 Java 中的 SMS 功能的 Twilio][howto_twilio_voice_sms_java]。 有关 TwiML 的详细信息可从[http://www.twilio.com/docs/api/twiml][twiml]，并详细了解**&lt;说&gt;**和[http://www.twilio.com/docs/api/twiml/say][twilio_say]可以找到其他 Twilio 动词。
* 阅读在[https://www.twilio.com/docs/security][twilio_docs_security]的 Twilio 安全准则。

Twilio 有关的其他信息，请参阅[https://www.twilio.com/docs][twilio_docs]。

## 请参见
* [如何使用语音和 Java 中的 SMS 功能的 Twilio][howto_twilio_voice_sms_java]
* [添加到 Java CA 证书存储区的证书][add_ca_cert]

[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/api
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[twilio_java_github]: http://github.com/twilio/twilio-java
[twimlet_message_url]: http://twimlets.com/message
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api_service]: http://api.twilio.com
[add_ca_cert]: java-add-certificate-ca-store.md
[azure_java_eclipse_hello_world]: http://msdn.microsoft.com/library/windowsazure/hh690944.aspx
[howto_twilio_voice_sms_java]: partner-twilio-java-how-to-use-voice-sms.md
[howto_blob_storage_java]: http://www.windowsazure.com/develop/java/how-to-guides/blob-storage/
[howto_sql_azure_java]: http://msdn.microsoft.com/library/windowsazure/hh749029.aspx
[azure_runtime_jsp]: http://msdn.microsoft.com/library/windowsazure/hh690948.aspx
[azure_javadoc]: http://dl.windowsazure.com/javadoc
[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say
[twilio_java]: ./media/partner-twilio-java-phone-call-example/WA_TwilioJavaCallForm.jpg
[twilio_java_response]: ./media/partner-twilio-java-phone-call-example/WA_TwilioJavaMakeCall.jpg

测试

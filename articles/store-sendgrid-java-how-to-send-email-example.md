---
ms.openlocfilehash: e828ed8f301d4819b7cd6a8d0b6985047166ddd0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="store-sendgrid-java-how-to-send-email-example" 
    description="如何将电子邮件发送在 Azure 部署中使用 Java 从 SendGrid" 
    services="" 
    documentationCenter="java" 
    authors="thinkingserious" 
    manager="sendgrid" 
    editor="mollybos"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="10/30/2014" 
    ms.author="vibhork;dominic.may@sendgrid.com;elmer.thomas@sendgrid.com"/>

# 如何将电子邮件发送在 Azure 的部署中使用 Java 从 SendGrid

下面的示例演示如何使用 SendGrid 从 Azure 中承载的网页发送电子邮件。 生成的应用程序将提示用户输入电子邮件值，如下面的屏幕快照中所示。

![电子邮件窗体][emailform]

生成电子邮件看起来类似于下面的屏幕快照。

![电子邮件][emailsent]

您将需要执行下列操作以使用本主题中的代码︰

1. 获得<http://www.oracle.com/technetwork/java/javamail/index.html>javax.mail Jar，例如。
2. 将 Jar 添加到 Java 生成路径。
3. 如果您使用 Eclipse 创建此 Java 应用程序，可以在应用程序部署文件 （战争） 使用 Eclipse 的部署组件特征包括 SendGrid 库。 如果您没有使用 Eclipse 创建此 Java 应用程序，请确保库都包含在同一 Azure 角色作为 Java 应用程序，并添加到您应用程序的类路径。


您还必须具有自己的 SendGrid 用户名和密码，以便能够发送电子邮件。 若要开始使用 SendGrid，请参阅[如何发送电子邮件使用 SendGrid 从 Java](store-sendgrid-java-how-to-send-email.md)。

此外，熟悉与信息在[创建 Eclipse 中的 Azure 的 Hello World 应用程序](http://msdn.microsoft.com/library/windowsazure/hh690944)，或其他技术用于承载 Azure 中的 Java 应用程序，如果您不使用 Eclipse，强烈建议。

## 创建 web 窗体用于发送电子邮件

下面的代码演示如何创建 web 窗体检索发送电子邮件的用户数据。 此内容而言，JSP 文件命名为**emailform.jsp**。

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Email form</title>
    </head>
    <body>
     <p>Fill in all fields and click <b>Send this email</b>.</p>
     <br/>
      <form action="sendemail.jsp" method="post">
       <table>
         <tr>
           <td>To:</td>
           <td><input type="text" size=50 name="emailTo">
           </td>
         </tr>
         <tr>
           <td>From:</td>
           <td><input type="text" size=50 name="emailFrom">
           </td>
         </tr>
         <tr>
           <td>Subject:</td>
           <td><input type="text" size=100 name="emailSubject" value="My email subject">
           </td>
         </tr>
         <tr>
           <td>Text:</td>
           <td><input type="text" size=400 name="emailText" value="Hello,<p>This is my message.</p>Thank you." />
           </td>
         </tr>
         <tr>
           <td>SendGrid user name:</td>
           <td><input type="text" name="sendGridUser">
           </td>
         </tr>
         <tr>
           <td>SendGrid password:</td>
           <td><input type="password" name="sendGridPassword">
           </td>
         </tr>
         <tr>
           <td colspan=2><input type="submit" value="Send this email">
           </td>
         </tr>
       </table>
     </form>
     <br/>
    </body>
    </html>

## 创建要发送电子邮件的代码

下面的代码，称为完成 emailform.jsp 中的窗体时，创建电子邮件并将其发送。 此内容而言，JSP 文件命名为**sendemail.jsp**。

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" import="javax.activation.*, javax.mail.*, javax.mail.internet.*, java.util.Date, java.util.Properties" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Email processing happens here</title>
    </head>
    <body>
        <b>This is my send mail page.</b><p/>
     <%
     
     final String sendGridUser = request.getParameter("sendGridUser");
     final String sendGridPassword = request.getParameter("sendGridPassword");
     
     class SMTPAuthenticator extends Authenticator
     {
       public PasswordAuthentication getPasswordAuthentication()
       {
            String username = sendGridUser;
            String password = sendGridPassword;
          
            return new PasswordAuthentication(username, password);   
       }
     }
     try
     {
         
         // The SendGrid SMTP server.
         String SMTP_HOST_NAME = "smtp.sendgrid.net";
    
         Properties properties;
        
         properties = new Properties();
         
         // Specify SMTP values.
         properties.put("mail.transport.protocol", "smtp");
         properties.put("mail.smtp.host", SMTP_HOST_NAME);
         properties.put("mail.smtp.port", 587);
         properties.put("mail.smtp.auth", "true");
         
         // Display the email fields entered by the user. 
         out.println("Value entered for email Subject: " + request.getParameter("emailSubject") + "<br/>");        
         out.println("Value entered for email      To: " + request.getParameter("emailTo") + "<br/>");
         out.println("Value entered for email    From: " + request.getParameter("emailFrom") + "<br/>");
         out.println("Value entered for email    Text: " + "<br/>" + request.getParameter("emailText") + "<br/>");
    
         // Create the authenticator object.
         Authenticator authenticator = new SMTPAuthenticator();
         
         // Create the mail session object.
         Session mailSession;
         mailSession = Session.getDefaultInstance(properties, authenticator);
         
         // Display debug information to stdout, useful when using the
         // compute emulator during development.
         mailSession.setDebug(true);
    
         // Create the message and message part objects.
         MimeMessage message;
         Multipart multipart;
         MimeBodyPart messagePart; 
         
         message = new MimeMessage(mailSession);
         
         multipart = new MimeMultipart("alternative");
         messagePart = new MimeBodyPart();
         messagePart.setContent(request.getParameter("emailText"), "text/html");
         multipart.addBodyPart(messagePart);            
    
         // Specify the email To, From, Subject and Content. 
         message.setFrom(new InternetAddress(request.getParameter("emailFrom")));
         message.addRecipient(Message.RecipientType.TO, new InternetAddress(request.getParameter("emailTo")));
         message.setSubject(request.getParameter("emailSubject")); 
         message.setContent(multipart);
         
         // Uncomment the following if you want to add a footer.
         // message.addHeader("X-SMTPAPI", "{\"filters\": {\"footer\": {\"settings\": {\"enable\":1,\"text/html\": \"<html>This is my <b>email footer</b>.</html>\"}}}}");
    
         // Uncomment the following if you want to enable click tracking.
         // message.addHeader("X-SMTPAPI", "{\"filters\": {\"clicktrack\": {\"settings\": {\"enable\":1}}}}");
         
         Transport transport;
         transport = mailSession.getTransport();
         // Connect the transport object.
         transport.connect();
         // Send the message.
         transport.sendMessage(message,  message.getRecipients(Message.RecipientType.TO));
         // Close the connection.
         transport.close();
     
        out.println("<p>Email processing completed.</p>");
         
     }
     catch (Exception e)
     {
         out.println("<p>Exception encountered: " + 
                            e.getMessage()     +
                            "</p>");   
     }
    %>
    
    </body>
    </html>

除了发送电子邮件，emailform.jsp 提供结果的用户;例如，下面的屏幕快照︰

![发送邮件结果][emailresult]

## 下一步行动

部署计算仿真程序对应用程序和浏览器内运行 emailform.jsp 在窗体中输入值，单击**发送这封电子邮件**，，然后请参阅 sendemail.jsp 中的结果。

提供此代码来向您展示如何在 Azure 上使用 Java 中的 SendGrid。 在生产环境中部署到 Azure 之前, 可能需要添加更多错误处理或其他功能。 例如︰ 

* 您可以使用 Azure 存储 blob 或 SQL 数据库来存储电子邮件地址和电子邮件，而不是使用 web 窗体。 有关使用 Azure 存储 blob，在 Java 中的信息，请参阅[如何使用 Java 中的 Blob 存储服务](http://www.windowsazure.com/develop/java/how-to-guides/blob-storage/)。 有关使用 Java 中的 SQL 数据库的信息，请参阅[在 Java 中使用 SQL 数据库](http://www.windowsazure.com/develop/java/how-to-guides/using-sql-azure-in-java/)。
* 您可以使用`RoleEnvironment.getConfigurationSettings`若要从您的部署配置设置，而不是使用 web 窗体中检索这些值检索 SendGrid 用户名和密码。 有关`RoleEnvironment`类，请参阅[使用 Azure 服务运行库在 JSP](http://msdn.microsoft.com/library/windowsazure/hh690948)和 Azure 服务运行时软件包文档，网址<http://dl.windowsazure.com/javadoc>。
* 有关使用 Java 中的 SendGrid 的详细信息，请参阅[如何发送电子邮件使用 SendGrid 从 Java](store-sendgrid-java-how-to-send-email.md)。

[emailform]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaEmailform.jpg
[emailsent]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaEmailSent.jpg
[emailresult]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaResult.jpg

测试

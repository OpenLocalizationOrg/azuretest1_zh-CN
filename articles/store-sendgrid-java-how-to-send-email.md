---
ms.openlocfilehash: cf1ac9342f783d8cb7a6dfe13a17a1adda3c7b34
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何使用 SendGrid 电子邮件服务 (Java) |Microsoft Azure" 
    description="了解如何在 Azure 上发送电子邮件的 SendGrid 电子邮件服务。 用 Java 编写的代码样本。" 
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
    ms.author="elmer.thomas@sendgrid.com; erika.berkland@sendgrid.com; vibhork"/>
# 如何发送电子邮件使用 Java 从 SendGrid

本指南说明如何执行常见的编程任务的 SendGrid 电子邮件服务在 Azure 上。 这些示例是用 Java 编写的。 所包含的方案包括**建立电子邮件**、**发送电子邮件**、**添加附件**、**使用筛选器**和**更新属性**。 SendGrid 和发送电子邮件的详细信息，请参阅[后续步骤](#next-steps)部分。

## SendGrid 电子邮件服务是什么？

SendGrid 是一种[基于云的电子邮件服务]，提供了可靠的[事务处理的电子邮件传递]、 可扩展性和灵活的 Api，轻松自定义集成以及实时分析。 常见的 SendGrid 使用情况包括︰

-   自动向客户发送回执
-   管理通讯组列表发送客户每月的电子活页资料和特别优惠
-   收集有关被阻止的电子邮件和客户响应能力等内容的实时度量标准
-   生成报告，以帮助确定趋势
-   发送客户查询
- 从应用程序的电子邮件通知

有关详细信息，请参阅<http://sendgrid.com>。

## 创建一个 SendGrid 帐户

[AZURE.INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## 如何︰ 使用 javax.mail 库

例如从<http://www.oracle.com/technetwork/java/javamail>获得的 javax.mail 库，并将它们导入到您的代码。 在较高级别，使用 javax.mail 库将使用 SMTP 电子邮件发送的过程是请执行下列操作︰

1.  指定 SMTP 值，包括 SMTP 服务器，即为 SendGrid smtp.sendgrid.net。
    
```
        import java.util.Properties;
        import javax.activation.*;
        import javax.mail.*;
        import javax.mail.internet.*;

        public class MyEmailer {
           private static final String SMTP_HOST_NAME = "smtp.sendgrid.net";
           private static final String SMTP_AUTH_USER = "your_sendgrid_username";
           private static final String SMTP_AUTH_PWD = "your_sendgrid_password";
        
           public static void main(String[] args) throws Exception{
              new MyEmailer().SendMail();
           }
        
           public void SendMail() throws Exception
           {
              Properties properties = new Properties();
              properties.put("mail.transport.protocol", "smtp");
              properties.put("mail.smtp.host", SMTP_HOST_NAME);
              properties.put("mail.smtp.port", 587);
              properties.put("mail.smtp.auth", "true");
              // …
```

2.  扩展的*javax.mail.Authenticator*类，并在*getPasswordAuthentication*方法的实现中，返回 SendGrid 用户名称和密码。  

        private class SMTPAuthenticator extends javax.mail.Authenticator {
        public PasswordAuthentication getPasswordAuthentication() {
           String username = SMTP_AUTH_USER;
           String password = SMTP_AUTH_PWD;
           return new PasswordAuthentication(username, password);
        }

3.  创建一个*javax.mail.Session*对象，通过已经过身份验证的电子邮件会话。  

        Authenticator auth = new SMTPAuthenticator();
        Session mailSession = Session.getDefaultInstance(properties, auth);

4.  创建邮件并分配****到**、**主题**和内容的值**。 这显示在[How To︰ 创建一封电子邮件](#bkmk_HowToCreateEmail)部分。
5.  通过发送邮件的*javax.mail.Transport*对象。 这显示在 [如何︰ 发送一封电子邮件] [如何︰ 发送一封电子邮件] 部分。

## 如何︰ 创建电子邮件

以下代码显示如何指定电子邮件的值。

    MimeMessage message = new MimeMessage(mailSession);
    Multipart multipart = new MimeMultipart("alternative");
    BodyPart part1 = new MimeBodyPart();
    part1.setText("Hello, Your Contoso order has shipped. Thank you, John");
    BodyPart part2 = new MimeBodyPart();
    part2.setContent(
        "<p>Hello,</p>
        <p>Your Contoso order has <b>shipped</b>.</p>
        <p>Thank you,<br>John</br></p>",
        "text/html");
    multipart.addBodyPart(part1);
    multipart.addBodyPart(part2);
    message.setFrom(new InternetAddress("john@contoso.com"));
    message.addRecipient(Message.RecipientType.TO,
       new InternetAddress("someone@example.com"));
    message.setSubject("Your recent order");
    message.setContent(multipart);

## 如何︰ 发送电子邮件

下面演示如何发送电子邮件。

    Transport transport = mailSession.getTransport();
    // Connect the transport object.
    transport.connect();
    // Send the message.
    transport.sendMessage(message, message.getAllRecipients());
    // Close the connection.
    transport.close();

## 如何︰ 添加附件

下面的代码演示了如何添加一个附件。

    // Local file name and path.
    String attachmentName = "myfile.zip";
    String attachmentPath = "c:\\myfiles\\"; 
    MimeBodyPart attachmentPart = new MimeBodyPart();
    // Specify the local file to attach.
    DataSource source = new FileDataSource(attachmentPath + attachmentName);
    attachmentPart.setDataHandler(new DataHandler(source));
    // This example uses the local file name as the attachment name.
    // They could be different if you prefer.
    attachmentPart.setFileName(attachmentName);
    multipart.addBodyPart(attachmentPart);

## 如何︰ 使用筛选器来启用页脚、 跟踪和分析

SendGrid 提供了通过使用*筛选器*的其他电子邮件功能。 这些都是可以添加到电子邮件中启用特定功能，如启用点击跟踪、 Google 分析、 跟踪、 预订等的设置。 有关筛选器的完整列表，请参阅[筛选器设置][]。

-   下面演示如何插入导致 HTML 文本出现在要发送的电子邮件底部的页脚筛选器。

        message.addHeader("X-SMTPAPI", 
            "{\"filters\": 
            {\"footer\": 
            {\"settings\": 
            {\"enable\":1,\"text/html\": 
            \"<html><b>Thank you</b> for your business.</html>\"}}}}");

-   筛选器的另一个例子是单击跟踪。 假设，您电子邮件的文本包含超链接，如下所示，并且要跟踪单击速率︰

        messagePart.setContent(
            "Hello,
            <p>This is the body of the message. Visit 
            <a href='http://www.contoso.com'>http://www.contoso.com</a>.</p>
            Thank you.", 
            "text/html");

-   若要启用点击跟踪，请使用下面的代码︰

        message.addHeader("X-SMTPAPI", 
            "{\"filters\": 
            {\"clicktrack\": 
            {\"settings\": 
            {\"enable\":1}}}}");

## 如何︰ 更新电子邮件属性

一些电子邮件，可以使用**设置*属性***或附加使用**覆盖属性添加*属性** *。

例如，要指定**回复**地址，请使用以下方法︰

    InternetAddress addresses[] = 
        { new InternetAddress("john@contoso.com"),
          new InternetAddress("wendy@contoso.com") };
    
    message.setReplyTo(addresses);

若要添加**抄送**收件人，请使用以下方法︰

    message.addRecipient(Message.RecipientType.CC, new 
    InternetAddress("john@contoso.com"));

## 如何︰ 使用附加 SendGrid 服务

SendGrid 提供了基于 web 的 Api，可用于利用从 Azure 应用程序的附加 SendGrid 功能。 有关完整的详细信息，请参阅[SendGrid API 文档][]。

## 下一步行动

现在，学 SendGrid 电子邮件服务的基本知识，按照这些链接以了解详细信息。

* 示例，它演示了如何使用 SendGrid 在 Azure 的部署︰[如何发送电子邮件使用 SendGrid 从 Java 在 Azure 的部署](store-sendgrid-java-how-to-send-email-example.md)
* SendGrid Java SDK: <https://sendgrid.com/docs/Code_Examples/java.html>
* SendGrid API 文档︰ <https://sendgrid.com/docs/API_Reference/index.html>
* SendGrid 特别优惠供 Azure 的客户︰ <https://sendgrid.com/windowsazure.html>

  [http://sendgrid.com]: https://sendgrid.com
  [http://sendgrid.com/pricing.html]: http://sendgrid.com/pricing.html
  [http://www.sendgrid.com/azure.html]: https://www.sendgrid.com/windowsazure.html
  [http://sendgrid.com/features]: https://sendgrid.com/features
  [http://www.oracle.com/technetwork/java/javamail]: http://www.oracle.com/technetwork/java/javamail/index.html
  [筛选器设置]: https://sendgrid.com/docs/API_Reference/Web_API/filter_settings.html
  [SendGrid API 文档]: https://sendgrid.com/docs/API_Reference/index.html
  [http://sendgrid.com/azure.html]: https://sendgrid.com/windowsazure.html
  [基于云的电子邮件服务]: https://sendgrid.com/email-solutions
  [事务处理的电子邮件传递]: https://sendgrid.com/transactional-email

测试

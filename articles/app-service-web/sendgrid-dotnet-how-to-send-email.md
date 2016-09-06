---
ms.openlocfilehash: 6d83202e5e43ba282c1ea056ea2a3a7c45fc0b79
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何使用 SendGrid 电子邮件服务 (.NET) |Microsoft Azure" 
    description="了解如何在 Azure 上发送电子邮件的 SendGrid 电子邮件服务。 用 C# 编写的示例的代码，并使用.NET API。" 
    services="app-service\web" 
    documentationCenter=".net" 
    authors="thinkingserious" 
    manager="sendgrid" 
    editor="erikre"/>

<tags 
    ms.service="app-service-web" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="02/24/2015" 
    ms.author="elmer.thomas@sendgrid.com; erika.berkland@sendgrid.com; vibhork"/>





# 如何发送电子邮件使用 Azure SendGrid

上次更新时间︰ Feburary 24 2015

## 概述

本指南说明如何执行常见的编程任务的 SendGrid 电子邮件服务在 Azure 上。 用 c 语言编写的示例\#
并使用.NET API。 所包含的方案包括**建立电子邮件**、**发送电子邮件**，**添加附件**和**使用筛选器**。 SendGrid 和发送电子邮件的详细信息，请参阅[后续步骤][]部分。

## SendGrid 电子邮件服务是什么？

SendGrid 是一种[基于云的电子邮件服务]，提供了可靠的[事务处理的电子邮件传递]、 可扩展性和灵活的 Api，轻松自定义集成以及实时分析。 常见的 SendGrid 使用情况包括︰

-   自动发送给客户的收据。
-   管理通讯组列表发送客户每月的电子活页资料和特别优惠。
-   正在收集实时阻止的电子邮件和客户响应能力等指标。
-   正在生成报告，以帮助确定的趋势。
-   转发客户查询。
-   处理传入电子邮件。

有关详细信息，请参阅[https://sendgrid.com](https://sendgrid.com)。

## 创建一个 SendGrid 帐户

[AZURE.INCLUDE [sendgrid-sign-up](../../includes/sendgrid-sign-up.md)]

## SendGrid.NET 类库的引用

[SendGrid NuGet 程序包](https://www.nuget.org/packages/Sendgrid)是最简单的方法来获取 SendGrid API 和配置应用程序的所有依赖项。 NuGet 是便于安装和更新库和工具的 Microsoft Visual Studio 2012 中包含 Visual Studio 扩展。 

> [AZURE.NOTE] 如果运行 Visual Studio 2012 以前版本的 Visual Studio 安装 NuGet，访问[http://www.nuget.org](http://www.nuget.org)，并单击**安装 NuGet**按钮。

若要在应用程序安装 SendGrid NuGet 程序包，请执行以下操作︰

1.  在**解决方案资源管理器**中右键单击**引用**，然后单击**管理 NuGet 程序包**。

2.  在**管理 NuGet 程序包**对话框的左侧窗格中，单击**联机**。

3.  **SendGrid**搜索并选择在结果列表中 （当前版本是第 5.0.0） **SendGrid**项。

    ![SendGrid NuGet 程序包][SendGrid-NuGet-package]

4.  单击**安装**以完成安装，并关闭此对话框。

SendGrid 的.NET 类库被称为**SendGridMail**。 它包含下列命名空间︰

-   对于创建和处理电子邮件项的**SendGridMail** 。
-   用于发送电子邮件**Web/其余部分**中使用**SMTP**协议或 HTTP 1.1 协议**SendGridMail.Transport** 。

将下面的代码命名空间声明添加到顶部的任何 C\#文件需以编程方式访问 SendGrid 电子邮件服务。
**System.Net**和**System.Net.Mail**都是因为它们包含的类型通常会使用与 SendGrid Api 所包含的.NET Framework 命名空间。

    using System;
    using System.Net;
    using System.Net.Mail;
    using SendGrid;

## 如何︰ 创建电子邮件

使用**SendGridMessage**对象来创建电子邮件。 创建消息对象后，您可以设置的属性和方法，包括电子邮件发件人、 收件人，以及主题和电子邮件的正文。

下面的示例演示如何创建一个完全填充电子邮件︰

    // Create the email object first, then add the properties.
    var myMessage = new SendGridMessage();

    // Add the message properties.
    myMessage.From = new MailAddress("john@example.com");

    // Add multiple addresses to the To field.
    List<String> recipients = new List<String>
    {
        @"Jeff Smith <jeff@example.com>",
        @"Anna Lidman <anna@example.com>",
        @"Peter Saddow <peter@example.com>"
    };

    myMessage.AddTo(recipients);

    myMessage.Subject = "Testing the SendGrid Library";

    //Add the HTML and Text bodies
    myMessage.Html = "<p>Hello World!</p>";
    myMessage.Text = "Hello World plain text!";

所有属性和方法支持的**SendGrid**类型的详细信息，请参阅[sendgrid csharp][] GitHub 上。

## 如何︰ 发送电子邮件

创建一封电子邮件之后, 您可以发送它使用 SendGrid 提供的 Web API。 或者，您可以使用[。NET 的库中生成](https://sendgrid.com/docs/Code_Examples/csharp.html)。

发送电子邮件要求您提供您的 SendGrid 帐户凭据 （用户名和密码）。 下面的代码演示如何在一个**NetworkCredential**对象包装您的凭据︰
    
    // Create network credentials to access your SendGrid account
    var username = "your_sendgrid_username";
    var pswd = "your_sendgrid_password";

    /* Alternatively, you may store these credentials via your Azure portal
       by clicking CONFIGURE and adding the key/value pairs under "app settings".
       Then, you may access them as follows: 
       var username = System.Environment.GetEnvironmentVariable("SENDGRID_USER"); 
       var pswd = System.Environment.GetEnvironmentVariable("SENDGRID_PASS");
       assuming you named your keys SENDGRID_USER and SENDGRID_PASS */

    var credentials = new NetworkCredential(username, pswd);

下面的示例演示如何使用 Web API 发送邮件。

    // Create the email object first, then add the properties.
    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    // Create credentials, specifying your user name and password.
    var credentials = new NetworkCredential("username", "password");

    // Create an Web transport for sending email.
    var transportWeb = new Web(credentials);

    // Send the email.
    // You can also use the **DeliverAsync** method, which returns an awaitable task.
    transportWeb.Deliver(myMessage);

## 如何︰ 添加附件

通过调用**AddAttachment**方法并指定的名称和您要附加的文件的路径，可以将附件添加到邮件中。
通过调用此方法，您要连接的每个文件后，您可以包括多个附件。 下面的示例演示如何添加到邮件中的附件︰

    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    myMessage.AddAttachment(@"C:\file1.txt");
    
您还可以从数据的**流**添加附件。 它通过调用上方， **AddAttachment**，使用相同方法，但通过传入的数据流，就可以完成和文件名您希望其显示的消息。 在这种情况下，您需要添加 System.IO 库。

    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    using (var attachmentFileStream = new FileStream(@"C:\file.txt", FileMode.Open))
    {
        myMessage.AddAttachment(attachmentFileStream, "My Cool File.txt");
    }


## 如何︰ 使用应用程序来启用页脚、 跟踪和分析

SendGrid 提供了其他电子邮件应用程序使用的功能。 这些都是可以添加到电子邮件中启用特定功能，如点击跟踪、 Google 分析、 跟踪、 订阅的设置等。 应用程序的完整列表，请参阅[应用程序设置][]。

应用程序可以使用方法为**SendGrid**类的一部分实现的**SendGrid**电子邮件应用。

下面的示例演示在页脚，然后单击跟踪筛选器︰

### 页脚

    // Create the email object first, then add the properties.
    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    // Add a footer to the message.
    myMessage.EnableFooter("PLAIN TEXT FOOTER", "<p><em>HTML FOOTER</em></p>");

### 单击跟踪

    // Create the email object first, then add the properties.
    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Html = "<p><a href=\"http://www.example.com\">Hello World Link!</a></p>";
    myMessage.Text = "Hello World!";
    
    // true indicates that links in plain text portions of the email 
    // should also be overwritten for link tracking purposes. 
    myMessage.EnableClickTracking(true);

## 如何︰ 使用附加 SendGrid 服务

SendGrid 提供了基于 web 的 Api，可用于利用从 Azure 应用程序的附加 SendGrid 功能的 webhooks。 有关完整的详细信息，请参阅[SendGrid API 文档][]。

## 下一步行动

现在，学 SendGrid 电子邮件服务的基本知识，按照这些链接以了解详细信息。

* SendGrid C\#库 repo: [sendgrid csharp][]
*   SendGrid API 文档︰ <https://sendgrid.com/docs>
*   SendGrid 特别优惠供 Azure 的客户︰ [https://sendgrid.com](https://sendgrid.com)

  [下一步行动]: #nextsteps
  [SendGrid 电子邮件服务是什么？]: #whatis
  [创建一个 SendGrid 帐户]: #createaccount
  [SendGrid.NET 类库的引用]: #reference
  [如何︰ 创建电子邮件]: #createemail
  [如何︰ 发送电子邮件]: #sendemail
  [如何︰ 添加附件]: #addattachment
  [如何︰ 使用筛选器来启用页脚、 跟踪和分析]: #usefilters
  [如何︰ 使用附加 SendGrid 服务]: #useservices
  
  
  [特价优惠]: https://www.sendgrid.com/windowsazure.html
  
  
  
  [SendGrid NuGet 程序包]: ./media/sendgrid-dotnet-how-to-send-email/sendgrid01.png
  [sendgrid csharp]: https://github.com/sendgrid/sendgrid-csharp
  [SMTP 与 Web API]: https://sendgrid.com/docs/Integrate/index.html
  [应用程序设置]: https://sendgrid.com/docs/API_Reference/SMTP_API/apps.html
  [SendGrid API 文档]: https://sendgrid.com/docs
  
  [基于云的电子邮件服务]: https://sendgrid.com/email-solutions
  [事务处理的电子邮件传递]: https://sendgrid.com/transactional-email
 

测试

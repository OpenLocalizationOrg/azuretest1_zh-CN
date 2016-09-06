---
ms.openlocfilehash: a1933b265b873e785eec6e6cb2fc16927bd7cc41
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何使用 SendGrid 电子邮件服务 (Node.js) |Microsoft Azure" 
    description="了解如何在 Azure 上发送电子邮件的 SendGrid 电子邮件服务。 示例使用 Node.js API 编写的代码。" 
    services="" 
    documentationCenter="nodejs" 
    authors="erikre" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="08/31/2015" 
    ms.author="erikre"/>
# 如何发送电子邮件使用从 Node.js SendGrid

本指南说明如何执行常见的编程任务的 SendGrid 电子邮件服务在 Azure 上。 这些示例是使用 Node.js API 来编写的。 所包含的方案包括**建立电子邮件**、**发送电子邮件**、**添加附件**、**使用筛选器**和**更新属性**。 SendGrid 和发送电子邮件的详细信息，请参阅[后续步骤](#next-steps)部分。

## SendGrid 电子邮件服务是什么？

SendGrid 是一种 [基于云的电子邮件服务] 提供可靠 [事务的电子邮件送达]，可扩展性和灵活的 Api，轻松自定义集成以及实时分析。 常见的 SendGrid 使用情况包括︰

-   自动向客户发送回执
-   管理通讯组列表发送客户每月的电子活页资料和特别优惠
-   收集有关被阻止的电子邮件和客户响应能力等内容的实时度量标准
-   生成报告，以帮助确定趋势
-   发送客户查询
-   从应用程序的电子邮件通知

有关详细信息，请参阅[https://sendgrid.com](https://sendgrid.com)。

## 创建一个 SendGrid 帐户

[AZURE.INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## 引用 SendGrid Node.js 模块

可以通过节点的包管理器 (npm) 安装 Node.js 的 SendGrid 模块，通过使用下面的命令︰

    npm install sendgrid

安装完成后，您可以要求模块应用程序中使用以下代码︰

    var sendgrid = require('sendgrid')(sendgrid_username, sendgrid_password);

SendGrid 模块导出**SendGrid**和**电子邮件**功能。
**SendGrid**负责**电子**封装的电子邮件时发送电子邮件通过 Web API。

## 如何︰ 创建电子邮件

创建电子邮件使用 SendGrid 模块包括第一次创建一封电子邮件，使用电子邮件功能，然后将使用 SendGrid 函数将其发送。 下面是创建新邮件使用电子邮件功能的示例︰

    var email = new sendgrid.Email({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.'
    });

此外可以通过将 html 属性设置支持其客户端的 HTML 邮件。 例如︰

    html: This is a sample <b>HTML<b> email message.

设置文本和 html 属性不能支持 HTML 邮件的客户端提供温和回退到文本内容。

有关支持的电子邮件功能的所有属性的详细信息，请参阅 [sendgrid-nodejs] []。

## 如何︰ 发送电子邮件

创建后使用电子邮件功能的电子邮件，您可以发送它使用 SendGrid 提供的 Web API。 

### API web

    sendgrid.send(email, function(err, json){
        if(err) { return console.error(err); }
        console.log(json);
    });

> [AZURE.NOTE] 上述示例显示电子邮件对象和回调函数中传递，同时还直接可以通过直接指定电子邮件属性调用发送函数。 例如︰  
>
>`````
sendgrid.send({
    to: 'john@contoso.com',
    from: 'anna@contoso.com',
    subject: 'test mail',
    text: 'This is a sample email message.'
});
`````

## How to: Add an Attachment

Attachments can be added to a message by specifying the file name(s) and
path(s) in the **files** property. The following example demonstrates
sending an attachment:

    sendgrid.send({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.',
        files: [
            {
                filename:     '',           // required only if file.content is used.
                contentType:  '',           // optional
                cid:          '',           // optional, used to specify cid for inline content
                path:         '',           //
                url:          '',           // == One of these three options is required
                content:      ('' | Buffer) //
            }
        ],
    });

> [AZURE.NOTE] When using the **files** property, the file must be accessible
through [fs.readFile](http://nodejs.org/docs/v0.6.7/api/fs.html#fs.readFile). If the file you wish to attach is hosted in Azure Storage, such as in a Blob container, you must first copy the file to local storage or to an Azure drive before it can be sent as an attachment using the **files** property.

## How to: Use Filters to Enable Footers and Tracking

SendGrid provides additional email functionality through the use of
filters. These are settings that can be added to an email message to
enable specific functionality such as enabling click tracking, Google
analytics, subscription tracking, and so on. For a full list of filters,
see [Filter Settings][].

Filters can be applied to a message by using the **filters** property.
Each filter is specified by a hash containing filter-specific settings.
The following examples demonstrate the footer and click tracking filters:

### Footer

    var email = new sendgrid.Email({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.'
    });
    
    email.setFilters({
        'footer': {
            'settings': {
                'enable': 1,
                'text/plain': 'This is a text footer.'
            }
        }
    });

    sendgrid.send(email);

### Click Tracking

    var email = new sendgrid.Email({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.'
    });
    
    email.setFilters({
        'clicktrack': {
            'settings': {
                'enable': 1
            }
        }
    });
    
    sendgrid.send(email);

## How to: Update Email Properties

Some email properties can be overwritten using **set*Property*** or
appended using **add*Property***. For example, you can add additional
recipients by using

    email.addTo('jeff@contoso.com');

or set a filter by using

    email.addFilter('footer', 'enable', 1);
    email.addFilter('footer', 'text/html', '<strong>boo</strong>');

For more information, see [sendgrid-nodejs][].

## How to: Use Additional SendGrid Services

SendGrid offers web-based APIs that you can use to leverage additional
SendGrid functionality from your Azure application. For full
details, see the [SendGrid API documentation][].

## Next Steps

Now that you've learned the basics of the SendGrid Email service, follow
these links to learn more.

-   SendGrid Node.js module repository: [sendgrid-nodejs][]
-   SendGrid API documentation:
    <https://sendgrid.com/docs>
-   SendGrid special offer for Azure customers:
    [http://sendgrid.com/azure.html](https://sendgrid.com/windowsazure.html)
  [special offer]: https://sendgrid.com/windowsazure.html
  [sendgrid-nodejs]: https://github.com/sendgrid/sendgrid-nodejs
  [Filter Settings]: https://sendgrid.com/docs/API_Reference/SMTP_API/apps.html
  [SendGrid API documentation]: https://sendgrid.com/docs
  [cloud-based email service]: https://sendgrid.com/email-solutions
  [transactional email delivery]: https://sendgrid.com/transactional-email

test

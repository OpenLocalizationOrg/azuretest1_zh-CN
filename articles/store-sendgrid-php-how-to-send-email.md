---
ms.openlocfilehash: d5862a8bf5fd6bbdd9eda72ad2ef80f1296c68f1
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何使用 SendGrid 电子邮件服务 (PHP) |Microsoft Azure" 
    description="了解如何在 Azure 上发送电子邮件的 SendGrid 电子邮件服务。 用 PHP 编写的代码样本。" 
    documentationCenter="php" 
    services="" 
    manager="sendgrid" 
    editor="mollybos" 
    authors="thinkingserious"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="PHP" 
    ms.topic="article" 
    ms.date="10/30/2014" 
    ms.author="elmer.thomas@sendgrid.com; erika.berkland@sendgrid.com; vibhork; matt.bernier@sendgrid.com"/>
# 如何使用 PHP 中的 SendGrid 电子邮件服务

本指南说明如何执行常见的编程任务的 SendGrid 电子邮件服务在 Azure 上。 示例是用 PHP 编写的。
所包含的方案包括**构造电子邮件**、**发送电子邮件**，并**添加附件**。 SendGrid 和发送电子邮件的详细信息，请参阅[后续步骤](#next-steps)部分。

## SendGrid 电子邮件服务是什么？

SendGrid 是一种[基于云的电子邮件服务]，提供了可靠的[事务处理的电子邮件传递]、 可扩展性和灵活的 Api，轻松自定义集成以及实时分析。 常见的 SendGrid 使用情况包括︰

-   自动向客户发送回执
-   管理通讯组列表发送客户每月的电子活页资料和特别优惠
-   收集有关被阻止的电子邮件和客户响应能力等内容的实时度量标准
-   生成报告，以帮助确定趋势
-   发送客户查询
- 从应用程序的电子邮件通知

有关详细信息，请参阅[https://sendgrid.com][]。

## 创建一个 SendGrid 帐户

[AZURE.INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## 使用 SendGrid 从 PHP 应用程序

Azure PHP 应用程序中使用 SendGrid 要求没有任何特殊配置或编码。 SendGrid 是一种服务，因为它可以从内部应用程序可以访问在云应用程序中完全相同的方式。

## 如何︰ 发送电子邮件

您可以发送电子邮件使用 SMTP 或 SendGrid 提供的 Web API。

### SMTP 的 API

若要发送电子邮件使用 SendGrid SMTP API，使用*Swift 邮件*，将电子邮件发送从 PHP 应用程序的基于组件的库。 您可以从[http://swiftmailer.org/download][] v5.3.0 （使用[编辑器]来安装 Swift 发件人） 来下载*Swift 邮件程序*库。 发送电子邮件与库创建的实例涉及到<span class="auto-style2">Swift\_SmtpTransport</span>， <span class="auto-style2">Swift\_发件人</span>，和<span class="auto-style2">Swift\_消息</span>类，设置相应的属性，并调用<span class="auto-style2">Swift\_Mailer::send</span>方法。

    <?php
     include_once "vendor/autoload.php";
     /*
      * Create the body of the message (a plain-text and an HTML version).
      * $text is your plain-text email
      * $html is your html version of the email
      * If the reciever is able to view html emails then only the html
      * email will be displayed
      */ 
     $text = "Hi!\nHow are you?\n";
     $html = "<html>
           <head></head>
           <body>
               <p>Hi!<br>
                   How are you?<br>
               </p>
           </body>
           </html>";
     // This is your From email address
     $from = array('someone@example.com' => 'Name To Appear');
     // Email recipients
     $to = array(
           'john@contoso.com'=>'Destination 1 Name',
           'anna@contoso.com'=>'Destination 2 Name'
     );
     // Email subject
     $subject = 'Example PHP Email';

     // Login credentials
     $username = 'yoursendgridusername';
     $password = 'yourpassword';
     
     // Setup Swift mailer parameters
     $transport = Swift_SmtpTransport::newInstance('smtp.sendgrid.net', 587);
     $transport->setUsername($username);
     $transport->setPassword($password);
     $swift = Swift_Mailer::newInstance($transport);

     // Create a message (subject)
     $message = new Swift_Message($subject);

     // attach the body of the email
     $message->setFrom($from);
     $message->setBody($html, 'text/html');
     $message->setTo($to);
     $message->addPart($text, 'text/plain');
     
     // send message 
     if ($recipients = $swift->send($message, $failures))
     {
         // This will let us know how many users received this message
         echo 'Message sent out to '.$recipients.' users';
     }
     // something went wrong =(
     else
     {
         echo "Something went wrong - ";
         print_r($failures);
     }

### API web

使用 PHP 的[卷曲函数][]发送电子邮件使用 SendGrid Web API。

    <?php

     $url = 'https://api.sendgrid.com/';
     $user = 'USERNAME';
     $pass = 'PASSWORD'; 

     $params = array(
          'api_user' => $user,
          'api_key' => $pass,
          'to' => 'john@contoso.com',
          'subject' => 'testing from curl',
          'html' => 'testing body',
          'text' => 'testing body',
          'from' => 'anna@contoso.com',
       );
       
     $request = $url.'api/mail.send.json';
     
     // Generate curl request
     $session = curl_init($request);
     
     // Tell curl to use HTTP POST
     curl_setopt ($session, CURLOPT_POST, true);
     
     // Tell curl that this is the body of the POST
     curl_setopt ($session, CURLOPT_POSTFIELDS, $params);
     
     // Tell curl not to return headers, but do return the response
     curl_setopt($session, CURLOPT_HEADER, false);
     curl_setopt($session, CURLOPT_RETURNTRANSFER, true);
     
     // obtain response
     $response = curl_exec($session);
     curl_close($session);
     
     // print everything out
     print_r($response);

SendGrid 的 Web API 是非常类似于一个 REST API，，但它不是真正 RESTful API 由于在大多数调用中，同时获得和开机自检谓词可以互换使用。

## 如何︰ 添加附件

### SMTP 的 API

发送使用 SMTP API 附件涉及额外一行代码发送与 Swift 发件人的电子邮件的示例脚本。

    <?php
     include_once "vendor/autoload.php";
     /*
      * Create the body of the message (a plain-text and an HTML version).
      * $text is your plain-text email
      * $html is your html version of the email
      * If the reciever is able to view html emails then only the html
      * email will be displayed
      */
     $text = "Hi!\nHow are you?\n";
      $html = "<html>
          <head></head>
          <body>
             <p>Hi!<br>
                How are you?<br>
             </p>
          </body>
          </html>";

     // This is your From email address
     $from = array('someone@example.com' => 'Name To Appear');
     
     // Email recipients
     $to = array(
          'john@contoso.com'=>'Destination 1 Name',
          'anna@contoso.com'=>'Destination 2 Name'
     );
     // Email subject
     $subject = 'Example PHP Email';
     
     // Login credentials
     $username = 'yoursendgridusername';
     $password = 'yourpassword';
     
     // Setup Swift mailer parameters
     $transport = Swift_SmtpTransport::newInstance('smtp.sendgrid.net', 587);
     $transport->setUsername($username);
     $transport->setPassword($password);
     $swift = Swift_Mailer::newInstance($transport);
     
     // Create a message (subject)
     $message = new Swift_Message($subject);
     
     // attach the body of the email
     $message->setFrom($from);
     $message->setBody($html, 'text/html');
     $message->setTo($to);
     $message->addPart($text, 'text/plain');
     $message->attach(Swift_Attachment::fromPath("path\to\file")->setFileName("file_name"));
     
     // send message 
     if ($recipients = $swift->send($message, $failures))
     {
          // This will let us know how many users received this message
          echo 'Message sent out to '.$recipients.' users';
     }
     // something went wrong =(
     else
     {
          echo "Something went wrong - ";
          print_r($failures);
     }

额外的代码行，如下所示︰

     $message->attach(Swift_Attachment::fromPath("path\to\file")->setFileName('file_name'));

这行代码在调用连接方法<span class="auto-style2">Swift\_消息</span>对象并使用静态方法<span class="auto-style2">fromPath</span>在<span class="auto-style2">Swift\_附件</span>类来获取并将文件附加到邮件中。

### API web

发送附件使用 Web API 是发送一封电子邮件，使用 Web API 非常相似。 但是，请注意，在下面的示例中，参数数组必须包含此元素︰

    'files['.$fileName.']' => '@'.$filePath.'/'.$fileName

示例：

    <?php

     $url = 'https://api.sendgrid.com/';
     $user = 'USERNAME';
     $pass = 'PASSWORD';
     
     $fileName = 'myfile';
     $filePath = dirname(__FILE__);

     $params = array(
         'api_user' => $user,
         'api_key' => $pass,
         'to' =>'john@contoso.com',
         'subject' => 'test of file sends',
         'html' => '<p> the HTML </p>',
         'text' => 'the plain text',
         'from' => 'anna@contoso.com',
         'files['.$fileName.']' => '@'.$filePath.'/'.$fileName
     );
     
     print_r($params);
     
     $request = $url.'api/mail.send.json';
     
     // Generate curl request
     $session = curl_init($request);
     
     // Tell curl to use HTTP POST
     curl_setopt ($session, CURLOPT_POST, true);
     
     // Tell curl that this is the body of the POST
     curl_setopt ($session, CURLOPT_POSTFIELDS, $params);
     
     // Tell curl not to return headers, but do return the response
     curl_setopt($session, CURLOPT_HEADER, false);
     curl_setopt($session, CURLOPT_RETURNTRANSFER, true);
     
     // obtain response
     $response = curl_exec($session);
     curl_close($session);
     
     // print everything out
     print_r($response);

## 如何︰ 使用筛选器来启用页脚、 跟踪和分析

SendGrid 提供了通过筛选器使用其他电子邮件功能。 这些都是可以添加到电子邮件中启用特定功能，如启用点击跟踪、 Google 分析、 跟踪、 预订等的设置。

通过使用筛选器属性，可以将滤镜应用于消息。 每个筛选器包含特定筛选器的设置哈希值来指定。 下面的示例启用页脚筛选器，并指定将附加到电子邮件底部的文本消息。
在本例中，我们将使用[sendgrid php 库]。
使用[编辑器]安装库︰
    
    php composer.phar require sendgrid/sendgrid 2.1.1

示例：    

    <?php
     /*
      * This example is used for sendgrid-php V2.1.1 (https://github.com/sendgrid/sendgrid-php/tree/v2.1.1)
      */
     include "vendor/autoload.php";

     $email = new SendGrid\Email();
     // The list of addresses this message will be sent to
     // [This list is used for sending multiple emails using just ONE request to SendGrid]
     $toList = array('john@contoso.com', 'anna@contoso.com');

     // Specify the names of the recipients
     $nameList = array('Name 1', 'Name 2');

     // Used as an example of variable substitution
     $timeList = array('4 PM', '5 PM');

     // Set all of the above variables
     $email->setTos($toList);
     $email->addSubstitution('-name-', $nameList);
     $email->addSubstitution('-time-', $timeList);

     // Specify that this is an initial contact message
     $email->addCategory("initial");

     // You can optionally setup individual filters here, in this example, we have 
     // enabled the footer filter
     $email->addFilter('footer', 'enable', 1);
     $email->addFilter('footer', "text/plain", "Thank you for your business");
     $email->addFilter('footer', "text/html", "Thank you for your business");

     // The subject of your email
     $subject = 'Example SendGrid Email';

     // Where is this message coming from. For example, this message can be from 
     // support@yourcompany.com, info@yourcompany.com
     $from = 'someone@example.com';

     // If you do not specify a sender list above, you can specifiy the user here. If 
     // a sender list IS specified above, this email address becomes irrelevant.
     $to = 'john@contoso.com';

     # Create the body of the message (a plain-text and an HTML version). 
     # text is your plain-text email 
     # html is your html version of the email
     # if the receiver is able to view html emails then only the html
     # email will be displayed

     /*
      * Note the variable substitution here =)
      */
     $text = "
     Hello -name-,
     Thank you for your interest in our products. We have set up an appointment to call you at -time- EST to discuss your needs in more detail.
     Regards,
     Fred";

     $html = "
     <html> 
     <head></head>
     <body>
     <p>Hello -name-,<br>
     Thank you for your interest in our products. We have set up an appointment
     to call you at -time- EST to discuss your needs in more detail.

     Regards,

     Fred<br>
     </p>
     </body>
     </html>";

     // set subject
     $email->setSubject($subject);

     // attach the body of the email
     $email->setFrom($from);
     $email->setHtml($html);
     $email->addTo($to);
     $email->setText($text);

     // Your SendGrid account credentials
     $username = 'sendgridusername@yourdomain.com';
     $password = 'example';

     // Create SendGrid object
     $sendgrid = new SendGrid($username, $password);

     // send message
     $response = $sendgrid->send($email);

     print_r($response);

## 下一步行动

现在，学 SendGrid 电子邮件服务的基本知识，按照这些链接以了解详细信息。

-   SendGrid 文档︰ <https://sendgrid.com/docs>
-   SendGrid PHP 库︰ <https://github.com/sendgrid/sendgrid-php>
-   SendGrid 特别优惠供 Azure 的客户︰ <https://sendgrid.com/windowsazure.html>

  [https://sendgrid.com]: https://sendgrid.com
  [https://sendgrid.com/transactional-email/pricing]: https://sendgrid.com/transactional-email/pricing
  [特价优惠]: https://www.sendgrid.com/windowsazure.html
  [打包和部署的 Azure 的 PHP 应用程序]: http://msdn.microsoft.com/library/windowsazure/hh674499(v=VS.103).aspx
  [http://swiftmailer.org/download]: http://swiftmailer.org/download
  [卷曲的函数]: http://php.net/curl
  [基于云的电子邮件服务]: https://sendgrid.com/email-solutions
  [事务处理的电子邮件传递]: https://sendgrid.com/transactional-email
  [sendgrid php 库]: https://github.com/sendgrid/sendgrid-php/tree/v2.1.1
  [作曲者]: https://getcomposer.org/download/

测试

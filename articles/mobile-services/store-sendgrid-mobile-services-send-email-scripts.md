---
ms.openlocfilehash: 3d81e39a9b3f5f99b469738d0377c6b0ef839e97
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="发送电子邮件使用 SendGrid |Microsoft Azure" 
    description="了解如何使用 SendGrid 服务从 Azure 移动服务应用程序发送电子邮件。" 
    services="mobile-services" 
    documentationCenter="" 
    authors="Erikre" 
    manager="sendgrid" 
    editor=""/>

<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="07/31/2015" 
    ms.author="Erikre"/>


# 从 SendGrid 的移动服务发送电子邮件

本主题演示如何可以添加到您的移动服务的电子邮件功能。 在本教程中，您将添加服务器端脚本发送邮件使用 SendGrid。 完成后，您的移动服务将插入记录每次发送一封电子邮件。

SendGrid 是一种[基于云的电子邮件服务]，提供了可靠的[事务处理的电子邮件传递]、 可扩展性和灵活的 Api，轻松自定义集成以及实时分析。 有关详细信息，请参阅<http://sendgrid.com>。

本教程将引导您完成以下基本步骤启用电子邮件功能︰

1. [创建一个 SendGrid 帐户]
2. [添加脚本来发送电子邮件]
3. [插入数据接收电子邮件]

本教程基于移动服务快速入门。 在开始本教程之前，您必须首先完成[开始使用移动服务]。 

## <a name="sign-up"></a>创建一个新的 SendGrid 帐户

[AZURE.INCLUDE [sendgrid-sign-up](../../includes/sendgrid-sign-up.md)]

## <a name="add-script"></a>注册一个新脚本将发送电子邮件

1. 登录到[Azure 管理门户网站]上，单击**移动服务**，然后单击移动服务。

2. 在管理门户中，单击**数据**选项卡，然后单击**TodoItem**表。 

    ![][1]

3. 在**todoitem**，单击**脚本**选项卡，然后选择**插入**。
   
    ![][2]

    这将显示插入发生**TodoItem**表中时将调用该函数。

4. 插入函数替换为以下代码︰

        var SendGrid = require('sendgrid').SendGrid;
        
        function insert(item, user, request) {    
            request.execute({
                success: function() {
                    // After the record has been inserted, send the response immediately to the client
                    request.respond();
                    // Send the email in the background
                    sendEmail(item);
                }
            });

            function sendEmail(item) {
                var sendgrid = new SendGrid('**username**', '**password**');       
                
                sendgrid.send({
                    to: '**email-address**',
                    from: '**from-address**',
                    subject: 'New to-do item',
                    text: 'A new to-do was added: ' + item.text
                }, function(success, message) {
                    // If the email failed to send, log it as an error so we can investigate
                    if (!success) {
                        console.error(message);
                    }
                });
            }
        }

5. 以上脚本中的占位符替换为正确的值︰

    - **_用户名_和_密码_**: SendGrid 凭据，您在中[创建的 SendGrid 帐户]标识。

    - **_电子邮件地址_**︰ 电子邮件发送到的地址。 在实际应用程序中，您可以使用表来存储和检索电子邮件地址。 在测试您的应用程序时，只需使用您自己的电子邮件地址。

    - **_发件人地址_**︰ 源自电子邮件的地址。 请考虑使用已注册的域名地址属于您的组织。 

     > [AZURE.NOTE] 如果您没有注册的域，您可以使用您的移动服务，*_您移动服务_.azure mobile.net @ 通知*的格式中的域。 但是，向您的移动服务的域发送的邮件将被忽略。

6. 单击**保存**按钮。 您现在已经配置脚本来发送电子邮件到**TodoItem**表中插入记录每次。

## <a name="insert-data"></a>插入测试数据接收电子邮件

1. 在客户端应用程序项目运行快速入门应用程序。 

    本主题介绍快速入门，Windows 存储区版本

2. 在应用程序中，请在**插入 TodoItem**，请键入文本，然后单击**保存**。

    ![][3]

3. 请注意，您将收到一封电子邮件，如下面的通知中所示。 

    ![][4]

    祝贺您，您已成功地配置移动服务通过使用 SendGrid 发送电子邮件。

## <a name="nextsteps"> </a>下一步行动

现在，您已看到使用移动服务的 SendGrid 电子邮件服务是多么容易，按照这些链接以了解更多有关 SendGrid。

-   SendGrid API 文档︰ <https://sendgrid.com/docs>
-   SendGrid 特别优惠供 Azure 的客户︰ <https://sendgrid.com/windowsazure.html>

<!-- Anchors. -->
[创建一个 SendGrid 帐户]: #sign-up
[添加脚本来发送电子邮件]: #add-script
[插入数据接收电子邮件]: #insert-data

<!-- Images. -->
[1]: ./media/store-sendgrid-mobile-services-send-email-scripts/mobile-portal-data-tables.png
[2]: ./media/store-sendgrid-mobile-services-send-email-scripts/mobile-insert-script-push2.png
[3]: ./media/store-sendgrid-mobile-services-send-email-scripts/mobile-quickstart-push1.png
[4]: ./media/store-sendgrid-mobile-services-send-email-scripts/mobile-receive-email.png

<!-- URLs. -->
[开始使用移动服务]: /develop/mobile/tutorials/get-started
[注册页]: https://sendgrid.com/windowsazure.html
[多个用户凭据页]: https://sendgrid.com/credentials
[Azure 的管理门户]: https://manage.windowsazure.com/
[基于云的电子邮件服务]: https://sendgrid.com/email-solutions
[事务处理的电子邮件传递]: https://sendgrid.com/transactional-email

 

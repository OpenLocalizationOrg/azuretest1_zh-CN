---
ms.openlocfilehash: d7942c32c5f11645e2065d5e6e565d0856c8d892
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="Twilio 用于语音、 VoIP 和 Azure 中的 SMS 消息" 
    description="了解如何拨打电话和发送短消息与 Twilio API 服务在 Azure 上。 用 Node.js 编写的代码样本。" 
    services="" 
    documentationCenter="nodejs" 
    authors="MikeWasson" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="11/25/2014" 
    ms.author="mwasson"/>


# Twilio 用于语音、 VoIP 和 Azure 中的 SMS 消息

本指南说明如何构建应用程序与 Twilio 和 node.js Azure 上的进行通信。

<a id="whatis"/>
## Twilio 是什么？

Twilio 是一个 API 的平台，便于开发人员进行和接听电话、 发送和接收文本消息和嵌入到移动应用程序的基于浏览器和本机的 VoIP 电话。  让我们简要温习这之前工作的方式。

### 接收电话和短消息

Twilio 允许开发人员[购买可编程的电话号码][purchase_phone]可以用来同时发送和接收电话和短信。  当 Twilio 数字接收的入站的调用或文本时，Twilio 将 web 应用程序 HTTP POST 或发送 GET 请求，询问如何处理呼叫或文字的说明。  您的服务器将响应与[TwiML][twiml]，一组简单的 XML 标记包含说明如何处理呼叫或文本的 Twilio 的 HTTP 请求。  一会儿，我们将看到 TwiML 的例子。

### 打电话和发送短信

通过 HTTP 请求到 Twilio web 服务 API，开发人员可以发送短信或启动出站电话呼叫。  对于出站呼叫，开发人员还必须指定返回 TwiML 说明如何处理出站呼叫，一旦它连接的 URL。

### 在用户界面代码 （JavaScript，iOS 或 Android） 中嵌入 VoIP 功能

Twilio 提供了可以将任何桌面 web 浏览器、 iOS 应用程序或 Android 应用程序变成 VoIP 电话客户端 SDK。  在本文中，我们将重点讨论如何使用 VoIP 在浏览器中调用。  除了 Twilio JavaScript SDK 在浏览器中运行，服务器端应用程序 （我们 node.js 应用程序） 必须用于向客户端 JavaScript 发出"功能标记"。  您可以阅读更多有关使用 VoIP node.js [Twilio 适用于开发人员博客上][voipnode]。

<a id="signup"/>
## 注册为 Twilio （Microsoft 折扣）

在使用 Twilio 服务之前, 必须第一个[注册帐户][注册]。  Microsoft Azure 客户收到特殊折扣-[请务必在此注册][注册]！

<a id="azuresite"/>
## 创建和部署 node.js Azure 网站

接下来，您将需要创建 node.js 网站对 Azure 上运行。  [这样的官方文档位于此处][azure_new_site]。  在较高级别，将进行以下︰

* 如果您还没有，注册，Azure 帐户，
* 使用 Azure 管理控制台创建新网站
* 添加源控件支持 （我们将假定您使用 git）
* 创建文件`server.js`与 node.js 简单 web 应用程序
* 此简单应用程序部署到 Azure

<a id="twiliomodule"/>
## 配置 Twilio 模块

接下来，我们将开始编写一个简单的 node.js 应用程序，从而使 Twilio API 的使用。  我们开始之前，我们需要配置我们的 Twilio 帐户凭据。  

### 在系统环境变量配置 Twilio 凭据

为使身份验证针对 Twilio 后端的请求，我们需要我们的帐户的 SID 和身份验证令牌，作为用户名和密码为我们 Twilio 帐户设置的函数。 使用 Azure 中的节点模块配置为使用这些最安全的办法是通过系统环境变量，可以直接在 Azure 的管理控制台中设置。

选择您的 node.js 网站，然后单击"配置"链接。  如果您向下的滚动一点，您将看到一个区域，您可以为应用程序设置配置属性。  输入您的 Twilio 帐户凭据 ([在 Twilio 面板上找到][twilio_dashboard])，如所示-请务必对其命名"TWILIO_ACCOUNT_SID"和"TWILIO_AUTH_TOKEN"，分别︰

![Azure 的管理控制台][azure-admin-console]

这些变量的配置之后，在 Azure 控制台重新启动您的应用程序。

### 声明中 package.json 的 Twilio 模块

接下来，我们需要创建 package.json，来管理我们通过[npm]的节点模块依赖关系。  在同一级别为 Azure/node.js 教程中创建的"server.js"文件，创建一个名为"package.json"文件。  在此文件中，将以下︰

  {"name":"应用程序的名称""版本":"0.0.1"，而"专用": 真"脚本": {"开始":"节点服务器"}，"依赖项": {"快速":"3.1.0"，"ejs":"*"，"twilio":"*"}}

此声明作为一个依赖项，以及流行[快速 web 框架][明确]和 EJS 模板引擎的 twilio 模块。  好了，现在我们都准备好，-让我们编写一些代码 ！

<a id="makecall"/>
## 进行出站呼叫

让我们创建一个简单的窗体，将打到很多，我们选择。  打开 server.js，然后输入以下代码。  请注意在那里显示"CHANGE_ME"的放在那里的 azure 网站名称︰

    // Module dependencies
    var express = require('express'), 
      path = require('path'), 
      http = require('http'), 
      twilio = require('twilio');

    // Create Express web application
    var app = express();

    // Express configuration
    app.configure(function(){
      app.set('port', process.env.PORT || 3000);
      app.set('views', __dirname + '/views');
      app.set('view engine', 'ejs');
      app.use(express.favicon());
      app.use(express.logger('dev'));
      app.use(express.bodyParser());
      app.use(express.methodOverride());
      app.use(app.router);
      app.use(express.static(path.join(__dirname, 'public')));
    });
    app.configure('development', function(){
      app.use(express.errorHandler());
    });

    // Render an HTML user interface for the application's home page
    app.get('/', function(request, response) {
      response.render('index');
    });

    // Handle the form POST to place a call
    app.post('/call', function(request, response) {
      var client = twilio();
      client.makeCall({
          // make a call to this number
          to:request.body.number,

          // Change to a Twilio number you bought - see:
          // https://www.twilio.com/user/account/phone-numbers/incoming
          from:'+15558675309',

          // A URL in our app which generates TwiML
          // Change "CHANGE_ME" to your app's name
          url:'https://CHANGE_ME.azurewebsites.net/outbound_call'
      }, function(error, data) {
          // Go back to the home page
          response.redirect('/');
      });
    });

    // Generate TwiML to handle an outbound call
    app.post('/outbound_call', function(request, response) {
      var twiml = new twilio.TwimlResponse();

      // Say a message to the call's receiver 
      twiml.say('hello - thanks for checking out Twilio and Azure', {
          voice:'woman'
      });

      response.set('Content-Type', 'text/xml');
      response.send(twiml.toString());
    });

    // Start server
    http.createServer(app).listen(app.get('port'), function(){
      console.log("Express server listening on port " + app.get('port'));
    });

接下来，创建一个名为"视图"-此目录的目录，创建一个名为"index.ejs"，其中内容如下文件︰

    <!DOCTYPE html>
    <html>
    <head>
      <title>Twilio Test</title>
      <style>
        input { height:20px; width:300px; font-size:18px; margin:5px; padding:5px; }
      </style>
    </head>
    <body>
      <h1>Twilio Test</h1>
      <form action="/call" method="POST">
          <input placeholder="Enter a phone number" name="number"/>
          <br/>
          <input type="submit" value="Call the number above"/>
      </form>
    </body>
    </html>

现在，将您的网站部署到 Azure 并且打开家里。  您应该能够在文本字段中，输入您的电话号码，并接收来自您的 Twilio 号的呼叫 ！

<a id="sendmessage"/>
## 发送 SMS 消息

现在，让我们设置的用户界面和处理逻辑的形式发送文本消息。  打开"server.js"，和"app.post"在最后一次调用之后添加以下代码︰

    app.post('/sms', function(request, response) {
      var client = twilio();
      client.sendSms({
          // send a text to this number
          to:request.body.number,

          // A Twilio number you bought - see:
          // https://www.twilio.com/user/account/phone-numbers/incoming
          from:'+15558675309',

          // The body of the text message
          body: request.body.message
          
      }, function(error, data) {
          // Go back to the home page
          response.redirect('/');
      });
    });

在"views/index.ejs"中，添加下第一个提交数字和文本消息的另一个窗体︰

    <form action="/sms" method="POST">
      <input placeholder="Enter a phone number" name="number"/>
      <br/>
      <input placeholder="Enter a message to send" name="message"/>
      <br/>
      <input type="submit" value="Send text to the number above"/>
    </form>

重新部署到 Azure，您的应用程序，您现在应该能够提交该窗体并发送您自己 （或任何的好朋友） 短信 ！

<a id="nextsteps"/>
## 下一步行动

现在，您已了解使用 node.js 和 Twilio 生成的应用程序进行通信的基础知识。  但是，这些示例只是划伤的什么是可能与 Twilio 和 node.js 表面。  与 node.js 使用 Twilio 的详细信息，查看以下资源︰

* [官方的模块文档][文档]
* [在 VoIP 与 node.js 应用程序教程][voipnode]
* [Votr-一个实时 SMS 投票应用程序与 node.js，CouchDB （三部分）][votr]
* [结对编程，在浏览器中与 node.js][对]

我们希望你喜欢在 Azure 上攻 node.js，Twilio ！

[purchase_phone]: https://www.twilio.com/user/account/phone-numbers/available/local
[twiml]: https://www.twilio.com/docs/api/twiml
[注册]: http://ahoy.twilio.com/azure
[azure_new_site]: http://www.windowsazure.com/develop/nodejs/tutorials/create-a-website-(mac)/
[twilio_dashboard]: https://www.twilio.com/user/account
[npm]: http://npmjs.org
[直通]: http://expressjs.com
[voipnode]: http://www.twilio.com/blog/2013/04/introduction-to-twilio-client-with-node-js.html
[文档]: http://twilio.github.io/twilio-node/
[votr]: http://www.twilio.com/blog/2012/09/building-a-real-time-sms-voting-app-part-1-node-js-couchdb.html
[对]: http://www.twilio.com/blog/2013/06/pair-programming-in-the-browser-with-twilio.html
[azure 管理控制台]: ./media/partner-twilio-nodejs-how-to-use-voice-sms/twilio_1.png




测试

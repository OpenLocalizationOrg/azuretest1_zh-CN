---
ms.openlocfilehash: df3df7ac3456c6140b5c42cf401239f92c3d0bab
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="开始使用 Azure 通知集线器 |Microsoft Azure"
    description="在本教程中，您将学习如何使用 Azure 通知集线器到 Chrome 应用程序推送通知。"
    services="notification-hubs"
    documentationCenter=""
    authors="wesmc7777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-chrome"
    ms.devlang="JavaScript"
    ms.topic="hero-article"
    ms.date="06/16/2015"
    ms.author="wesmc"/>

# 开始使用通知集线器

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

本主题演示如何使用 Azure 通知集线器到 Chrome 应用程序发送推式通知。

使用 Chrome 应用程序通知的主要好处之一是通知 Google Chrome 浏览器的上下文中显示出来。 您不必在 Chrome 的应用程序运行或 （尽管必须运行 Chrome 浏览器本身） 在浏览器中打开。 您还在 Chrome 通知窗口中获取所有通知的一个整合的视图。

>[AZURE.NOTE] 这不是一般的浏览器推式通知和特定于应用程序镶边 — 详细信息，请参阅[铬应用程序概述]。 铬色应用程序以前称为"打包的应用程序"和不同于简单"承载的应用程序"。 上的差异，请参阅[安装 Web 应用程序]。 Chrome 的应用程序还可以运行在移动 （Android 和 iOS） 通过 Apache Cordova。 [铬在手机上的应用程序]若要了解详细信息，请参阅。

在本教程中，我们将创建铬应用程序通过使用 Google 云消息 (GCM) 接收推式通知。 完成本教程后，您将能够广播给所有已安装了此应用程序中铬的 Chrome 用户的推式通知。

本教程将引导您完成以下基本步骤启用推式通知︰

* [启用消息的 Google 云](#register)
* [配置通知中心](#configure-hub)
* [将铬应用程序连接到通知中心](#connect-app)
* [将通知发送到您镶边的应用程序](#send)
* [下一步行动](#next-steps)

本教程演示中使用通知集线器的简单广播的方案。 GCM 和 Azure 通知集线器配置等同于 Android，因为[Google Chrome 的云的消息]已被否决，并且同一 GCM 现在支持 Android 设备和 Chrome 实例配置。

请确保在"下一步行动"部分以查看如何使用通知集线器来解决特定的用户和组的设备按照教程。

>[AZURE.NOTE] 若要完成本教程，您必须为活动 Azure 帐户。 如果您没有帐户，您可以在几分钟创建免费的试用帐户。 有关详细信息，请参阅[Azure 免费试用版](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%notification-hubs-chrome-get-started%2F)。

##<a id="register"></a>启用消息的 Google 云

1. 导航至[Google 云控制台]网站使用您的 Google 帐户凭据登录，然后单击**创建项目**按钮。 提供适当的**项目名称**，然后再单击**创建**按钮。

    ![][1]

2. 记下您刚才创建的项目的**项目**页上的**项目数**。 您将使用这中铬 App 的**GCM 发件人 ID**作为 GCM 注册。

    ![][2]

3. 在左窗格中， **Api 和身份验证**，请单击，然后向下滚动并单击切换启用**Google Android 的消息传递的云**。 您不必启用**Google Chrome 的云的消息**。

    ![][3]

4. 在左窗格中，单击**凭据** > **创建新密钥** > **服务器密钥** > **创建**。

    ![][4]

5. 记下服务器**API 密钥**。 您将配置这样通知中心下一步，以使其可以向 GCM 发送推式通知。

    ![][5]

##<a id="configure-hub"></a>配置通知中心

1. 登录到[Azure 门户]，然后单击**+ 新**屏幕的左下角。

2. 单击**服务应用程序** > **服务总线** > **通知中心** > **快速创建**。 通知中心为键入一个名称，选择您需要的区域，然后单击**创建新的通知中心**。

    ![][6]

4. 转到您刚才创建的通知中心。 单击可容纳通知网络集线器 (通常为***通知中心名称*-ns**) 的命名空间。

    ![][7]

5. 单击**通知集线器**标签的顶部。

    ![][8]

6. 单击顶部的**配置**选项卡。

    ![][9]

7. 在**配置**选项卡上，向下滚动到**google 云消息设置**，输入您在上一节中，获得的**API 密钥**值，然后单击**保存**。

    ![][10]

8. 选择顶部，**仪表板**选项卡，然后单击底部的**连接信息**。

    ![][11]

9. 请注意**DefaultListenSharedAccessSignature** （这将需要镶边 App 上注册通知中心） 和**DefaultFullSharedAccessSignature** （这将需要发送通知）。

    ![][12]

通知中心现已配置为使用 GCM，并有进一步配置所需的连接字符串。

##<a id="connect-app"></a>将铬应用程序连接到通知中心

###创建新的铬应用程序

下面的示例基于[Chrome GCM 示例应用程序]，并使用创建一个 Chrome 应用程序的推荐的方式。 在下面的章节中，我们将重点介绍与 Azure 通知集线器相关的步骤。 我们建议这个 Chrome 应用程序从[Chrome 应用程序通知中心示例]下载源。

Chrome 的应用程序将创建通过 JavaScript，并可以使用任何首选的 word 编辑器来创建它。 下面是此应用程序中铬的外观。

    ![][15]

2. 创建一个文件夹并将其命名**ChromePushApp**或所需的任何操作。

3. 从这个文件夹中的[加密 js 库]下载**cryto js 库**。 此库文件夹将包含两个子文件夹︰**组件**和**汇总**。

4. 创建一个**manifest.json**文件。 Chrome 的所有应用程序都描述元数据的应用程序清单文件和后盾，特别情况下，权限，可供该应用程序。

        {
          "name": "NH-GCM Notifications",
          "description": "Chrome platform app.",
          "manifest_version": 2,
          "version": "0.1",
          "app": {
            "background": {
              "scripts": ["background.js"]
            }
          },
          "permissions": ["gcm", "storage", "notifications", "https://*.servicebus.windows.net/*"],
          "icons": { "128": "gcm_128.png" }
        }

    注意**权限**元素，它指定此应用程序中铬能够从 GCM 接收推式通知。 它还必须指定镶边的应用程序将其余部分调用注册 Azure 通知集线器 URI。
    这将使用一个图标文件，gcm_128.png，，您会发现在重用原始 GCM 示例的源。 您可以使用任何所需的图像。

5. 创建一个名为**background.js** ，用下面的代码文件︰

        // Returns a new notification ID used in the notification.
        function getNotificationId() {
          var id = Math.floor(Math.random() * 9007199254740992) + 1;
          return id.toString();
        }

        function messageReceived(message) {
          // A message is an object with a data property that
          // consists of key-value pairs.

          // Concatenate all key-value pairs to form a display string.
          var messageString = "";
          for (var key in message.data) {
            if (messageString != "")
              messageString += ", "
            messageString += key + ":" + message.data[key];
          }
          console.log("Message received: " + messageString);

          // Pop up a notification to show the GCM message.
          chrome.notifications.create(getNotificationId(), {
            title: 'GCM Message',
            iconUrl: 'gcm_128.png',
            type: 'basic',
            message: messageString
          }, function() {});
        }

        var registerWindowCreated = false;

        function firstTimeRegistration() {
          chrome.storage.local.get("registered", function(result) {

            registerWindowCreated = true;
            chrome.app.window.create(
              "register.html",
              {  width: 520,
                 height: 500,
                 frame: 'chrome'
              },
              function(appWin) {}
            );
          });
        }

        // Set up a listener for GCM message event.
        chrome.gcm.onMessage.addListener(messageReceived);

        // Set up listeners to trigger the first-time registration.
        chrome.runtime.onInstalled.addListener(firstTimeRegistration);
        chrome.runtime.onStartup.addListener(firstTimeRegistration);

    这是弹出镶边的应用程序窗口 (**register.html**) 的 HTML，还定义了处理程序**messageReceived**来处理传入的推式通知的文件。

6. 创建名为**register.html**定义镶边的应用程序的用户界面的文件。 请注意，此示例使用*CryptoJS v3.1.2*。 如果您下载任何其他版本，然后修复脚本 src 路径。

        <html>

        <head>
        <title>GCM Registration</title>
        <script src="register.js"></script>
        <script src="CryptoJS v3.1.2/rollups/hmac-sha256.js"></script>
        <script src="CryptoJS v3.1.2/components/enc-base64-min.js"></script>
        </head>

        <body>

        Sender ID:<br/><input id="senderId" type="TEXT" size="20"><br/>
        <button id="registerWithGCM">Register with GCM</button>
        <br/>
        <br/>
        <br/>

        Notification Hub Name:<br/><input id="hubName" type="TEXT" style="width:400px"><br/><br/>
        Connection String:<br/><textarea id="connectionString" type="TEXT" style="width:400px;height:60px"></textarea>

        <br/>

        <button id="registerWithNH" disabled="true">Register with Azure Notification Hubs</button>

        <br/>
        <br/>

        <textarea id="console" type="TEXT" readonly style="width:500px;height:200px;background-color:#e5e5e5;padding:5px"></textarea>

        </body>

        </html>

7. 创建一个名为**register.js** ，使用下面的代码文件。 此文件指定**register.html**背后的脚本。 镶边的应用程序不允许内联执行，所以您需要为您的 UI 创建单独的备份脚本。

        var registrationId = "";
        var hubName        = "", connectionString = "";
        var originalUri    = "", targetUri = "", endpoint = "", sasKeyName = "", sasKeyValue = "", sasToken = "";

        window.onload = function() {
           document.getElementById("registerWithGCM").onclick = registerWithGCM;  
           document.getElementById("registerWithNH").onclick = registerWithNH;
           updateLog("You have not registered yet. Please provider sender ID and register with GCM and then with  Notification Hubs.");
        }

        function updateLog(status) {
          currentStatus = document.getElementById("console").innerHTML;
          if (currentStatus != "") {
            currentStatus = currentStatus + "\n\n";
          }

          document.getElementById("console").innerHTML = currentStatus  + status;
        }

        function registerWithGCM() {
          var senderId = document.getElementById("senderId").value.trim();
          chrome.gcm.register([senderId], registerCallback);

          // Prevent register button from being clicked again before the registration finishes.
          document.getElementById("registerWithGCM").disabled = true;
        }

        function registerCallback(regId) {
          registrationId = regId;
          document.getElementById("registerWithGCM").disabled = false;

          if (chrome.runtime.lastError) {
            // When the registration fails, handle the error and retry the
            // registration later.
            updateLog("Registration failed: " + chrome.runtime.lastError.message);
            return;
          }

          updateLog("Registration with GCM succeeded.");
          document.getElementById("registerWithNH").disabled = false;

          // Mark that the first-time registration is done.
          chrome.storage.local.set({registered: true});
        }

        function registerWithNH() {
          hubName = document.getElementById("hubName").value.trim();
          connectionString = document.getElementById("connectionString").value.trim();
          splitConnectionString();
          generateSaSToken();
          sendNHRegistrationRequest();
        }

        // From http://msdn.microsoft.com/library/dn495627.aspx
        function splitConnectionString()
        {
          var parts = connectionString.split(';');
          if (parts.length != 3)
          throw "Error parsing connection string";

          parts.forEach(function(part) {
            if (part.indexOf('Endpoint') == 0) {
            endpoint = 'https' + part.substring(11);
            } else if (part.indexOf('SharedAccessKeyName') == 0) {
            sasKeyName = part.substring(20);
            } else if (part.indexOf('SharedAccessKey') == 0) {
            sasKeyValue = part.substring(16);
            }
          });

          originalUri = endpoint + hubName;
        }

        function generateSaSToken()
        {
          targetUri = encodeURIComponent(originalUri.toLowerCase()).toLowerCase();
          var expiresInMins = 10; // 10 minute expiration

          // Set expiration in seconds.
          var expireOnDate = new Date();
          expireOnDate.setMinutes(expireOnDate.getMinutes() + expiresInMins);
          var expires = Date.UTC(expireOnDate.getUTCFullYear(), expireOnDate
            .getUTCMonth(), expireOnDate.getUTCDate(), expireOnDate
            .getUTCHours(), expireOnDate.getUTCMinutes(), expireOnDate
            .getUTCSeconds()) / 1000;
          var tosign = targetUri + '\n' + expires;

          // Using CryptoJS.
          var signature = CryptoJS.HmacSHA256(tosign, sasKeyValue);
          var base64signature = signature.toString(CryptoJS.enc.Base64);
          var base64UriEncoded = encodeURIComponent(base64signature);

          // Construct authorization string.
          sasToken = "SharedAccessSignature sr=" + targetUri + "&sig="
                          + base64UriEncoded + "&se=" + expires + "&skn=" + sasKeyName;
        }

        function sendNHRegistrationRequest()
        {
          var registrationPayload =
          "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
          "<entry xmlns=\"http://www.w3.org/2005/Atom\">" +
              "<content type=\"application/xml\">" +
                  "<GcmRegistrationDescription xmlns:i=\"http://www.w3.org/2001/XMLSchema-instance\" xmlns=\"http://schemas.microsoft.com/netservices/2010/10/servicebus/connect\">" +
                      "<GcmRegistrationId>{GCMRegistrationId}</GcmRegistrationId>" +
                  "</GcmRegistrationDescription>" +
              "</content>" +
          "</entry>";

          // Update the payload with the registration ID obtained earlier.
          registrationPayload = registrationPayload.replace("{GCMRegistrationId}", registrationId);

          var url = originalUri + "/registrations/?api-version=2014-09";
          var client = new XMLHttpRequest();

          client.onload = function () {
            if (client.readyState == 4) {
              if (client.status == 200) {
                updateLog("Notification Hub Registration succesful!");
                updateLog(client.responseText);
              } else {
                updateLog("Notification Hub Registration did not succeed!");
                updateLog("HTTP Status: " + client.status + " : " + client.statusText);
                updateLog("HTTP Response: " + "\n" + client.responseText);
              }
            }
          };

          client.onerror = function () {
                updateLog("ERROR - Notification Hub Registration did not succeed!");
          }

          client.open("POST", url, true);
          client.setRequestHeader("Content-Type", "application/atom+xml;type=entry;charset=utf-8");
          client.setRequestHeader("Authorization", sasToken);
          client.setRequestHeader("x-ms-version", "2014-09");

          try {
              client.send(registrationPayload);
          }
          catch(err) {
              updateLog(err.message);
          }
        }

    以上脚本具有以下优点︰
    - *window.onload*定义了用户界面的两个按钮的按钮单击事件。 一个与 GCM，注册和其他使用注册与 GCM 注册 Azure 通知集线器后返回注册 ID。
    - *updateLog*是定义了一个简单的日志记录函数的函数。
    - *registerWithGCM*是第一个按钮的 click 处理程序中，这样 GCM 注册此镶边的应用程序实例调用**chrome.gcm.register** 。
    - *registerCallback*是在上面的 GCM 注册调用返回时调用的回调函数。
    - *registerWithNH*是第二个按钮单击处理程序，它注册通知集线器。 它获取**hubName**和**连接字符串**（该用户已指定） 和工艺品的通知集线器注册 REST API 调用。
    - *splitConnectionString*和*generateSaSToken*都是创建所有 REST API 调用中必须发送一个 SaS 令牌的 JavaScript 实现。 有关详细信息，请参阅[常见的概念](http://msdn.microsoft.com/library/dn495627.aspx)。
    - *sendNHRegistrationRequest*是使 HTTP REST 调用的函数。
    - *registrationPayload*定义注册 XML 有效负载。 有关详细信息，请参阅[创建注册 NH REST API]。 我们用我们所收到的来自 GCM 更新的注册 ID。
    - *客户端*是我们用来进行 HTTP POST 请求的**XMLHttpRequest**的实例。 请注意我们使用**sasToken**更新**授权**标头。 成功完成此调用将此镶边的应用程序实例注册 Azure 通知集线器。


末尾的此文件夹，您应看到以下视图︰
    ![][21]

###设置并测试您镶边的应用程序

1. 打开您的 Chrome 浏览器。 打开**Chrome 扩展**并使**开发人员模式运行**。

    ![][16]

2. 单击**加载解包的扩展**并导航到您在其中创建文件的文件夹。 您还可以使用**铬应用程序和扩展开发人员工具**。 此工具本身 （Chrome Web 存储区中安装） 中铬的应用程序并提供高级调试功能镶边的应用程序开发。

    ![][17]

3. 如果没有任何错误创建镶边的应用程序，则您将看到镶边应用程序显示出来。

    ![][18]

4. 输入**项目编号**，你前面从**Google 云控制台**作为发件人 ID，然后单击**与 GCM 注册**。 必须看到消息**注册成功的 GCM。**

    ![][19]

5. 输入您的**通知中心名称**和您之前，获得从 Azure 的门户网站**DefaultListenSharedAccessSignature** ，然后单击**注册 Azure 通知中心**。 必须看到消息**通知中心注册成功 ！** 和 id 的注册响应，其中包含的 Azure 通知集线器注册详细信息。

    ![][20]  

##<a name="send"></a>将通知发送到您镶边的应用程序

在本教程中，您可以通过使用.NET 控制台应用程序发送通知。 但是，您可以从任何后端通过使用通知集线器发送通知<a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">REST 接口</a>。

如何从通知集线器与集成 Azure 移动服务后端发送通知的示例，请参阅"入门使用推式通知中移动服务"([.NET 后端](../mobile-services-javascript-backend-android-get-started-push.md) | [JavaScript 后端](../mobile-services-javascript-backend-android-get-started-push.md))。  
有关如何通过使用 REST Api 发送通知的示例，请参阅"如何使用通知集线器从 Java/PHP/Python"([Java](notification-hubs-java-backend-how-to.md) | [PHP](notification-hubs-php-backend-how-to.md) | [Python](notification-hubs-python-backend-how-to.md))。

1. 在 Visual Studio 中，从**文件**菜单中，选择**新建**，然后选择**项目**。 在**C#**， **Windows**和**控制台应用程序**，请单击，然后单击**确定**。  这将创建一个新的控制台应用程序项目。

2. 从**工具**菜单上，单击**库程序包管理器**，然后**程序包管理器控制台**。 此时将显示程序包管理器控制台。

3. 在控制台窗口中，执行以下命令︰

        Install-Package WindowsAzure.ServiceBus

    这将引用添加到带有 Azure 服务总线 SDK <a href="http://nuget.org/packages/  WindowsAzure.ServiceBus/">WindowsAzure.ServiceBus NuGet 程序包</a>。

4. 打开**Program.cs**文件，然后添加以下`using`语句︰

        using Microsoft.ServiceBus.Notifications;

5. 在**程序**类中，添加下面的方法︰

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            String message = "{\"data\":{\"message\":\"Hello Chrome from Azure Notification Hubs\"}}";
            await hub.SendGcmNativeNotificationAsync(message);
        }

    请确保*集线器名称*的占位符替换为通知中心门户**通知集线器**选项卡中显示的名称。 此外，将连接字符串占位符替换调用**DefaultFullSharedAccessSignature**获取"配置通知中心。"一节中的连接字符串

    >[AZURE.NOTE] 请确保您具有**完全**访问权限，不**听**访问使用的连接字符串。 **聆听**访问字符串没有发送通知的权限。

5. **Main**方法中添加以下行︰

         SendNotificationAsync();
         Console.ReadLine();

6. 请确保您的 Chrome 浏览器处于打开状态。 铬色应用程序不需要此打开。 在您的桌面上应该看到以下通知弹出。

    ![][13]

7. 您还可以通过使用 Chrome 通知窗口任务栏上 （在 Windows 中) 查看所有通知镶边运行时。

    ![][14]

## <a name="next-steps"> </a>下一步行动

在此简单示例中，您将广播通知于 Chrome 应用程序。
了解有关通知[通知集线器概述]中的集线器。
要瞄准特定的用户，请参阅本教程[Azure 通知集线器通知用户]。 如果您想要段由有兴趣的集团用户，您可以阅读[Azure 通知集线器爆炸性新闻]。

<!-- Images. -->
[1]: ./media/notification-hubs-chrome-get-started/GoogleConsoleCreateProject.PNG
[2]: ./media/notification-hubs-chrome-get-started/GoogleProjectNumber.png
[3]: ./media/notification-hubs-chrome-get-started/EnableGCM.png
[4]: ./media/notification-hubs-chrome-get-started/CreateServerKey.png
[5]: ./media/notification-hubs-chrome-get-started/ServerKey.png
[6]: ./media/notification-hubs-chrome-get-started/CreateNH.png
[7]: ./media/notification-hubs-chrome-get-started/NHNamespace.png
[8]: ./media/notification-hubs-chrome-get-started/NamespaceConfigure.png
[9]: ./media/notification-hubs-chrome-get-started/NHConfigure.png
[10]: ./media/notification-hubs-chrome-get-started/NHConfigureGCM.png
[11]: ./media/notification-hubs-chrome-get-started/NHDashboard.png
[12]: ./media/notification-hubs-chrome-get-started/NHConnString.png
[13]: ./media/notification-hubs-chrome-get-started/ChromeNotification.png
[14]: ./media/notification-hubs-chrome-get-started/ChromeNotificationWindow.png
[15]: ./media/notification-hubs-chrome-get-started/ChromeApp.png
[16]: ./media/notification-hubs-chrome-get-started/ChromeExtensions.png
[17]: ./media/notification-hubs-chrome-get-started/ChromeLoadExtension.png
[18]: ./media/notification-hubs-chrome-get-started/ChromeAppLoaded.png
[19]: ./media/notification-hubs-chrome-get-started/ChromeAppGCM.png
[20]: ./media/notification-hubs-chrome-get-started/ChromeAppNH.png
[21]: ./media/notification-hubs-chrome-get-started/FinalFolderView.png

<!-- URLs. -->
[铬色应用程序通知中心示例]: http://google.com
[Google 云控制台]: http://cloud.google.com/console
[Azure 门户]: https://manage.windowsazure.com/
[通知集线器概述]: http://msdn.microsoft.com/library/jj927170.aspx
[铬色应用程序概述]: https://developer.chrome.com/apps/about_apps
[铬色 GCM 应用程序示例]: https://github.com/GoogleChrome/chrome-app-samples/tree/master/samples/gcm-notifications
[可安装的 Web 应用程序]: https://developers.google.com/chrome/apps/docs/
[铬在手机上的应用程序]: https://developer.chrome.com/apps/chrome_apps_on_mobile
[创建注册 NH REST API，]: http://msdn.microsoft.com/library/azure/dn223265.aspx
[加密 js 库]: http://code.google.com/p/crypto-js/
[GCM 与铬应用程序]: https://developer.chrome.com/apps/cloudMessaging
[Google Chrome 讯息的云]: https://developer.chrome.com/apps/cloudMessagingV1
[Azure 通知集线器通知用户]: notification-hubs-aspnet-backend-windows-dotnet-notify-users.md
[Azure 通知集线器的新闻]: notification-hubs-windows-store-dotnet-send-breaking-news.md

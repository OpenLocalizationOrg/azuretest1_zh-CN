---
ms.openlocfilehash: cc2e0e47fc922d8e447dd837f71dfdd6671ab834
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何使用 Python 的通知集线器" 
    description="了解如何从一个后端的 Python 中使用 Azure 通知集线器。" 
    services="notification-hubs" 
    documentationCenter="" 
    authors="ysxu" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="notification-hubs" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="python" 
    ms.devlang="php" 
    ms.topic="article" 
    ms.date="07/17/2015" 
    ms.author="yuaxu"/>

# 如何使用 Python 从通知集线器
> [AZURE.SELECTOR] 
- [Java](notification-hubs-php-backend-how-to.md)
- [PHP](notification-hubs-python-backend-how-to.md)
- [Python](notification-hubs-nodejs-how-to-use-notification-hubs.md)
- [Node.js](notification-hubs-nodejs-how-to-use-notification-hubs.md)
        
从 Java/PHP/Python/拼音后端使用通知中心 REST 接口[通知集线器 REST Api](http://msdn.microsoft.com/library/dn223264.aspx)的 MSDN 主题中所述，您可以访问所有通知集线器功能。

> [AZURE.NOTE] 这是在 Python 中实现通知发送的示例参考实现，并不是正式支持的通知中心 Python SDK。

> [AZURE.NOTE] 此示例是使用 Python 3.4 编写的。

在本主题中，我们将介绍如何︰

* 生成通知集线器功能在 Python 中的其他客户端。
* 发送给通知中心 REST Api 使用 Python 的通知。 
* 获得其他 HTTP 请求/响应的转储调试/教育的目的。 

您可以按照[获得入门的教程](notification-hubs-windows-store-dotnet-get-started.md)为您移动平台的选择，在 Python 中实现的后端部分。

> [AZURE.NOTE] 样本的范围只限于发送通知，则不执行任何注册管理。

## 客户端界面
主客户端接口可以提供[.NET 通知集线器 SDK](http://msdn.microsoft.com/library/jj933431.aspx)中提供的同一方法。 这将允许您直接翻译的教程和示例在该网站上，目前，互联网社区的贡献。

您可以查找在[Python 其余包装样本]中可用的所有代码。

例如，要创建一个客户端︰

    hub = NotificationHub("myConnectionString", "myNotificationHubName", isDebug)
    
若要发送 Windows toast 通知︰
    
    wns_payload = """<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Hello world!</text></binding></visual></toast>"""
    hub.send_windows_notification(wns_payload)
    
## 实现
如果不是已经忘了，请按照最多可以用来实现后端的最后一节我们[获得入门的教程]。

若要实现完全的其余部分包装的所有详细信息可以在[MSDN](http://msdn.microsoft.com/library/dn530746.aspx)中找到。 在本节中我们将介绍访问通知集线器 REST 端点和发送通知所需的主要步骤的 Python 实现

1. 分析连接字符串
2. 生成授权令牌
3. 发送通知使用 HTTP REST API，

### 分析连接字符串

下面是实现客户端，其构造函数来分析连接字符串的主类︰

    class NotificationHub:
        API_VERSION = "?api-version=2013-10"
        DEBUG_SEND = "&test"
    
        def __init__(self, connection_string=None, hub_name=None, debug=0):
            self.HubName = hub_name
            self.Debug = debug
    
            # Parse connection string
            parts = connection_string.split(';')
            if len(parts) != 3:
                raise Exception("Invalid ConnectionString.")
    
            for part in parts:
                if part.startswith('Endpoint'):
                    self.Endpoint = 'https' + part[11:]
                if part.startswith('SharedAccessKeyName'):
                    self.SasKeyName = part[20:]
                if part.startswith('SharedAccessKey'):
                    self.SasKeyValue = part[16:]


### 创建安全令牌
创建安全令牌的详细信息可[在此处](http://msdn.microsoft.com/library/dn495627.aspx)。
下列方法必须被添加到**NotificationHub**类以创建基于当前请求和凭据在连接字符串中提取的 URI 的标记。

    @staticmethod
    def get_expiry():
        # By default returns an expiration of 5 minutes (=300 seconds) from now
        return int(round(time.time() + 300))

    @staticmethod
    def encode_base64(data):
        return base64.b64encode(data)

    def sign_string(self, to_sign):
        key = self.SasKeyValue.encode('utf-8')
        to_sign = to_sign.encode('utf-8')
        signed_hmac_sha256 = hmac.HMAC(key, to_sign, hashlib.sha256)
        digest = signed_hmac_sha256.digest()
        encoded_digest = self.encode_base64(digest)
        return encoded_digest

    def generate_sas_token(self):
        target_uri = self.Endpoint + self.HubName
        my_uri = urllib.parse.quote(target_uri, '').lower()
        expiry = str(self.get_expiry())
        to_sign = my_uri + '\n' + expiry
        signature = urllib.parse.quote(self.sign_string(to_sign))
        auth_format = 'SharedAccessSignature sig={0}&se={1}&skn={2}&sr={3}'
        sas_token = auth_format.format(signature, expiry, self.SasKeyName, my_uri)
        return sas_token

### 发送通知使用 HTTP REST API，
第一，但仍使使用定义类表示一个通知。

    class Notification:
        def __init__(self, notification_format=None, payload=None, debug=0):
            valid_formats = ['template', 'apple', 'gcm', 'windows', 'windowsphone', "adm", "baidu"]
            if not any(x in notification_format for x in valid_formats):
                raise Exception(
                    "Invalid Notification format. " +
                    "Must be one of the following - 'template', 'apple', 'gcm', 'windows', 'windowsphone', 'adm', 'baidu'")
    
            self.format = notification_format
            self.payload = payload
    
            # array with keynames for headers
            # Note: Some headers are mandatory: Windows: X-WNS-Type, WindowsPhone: X-NotificationType
            # Note: For Apple you can set Expiry with header: ServiceBusNotification-ApnsExpiry
            # in W3C DTF, YYYY-MM-DDThh:mmTZD (for example, 1997-07-16T19:20+01:00).
            self.headers = None

此类是本机的通知正文或一组模板通知，其中包含格式 （本机平台或模板） 和平台特定的属性 （如苹果过期属性和 WNS 头） 头一在属性的容器。

请参考[通知集线器 REST Api 文档](http://msdn.microsoft.com/library/dn495827.aspx)和所有可用的选项的特定通知平台的格式。

现在使用此类，我们可以编写发送通知内的**NotificationHub**类的方法。

    def make_http_request(self, url, payload, headers):
        parsed_url = urllib.parse.urlparse(url)
        connection = http.client.HTTPSConnection(parsed_url.hostname, parsed_url.port)

        if self.Debug > 0:
            connection.set_debuglevel(self.Debug)
            # adding this querystring parameter gets detailed information about the PNS send notification outcome
            url += self.DEBUG_SEND
            print("--- REQUEST ---")
            print("URI: " + url)
            print("Headers: " + json.dumps(headers, sort_keys=True, indent=4, separators=(' ', ': ')))
            print("--- END REQUEST ---\n")

        connection.request('POST', url, payload, headers)
        response = connection.getresponse()

        if self.Debug > 0:
            # print out detailed response information for debugging purpose
            print("\n\n--- RESPONSE ---")
            print(str(response.status) + " " + response.reason)
            print(response.msg)
            print(response.read())
            print("--- END RESPONSE ---")

        elif response.status != 201:
            # Successful outcome of send message is HTTP 201 - Created
            raise Exception(
                "Error sending notification. Received HTTP code " + str(response.status) + " " + response.reason)

        connection.close()

    def send_notification(self, notification, tag_or_tag_expression=None):
        url = self.Endpoint + self.HubName + '/messages' + self.API_VERSION

        json_platforms = ['template', 'apple', 'gcm', 'adm', 'baidu']

        if any(x in notification.format for x in json_platforms):
            content_type = "application/json"
            payload_to_send = json.dumps(notification.payload)
        else:
            content_type = "application/xml"
            payload_to_send = notification.payload

        headers = {
            'Content-type': content_type,
            'Authorization': self.generate_sas_token(),
            'ServiceBusNotification-Format': notification.format
        }

        if isinstance(tag_or_tag_expression, set):
            tag_list = ' || '.join(tag_or_tag_expression)
        else:
            tag_list = tag_or_tag_expression

        # add the tags/tag expressions to the headers collection
        if tag_list != "":
            headers.update({'ServiceBusNotification-Tags': tag_list})

        # add any custom headers to the headers collection that the user may have added
        if notification.headers is not None:
            headers.update(notification.headers)

        self.make_http_request(url, payload_to_send, headers)

上述方法通知网络集线器，具有正确的正文和标头发送通知的 /messages 端点发送 HTTP POST 请求。

### 使用调试属性来启用详细日志记录
初始化通知中心时启用调试属性将写出详细日志记录信息的 HTTP 请求和响应转储以及详细的通知消息发送结果。 我们最近添加此属性调用[通知集线器 TestSend 属性](http://msdn.microsoft.com/library/microsoft.servicebus.notifications.notificationhubclient.enabletestsend.aspx)返回有关通知发送结果的详细的信息。 若要使用它的初始化使用以下︰

    hub = NotificationHub("myConnectionString", "myNotificationHubName", isDebug)

通知中心发送请求 HTTP URL 获取结果附加"test"查询字符串。 

##<a name="complete-tutorial"></a>完成教程
现在您可以通过从 Python 后端发送通知完成入门教程。

初始化客户端通知集线器 （替代示[获得入门的教程]中的连接字符串和中心名称）︰

    hub = NotificationHub("myConnectionString", "myNotificationHubName")

然后添加发送代码，具体取决于您目标的移动平台。 此示例还添加了高级别的方法，以允许基于平台如 send_windows_notification 窗口; 发送通知send_apple_notification （对于苹果） 等。 

### Windows 应用商店和 Windows Phone 8.1 (非 Silverlight)

    wns_payload = """<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Test</text></binding></visual></toast>"""
    hub.send_windows_notification(wns_payload)

### Windows Phone 8.0 和 8.1 Silverlight

    hub.send_mpns_notification(toast)

### iOS

    alert_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_apple_notification(alert_payload)

### Android
    gcm_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_gcm_notification(gcm_payload)

### Kindle 火灾
    adm_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_adm_notification(adm_payload)

### Baidu
    baidu_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_baidu_notification(baidu_payload)

运行 Python 代码应生成显示在目标设备上的通知。

## 示例︰

### 启用调试属性
当初始化 NotificationHub，则会显示详细的 HTTP 请求和响应转储以及 NotificationOutcome 如下可以了解请求中传递何种 HTTP 标头，并从通知中心接收到 HTTP 响应时启用调试标志︰
    ![][1]

您将看到如详细通知中心结果 

- 当消息成功发送推送通知服务中。 
    
        <Outcome>The Notification was successfully sent to the Push Notification System</Outcome>

- 如果不没有找到任何推式通知的任何目标然后可能要查看响应 （即没有发现可能传递通知，因为登记的某些不匹配的标记没有登记） 中的以下信息

        '<NotificationOutcome xmlns="http://schemas.microsoft.com/netservices/2010/10/servicebus/connect" xmlns:i="http://www.w3.org/2001/XMLSchema-instance"><Success>0</Success><Failure>0</Failure><Results i:nil="true"/></NotificationOutcome>'

### 广播到 Windows toast 通知 

请注意当您要向 Windows 客户端发送广播的 toast 通知被发送出去的标头。 

    hub.send_windows_notification(wns_payload)

![][2]

### 发送通知指定的标记或标记表达式）

请注意标签 HTTP 标头添加到 HTTP 请求中获取的 （在下面的示例中，我们将发送通知仅对具有 '体育' 负载的登记）

    hub.send_windows_notification(wns_payload, "sports")

![][3]

### 发送通知指定多个标记

请注意标签 HTTP 标头时发送多个标记的更改。 
    
    tags = {'sports', 'politics'}
    hub.send_windows_notification(wns_payload, tags)

![][4]

### 模板化的通知

请注意，更改格式 HTTP 标头和有效负载正文发送 HTTP 请求正文的一部分︰

**客户端的已注册的模板**

        var template =
                        @"<toast><visual><binding template=""ToastText01""><text id=""1"">$(greeting_en)</text></binding></visual></toast>";
            
**服务器端-发送有效负载**
        
        template_payload = {'greeting_en': 'Hello', 'greeting_fr': 'Salut'}
        hub.send_template_notification(template_payload)

![][5]


## 下一步行动
本主题介绍如何创建一个简单的 Python 其余部分客户端通知集线器的。 从这里，您可以︰

* 下载完整的[Python 其余包装的示例]，其中包含上面的所有代码。
* 继续学习[新新闻教程]中标记功能通知集线器
* 在[本地化新闻教程]继续学习有关集线器的通知模板功能

<!-- URLs -->
[Python 其余包装示例]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-python
[获取启动的教程]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/
[重大新闻教程]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-send-breaking-news/
[本地化新闻教程]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-send-localized-breaking-news/

<!-- Images. -->
[1]: ./media/notification-hubs-python-backend-how-to/DetailedLoggingInfo.png
[2]: ./media/notification-hubs-python-backend-how-to/BroadcastScenario.png
[3]: ./media/notification-hubs-python-backend-how-to/SendWithOneTag.png
[4]: ./media/notification-hubs-python-backend-how-to/SendWithMultipleTags.png
[5]: ./media/notification-hubs-python-backend-how-to/TemplatedNotification.png
 

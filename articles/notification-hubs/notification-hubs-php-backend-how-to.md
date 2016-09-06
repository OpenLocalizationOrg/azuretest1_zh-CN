---
ms.openlocfilehash: 734dffaa1ea87eb7fd15cc776025d1a5dfc9f5c0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何使用 PHP 通知集线器" 
    description="了解如何从一个 PHP 后端使用 Azure 通知集线器。" 
    services="notification-hubs" 
    documentationCenter="" 
    authors="ysxu" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="notification-hubs" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="php" 
    ms.devlang="php" 
    ms.topic="article" 
    ms.date="07/17/2015" 
    ms.author="yuaxu"/>

# 如何使用 PHP 从通知集线器
> [AZURE.SELECTOR] 
- [Java](notification-hubs-php-backend-how-to.md)
- [PHP](notification-hubs-python-backend-how-to.md)
- [Python](notification-hubs-nodejs-how-to-use-notification-hubs.md)
- [Node.js](notification-hubs-nodejs-how-to-use-notification-hubs.md)

从 Java/PHP/拼音后端使用通知中心 REST 接口[通知集线器 REST Api](http://msdn.microsoft.com/library/dn223264.aspx)的 MSDN 主题中所述，您可以访问所有通知集线器功能。

在本主题中，我们将介绍如何︰

* 生成的其他客户端的 PHP; 通知集线器功能
* 按照[获取入门的教程](notification-hubs-ios-get-started.md)为您移动平台的选择，在 PHP 中实现的后端部分。

## 客户端界面
主客户端界面可提供[.NET 通知集线器 SDK](http://msdn.microsoft.com/library/jj933431.aspx)中提供的相同方法，这将允许您直接翻译的教程和示例当前可在此网站上，和上互联网社区的贡献。

您可以查找所有[PHP 其余包装示例]中提供的代码。

例如，要创建一个客户端︰

    $hub = new NotificationHub("connection string", "hubname"); 

若要发送 iOS 本机的通知︰
    
    $notification = new Notification("apple", '{"aps":{"alert": "Hello!"}}');
    $hub->sendNotification($notification);

## 实现
如果不是已经忘了，请按照最多可以用来实现后端的最后一节我们[获得入门的教程]。
此外，如果您希望可以使用[PHP 其余包装示例]中的代码并直接进入[完成本教程](#complete-tutorial)部分。

若要实现完全的其余部分包装的所有详细信息可以在[MSDN](http://msdn.microsoft.com/library/dn530746.aspx)中找到。 在本节中我们将介绍的 PHP 实现的访问通知集线器 REST 端点所需的主要步骤︰

1. 分析连接字符串
2. 生成授权令牌
3. 执行 HTTP 调用

### 分析连接字符串

下面是实现客户端操作来分析连接字符串的构造函数的主类︰

    class NotificationHub {
        const API_VERSION = "?api-version=2013-10";
    
        private $endpoint;
        private $hubPath;
        private $sasKeyName;
        private $sasKeyValue;
    
        function __construct($connectionString, $hubPath) {
            $this->hubPath = $hubPath;
    
            $this->parseConnectionString($connectionString);
        }
    
        private function parseConnectionString($connectionString) {
            $parts = explode(";", $connectionString);
            if (sizeof($parts) != 3) {
                throw new Exception("Error parsing connection string: " . $connectionString);
            }
    
            foreach ($parts as $part) {
                if (strpos($part, "Endpoint") === 0) {
                    $this->endpoint = "https" . substr($part, 11);
                } else if (strpos($part, "SharedAccessKeyName") === 0) {
                    $this->sasKeyName = substr($part, 20);
                } else if (strpos($part, "SharedAccessKey") === 0) {
                    $this->sasKeyValue = substr($part, 16);
                }
            }
        }
    }


### 创建安全令牌
创建安全令牌的详细信息可[在此处](http://msdn.microsoft.com/library/dn495627.aspx)。
下面的方法已被添加到**NotificationHub**类以创建基于当前请求和凭据在连接字符串中提取的 URI 的标记。

    private function generateSasToken($uri) {
        $targetUri = strtolower(rawurlencode(strtolower($uri)));

        $expires = time();
        $expiresInMins = 60;
        $expires = $expires + $expiresInMins * 60;
        $toSign = $targetUri . "\n" . $expires;

        $signature = rawurlencode(base64_encode(hash_hmac('sha256', $toSign, $this->sasKeyValue, TRUE)));

        $token = "SharedAccessSignature sr=" . $targetUri . "&sig="
                    . $signature . "&se=" . $expires . "&skn=" . $this->sasKeyName;

        return $token;
    }

### 发送通知
第一，但仍使使用定义类表示一个通知。

    class Notification {
        public $format;
        public $payload;
    
        # array with keynames for headers
        # Note: Some headers are mandatory: Windows: X-WNS-Type, WindowsPhone: X-NotificationType
        # Note: For Apple you can set Expiry with header: ServiceBusNotification-ApnsExpiry in W3C DTF, YYYY-MM-DDThh:mmTZD (for example, 1997-07-16T19:20+01:00).
        public $headers;
    
        function __construct($format, $payload) {
            if (!in_array($format, ["template", "apple", "windows", "gcm", "windowsphone"])) {
                throw new Exception('Invalid format: ' . $format);
            }
    
            $this->format = $format;
            $this->payload = $payload;
        }
    }

此类是本机通知主体，或一组属性对用例的模板通知和一套标头包含格式 （本机平台或模板） 和特定于平台的属性 （如苹果过期属性和 WNS 标题） 的容器。

请参考[通知集线器 REST Api 文档](http://msdn.microsoft.com/library/dn495827.aspx)和所有可用的选项的特定通知平台的格式。

有了此类，我们现在可以编写发送通知内的**NotificationHub**类的方法。

    public function sendNotification($notification) {
        $this->sendNotification($notification, "");
    }

    public function sendNotification($notification, $tagsOrTagExpression) {
        if (is_array($tagsOrTagExpression)) {
            $tagExpression = implode(" || ", $tagsOrTagExpression);
        } else {
            $tagExpression = $tagsOrTagExpression;
        }

        # build uri
        $uri = $this->endpoint . $this->hubPath . "/messages" . NotificationHub::API_VERSION;
        $ch = curl_init($uri);

        if (in_array($notification->format, ["template", "apple", "gcm"])) {
            $contentType = "application/json";
        } else {
            $contentType = "application/xml";
        }

        $token = $this->generateSasToken($uri);

        $headers = [
            'Authorization: '.$token,
            'Content-Type: '.$contentType,
            'ServiceBusNotification-Format: '.$notification->format
        ];

        if ("" !== $tagExpression) {
            $headers[] = 'ServiceBusNotification-Tags: '.$tagExpression;
        }

        # add headers for other platforms
        if (is_array($notification->headers)) {
            $headers = array_merge($headers, $notification->headers);
        }
        
        curl_setopt_array($ch, array(
            CURLOPT_POST => TRUE,
            CURLOPT_RETURNTRANSFER => TRUE,
            CURLOPT_SSL_VERIFYPEER => FALSE,
            CURLOPT_HTTPHEADER => $headers,
            CURLOPT_POSTFIELDS => $notification->payload
        ));

        // Send the request
        $response = curl_exec($ch);

        // Check for errors
        if($response === FALSE){
            throw new Exception(curl_error($ch));
        }

        $info = curl_getinfo($ch);

        if ($info['http_code'] <> 201) {
            throw new Exception('Error sending notificaiton: '. $info['http_code'] . ' msg: ' . $response);
        }
    } 

上述方法通知网络集线器，具有正确的正文和标头发送通知的 /messages 端点发送 HTTP POST 请求。

##<a name="complete-tutorial"></a>完成教程
现在您可以通过从一个 PHP 后端发送通知完成入门教程。

初始化客户端通知集线器 （连接字符串和中心命名为示[获得入门的教程]中的替代品）︰ $hub = 新的 NotificationHub （"连接字符串"、"hubname"）; 

然后添加发送代码，具体取决于您目标的移动平台。

### Windows 应用商店和 Windows Phone 8.1 (非 Silverlight)

    $toast = '<toast><visual><binding template="ToastText01"><text id="1">Hello from PHP!</text></binding></visual></toast>';
    $notification = new Notification("windows", $toast);
    $notification->headers[] = 'X-WNS-Type: wns/toast';
    $hub->sendNotification($notification);

### iOS

    $alert = '{"aps":{"alert":"Hello from PHP!"}}';
    $notification = new Notification("apple", $alert);
    $hub->sendNotification($notification);

### Android
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("gcm", $message);
    $hub->sendNotification($notification);

### Windows Phone 8.0 和 8.1 Silverlight

    $toast = '<?xml version="1.0" encoding="utf-8"?>' .
                '<wp:Notification xmlns:wp="WPNotification">' .
                   '<wp:Toast>' .
                        '<wp:Text1>Hello from PHP!</wp:Text1>' .
                   '</wp:Toast> ' .
                '</wp:Notification>';
    $notification = new Notification("mpns", $toast);
    $notification->headers[] = 'X-WindowsPhone-Target : toast';
    $notification->headers[] = 'X-NotificationClass : 2';
    $hub->sendNotification($notification);


### Kindle 火灾
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("adm", $message);
    $hub->sendNotification($notification);

运行您的 PHP 代码，应产生现在显示在目标设备上的通知。


## 下一步行动
本主题介绍如何创建一个简单的 Java REST 客户端通知集线器的。 从这里，您可以︰

* 下载完整的[PHP 其余包装的示例]，其中包含上面的所有代码。
* 继续学习有关 [新新闻教程] 中标记功能通知集线器
* 了解如何在 [通知用户教程] 于单独的用户推送通知


[PHP 其余包装示例]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-php
[获取启动的教程]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/
 

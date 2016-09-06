---
ms.openlocfilehash: a177cc3a52e1fb0e90f0e27aea4b8894ddba0c3f
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="共享的访问签名概览"
   description="共享访问权限签名是什么，它们是如何工作，以及如何使用它们从节点、 PHP，和 C#。"
   services="service-bus,event-hubs"
   documentationCenter="na"
   authors="djrosanova"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-bus"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="07/24/2015"
   ms.author="darosa"/>

# 共享的访问签名

*共享访问权限签名*(SA) 是服务总线，包括事件集线器，通过代理消息 （队列和主题） 的主要安全机制和中继邮件。 本文讨论共享访问签名、 工作方式和如何使用它们以与平台无关的方式。

## SAS 的概述

共享访问权限签名是基于 sha-256 散列或 Uri 的身份验证机制。 SAS 是非常强大的机制使用的所有服务总线服务。 SAS 在实际使用中，有两个组件︰ 一个*共享访问策略*以及*共享访问签名*（通常称为*标记*）。

[共享访问签名使用身份验证服务总线](https://msdn.microsoft.com/library/azure/dn170477.aspx)中，可以找到更多详细的信息与服务总线的共享访问签名。

## 共享的访问策略

了解 SAS 的一点是，这一切开始与策略。 对于每个策略，确定了三条信息︰**名称**、**范围**和**权限**。 **名称**就是结果，;在该范围内唯一的名称。 作用域就很简单︰ 它是涉及资源的 URI。 服务总线的名称空间，范围是完全限定的域名 (FQDN)，如**`https://<yournamespace>.servicebus.windows.net/`**。

策略中的可用的权限有很大程度上一目了然︰

  + 发送
  + 侦听
  + 管理

创建策略后，指派*为主键*和*辅助键*。 这些都是强加密密钥。 不要失去它们或它们泄漏-总是会在门户中可用。 您可以使用两个生成的密钥，并可在任何时候进行再生。 但是，如果您重新生成或更改主键的策略中，从它创建的任何共享访问签名都将无效。

创建服务总线命名空间时，称为**RootManageSharedAccessKey**，整个名称空间中自动创建一个策略，该策略具有的所有权限。 您没有登录为**根**，因此，不要使用此策略，除非有非常好的原因。 在 Azure 管理门户中的命名空间的**配置**选项卡，您可以创建其他策略。 一定要注意单个树服务总线中的级别 (命名空间中，队列，事件中心等) 只能有最多 12 个策略附加到它。

## 共享访问权限签名 （标记）

策略本身不是服务总线的访问令牌。 它是从其生成的访问令牌-它使用的主要或次要的键的对象。 通过仔细编制以下格式的字符串来生成标记︰

```
SharedAccessSignature sig=<signature-string>&se=<expiry>&skn=<keyName>&sr=<URL-encoded-resourceURI>
```

其中`signature-string`是 sha-256 散列标记 （为**作用域**上一节中所述） 的范围，通过追加 CRLF 和到期时间 (自纪元以来的秒数︰ `00:00:00 UTC` 1970 年 1 1 月)。

希看起来类似于下面的伪代码，并返回 32 个字节。

```
SHA-256('https://<yournamespace>.servicebus.windows.net/'+'\n'+ 1438205742)
```

非散列值是在**SharedAccessSignature**字符串，因此收件人可以计算哈希值相同的参数，以确保它返回相同的结果。 URI 指定的作用域，并键名称标识要用于计算哈希值的策略。 这一点从安全的角度看。 如果签名不匹配的收件人 （服务总线） 计算，访问被拒绝。 在这里，我们可以确保发件人必须对密钥的访问应被授予在策略中指定的权限。

## 从策略生成签名

如何真正做到这在代码？ 让我们看一看其中的一些。

### NodeJS

```
function createSharedAccessToken(uri, saName, saKey) { 
    if (!uri || !saName || !saKey) { 
            throw "Missing required parameter"; 
        } 
    var encoded = encodeURIComponent(uri); 
    var now = new Date(); 
    var week = 60*60*24*7;
    var ttl = Math.round(now.getTime() / 1000) + week;
    var signature = encoded + '\n' + ttl; 
    var signatureUTF8 = utf8.encode(signature); 
    var hash = crypto.createHmac('sha256', saKey).update(signatureUTF8).digest('base64'); 
    return 'SharedAccessSignature sr=' + encoded + '&sig=' +  
        encodeURIComponent(hash) + '&se=' + ttl + '&skn=' + saName; 
}
``` 

### Java

```
private static String GetSASToken(String resourceUri, String keyName, String key)
  {
      long epoch = System.currentTimeMillis()/1000L;
      int week = 60*60*24*7;
      String expiry = Long.toString(epoch + week);

      String sasToken = null;
      try {
          String stringToSign = URLEncoder.encode(resourceUri, "UTF-8") + "\n" + expiry;
          String signature = getHMAC256(key, stringToSign);
          sasToken = "SharedAccessSignature sr=" + URLEncoder.encode(resourceUri, "UTF-8") +"&sig=" +
                  URLEncoder.encode(signature, "UTF-8") + "&se=" + expiry + "&skn=" + keyName;
      } catch (UnsupportedEncodingException e) {

          e.printStackTrace();
      }

      return sasToken;
  }


public static String getHMAC256(String key, String input) {
    Mac sha256_HMAC = null;
    String hash = null;
    try {
        sha256_HMAC = Mac.getInstance("HmacSHA256");
        SecretKeySpec secret_key = new SecretKeySpec(key.getBytes(), "HmacSHA256");
        sha256_HMAC.init(secret_key);
        Encoder encoder = Base64.getEncoder();

        hash = new String(encoder.encode(sha256_HMAC.doFinal(input.getBytes("UTF-8"))));

    } catch (InvalidKeyException e) {
        e.printStackTrace();
    } catch (NoSuchAlgorithmException e) {
        e.printStackTrace();
   } catch (IllegalStateException e) {
        e.printStackTrace();
    } catch (UnsupportedEncodingException e) {
        e.printStackTrace();
    }

    return hash;
}
```

### PHP

```
function generateSasToken($uri, $sasKeyName, $sasKeyValue) 
{ 
$targetUri = strtolower(rawurlencode(strtolower($uri))); 
$expires = time();  
$expiresInMins = 60; 
$week = 60*60*24*7;
$expires = $expires + $week; 
$toSign = $targetUri . "\n" . $expires; 
$signature = rawurlencode(base64_encode(hash_hmac('sha256',             
 $toSign, $sasKeyValue, TRUE))); 

$token = "SharedAccessSignature sr=" . $targetUri . "&sig=" . $signature . "&se=" . $expires .      "&skn=" . $sasKeyName; 
return $token; 
}
```
 
### C & #35;

```
private static string createToken(string resourceUri, string keyName, string key)
{
    TimeSpan sinceEpoch = DateTime.UtcNow - new DateTime(1970, 1, 1);
    var week = 60 * 60 * 24 * 7;
    var expiry = Convert.ToString((int)sinceEpoch.TotalSeconds + week);
    string stringToSign = HttpUtility.UrlEncode(resourceUri) + "\n" + expiry;
    HMACSHA256 hmac = new HMACSHA256(Encoding.UTF8.GetBytes(key));
    var signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(stringToSign)));
    var sasToken = String.Format(CultureInfo.InvariantCulture, "SharedAccessSignature sr={0}&sig={1}&se={2}&skn={3}", HttpUtility.UrlEncode(resourceUri), HttpUtility.UrlEncode(signature), expiry, keyName);
    return sasToken;
}
```

## （在 HTTP 级别） 上使用共享访问签名
 
现在，您将知道如何为服务总线中的所有实体创建共享访问签名，您就可以执行 HTTP POST:

```
POST https://<yournamespace>.servicebus.windows.net/<yourentity>/messages
Content-Type: application/json
Authorization: SharedAccessSignature sr=https%3A%2F%2F<yournamespace>.servicebus.windows.net%2F<yourentity>&sig=<yoursignature from code above>&se=1438205742&skn=KeyName
ContentType: application/atom+xml;type=entry;charset=utf-8
``` 
    
请记住，这适用于一切。 您可以创建 SA 队列、 主题、 订阅、 事件中心或中继。 如果您使用每个发布者的身份对事件集线器时，只需追加`/publishers/< publisherid>`。

如果允许的发件人或客户端 SAS 令牌，直接，他们没有使用密钥和它们不能反转的哈希值来获取它。 在这种情况下，可以控制通过它们可以访问，以及多长时间。 需要记住的重要一点是如果要更改策略中的主关键字，将失效从中创建任何共享访问签名。

## 使用共享访问签名 （AMQP 等级）

在前面的部分中，您看到了如何使用 HTTP POST 请求用于将数据发送到服务总线的 SAS 令牌。 如您所知，您可以访问服务总线使用 AMQP （高级消息队列协议） 协议是主要和首选出于性能原因，在许多方案中使用的协议。 AMQP SAS 令牌使用所述下列文档[AMQP Claim-Based 安全版本 1.0](https://www.oasis-open.org/committees/download.php/50506/amqp-cbs-v1%200-wd02%202013-08-12.doc)中的工作草案自 2013年，但它很好地支持 Azure 今天。

在开始将数据发送到服务总线之前, 需要将 SAS 标记位于 AMQP 消息发送到名为**"$cbs"**的明确定义 AMQP 节点发布服务器 （您可以看到它像一个"特殊"的队列服务使用来获取和验证 SAS 的所有标记）。 出版商需要指定的**"回复"**字段位于 AMQP 消息;这是位置服务将答复标记验证 （发布服务器和服务之间的简单的请求/应答模式） 的结果与发布服务器的节点。 此回复节点创建动态"讲话关于"远程节点的动态创建"述 AMQP 1.0 规范。 检查 SAS 令牌无效之后, 发布服务器可以前进，开始向服务发送数据。

下面的步骤将演示如何通过使用[AMQP.Net 精简](http://amqpnetlite.codeplex.com)库很有用，如果您不能使用正式的服务总线 SDK AMQP 协议发送 SAS 令牌 (例如在 WinRT，.Net 框架精简版、.Net 微框架和单声道) C & #35; 在开发。 Corse，此库将有助于理解 AMQP 级别基于声明的安全工作，正如您看到的 HTTP 级别 （包括 HTTP POST 请求和授权标头中发送的 SAS 令牌） 的工作方式。 不过，别担心 ！ 如果不需要 AMQP 有关此类深层知识，则可以使用官方服务总线 SDK 与.Net 框架的应用程序将为您或所有其他平台 （请参见上面的信息） 的[Azure SB 精简](http://azuresblite.codeplex.com)库完成它。

### C & #35;

```
/// <summary>
/// Send Claim Based Security (CBS) token
/// </summary>
/// <param name="shareAccessSignature">Shared access signature (token) to send</param>
private bool PutCbsToken(Connection connection, string sasToken)
{
    bool result = true;
    Session session = new Session(connection);

    string cbsClientAddress = "cbs-client-reply-to";
    var cbsSender = new SenderLink(session, "cbs-sender", "$cbs");
    var cbsReceiver = new ReceiverLink(session, cbsClientAddress, "$cbs");

    // construct the put-token message
    var request = new Message(sasToken);
    request.Properties = new Properties();
    request.Properties.MessageId = "1";
    request.Properties.ReplyTo = cbsClientAddress;
    request.ApplicationProperties = new ApplicationProperties();
    request.ApplicationProperties["operation"] = "put-token";
    request.ApplicationProperties["type"] = "servicebus.windows.net:sastoken";
    request.ApplicationProperties["name"] = Fx.Format("amqp://{0}/{1}", sbNamespace, entity);
    cbsSender.Send(request);

    // receive the response
    var response = cbsReceiver.Receive();
    if (response == null || response.Properties == null || response.ApplicationProperties == null)
    {
        result = false;
    }
    else
    {
        int statusCode = (int)response.ApplicationProperties["status-code"];
        if (statusCode != (int)HttpStatusCode.Accepted && statusCode != (int)HttpStatusCode.OK)
        {
            result = false;
        }
    }

    // the sender/receiver may be kept open for refreshing tokens
    cbsSender.Close();
    cbsReceiver.Close();
    session.Close();

    return result;
}
```

上面的*PutCbsToken()*方法接收*连接*（AMQP 连接类实例作为提供由 AMQP.Net 精简库） 表示 TCP 连接到服务和要发送的 SAS 标记的*sasToken*参数。 注︰ 务必使用**SASL 验证机制设置为外部**（和不使用用户名和密码不需要发送的 SAS 标记时使用的默认纯） 创建的连接。

下一步发布服务器上创建两个 AMQP 链接发送的 SAS 标记和从服务接收答复 （令牌验证结果）。

AMQP 消息为具有属性和简单的信息相比，它的详细信息的一组复杂的 litte。 SAS 标记将作为邮件正文中 （使用其构造函数）。 **"回复"**属性设置为用于接收 （您可以更改它的名称所需，将由服务动态创建） 的接收器链接验证结果节点的名称。 服务使用的最后三个应用程序/自定义属性来了解它有执行何种操作。 它们必须是**操作名称**（因此"传令牌"），**键入的标记**会使 CBS 草案规范所述 (因此"servicebus.windows.net:sastoken") 和最后**访问群体的"名称"** （整个实体） 应用于标记。

发送后 SAS 令牌发件人链接，发布服务器需要读取接收器链接上的回复。 答复是与应用程序属性，名为**"状态代码"**可以包含相同的 HTTP 状态代码值的简单 AMQP 消息。 

## 下一步行动

[服务总线 REST API 参考](https://msdn.microsoft.com/library/azure/hh780717.aspx)有关如何使用这些 SAS 令牌的详细信息，请参阅。

有关 SA 的详细信息，请参阅 MSDN 上的[服务总线身份验证](https://msdn.microsoft.com/library/azure/dn155925.aspx)节点。 在 C# 的 SAS 和 Java 脚本[Damir 的博客](http://developers.de/blogs/damir_dobric/archive/2013/10/17/how-to-create-shared-access-signature-for-service-bus.aspx)中有关更多示例


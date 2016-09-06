---
ms.openlocfilehash: 353cb852535849dfb6420e7ef7edc0933be60d6c
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何使用 Java 服务总线 API AMQP 1.0 |Microsoft Azure" 
    metakeywords="Java Messsage AMQP, Service Bus AMQP, download AMQP JMS library" 
    description="了解如何使用 Azure 服务总线和高级消息队列 Protodol (AMQP) 1.0 中使用 Java 消息服务 (JMS)。" 
    authors="sethmanheim" 
    documentationCenter="java" 
    writer="sethm" 
    manager="timlt" 
    editor="" 
    services="service-bus"/>

<tags 
    ms.service="service-bus" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="java" 
    ms.topic="article" 
    ms.date="07/21/2015" 
    ms.author="sethm"/>


# 如何使用 Java 消息服务 (JMS) API 服务总线与 AMQP 1.0

高级消息队列协议 (AMQP) 1.0 是高效、 可靠、 不是线级的消息传递协议可以用来构建可靠、 跨平台的消息应用程序。 AMQP 1.0 支持添加到 Azure 服务总线在 2012 年 10 月，并在 5 月 2013年转换到常规可用性 (GA)。

AMQP 1.0 的增加意味着它，现在可以利用队列和发布/订阅的服务总线具有邮件功能从一系列平台使用高效的二进制协议。 此外，您可以生成应用程序使用的语言、 框架和操作系统组合构建的组件组成。

本操作指南解释了如何使用使用流行的 Java 消息服务 (JMS) API 标准的 Java 应用程序中的中转服务总线消息传递功能 （队列和发布/订阅的主题）。

## 开始使用服务总线

本指南假定您已经服务总线命名空间包含队列名为"queue1"。 如果不这样做，，您可以创建的命名空间和队列使用[Azure 管理门户](http://manage.windowsazure.com)。 有关如何创建服务总线命名空间和队列的详细信息，请参阅[如何使用服务总线队列](service-bus-dotnet-how-to-use-queues.md)。

### 下载 AMQP 1.0 JMS 客户端库

要下载最新版本的 Apache Qpid JMS AMQP 1.0 客户端库的信息，请访问[http://people.apache.org/~rgodfrey/qpid-java-amqp-1-0-client-jms.html](http://people.apache.org/~rgodfrey/qpid-java-amqp-1-0-client-jms.html)。

在生成和使用服务总线运行 JMS 应用程序时，必须添加从 Apache Qpid JMS AMQP 1.0 分发存档到 Java 类路径中的以下四个 JAR 文件︰

*    geronimo jms\_1.1\_规范 1.0.jar
*    qpid-amqp-1-0-client-[version].jar
*    qpid-amqp-1-0-client-jms-[version].jar
*    qpid-amqp-1-0-common-[version].jar

## Java 应用程序的编码

### Java 命名和目录接口 (JNDI)

JMS 使用 Java 命名和目录接口 (JNDI) 来创建逻辑名称和物理名称之间的分隔。 两种类型的 JMS 对象使用 JNDI 解析︰ ConnectionFactory 和目标。 JNDI 使用提供程序模型可以插入不同的目录服务来处理名称解析职责。 Apache Qpid JMS AMQP 1.0 库附带了一个简单的属性将使用以下属性文件进行配置的基于文件的 JNDI 提供格式设置︰

```
# servicebus.properties – sample JNDI configuration
    
# Register a ConnectionFactory in JNDI using the form:
# connectionfactory.[jndi_name] = [ConnectionURL]
connectionfactory.SBCF = amqps://[username]:[password]@[namespace].servicebus.windows.net
    
# Register some queues in JNDI using the form
# queue.[jndi_name] = [physical_name]
# topic.[jndi_name] = [physical_name]
queue.QUEUE = queue1
```

#### 配置 ConnectionFactory

用于定义**ConnectionFactory** Qpid 属性文件 JNDI 提供程序中的项是以下格式的︰

```
connectionfactory.[jndi_name] = [ConnectionURL]
```

在**[jndi_name]**和**[ConnectionURL]**具有以下含义︰

- **[jndi_name]**: ConnectionFactory 的逻辑名称。 这是将在 Java 应用程序中使用的 JNDI IntialContext.lookup() 方法解析的名称。
- **[ConnectionURL]**: 到 AMQP 中介将 JMS 库提供所需的信息的 URL。

**ConnectionURL**的格式如下所示︰

```
amqps://[username]:[password]@[namespace].servicebus.windows.net
```

其中**[命名空间]**、 **[用户名]**和**[口令]**具有以下含义︰

- **[命名空间]**: 服务总线命名空间。
- **[用户名]**: 服务总线颁发者名称。
- **[口令]**: URL 编码形式的服务总线颁发者的密钥。

> [AZURE.NOTE] 您必须对进行 URL 编码密码手动。 [Http://www.w3schools.com/tags/ref_urlencode.asp](http://www.w3schools.com/tags/ref_urlencode.asp)提供了一个有用的 URL 编码实用工具。

#### 配置目标

使用 Qpid 属性文件 JNDI 提供程序中定义目标的项是以下格式的︰

```
queue.[jndi_name] = [physical_name]
```
或

```
topic.[jndi_name] = [physical_name]
```

其中**[jndi\_名称]**和**[物理\_名称]**具有以下含义︰

- **[jndi_name]**: 目标位置的逻辑名称。 这是将在 Java 应用程序中使用的 JNDI IntialContext.lookup() 方法解析的名称。
- **[physical_name]**: 指应用程序发送或接收消息的服务总线实体的名称。

> [AZURE.NOTE] 在接收来自服务总线主题订阅时，指定 JNDI 中的物理名称应为主题的名称。 JMS 应用程序代码中创建持久订阅时提供订阅名称。 [服务总线 AMQP 1.0 开发人员指南 》](http://msdn.microsoft.com/library/jj841071.aspx)提供了详细信息在处理从 JMS 服务总线主题订阅。

### 编写 JMS 应用程序

没有特殊的 Api 或通过服务总线使用 JMS 时所需的选项。 不过，有将稍后介绍的一些限制。 因为任何 JMS 应用程序中，第一件工作需要 JNDI 环境，以便能够解决**ConnectionFactory**和目标的配置。

#### 配置 JNDI InitialContext

JNDI 环境配置到 javax.naming.InitialContext 类的构造函数中传递的配置信息的哈希表。 这两个必需类名的初始上下文工厂和提供程序 URL 的哈希表中的元素。 下面的代码演示如何配置 JNDI 环境使用 Qpid 属性文件基于 JNDI 提供程序与一个名为**servicebus.properties**的属性文件。

```
Hashtable<String, String> env = new Hashtable<String, String>(); 
env.put(Context.INITIAL_CONTEXT_FACTORY, "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory"); 
env.put(Context.PROVIDER_URL, "servicebus.properties"); 
InitialContext context = new InitialContext(env);
``` 

### 一个简单的 JMS 应用程序使用服务总线队列

下面的示例程序将 JMS TextMessages 发送到服务总线队列的队列中，逻辑 JNDI 名称并接收返回的消息。

    // SimpleSenderReceiver.java
    
    import javax.jms.*;
    import javax.naming.Context;
    import javax.naming.InitialContext;
    import java.io.BufferedReader;
    import java.io.InputStreamReader;
    import java.util.Hashtable;
    import java.util.Random;
    
    public class SimpleSenderReceiver implements MessageListener {
        private static boolean runReceiver = true;
        private Connection connection;
        private Session sendSession;
        private Session receiveSession;
        private MessageProducer sender;
        private MessageConsumer receiver;
        private static Random randomGenerator = new Random();
    
        public SimpleSenderReceiver() throws Exception {
            // Configure JNDI environment
            Hashtable<String, String> env = new Hashtable<String, String>();
            env.put(Context.INITIAL_CONTEXT_FACTORY, 
                    "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory");
            env.put(Context.PROVIDER_URL, "servicebus.properties");
            Context context = new InitialContext(env);
    
            // Lookup ConnectionFactory and Queue
            ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCF");
            Destination queue = (Destination) context.lookup("QUEUE");
    
            // Create Connection
            connection = cf.createConnection();
    
            // Create sender-side Session and MessageProducer
            sendSession = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
            sender = sendSession.createProducer(queue);
    
            if (runReceiver) {
                // Create receiver-side Session, MessageConsumer,and MessageListener
                receiveSession = connection.createSession(false, Session.CLIENT_ACKNOWLEDGE);
                receiver = receiveSession.createConsumer(queue);
                receiver.setMessageListener(this);
                connection.start();
            }
        }
    
        public static void main(String[] args) {
            try {
    
                if ((args.length > 0) && args[0].equalsIgnoreCase("sendonly")) {
                    runReceiver = false;
                }
    
                SimpleSenderReceiver simpleSenderReceiver = new SimpleSenderReceiver();
                System.out.println("Press [enter] to send a message. Type 'exit' + [enter] to quit.");
                BufferedReader commandLine = new java.io.BufferedReader(new InputStreamReader(System.in));
    
                while (true) {
                    String s = commandLine.readLine();
                    if (s.equalsIgnoreCase("exit")) {
                        simpleSenderReceiver.close();
                        System.exit(0);
                    } else {
                        simpleSenderReceiver.sendMessage();
                    }
                }
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    
        private void sendMessage() throws JMSException {
            TextMessage message = sendSession.createTextMessage();
            message.setText("Test AMQP message from JMS");
            long randomMessageID = randomGenerator.nextLong() >>>1;
            message.setJMSMessageID("ID:" + randomMessageID);
            sender.send(message);
            System.out.println("Sent message with JMSMessageID = " + message.getJMSMessageID());
        }
    
        public void close() throws JMSException {
            connection.close();
        }
    
        public void onMessage(Message message) {
            try {
                System.out.println("Received message with JMSMessageID = " + message.getJMSMessageID());
                message.acknowledge();
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }   

### 运行应用程序

运行该应用程序将生成以下输出︰

```
> java SimpleSenderReceiver
Press [enter] to send a message. Type 'exit' + [enter] to quit.
    
Sent message with JMSMessageID = ID:2867600614942270318
Received message with JMSMessageID = ID:2867600614942270318
    
Sent message with JMSMessageID = ID:7578408152750301483
Received message with JMSMessageID = ID:7578408152750301483
    
Sent message with JMSMessageID = ID:956102171969368961
Received message with JMSMessageID = ID:956102171969368961
exit
```

## 交叉平台消息传送 JMS 和.NET 之间

本指南介绍了如何发送和接收消息的服务总线与使用 JMS。 但是，AMQP 1.0 的主要优点之一是它使应用程序可以从组件中不同的语言，用交换可靠地和完全可靠的消息。

使用上面描述的示例 JMS 应用程序和来自配套指南，[如何使用与.NET 服务总线.NET API AMQP 1.0](service-bus-dotnet-advanced-message-queuing.md)，类似的.NET 应用程序可以交换.NET 和 Java 之间的消息。 

有关交叉平台消息传送使用服务总线和 AMQP 1.0 的详细信息，请参阅[服务总线 AMQP 1.0 开发人员指南 》](http://msdn.microsoft.com/library/jj841071.aspx)。

### 为.NET 的 JMS

演示.NET 消息到 JMS:

* 启动.NET 示例应用程序不使用任何命令行参数。
* 用"sendonly"命令行参数启动的 Java 示例应用程序。 在此模式下，应用程序不会收到消息从队列，它才会发送。
* 按**enter 键**几次在 Java 应用程序控制台中，将导致要发送的消息。
* 由.NET 应用程序接收这些消息。

#### 从 JMS 应用程序的输出

```
> java SimpleSenderReceiver sendonly
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Sent message with JMSMessageID = ID:4364096528752411591
Sent message with JMSMessageID = ID:459252991689389983
Sent message with JMSMessageID = ID:1565011046230456854
exit
```

#### .NET 应用程序的输出

```
> SimpleSenderReceiver.exe  
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Received message with MessageID = 4364096528752411591
Received message with MessageID = 459252991689389983
Received message with MessageID = 1565011046230456854
exit
```

### 对 JMS 的.NET

演示.NET 到 JMS 消息传递︰

* 用"sendonly"命令行参数启动.NET 示例应用程序。 在此模式下，应用程序不会收到消息从队列，它才会发送。
* 启动 Java 示例应用程序不使用任何命令行参数。
* 按**enter 键**几次在.NET 应用程序控制台中，将导致要发送的消息。
* 通过 Java 应用程序来接收这些消息。

#### .NET 应用程序的输出

```
> SimpleSenderReceiver.exe sendonly
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Sent message with MessageID = d64e681a310a48a1ae0ce7b017bf1cf3  
Sent message with MessageID = 98a39664995b4f74b32e2a0ecccc46bb
Sent message with MessageID = acbca67f03c346de9b7893026f97ddeb
exit
```

#### 从 JMS 应用程序的输出

```
> java SimpleSenderReceiver 
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Received message with JMSMessageID = ID:d64e681a310a48a1ae0ce7b017bf1cf3
Received message with JMSMessageID = ID:98a39664995b4f74b32e2a0ecccc46bb
Received message with JMSMessageID = ID:acbca67f03c346de9b7893026f97ddeb
exit
```

## 不支持的功能和限制

即通过服务总线与 AMQP 1.0 中使用 JMS 时存在以下限制︰

* 每个**会话**允许只有一个**MessageProducer**或**MessageConsumer** 。 如果您需要在应用程序中创建多个**MessageProducers**或**MessageConsumers** ，请为每个创建专用的**会话**。
* 目前不支持易失性主题的订阅。
* 目前不支持**MessageSelectors** 。
* 临时的目标，即**TemporaryQueue**、 **TemporaryTopic**是目前不支持，以及**QueueRequestor**和**TopicRequestor**使用他们的 Api。
* 不支持事务处理的会话和分布式的事务。

## 摘要

本帮助指南演示了如何使用服务总线中转邮件功能 （队列和发布/订阅主题） 从 Java 使用流行的 JMS API 和 AMQP 1.0。

您还可以从其他语言版本，包括.NET、 C，Python 和 PHP 使用服务总线 AMQP 1.0。 使用这些不同的语言构建的组件可以可靠地交换信息和在使用 AMQP 1.0 的完全保真的支持服务总线中。 有关详细信息，请参阅[服务总线 AMQP 1.0 开发人员指南 》](http://msdn.microsoft.com/library/jj841071.aspx)。

## 下一步行动

* [AMQP 1.0 在 Azure 服务总线的支持](service-bus-amqp-overview.md)
* [如何使用服务总线.NET API AMQP 1.0](service-bus-dotnet-advanced-message-queuing.md)
* [服务总线 AMQP 1.0 开发人员指南 》](http://msdn.microsoft.com/library/jj841071.aspx)
* [如何使用服务总线队列](service-bus-dotnet-how-to-use-queues.md)
 

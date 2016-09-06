---
ms.openlocfilehash: 401fa324c9497ee44db0db480a4c86be5f32fd26
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
## 将消息发送到事件集线器
在本节中，我们将编写一个 Java 控制台应用程序，将事件发送到事件中心。 我们将会使用 JMS AMQP 从[Apache Qpid 项目](http://qpid.apache.org/)的提供程序。 这是类似于 AMQP 通过 Java 作为显示[在此处](../articles/service-bus/service-bus-java-how-to-use-jms-api-amqp.md)使用服务总线队列和主题。 有关详细信息，请参阅[Qpid JMS 文档](http://qpid.apache.org/releases/qpid-0.30/programming/book/QpidJMS.html)和[Java 消息服务](http://www.oracle.com/technetwork/java/jms/index.html)。

1. 在 Eclipse 中，安装了[Eclipse 的 Azure Toolkit](https://msdn.microsoft.com/library/azure/hh690946.aspx)。 这包括 Qpid JMS AMQP 客户端库。

2. 在 Eclipse 中，将创建一个名为**发件人**的新的 Java 项目。

3. 日蚀式包资源管理器中用鼠标右键单击**发件人**项目，并选择**属性**。 在对话框的左窗格中，单击**Java 构建路径**，然后单击**库**选项卡，然后单击**添加库**按钮。 选择**包的 Apache Qpid （通过 MS 打开技术） 的 JMS 客户端库**，请单击**下一步**，然后单击**完成**。

    ![][8]

4. 创建名为**servicebus.properties** ，**发件人**项目中，使用以下内容的根目录中的文件。 记住要使用的值替换您︰
    - 事件中心的名称。
    - 命名空间名称 (后者通常是`{event hub name}-ns`)。
    - URL 编码**SendRule**的密钥 （您记下此注册表项时创建事件中心）。 您可以进行 URL 编码它[这里](http://www.w3schools.com/tags/ref_urlencode.asp)。

            # servicebus.properties - sample JNDI configuration

            # Register a ConnectionFactory in JNDI using the form:
            # connectionfactory.[jndi_name] = [ConnectionURL]
            connectionfactory.SBCF = amqps://SendRule:{Send Rule key}@{namespace name}.servicebus.windows.net/?sync-publish=false

            # Register some queues in JNDI using the form
            # queue.[jndi_name] = [physical_name]
            # topic.[jndi_name] = [physical_name]
            queue.EventHub = {event hub name}

5. 创建一个名为**发件人**的新类。 添加以下`import`语句︰

        import java.io.BufferedReader;
        import java.io.IOException;
        import java.io.InputStreamReader;
        import java.io.UnsupportedEncodingException;
        import java.util.Hashtable;

        import javax.jms.BytesMessage;
        import javax.jms.Connection;
        import javax.jms.ConnectionFactory;
        import javax.jms.Destination;
        import javax.jms.JMSException;
        import javax.jms.MessageProducer;
        import javax.jms.Session;
        import javax.naming.Context;
        import javax.naming.InitialContext;
        import javax.naming.NamingException;

6. 然后，将下面的代码添加到它︰

        public static void main(String[] args) throws NamingException,
                JMSException, IOException, InterruptedException {
            // Configure JNDI environment
            Hashtable<String, String> env = new Hashtable<String, String>();
            env.put(Context.INITIAL_CONTEXT_FACTORY,
                    "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory");
            env.put(Context.PROVIDER_URL, "servicebus.properties");
            Context context = new InitialContext(env);

            ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCF");

            Destination queue = (Destination) context.lookup("EventHub");

            // Create Connection
            Connection connection = cf.createConnection();

            // Create sender-side Session and MessageProducer
            Session sendSession = connection.createSession(false,
                    Session.AUTO_ACKNOWLEDGE);
            MessageProducer sender = sendSession.createProducer(queue);

            System.out.println("Press Ctrl-C to stop the sender process");
            System.out.println("Press Enter to start now");
            BufferedReader commandLine = new java.io.BufferedReader(
                    new InputStreamReader(System.in));
            commandLine.readLine();

            while (true) {
                sendBytesMessage(sendSession, sender);
                Thread.sleep(200);
            }
        }

        private static void sendBytesMessage(Session sendSession, MessageProducer sender) throws JMSException, UnsupportedEncodingException {
            BytesMessage message = sendSession.createBytesMessage();
            message.writeBytes("Test AMQP message from JMS".getBytes("UTF-8"));
            sender.send(message);
            System.out.println("Sent message");
        }



<!-- Links -->
[Azure 的管理门户]: https://manage.windowsazure.com/


<!-- Images -->
[8]: ./media/service-bus-event-hubs-getstarted/create-sender-java1.png

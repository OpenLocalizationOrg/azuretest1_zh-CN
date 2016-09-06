---
ms.openlocfilehash: e21273836bfd324ba96752d365e4172096e1c10a
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
## 什么是服务总线主题和订阅？

服务总线主题和订阅支持*发布/订阅*消息传递通信模型。 使用主题和订阅时，分布式应用程序的组件不直接相互通信;相反他们交换通过主题，充当中介的消息。

![TopicConcepts](./media/service-bus-java-how-to-create-topic/sb-topics-01.png)

与单一使用者，在其中处理每个消息的服务总线队列不同主题和订阅提供了"一多"形式的通信，使用发布/订阅模式。 就可以注册多个订阅的主题。 当邮件发送到某个主题时，它然后进行，到每个订阅可用以独立句柄/处理。

对一个主题的订阅类似于虚拟队列接收已发送到该主题的消息的副本。 （可选） 可以注册筛选器规则在每个订阅，以便您可以限制筛选器/由哪些主题订阅接收到某个主题的邮件主题。

服务总线主题和订阅使您能够扩展规模以跨越很大数量的用户和应用程序处理的邮件数量非常大。

## 创建服务命名空间

若要开始使用 Azure 服务总线主题和订阅，您必须先创建服务命名空间。 服务命名空间用于解决应用程序中的服务总线资源提供作用域容器。

若要创建服务命名空间︰

1.  登录到[Azure 的管理门户][]。

2.  在管理门户的左侧的导航窗格中，单击**服务总线**。

3.  在管理门户的下部窗格中，单击**创建**。
    ![][0]

4.  在**添加新的名称空间**对话框中，输入命名空间名称。
    系统将立即检查该名称可用。
    ![][2]

5.  确保命名空间名称是可用后，选择的国家或地区应在其中托管您的名称空间 （请确保您使用同一个国家/地区部署计算资源）。

    重要提示︰ 选择您打算部署您的应用程序中选择**相同的区域**。 这将使您获得最佳的性能。

6.  在对话框中的其他字段留下它们的默认值 （**消息传送**和**标准层**），然后单击复选标记。 现在，系统将创建服务命名空间，并使它。 您可能需要等待几分钟为系统资源调配资源，为您的帐户。

    ![][6]


## 获取默认命名空间的管理凭据

为了执行管理操作，例如创建新的命名空间，一个主题或订阅，您必须获取命名空间的管理凭据。 从 Azure 的管理门户，您可以获得这些凭据。

### 要获得该门户的管理凭据

1.  在左侧的导航窗格中，单击**服务总线**节点以显示可用的命名空间的列表︰![][0]

2.  刚才从显示的列表中创建的命名空间，请单击︰![][3]

3.  单击以查看您的名称空间的共享的访问策略的**配置**。
    ![](./media/service-bus-java-how-to-create-topic/sb-queues-14.png)

4.  记下主键中，或将其复制到剪贴板。


  [Azure 的管理门户]: http://manage.windowsazure.com
  [0]: ./media/service-bus-java-how-to-create-topic/sb-queues-13.png
  [2]: ./media/service-bus-java-how-to-create-topic/sb-queues-04.png
  [3]: ./media/service-bus-java-how-to-create-topic/sb-queues-09.png
  [4]: ./media/service-bus-java-how-to-create-topic/sb-queues-06.png

  [6]: ./media/service-bus-java-how-to-create-topic/getting-started-multi-tier-27.png
  [34]: ./media/service-bus-java-how-to-create-topic/VSProperties.png

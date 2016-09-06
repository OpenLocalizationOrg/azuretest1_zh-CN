---
ms.openlocfilehash: 884106fd7f086e6a06ec97bd04d9bd9ea64df46b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
## 服务总线队列有哪些？

服务总线队列支持**具有消息**通信模型。 分布式应用程序的组件时使用的队列，不能直接与对方;而在交换消息的队列，担当媒介 （代理） 通过。 消息创建者 （发送方） 移交将消息发送到队列，然后继续进行处理。 异步消息使用者 （接收器） 将消息从队列中提取并处理它。 制造者不必等待从使用者的答复以便继续处理，并向您发送其他消息。 队列提供了**第一个在中，先出 (FIFO)**消息传递给一个或多个相互竞争的使用者。 也就是说，消息通常接收和处理的顺序在其中添加到队列中，每个消息进行接收和处理者只有一个消息接收器。

![QueueConcepts](./media/howto-service-bus-queues/sb-queues-08.png)

服务总线队列是一种通用的技术，可用于各种不同的情形︰

-   多层 Azure 应用程序中的 web 和辅助角色之间的通信。
-   内部应用程序和 Azure 之间的通讯承载混合解决方案中的应用程序。
-   分布式应用程序运行在不同的组织或组织部门内部的组件之间进行通信。

使用队列使您能够更轻松地扩展您的应用程序，并使您的体系结构的更多弹性。

## 创建服务命名空间

要开始使用在 Azure 服务总线队列，必须先创建一个服务的命名空间。 命名空间提供用于解决应用程序中的服务总线资源的作用域容器。

若要创建服务命名空间︰

1.  登录到[Azure 的管理门户][]。

2.  在管理门户的左侧的导航窗格中，单击**服务总线**。

3.  在管理门户的下部窗格中，单击**创建**。
    ![](./media/howto-service-bus-queues/sb-queues-03.png)

4.  在**添加新的名称空间**对话框中，输入命名空间名称。 系统将立即检查该名称可用。   
    ![](./media/howto-service-bus-queues/sb-queues-04.png)

5.  确保命名空间名称是可用后，选择的国家或地区应在其中托管您的名称空间 （请确保您使用同一个国家/地区部署计算资源）。

    重要提示︰ 选择您打算部署您的应用程序中选择**相同的区域**。 这将使您获得最佳的性能。

6.  在对话框中的其他字段留下它们的默认值 （**消息传送**和**标准层**），然后单击复选标记。 系统现在创建您的命名空间，并使它。 您可能需要等待几分钟为系统资源调配资源，为您的帐户。

    ![](./media/howto-service-bus-queues/getting-started-multi-tier-27.png)

您所创建的名称空间花费一点时间来激活，然后会出现在管理门户。 等待，直到在继续操作之前命名空间状态处于**活动状态**。

## 获取默认命名空间的管理凭据

为了执行管理操作，例如创建新的命名空间，在队列，您必须获取命名空间的管理凭据。 从 Azure 管理门户或 Visual Studio 服务器资源管理器，您可以获得这些凭据。

###要获得该门户的管理凭据

1.  在左侧的导航窗格中，单击**服务总线**节点，以显示可用的命名空间的列表︰   
    ![](./media/howto-service-bus-queues/sb-queues-13.png)

2.  选择您刚创建从显示的列表的名称空间︰   
    ![](./media/howto-service-bus-queues/sb-queues-09.png)

3.  单击**连接信息**。   
    ![](./media/howto-service-bus-queues/sb-queues-06.png)

4.  在**访问连接信息**窗格中，查找包含 SAS 键和键名称的连接字符串。   

    ![](./media/howto-service-bus-queues/multi-web-45.png)
    
5.  记下密钥，或将其复制到剪贴板。

### 从服务器资源管理器获取管理凭据

获取管理门户中使用 Visual Studio 的连接信息，请按照所描述的过程[在此处](http://msdn.microsoft.com/library/ff687127.aspx)，**连接到 Azure 从 Visual Studio 的**章节中。 当您登录到 Azure 时， **Azure**树下在服务器资源管理器的**服务总线**节点会自动填充与已经创建的任何命名空间。 用鼠标右键单击任何命名空间，然后单击**属性**以查看在连接字符串和其他元数据与在 Visual Studio 的**属性**窗格中显示此命名空间相关联。 

记下的**SharedAccessKey**值，或将其复制到剪贴板︰

![][34]

  [Azure 的管理门户]: http://manage.windowsazure.com
  [Azure 的管理门户]: http://manage.windowsazure.com

  [34]: ./media/howto-service-bus-queues/VSProperties.png

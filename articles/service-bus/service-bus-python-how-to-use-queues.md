---
ms.openlocfilehash: 522ae5f52b72df914fce58da09dab41ca5889cb5
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何使用服务总线队列 (Python) |Microsoft Azure" 
    description="了解如何使用 Python 从 Azure 服务总线队列。" 
    services="service-bus" 
    documentationCenter="python" 
    authors="huguesv" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="07/06/2015" 
    ms.author="huvalo"/>


# 如何使用服务总线队列

本指南介绍了如何使用服务总线队列。 这些示例用 Python 编写和使用[Python Azure 包][]。 所包含的方案包括**创建队列、 发送和接收邮件**，并**删除队列**。

[AZURE.INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

**注意︰**若要安装 Python 或者[Python Azure 包][]，请参阅[Python 安装指南 》](../python-how-to-install.md)。

## 如何创建队列

**ServiceBusService**对象使您能够使用队列。 添加您希望以编程方式访问 Azure 服务总线在其中任何 Python 文件的顶部附近，下面的代码︰

    from azure.servicebus import ServiceBusService, Message, Queue

下面的代码创建一个**ServiceBusService**对象。 您的命名空间、 共享的访问签名 (SAS) 键名和值替换根中提供、 sharedaccesskeyname 和 sharedaccesskey。

    bus_service = ServiceBusService(
        service_namespace='mynamespace',
        shared_access_key_name='sharedaccesskeyname',
        shared_access_key_value='sharedaccesskey')

SAS 名称的密钥值和值可以找到在 Azure 门户连接信息，或 Visual Studio 的**属性**窗口中 （如前一部分中所示），在服务器资源管理器中选择的服务总线命名空间时。

    bus_service.create_queue('taskqueue')

**create_queue**还支持其他的选项，使您可以重写默认队列设置，例如实时或最大队列大小的邮件时间。 下面的示例将最大队列大小设置为 5 GB 和时间存活 1 分钟的值︰

    queue_options = Queue()
    queue_options.max_size_in_megabytes = '5120'
    queue_options.default_message_time_to_live = 'PT1M'

    bus_service.create_queue('taskqueue', queue_options)

## 如何将消息发送到队列

若要将消息发送到服务总线队列，应用程序调用**发送\_队列\_消息** **ServiceBusService**对象上的方法。

下面的示例演示如何对名为*taskqueue 使用*的队列发送测试消息**发送\_队列\_消息**:

    msg = Message(b'Test Message')
    bus_service.send_queue_message('taskqueue', msg)

服务总线队列支持的最大邮件大小为 256 KB （头，其中包括标准和自定义应用程序属性，可以有的最大大小为 64 KB）。 保留在队列中的消息的数量没有限制，但持有队列消息的总大小没有一个盖帽。 此队列大小被定义在创建时，具有 5 GB 的上限。

## 如何从队列接收消息

接收消息的队列使用从**接收\_队列\_消息** **ServiceBusService**对象上的方法︰

    msg = bus_service.receive_queue_message('taskqueue', peek_lock=False)
    print(msg.body)

在读取时被删除从队列消息参数**查看\_锁**设置为**False**。 您可以阅读 (peek) 并锁定消息但不从队列删除它通过将参数设置**查看\_锁**为**True**。

读取和接收操作的一部分是最简单的模型，以及最适合的应用程序能够容忍不处理发生故障消息的情况下删除该邮件的行为。 要理解这一点，考虑一个方案，其中使用者发出接收请求，然后处理它之前崩溃。 因为服务总线将被标记为正在使用的消息，然后当该应用程序将重新启动，并开始使用消息再次强调，它将丢失了消耗在崩溃之前的消息。

如果**查看\_锁**参数设置为**True**，接收两个阶段的运算复杂度，这使得支持的应用程序不能容忍丢失邮件。 服务总线接收请求时，它查找下一条消息中，要消耗就锁定以防止其他使用者，接收它，，然后将它返回给应用程序。 应用程序完成处理消息 （或可靠地存储以备将来处理后），它会通过**消息**对象调用**delete**方法完成接收过程的第二个阶段。 **Delete**方法将标记如所消耗的消息并从队列中移除它。

    msg = bus_service.receive_queue_message('taskqueue', peek_lock=True)
    print(msg.body)

    msg.delete()

## 如何处理应用程序崩溃和无法读取消息

服务总线提供的功能来帮助您从您的应用程序或困难处理消息中的错误正常恢复。 如果接收方应用程序不能处理该消息，由于某种原因，然后它可以**解锁**方法对象上调用**消息**。 这将导致服务总线来解锁队列中的邮件并使其可再次，同一个使用应用程序或其他使用的应用程序接收。

此外没有与消息在队列中，锁定超时，并且如果应用程序不能处理该消息之前锁定超时过期 （例如，如果应用程序崩溃时），则服务总线将自动解锁消息并使其可再次收到。

在该应用程序崩溃之后处理该消息，但调用**delete**方法之前，然后消息将被重新传递给应用程序重新启动时。 这通常称为**至少处理一次**，即将至少一次处理每个消息，但在某些情况下可能重新发送同样的消息。 如果方案不能容忍重复处理，应用程序开发人员应该向其应用程序来处理传递重复的邮件添加额外的逻辑。 这通常是使用将保持不变在传递尝试的邮件的**邮件 Id**属性实现的。

## 下一步行动

现在，您已经学习了基本的服务总线队列，请按照这些链接以了解详细信息。

-   请参阅 MSDN 参考︰[队列、 主题和订阅][]。

[Azure 的管理门户]: http://manage.windowsazure.com
[Python Azure 包]: https://pypi.python.org/pypi/azure  
[队列、 主题和订阅]: http://msdn.microsoft.com/library/azure/hh367516.aspx
 

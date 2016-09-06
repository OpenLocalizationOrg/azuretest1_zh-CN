---
ms.openlocfilehash: b7a86ccbb674f6801097a17b849b8758b6a74e1f
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何使用服务总线主题 (Python) |Microsoft Azure" 
    description="了解如何使用 Azure 服务总线主题和从 Python 的订阅。" 
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

# 如何使用服务总线主题和订阅

本指南介绍了如何使用服务总线主题和订阅。 这些示例用 Python 编写和使用[Python Azure 包][]。 所包含的方案包括为主题、**接收来自订阅邮件**并**删除主题和订阅****创建主题和创建订阅筛选器，发送消息的订阅**。 有关主题和订阅的详细信息，请参阅[后续步骤](#next-steps)部分。

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

**注意︰**如果您需要安装 Python 或者[Python Azure 包][]，请参阅[Python 安装指南 》](../python-how-to-install.md)。

## 如何创建主题

**ServiceBusService**对象，您可以使用主题。 添加任何您希望以编程方式访问 Azure 服务总线的 Python 文件的顶部附近以下项︰

    from azure.servicebus import ServiceBusService, Message, Topic, Rule, DEFAULT_RULE_NAME

下面的代码创建一个**ServiceBusService**对象。 更换`mynamespace`， `sharedaccesskeyname`，和`sharedaccesskey`与实际命名空间、 共享的访问签名 (SAS) 键名和值。

    bus_service = ServiceBusService(
        service_namespace='mynamespace',
        shared_access_key_name='sharedaccesskeyname',
        shared_access_key_value='sharedaccesskey')

您可以获得 SAS 键名的值和值从 Azure 管理门户连接信息，或在 Visual Studio 的**属性**窗口中 （如前一部分中所示），在服务器资源管理器中选择的服务总线命名空间时。

    bus_service.create_topic('mytopic')

**创建\_主题**还支持其他的选项，使您可以重写默认主题设置，例如消息生存时间或最大主题的大小。 下面的示例将主题最大大小设置为 5 GB，并且一次活的为 1 分钟︰

    topic_options = Topic()
    topic_options.max_size_in_megabytes = '5120'
    topic_options.default_message_time_to_live = 'PT1M'

    bus_service.create_topic('mytopic', topic_options)

## 如何创建订阅

此外带有**ServiceBusService**对象创建主题订阅。 被命名为订阅，并且可以具有可选的筛选，用于限制的邮件传递到预订的虚拟队列集。

**注意**︰ 订阅是持久的和将继续存在直到或者它们或它们是相关联的主题，都将被删除。

### 使用默认值 (MatchAll) 筛选器创建订阅

**MatchAll**过滤器是如果没有筛选器指定创建新订阅时使用的默认筛选器。 当使用**MatchAll**筛选器时，预订的虚拟队列中放置发布到该主题的所有邮件。 下面的示例创建一个名为 AllMessages 的订阅，并使用默认**MatchAll**筛选器。

    bus_service.create_subscription('mytopic', 'AllMessages')

### 使用筛选器创建订阅

您还可以定义筛选器，使您可以指定特定主题订阅中应显示发送到某个主题的消息。

最灵活的一种支持订阅筛选器是**SqlFilter**，它实现的 SQL92 子集。 SQL 筛选器操作的属性发布到该主题的消息。 有关可与 SQL 筛选中使用表达式的详细信息，请参阅[SqlFilter.SqlExpression][]语法。

可以通过使用为订阅添加筛选器**创建\_规则** **ServiceBusService**对象的方法。 此方法允许您将新筛选器添加到现有订阅。

**请注意**︰ 由于默认筛选器自动应用于所有新的订阅，您必须首先删除默认筛选器或**MatchAll**将重写您可能指定的任何其他筛选器。 您可以通过删除默认规则**删除\_规则** **ServiceBusService**对象的方法。

下面的示例创建一个名为的订阅`HighMessages`与**SqlFilter** ，只选择具有自定义**messagenumber**属性大于 3 的消息︰

    bus_service.create_subscription('mytopic', 'HighMessages')

    rule = Rule()
    rule.filter_type = 'SqlFilter'
    rule.filter_expression = 'messagenumber > 3'

    bus_service.create_rule('mytopic', 'HighMessages', 'HighMessageFilter', rule)
    bus_service.delete_rule('mytopic', 'HighMessages', DEFAULT_RULE_NAME)

同样，下面的示例创建名为的订阅`LowMessages`与**SqlFilter** ，只选择具有到**messagenumber**属性小于或等于 3 的消息︰

    bus_service.create_subscription('mytopic', 'LowMessages')

    rule = Rule()
    rule.filter_type = 'SqlFilter'
    rule.filter_expression = 'messagenumber <= 3'

    bus_service.create_rule('mytopic', 'LowMessages', 'LowMessageFilter', rule)
    bus_service.delete_rule('mytopic', 'LowMessages', DEFAULT_RULE_NAME)

当一条消息，现在发送到`mytopic`，它总是发送到接收器的**AllMessages**主题订阅，订阅和有选择地传送到接收方 （取决于消息内容） 的**HighMessages**和**LowMessages**主题订阅订阅。

## 如何将消息发送到的主题

若要将消息发送到服务总线主题，您的应用程序必须使用**发送\_主题\_消息** **ServiceBusService**对象的方法。

下面的示例演示如何发送五个测试邮件到`mytopic`。 请注意，每个消息的**messagenumber**属性值各不相同的循环迭代 （确定哪些订阅接收它）︰

    for i in range(5):
        msg = Message('Msg {0}'.format(i).encode('utf-8'), custom_properties={'messagenumber':i})
        bus_service.send_topic_message('mytopic', msg)

服务总线主题支持的最大邮件大小为 256 MB （头，其中包括标准和自定义应用程序属性，可以有最大为 64 MB）。 保留在主题中的消息的数量没有限制，但上持有的主题的消息的总大小没有一个盖帽。 此主题大小定义在创建时，具有 5 GB 的上限。

## 如何从订阅接收消息

收到的邮件订阅使用**收到\_订阅\_消息** **ServiceBusService**对象上的方法︰

    msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=False)
    print(msg.body)

在读取时被删除从订阅消息参数**查看\_锁**设置为**False**。 您可以阅读 (peek) 并锁定消息但不从队列删除它通过将参数设置**查看\_锁**为**True**。

读取和接收操作的一部分是最简单的模型，以及最适合的应用程序能够容忍不处理发生故障消息的情况下删除该邮件的行为。 要理解这一点，考虑一个方案，其中使用者发出接收请求，然后处理它之前崩溃。 因为服务总线将被标记为正在使用的消息，然后当该应用程序将重新启动，并开始使用消息再次强调，它将丢失了消耗在崩溃之前的消息。


如果**查看\_锁**参数设置为**True**，接收两个阶段的运算复杂度，这使得支持的应用程序不能容忍丢失邮件。 服务总线接收请求时，它查找下一条消息中，要消耗就锁定以防止其他使用者，接收它，，然后将它返回给应用程序。 应用程序完成处理消息 （或可靠地存储以备将来处理后），它会通过**消息**对象调用**delete**方法完成接收过程的第二个阶段。 **Delete**方法将标记该邮件所占用，并从预订中移除。

    msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=True)
    print(msg.body)

    msg.delete()


## 如何处理应用程序崩溃和无法读取消息

服务总线提供的功能来帮助您从您的应用程序或困难处理消息中的错误正常恢复。 如果接收方应用程序不能处理该消息，由于某种原因，然后它可以**解锁**方法对象上调用**消息**。 这将导致服务总线来解锁内订阅消息并使其可再次，同一个使用应用程序或其他使用的应用程序接收。

还有与内订阅，锁定超时，如果应用程序不能处理该消息之前锁定超时过期 （例如，如果应用程序崩溃时），然后服务总线自动解锁消息并使其可供再次接收。

在该应用程序崩溃之后处理该消息，但调用**delete**方法之前，然后消息将被重新传递给应用程序重新启动时。 这通常称为**至少处理一次**，即将至少一次处理每个消息，但在某些情况下可能重新发送同样的消息。 如果方案不能容忍重复处理，应用程序开发人员应该向其应用程序来处理传递重复的邮件添加额外的逻辑。 这通常是使用将保持不变在传递尝试的邮件的**邮件 Id**属性实现的。

## 如何删除主题和订阅

主题和订阅是持久的必须通过 Azure 管理门户或通过编程方式显式删除。 下面的示例演示如何删除名为的主题`mytopic`:

    bus_service.delete_topic('mytopic')

删除某个主题也会删除注册该主题的所有订阅。 也可以单独删除订阅。 下面的代码演示如何删除名为的订阅`HighMessages`'mytopic' 主题︰

    bus_service.delete_subscription('mytopic', 'HighMessages')

## 下一步行动

既然已经了解服务总线主题的基本知识，按照这些链接以了解详细信息。

-   请参阅 MSDN 参考︰[队列、 主题和订阅][]。
-   [SqlFilter.SqlExpression][]的参考资料。

[Azure 的管理门户]: http://manage.windowsazure.com
[Python Azure 包]: https://pypi.python.org/pypi/azure  
[队列、 主题和订阅]: http://msdn.microsoft.com/library/azure/hh367516.aspx
[SqlFilter.SqlExpression]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
 

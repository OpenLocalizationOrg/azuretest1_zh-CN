---
ms.openlocfilehash: 52413355c6adfe5a4bfda4e57c18764e34dbefe1
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
下表列出了特定于服务总线消息配额信息。 事件集线器限制将包含在此表中，但有关事件集线器的更具体信息，请参阅[事件集线器定价](http://azure.microsoft.com/pricing/details/event-hubs/)。 服务总线上的其他配额和定价信息，请参阅[服务总线定价](http://azure.microsoft.com/pricing/details/service-bus/)概述。

|配额名称|Scope|类型|当超出的行为|值|
|---|---|---|---|---|
| 每个 Azure 订阅的命名空间的最大数量|命名空间|静态|通过 Azure 的管理门户，其他命名空间的后续请求将被拒绝。|100|
|队列/主题大小|Entity|定义在创建队列/主题时。|传入邮件将被拒绝并调用代码就会收到一个异常。|1,2,3,4 或 5 GB。<br /><br />如果启用了[分区](https://msdn.microsoft.com/library/dn520246.aspx)，每个主题最大队列大小是 80 GB。|
|在命名空间上的并发连接数|命名空间|静态|其他的连接的后续请求将被拒绝并调用代码就会收到一个异常。 其他操作不受并发 TCP 连接。|NetMessaging: 1000<br /><br />AMQP: 5000|
|在一个队列/主题/订阅的实体上的并发连接数|Entity|静态|其他的连接的后续请求将被拒绝并调用代码就会收到一个异常。 其他操作不受并发 TCP 连接。|限制通过命名空间每个并发连接数的限制。|
|接收队列/主题/订阅实体上的请求的并发的数|Entity|静态|以后收到的请求将被拒绝并调用代码就会收到一个异常。 此配额应用于组合的并发数个关于某个主题的所有订阅接收操作。|5000|
|在中继上并发监听器数目|Entity|静态|其他的连接的后续请求将被拒绝并调用代码就会收到一个异常。|25|
|并发的中继监听器数目|整个系统|静态|其他的连接的后续请求将被拒绝并调用代码就会收到一个异常。|2000|
|每个中继服务命名空间中的所有终结点中继并发连接数|整个系统|静态|-|5000|
|每个服务名称空间的中继终结点数|整个系统|静态|-|10000|
|每个服务名称空间主题/队列的数目|整个系统|静态|创建新的主题或队列的服务命名空间的后续请求将被拒绝。 因此，如果通过管理门户配置，则将生成错误消息。 如果从管理 API 调用，调用代码就会收到异常。|10000<br /><br />主题和队列服务命名空间中的总次数必须小于或等于 10000。|
|每个服务名称空间的分区主题/队列的数目|整个系统|静态|创建新的分区的主题或队列的服务命名空间的后续请求将被拒绝。 因此，如果通过管理门户配置，则将生成错误消息。 如果从管理 API 调用，调用代码就会收到**QuotaExceededException**异常。|100<br /><br />每个分区的队列或主题都将计入每个名称空间的 10000 实体的配额。|
|消息的任何实体名称的最大大小︰ 命名空间、 队列、 主题、 预订或事件的中心|Entity|静态|-|50 个字符|
|事件集线器事件的最大大小|整个系统|静态|-|256 KB|
|消息队列/主题/订阅实体大小|整个系统|静态|超过这些配额的传入邮件将被拒绝并调用代码就会收到一个异常。|最大邮件大小︰ 256 KB。 <br /><br />**请注意**由于系统开销，而这一限制通常是略少于 256 KB。<br /><br />大标题的大小︰ 64 KB<br /><br />属性包中的标头属性的最大数量︰**如何**<br /><br />属性包中的属性的最大大小︰ 没有明确的限制。 受最大标头大小的限制。|
|[NetOnewayRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.netonewayrelaybinding.aspx)和[NetEventRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.neteventrelaybinding.aspx)继电器的邮件大小|整个系统|静态|超过这些配额的传入邮件将被拒绝并调用代码就会收到一个异常。|64 KB
|[HttpRelayTransportBindingElement](https://msdn.microsoft.com/library/microsoft.servicebus.httprelaytransportbindingelement.aspx)和[NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx)继电器的邮件大小|整个系统|静态|-|无限|
|消息队列/主题/订阅实体属性大小|整个系统|静态|将生成**SerializationException**异常。|最大邮件的每个属性的属性大小是 32k。 所有属性的累计大小不能超过 64k。 这适用于整个头[BrokeredMessage](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.aspx)，它同时具有系统属性 （如[序列号](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.sequencenumber.aspx)，消息[标签](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.label.aspx)、[邮件 Id](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx)和等等） 以及用户属性。|
|每个主题的订阅数|整个系统|静态|创建附加订阅该主题的后续请求将被拒绝。 因此，如果通过管理门户配置，将显示一条错误消息。 如果从管理 API 调用调用代码就会收到异常。|2000|
|每个主题的 SQL 筛选数量的数字|整个系统|静态|创建主题的其他筛选器的后续请求将被拒绝并调用代码就会收到一个异常。|2000|
|每个主题的相关筛选器的数目|整个系统|静态|创建主题的其他筛选器的后续请求将被拒绝并调用代码就会收到一个异常。|100000|
|SQL 的筛选器操作的大小|整个系统|静态|用于创建的其他筛选器的后续请求将被拒绝并调用代码就会收到一个异常。|筛选器条件字符串的最大长度︰ 1024 (1 K)。<br /><br />规则操作字符串的最大长度︰ 1024 (1 K)。<br /><br />每个规则操作的表达式的最大数量︰ 32。|
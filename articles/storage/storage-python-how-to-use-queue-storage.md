---
ms.openlocfilehash: fe063d9c5025e867409a419639f57ffd8b8a46fe
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何使用队列存储从 Python |Microsoft Azure" 
    description="了解如何使用 Python 中的 Azure 队列服务创建和删除队列，并插入、 获取，和删除消息。" 
    services="storage" 
    documentationCenter="python" 
    authors="emgerner-msft" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="08/25/2015" 
    ms.author="emgerner"/>

# 如何使用 Python 从队列存储

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]

## 概述

本指南介绍了如何执行使用 Azure 队列存储服务的常见方案。 这些示例用 Python 编写和使用[Python Azure 存储软件包][]。 所包含的方案包括**插入**、**查看**、**获取**、 和**删除**队列的消息，以及**创建和删除队列**。 有关队列的详细信息，请参阅 [下一步行动] [] 部分。

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]


> [AZURE.NOTE] 如果您需要安装 Python 或者[Python Azure 包][]，请参阅[Python 安装指南 》](../python-how-to-install.md)。

## 如何︰ 创建队列

该**队列服务**对象，可以使用队列。 下面的代码创建一个**队列服务**对象。 添加您希望以编程方式访问 Azure 存储任何 Python 文件的顶部附近以下项︰

    from azure.storage.queue import QueueService

下面的代码创建一个**队列服务**对象，该对象使用存储帐户的用户名和帐户密码。 真正的帐户和密钥替换我的帐户和 mykey 派生而来。

    queue_service = QueueService(account_name='myaccount', account_key='mykey')

    queue_service.create_queue('taskqueue')


## 如何︰ 将消息插入到队列

若要将消息插入到队列，请使用**放入\_消息**方法来创建新邮件并将其添加到队列。

    queue_service.put_message('taskqueue', 'Hello World')


## 如何︰ 查看下一条消息

您可以查看在队列前面的消息但不从队列移除它通过调用**查看\_消息**方法。 默认情况下，**查看\_消息**查看单个邮件。

    messages = queue_service.peek_messages('taskqueue')
    for message in messages:
        print(message.message_text)


## 如何︰ 取消排队的下一封邮件

您的代码从两个步骤中的队列中移除消息。 当您调用**获得\_消息**，默认情况下中的队列中获取下一条消息。 从返回的消息**获得\_消息**变得看不到从该队列中读取消息的任何其他代码。 默认情况下，此消息将 30 秒保持不可见。 要从队列中移除该消息，则必须调用**删除\_消息**。 删除邮件这两步过程可确保当您的代码无法处理由于硬件或软件故障的消息，您的代码的另一个实例可以得到同样的消息，然后再试。 您的代码调用**删除\_消息**在处理完邮件后右。

    messages = queue_service.get_messages('taskqueue')
    for message in messages:
        print(message.message_text)
        queue_service.delete_message('taskqueue', message.message_id, message.pop_receipt)


## 如何︰ 更改排队的邮件的内容

您可以更改邮件的就地在队列中的内容。 如果该消息表示的工作任务，可以使用此功能来更新工作任务的状态。 使用下面的代码**更新\_消息**方法来更新的消息。

    messages = queue_service.get_messages('taskqueue')
    for message in messages:
        queue_service.update_message('taskqueue', message.message_id, 'Hello World Again', message.pop_receipt, 0)

## 如何︰ 附加选项出列邮件

有两种方法，您可以自定义消息从队列中的检索。
首先，您可以获取一批消息 （最多 32 个)。 第二，您可以设置隐蔽性更长或更短的超时值，允许您的代码更多或更少的时间来充分处理每条消息。 下面的代码示例使用**获得\_消息**方法在一个调用中获取 16 的消息。 然后它处理每个消息都使用 for 循环。 此外将隐蔽性超时设置为 5 分钟，每一封邮件。

    messages = queue_service.get_messages('taskqueue', numofmessages=16, visibilitytimeout=5*60)
    for message in messages:
        print(message.message_text)
        queue_service.delete_message('taskqueue', message.message_id, message.pop_receipt)

## 如何︰ 获取队列长度

您可以队列中获取消息数的估计值。 **获取\_队列\_元数据**方法询问队列服务返回队列中的元数据和**x-ms-近似的邮件-计数**作索引到返回的词典来查找该计数。
由于消息可以添加或删除队列服务响应您的请求后，才近似结果。

    queue_metadata = queue_service.get_queue_metadata('taskqueue')
    count = queue_metadata['x-ms-approximate-messages-count']

## 如何︰ 删除队列

若要删除队列中包含的所有消息，调用**都删除\_队列**方法。

    queue_service.delete_queue('taskqueue')

## 下一步行动

现在，您已经学习了队列存储的基本知识，按照这些链接以了解更复杂的存储任务。

-   请参阅 MSDN 参考︰[在 Azure 存储和访问数据][]
-   访问[Azure 存储团队博客][]

[存储和访问 Azure 中的数据]: http://msdn.microsoft.com/library/azure/gg433040.aspx
[Azure 存储团队博客]: http://blogs.msdn.com/b/windowsazurestorage/
[Python Azure 包]: https://pypi.python.org/pypi/azure
[Python Azure 存储软件包]: https://pypi.python.org/pypi/azure-storage 
 
